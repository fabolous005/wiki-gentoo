<!-- source: https://wiki.gentoo.org/wiki/OpenRC/Legacy_user_services | group: Gentoo Wiki (Main) | wiki-title: OpenRC/Legacy user services -->
---
title: OpenRC/Legacy user services
url: https://wiki.gentoo.org/wiki/OpenRC/Legacy_user_services
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-03"
fingerprint: "3e0bb9d8350502e5"
license: CC BY-SA 4.0
---

# OpenRC/Legacy user services

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

In comparison with systemd, OpenRC doesn't have the concept of user services. There are still several ways for users to launch background processes: using simple commands, using their shell (\~/.bashrc, \~/.bashprofile, \~/.zshrc, etc), using the autostart function of their desktop environment, or using initial scripts for non xdg-compliant windows managers (\~/.xinitrc, \~/.config/i3/config, etc). The user's dbus session will normally launch background-processes like a notification daemon or a gpg-agent as needed. Desktop environments like Xfce usually offer mechanisms to restart those long-term processes if they unexpectedly stop, and dbus can relaunch services if they stopped, but otherwise most long-term processes run unsupervised.

A [process supervisor](https://wiki.gentoo.org/wiki/Process_supervision) offers two main benefits:

- It can restart a service after a crash or an unexpected end. A script can generally be used to clean the environment before restarting the service.

- It can catch the output of the process (stdout and stderr) and pipe it to a logger utility or to a file.

It also offers minor benefits:

- It offers a common and easy interface for managing different processes: once the service scripts are created, conventional commands like sudo rc-service *service* start/stop or sv up/down/status *service* leverage the whole work. It is convenient when processes that are used regularly require a lot of arguments or a complex syntax.
- Services are not necessarily bound to a dbus-session, to a desktop environment or even to a login session. For users who regularly switch between a console and a graphical environment or between different environments, it might be an interesting feature.

This page tries to present a few workarounds to implement user services on OpenRC-systems, first using OpenRC itself in combination with PAM, then using a third-party application: the daemontools-inspired [Runit](https://wiki.gentoo.org/wiki/Runit). A same approach can be used with daemontools itself or with s6-svscan though.

## Using OpenRC

### Starting a service on behalf of a user with OpenRC

OpenRC, especially the supervise-daemon utility, can start processes for a user when `command_user` is set. Here is an example of an init file for a larry's mpd daemon:

**`/etc/init.d/larry_mpd`**

```
#!/sbin/openrc-run
supervisor=supervise-daemon
USER=larry
MPDCONF=/home/${USER}/.config/mpd/mpd.conf
command_user="${USER}"
command_args="--no-daemon ${MPDCONF}"
start_pre()
{
    checkpath -d ${MPDCONF}
}
```
Note that for supervise-daemon to work, the supervised process must not fork, hence the `--no-daemon` argument. Then simply run:

`root #``rc-service larry_mpd start`
A more genuine and portable way would be to rename the init script user\_mpd, replace the USER variable in the init script with `USER=${RC_SVCNAME##*.}`, then create a symlink:

`root #``ln -s /etc/init.d/user_mpd /etc/init.d/user_mpd.larry`
The same script can then be used for other users:

`root #``ln -s /etc/init.d/user_mpd /etc/init.d/user_mpd.foo`
This can be used to launch a background process for a user, even when this user is not connected. It might be useful if a machine provides services on the network (a user's music-streaming daemon).

### Using PAM to triggers user's service on login

In most cases though, the user's services should probably not be started when the user is not logged in. An option is to use pam\_exec to toggle the service on and off on session events. First, a script for PAM is needed.

**`/usr/local/bin/pam_user_service`**

```
#!/bin/sh
if [ "$PAM_TYPE" = "open_session" ]
then
	rc-service user_mpd.${PAM_USER} start
elif [ "$PAM_TYPE" = "close_session" ]
then
	rc-service user_mpd.${PAM_USER} stop
fi
```
Then, PAM must be told to run this script on session events.

As we only wants `user_service` to start on local login, we could append a line to /etc/pam.d/system-local-login, like this :

**`/etc/pam.d/system-local-login`**

**PAM example 1**

Now, when user larry logs in, PAM will tell OpenRC to start `user_mpd.larry`, and on logout to stop it. The daemon will be supervised during the whole user's session and restarted if needed.
Note that this setting can also be used to start services with root privileges: for example, if a user needs a vpn connection, but is not part of the netdev group and doesn't have admin privileges, the system admin could use this to start the vpn service only during this user's sessions without granting additional privileges.

### Using a stacked runlevel

If several user's services exist, it might be more convenient to create a whole new runlevel on top of the default one. First, a [stacked runlevel](https://wiki.gentoo.org/wiki/OpenRC/Stacked_runlevel) has to be create:

`root #``mkdir /etc/runlevel/larry_session``root #``rc-update -s add default larry_session`
The services can then be added to the runlevel "larry\_session" as needed with the usual rc-update add/del command. The PAM script must then be modified to enter and exit the user's runlevel instead of starting a single service:

**`/usr/local/bin/pam_user_service`**

**Pam example 2**

```
#!/bin/sh
if [ "$PAM_TYPE" = "open_session" ]
then
	openrc ${PAM_USER}_session
elif [ "$PAM_TYPE" = "close_session" ]
then
	openrc default
fi
```
### Possible drawbacks of using OpenRC

OpenRC always requires admin privileges to interact with a daemon or to change the init script. For example, a user cannot start and stop the mpd daemon as and when he wishes to: sending a SIGTERM like killall mpd will work, because the user formally owns the process, yet supervise-daemon will restart it immediately. The only workaround would be to start a completely new instance of OpenRC, which is not an easy task.

The other drawback depends on the what kind of long term processes the user wants to run. Unlike systemd, OpenRC doesn't interact with dbus, and all its child processes are not aware of environment variables like DISPLAY, DBUS\_SESSION\_BUS\_ADDRESSES, etc. For services like a vpn, an emacs daemon or a streaming daemon, it doesn't matter, but OpenRC cannot be used to start a notification daemon or a graphical-related software. The only way to make a service aware of those environment variables would be to source the variables or set the variables in the service init script itself. Here is a possible way of making the init script try to "guess" a user's dbus session address by looking for a socket:

**`/etc/init.d/user_mpd`**

```
export USER=$USER
start_pre() {
 if [ -O /tmp/dbus-* ]  ; then 
   dbus_socket=$(find /tmp/dbus-* -user ${USER} )
   UID=$(id -u $user)
   export DBUS_SESSION_BUS_ADDRESS=unix:path=$dbus_socket
   export XDG_RUNTIME_DIR=/var/run/${UID}
else
   eerror "The user's session bus was not found."
   return 1
}
```
This script is still fragile: if for some reasons the user has two dbus sockets active (which on sane systems should **not** be the case), the variable will have a wrong syntax. Those variables will only be set for the child process,

## Using runsvdir (part of Runit)

Runit can be used as an init system or system-wide daemon-supervisor, both as a standalone program or in conjunction with OpenRC, but in this setup, we will not use it that way. The package [sys-process/runit](https://packages.gentoo.org/packages/sys-process/runit) offers the executable runsvdir, that, as its name implies, runs a service directory - *any* given directory. Like any UNIX process, runsvdir inherits the privileges and environment of its parent process: if started by the user, it will run services under that user account. Using runsvdir instead of OpenRC offers a major benefit: if run as a user, runsvdir doesn't expect admin privileges to interact with the daemons, yet it offers the advantages of a process supervisor.

### An example service directory

This presentation is very basic. Please refer to the runsvdir, runsv, and sv [documentation](http://smarden.org/runit/) for detailed explanations on how runsvdir works.

As previously mentioned, runsvdir requires a service directory that will contain the configuration for the services it should run. This can be anywhere the user has read and write access. The name of the directory doesn't matter. Possible locations could be:

- $HOME/service
- §HOME/.service
- $HOME/.local/service
- $HOME/larry\_incredible\_service\_directory

The directory is the main argument when invoking runsvdir: runsvdir $HOME/.service.

Runsvdir will start a runsv instance for each service defined in the service directory. Those runsv instances are controlled via the sv utility. sv requires two arguments: the command (up, down, status, etc.) to run, and one or more services to act on. The services can be given as absolute paths:

`user $``sv up $HOME/.service/`*foobar*
or as relative paths which will be interpreted relative to the service directory, which should be specified in the environment variable `SVDIR`

`user $``SVDIR=$HOME/.service sv up` *foobar*
`SVDIR` is globally set to /etc/service (or formerly /var/service) on Gentoo systems. When managing user services, it is probably more convenient to overwrite this variable in the user's shell configuration file, e.g. for bash:

**`.bashrc`**

```
export SVDIR=$HOME/.service
```
While service definitions can be made and edited directly in the service directory `SVDIR`, it's common to have the service directory contain symlinks to another directory that contains the service definitions. Since runsvdir runs all services in the service directory when it starts up (except those with a down file, see below), using symlinks makes it easier to control which services get started.

For example, a user might create $HOME/.config/sv and symlink the service directory from this directory to the `SVDIR`. It is important to use the full path for the symlink:

`user $``ln -s /home/larry/.config/sv/`*myservice* /home/larry/.service
Each service definition is a directory which contains at least an executable file named run. The executable must start one program in the foreground. Typically it will be a shell script that performs any necessary setup and then invokes the real service using exec. Here is an example of a run file for user larry's mpd-daemon:

**`/home/larry/.config/sv/mpd/run`**

```
#!/bin/sh
exec mpd --no-daemon /home/larry/.config/mpd/mpd.conf
```
Besides run, the service directory can contain some other files that affect how the service is managed:

- The presence of an empty file named down tells runsvdir that when it starts up, it should not start the service. A service with a down file will only be started when explicitly enabled with sv up *myservice*.
- If an executable file named finish exists, it will be executed when the service stops (either when running sv down *myservice*, or if the service exits on its own). finish takes some arguments that are described in the [runsv man page](http://smarden.org/runit/runsv.8.html).
- If an executable file log/run exists, it will be executed just prior to the main service starting up, and the standard output of the main service will be piped into the standard input of log/run. runit provides an executable svlogd which works as a good log collector; it takes one argument which should be a directory in which it should write log files.
- If an executable file log/finish exists, it will be executed after log/run exits.
- A subdirectory control can be used to customize how the process is controlled. This is advanced usage that will not be necessary for most cases. Details can be found in the man page of runsv.

Here is a complete example of user larry starting his mpd service: larry created a directory $HOME/.local/service, exported the new value of `SVDIR` in his shell, and created a directory mpd in $HOME/.config/sv with an executable named run and an empty file named down. Then to bring up the service, larry will run

`user $````
runsvdir /home/larry/.local/service & disown
```
`user $````
ln -s /home/larry/.config/sv/mpd /home/larry/.local/service
```
`user $````
sv up mpd
```
`user $````
mpc play
```
From now on, mpd is supervised by runsvdir and can be toggled up and down with sv up mpd, sv down mpd. The next step for larry is to start runsvdir with every session, so that he only needs one command to start his supervised mpd daemon.

### Starting runsvdir as a user

runsvdir can be started like any other long term processes: in the user's shell, by the desktop environment or the window managers, manually, etc.

Letting the user start runsvdir offers some advantages:

- No risk of privilege escalations: everything is run by the user for the user, without root privileges.
- Just like OpenRC, runit does not communicate with DBUS in any way, but like any UNIX process, it inherits the environment from its parent process. If runsvdir is launched in an environment that is aware of DBUS variables, like a desktop environment or a window manager (launched by a display-manager or with dbus-launch), runsvdir and consequently the supervised processes will inherit those variables. Depending of the type of background processes the user wants to run, it is by far the simplest option to let the user starting runsvdir as needed.

#### An example setup in a Wayland context

Here is a simple case: our user Larry uses the window manager *sway* with the terminal emulator foot and the Wayland native program wlsunset for gamma adjustment. He could have those two programs started directly by sway, which is the most common setup, but:

- Foot has an optional server/client architecture. Larry prefers this setup (less memory used and quicker startup), so he defined a shortcut for the command footclient in his sway config, but if the server stops for some reason, the shortcut won't work. Larry wants the foot server to be automatically restarted (basically emulating the systemd user service). Here is a simple run script:

**`SVDIR/foot/run`**

```
#!/bin/sh 
exec foot --server
```
- wlsunset doesn't use a config file: all variables have to be defined in the launching command. Moreover, wlsunset doesn't have a "toggle" option. Let's write a service script to easily start and stop the program:

**`SVDIR/wlsunset/run`**

```
#!/bin/sh
exec wlsunset -t 3500 -T 5700 -S 08:00 -s 19:00
```
Those processes need or preferably need the environment variables `XDG_RUNTIME_DIR`, `WAYLAND_DISPLAY`, and `DBUS_SESSION_BUS_ADDRESS` to be set. If sway is launched with dbus-launch - which is highly recommended - and if sway launches runsvdir through its config file (the same way it would have spawn the programs), then everything should work fine out of the box. Now the processes will be supervised, restarted then needed, and wlsunset can be toggled on and off using sv up/down wlsunset.

#### Possible drawbacks

When started this way, runsvdir itself will run unsupervised. It is probably not a serious problem: as runit is designed to work as an init system, it is fairly solid and reliable.

The other drawback is that, when started by a window manager or desktop environment, runsvdir and its services are still more or less bound to this environment, although it is not designed for that. Normally this means all processes and sub-processes started by runsvdir would terminate when the desktop environment terminates, which might or might not be the behavior the user wants. It is possible to start runsvdir independently (from the shell in a non-graphical session), but then the supervised processes won't be aware of the environment variables that are set with the X11 server, the DBUS session, or the Wayland display, unless the run scripts source or set those variables as needed.

The best setup clearly depends on the use case.

It is also possible to run multiple different runsvdir instances depending on the context, or even simultaneously, or to dynamically switch between different runsvdir instances using the runsvchdir utility. This is used by runit to implement runlevels.

Here is an example bashrc that sets `SVDIR` to refer to either a basic, non-graphical service directory if no Wayland display is found, or a Wayland-specific service directory if one is found:

**`.bashrc`**

```
if [ -z "$WAYLAND_DISPLAY" ]; then 
  export SVDIR=$HOME/.service 
else 
  export SVDIR=$HOME/.wayland-service
fi
```
### Starting user's runsvdir with OpenRC

Void Linux's wiki [a way to start a user's runsvdir from a system-wide runit instance](https://docs.voidlinux.org/config/services/user-services.htmldescribes). The following setup tries to emulate this from an OpenRC-based system: as in the examples for OpenRC user's services above, OpenRC itself could be used to start and supervise the user's runsvdir. Taking inspiration from Gentoo's default runsvdir service, an init script could look like this:

**`/etc/init.d/user_runsvdir`**

```
#!/sbin/openrc-run
supervisor=supervise-daemon
command=/bin/runsvdir
USER=${RC_SVCNAME##*.}
USER_SVCDIR="/home/${USER}/.local/service"
export USER=$USER
UID=$(id -u ${USER})
export XDG_RUNTIME_DIR=/run/user/${UID}
command_user="${USER}" 
command_args="$USER_SVCDIR"
start_pre()
{
	checkpath -d ${USER_SVCDIR}
}
stop_pre()
{
	sv force-shutdown ${USER_SVCDIR}/*
}
```
The next step is to create a symlink to this script for a specific user, like user\_runsvdir.larry, and (if desired) to create a script for PAM as shown above to start the service (user\_runsvdir.$PAM\_USER) on larry's login and to stop on larry's logout. Now runsvdir itself is supervised by OpenRC, and as it is run on behalf of the user, they can still define services in their own service directory, and start them or stop them without admin privileges.

Since runsvdir inherits its environment from OpenRC, all of its child processes by default don't know anything about the DBUS session, the graphical session, etc. The script above exports at least the `XDG_RUNTIME_DIR` variable, since it is easy to guess, not really prone to error, and might prove useful.
It can be annoying, but since the users can now interact with their services (restart them as needed, etc.) without being given admin privileges, they are more workarounds available.  If we defined a foot server service as in the example above, we can define its initial state as down, and let sway start the service while passing the environment variables in the command. (Letting sway restart the service won't work because that does not allow changing the arguments.) For a more durable variant, we could also have sway at first print its environment variable to a file in `XDG_RUNTIME_DIR`, sourcing this file in the service run script, and also letting sway bring the service up. Here are some examples adapted from the author's config with the river window manager:

**`$HOME/.config/river/init`**

```
#!/bin/sh
printenv > $XDG_RUNTIME_DIR/river_env && chmod 600 $XDG_RUNTIME_DIR/river_env
## Rest of the config here
...
## then 
sv down $HOME/.local/service/dbus_session
sv up $HOME/.local/service/foot
sv up $HOME/.local/service/wlsunset
sv up $HOME/.local/service/swayidle
sv reload $HOME/.local/service/pipewire
sv reload $HOME/.local/service/pipewire_pulse
sv reload $HOME/.local/service/wireplumber
sv up $HOME/.local/service/mpd
```
Here is again a more complex "run" file for the "foot" service:

**`$HOME/.config/sv/foot/run`**

```
#!/bin/sh 
if pgrep -x river && [ -f $XDG_RUNTIME_DIR/river_env ]; then
   . $XDG_RUNTIME_DIR/river_env
elif pgrep -x sway && [ -f $XDG_RUNTIME_DIR/sway_env ]; then
   . $XDG_RUNTIME_DIR/sway_env
else
   echo "env not set" && return 1
fi
export WAYLAND_DISPLAY
export DBUS_SESSION_BUS_ADDRESS
exec foot --server
```
The flexibility of the "run" scripts allow a lot of creativity for the creation of "services" within the bigger frame of user's permissions: streaming a specific radio station with mpv in the background, a utility that in an infinite loop reads the timestamp of the Gentoo ebuild repo's metadata file every hour and sends a notification if it was not synced in a week, etc....  The command sudo rc-service user\_runsvdir.*larry* stop will recursively terminate all processes. If using pam\_exec, this command will be sent to OpenRC on logout.

## Last words

All those tricks and workarounds can be mixed, as OpenRC and runsvdir don't interfere with each other: we can easily define a stacked runlevel that is entered on user's login through a pam module, in which OpenRC starts a vpn connection with root privileges, starts a service as a user, and starts the user's runsvdir on top of it... Be creative!
