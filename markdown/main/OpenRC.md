<!-- source: https://wiki.gentoo.org/wiki/OpenRC | group: Gentoo Wiki (Main) | wiki-title: OpenRC -->
---
title: OpenRC
url: https://wiki.gentoo.org/wiki/OpenRC
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-11-08"
fingerprint: b718ab5907028ac4
license: CC BY-SA 4.0
---

# OpenRC

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Article status**

- Review for accuracy & test.
- Complete to cover most basic usage under Gentoo.
- Rework for precision, readability, concision...
- Should the "Dependency behavior" and "Selecting a specific runlevel at boot" sections be moved into the new [OpenRC/Advanced](https://wiki.gentoo.org/wiki/OpenRC/Advanced) page?
- Should the user services sectons be moved to a new subpage?

**OpenRC** is a dependency-based [init system](https://wiki.gentoo.org/wiki/Init_system) for Unix-like systems that maintains compatibility with the system-provided init system, normally located in /sbin/init.

OpenRC will start necessary system services in the correct order at boot, manage them while the system is in use, and stop them at shutdown. It can manage daemons installed from the Gentoo repository, can optionally supervise the processes it launches, and has the possibility to start appropriate processes in parallel to shorten boot time.

OpenRC was developed for Gentoo, but is designed to be used in [other Linux distributions and BSD systems](https://wiki.gentoo.org/wiki/OpenRC/Users). By default, OpenRC is invoked by [sysvinit](https://wiki.gentoo.org/wiki/Sysvinit), on Gentoo.

Gentoo officially supports both OpenRC and [systemd](https://wiki.gentoo.org/wiki/Systemd), but [other init systems are available](https://wiki.gentoo.org/wiki/Comparison_of_init_systems).

Daemons installed from outside the Gentoo repository, e.g. software downloaded as source code and compiled manually, may sometimes need to be adapted to work with OpenRC (which isn't necessarily very complex).

More advanced setups are covered in [OpenRC/Advanced](https://wiki.gentoo.org/wiki/OpenRC/Advanced).

## Implementation

OpenRC follows standard practices for Unix-like systems, and is a component of a modular and flexible system, integrated with other system software. It is designed to be fast, lightweight, easily configurable, and adaptable. It has only a few basic dependencies, on core system components.

As a modern init system, OpenRC provides a number of useful features:

- [cgroups](https://wiki.gentoo.org/wiki/OpenRC/CGroups) support.
- Process supervision.
- Dependency-based launch, with parallel startup of services.
- Automatic resolving, and ordering, of dependencies.
- Hardware initiated initscripts.
- Setting `ulimit` and `nice` values per service through the `rc_ulimit` variable.
- Permits complex init scripts that start multiple components.
- Modular architecture, fitting into existing infrastructure.
- OpenRC has its own optional init system called openrc-init, see [OpenRC/openrc-init](https://wiki.gentoo.org/wiki/OpenRC/openrc-init) for details.
- OpenRC has its own optional process supervisor, see [OpenRC/supervise-daemon](https://wiki.gentoo.org/wiki/OpenRC/supervise-daemon) for details.

See the [comparison of init systems](https://wiki.gentoo.org/wiki/Comparison_of_init_systems) article for more information on init systems.

## Installation

OpenRC generally does not need installing manually, and is provided as part of an OpenRC [profile](<https://wiki.gentoo.org/wiki/Profile_(Portage)>) on [installation](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Choosing_the_right_profile). It will be present in the [stage 3 tarball](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage#OpenRC), and will be maintained across system updates.

### USE flags


### USE flags for
            [sys-apps/openrc](https://packages.gentoo.org/packages/sys-apps/openrc)
            
            OpenRC manages the services, startup and shutdown of a host

| [+netifrc](https://packages.gentoo.org/useflags/+netifrc) | enable Gentoo's network stack (net.\* scripts) | 
| [+sysvinit](https://packages.gentoo.org/useflags/+sysvinit) | control the dependency on sysvinit (experimental) | 
| [audit](https://packages.gentoo.org/useflags/audit) | Enable support for Linux audit subsystem using sys-process/audit | 
| [bash](https://packages.gentoo.org/useflags/bash) | enable the use of bash in service scripts (experimental) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [newnet](https://packages.gentoo.org/useflags/newnet) | enable the new network stack (experimental) | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [s6](https://packages.gentoo.org/useflags/s6) | install s6-linux-init | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [sysv-utils](https://packages.gentoo.org/useflags/sysv-utils) | Install sysvinit compatibility scripts for halt, init, poweroff, reboot and shutdown | 
| [unicode](https://packages.gentoo.org/useflags/unicode) | Add support for Unicode | 

If the USE flags are modified, the package may be rebuilt to apply changes. For OpenRC [profiles](<https://wiki.gentoo.org/wiki/Profile_(Portage)>), [sys-apps/openrc](https://packages.gentoo.org/packages/sys-apps/openrc) is pulled in as a dependency of [virtual/service-manager](https://packages.gentoo.org/packages/virtual/service-manager), thus it should never be added to the [selected-packages set](<https://wiki.gentoo.org/wiki/Selected-packages_set_(Portage)>) (/var/lib/portage/world file) - the [`--oneshot` option](https://wiki.gentoo.org/wiki/Emerge#Do_not_add_dependencies_to_the_world_file) avoids adding OpenRC to this set.

`root #``emerge --ask --oneshot sys-apps/openrc`
## Configuration

### Files

Configuration ordering hierarchy is:

- /etc/rc.conf — System-wide OpenRC configuration. **Includes extensive comments documenting OpenRC configuration.**
- /etc/user/rc.conf — System-wide configuration for user services.
- [${XDG\_CONFIG\_HOME}](https://wiki.gentoo.org/wiki/XDG/Base_Directories)/rc/rc.conf — Per-user configuration for user services. Overrides settings in /etc/rc.conf if present. Defaults to \~/.config/rc/rc.conf.

Other files are:

- /etc/init.d - Directory of service scripts for system services.
- /etc/conf.d - Directory of configuration files for service scripts in /etc/init.d.
- /etc/user/init.d - Directory of service scripts for user services. These ones are available to all users, so usually distribution-provided scripts are installed here.
- /etc/user/conf.d - Directory of configuration files for service scripts in /etc/user/init.d.
- ${XDG\_CONFIG\_HOME}/rc/init.d - Directory of service scripts for user services. These are specific to one single user, so usually user-written ones are placed here.
- ${XDG\_CONFIG\_HOME}/rc/conf.d - Directory of configuration files for user service scripts. Scripts in ${XDG\_CONFIG\_HOME}/rc/init.d only use files in this directory. Additionally, configuration files in this directory can be used to override settings in /etc/user/conf.d. These overrides are per-user.

### Logging

OpenRC doesn't log anything by default. To log OpenRC's output during boot, uncomment and set the `rc_logger` option in /etc/rc.conf. The log will be saved at /var/log/rc.log by default.

**`/etc/rc.conf`**

```
rc_logger="YES"
#rc_log_path="/var/log/rc.log"
```
### Network management

Although OpenRC doesn't require use of a network manager, it can nevertheless be used with a variety of network managers. By default, with Gentoo's [OpenRC profiles](<https://wiki.gentoo.org/wiki/Profile_(Portage)>), [netifrc](https://wiki.gentoo.org/wiki/Netifrc) scripts manage network connections.

Refer to the [Network management](https://wiki.gentoo.org/wiki/Network_management) article for network management options.

### Dependency behavior

More complex setups might require changing the default dependencies of services; for details, refer to the *rc\_depend\_strict* option in /etc/rc.conf. The following examples show how flexible OpenRC can be.

#### Multiple network interfaces

If the [`sshd` service](https://wiki.gentoo.org/wiki/SSH) must come up only on the internal network, and not on the external network, e.g. only on when `eth0` is active and not when only `wlan0` is active, overrule the `net` dependency from /etc/init.d/sshd to instead depend on `net.eth0`:

**`/etc/conf.d/sshd`**

```
rc_need="!net net.eth0"
```
#### Multiple network interfaces in multiple runlevels

If the `sshd` service must start with `eth0` and not `wlan0` in the "default" runlevel, but must start with `wlan0` and not `eth0` in the "office" runlevel, keep the default value of `YES` for the *rc\_depend\_strict* option:

**`/etc/rc.conf`**

```
#rc_depend_strict="YES"
```
Then make additional symlinks to the `sshd` service with the relevant interface names:

`root #````
ln -s /etc/init.d/sshd /etc/init.d/sshd.eth0
```
`root #````
ln -s /etc/init.d/sshd /etc/init.d/sshd.wlan0
```
Create the associated /etc/conf.d/sshd.eth0 and /etc/conf.d/sshd.wlan0 configuration files:

`root #````
cp /etc/conf.d/sshd /etc/conf.d/sshd.eth0
```
`root #````
cp /etc/conf.d/sshd /etc/conf.d/sshd.wlan0
```
Add the dependencies:

`root #````
echo 'rc_need="!net net.eth0"' >> /etc/conf.d/sshd.eth0
```
`root #````
echo 'rc_need="!net net.wlan0"' >> /etc/conf.d/sshd.wlan0
```
Then, to have `net.eth0` and `net.wlan0` read their settings from /etc/conf.d/net or /etc/conf.d/net.office, depending on the active runlevel, add them to the appropriate runlevels:

`root #````
rc-update add sshd.eth0 default
```
`root #````
rc-update add sshd.wlan0 office
```
`root #````
rc-update add net.eth0 default office
```
`root #````
rc-update add net.wlan0 default office
```
Finally, to switch between the "default" runlevel and the "office" runlevel without rebooting the computer, change to the "nonetwork" runlevel in between:

"default" runlevel \<---> "nonetwork" runlevel \<---> "office" runlevel

This way, the network interfaces will be stopped, and their runlevel-specific configuration subsequently re-read.

This works best when "nonetwork" is a [stacked runlevel](https://wiki.gentoo.org#Stacked_runlevels) in both the "default" and "office" runlevels, and non-network services (e.g. the display manager) are added to the "nonetwork" runlevel only:

`root #````
openrc nonetwork && openrc office
```
`root #````
openrc nonetwork && openrc default
```
### Selecting a specific runlevel at boot

OpenRC reads the kernel command-line used at boot time, and will start the runlevel specified by the `softlevel` parameter if provided. If no `softlevel` parameter is provided, the *default* runlevel will be used.

The following example shows a [grub](https://wiki.gentoo.org/wiki/GRUB) configuration allowing to choose to boot into either the *default* or *nonetwork* runlevels:

**`/boot/grub/grub.conf`**

**Example grub.conf (GRUB Legacy)**

```
title=Regular Start-up
 
kernel (hd0,0)/boot/kernel-3.7.10-gentoo-r1 root=/dev/sda3
 
title=Start without Networking
 
kernel (hd0,0)/boot/kernel-3.7.10-gentoo-r1 root=/dev/sda3 softlevel=nonetwork
```
See below for a description of how to add additional runlevels.

## Usage

### Runlevels

OpenRC can be controlled and configured using openrc, rc-update and rc-status commands.

Delete a service from default runlevel, where `<service>` is the name of the service to be removed:

`root #``rc-update delete <service> default`
#### Listing

It is not necessary to have root permissions to list runlevels and services (init scripts) assigned to them.

Use rc-update show -v to display all available init scripts and their current runlevel (if they have been added to one):

`user $``rc-update show -v`
Running rc-update or rc-update show will display only the init scripts that have been added to a runlevel.

Alternatively, the rc-status command can be used with the `--servicelist` (`-s`) option to view the state of all services:

`user $``rc-status --servicelist`
#### Named runlevels

OpenRC runlevels are implemented as directories living in /etc/runlevels.  Additional runlevels (indicated as `<runlevel>` below) may be created by using:

`root #``install -d /etc/runlevels/<runlevel>`
Additional runlevels are helpful to provide alternative system start-up profiles.

#### Stacked runlevels

Stacked runlevels are used to allow a runlevel to inherit the actions of one or more other runlevels. The command variant used to create stacked runlevels is rc-update -s. Adding a runlevel to another runlevel causes a dependency to be created such that any init scripts (services) in any dependent (stacked) runlevels are started or stopped when the target runlevel is started or stopped.

An usage example for using stacked runlevel on laptop to group networking services based on location is at [OpenRC/Stacked runlevel](https://wiki.gentoo.org/wiki/OpenRC/Stacked_runlevel).

### Prefix

[Gentoo Prefix](https://wiki.gentoo.org/wiki/Project:Prefix) installs Gentoo within an offset, known as a prefix, allowing users to install Gentoo in another location in the filesystem hierarchy, hence avoiding conflicts. Next to this offset, Gentoo Prefix runs unprivileged, meaning no root user or rights are required to use it.

By using an offset (the "prefix" location), it is possible for many "alternative" user groups to benefit from a large part of the packages in the Gentoo Linux Portage tree. Currently users of the following systems successfully run Gentoo Prefix: Mac OS X on PPC and x86, Linux on x86, x86\_64 and ia64, Solaris 10 on Sparc, Sparc/64, x86 and x86\_64, FreeBSD on x86, AIX on PPC, Interix on x86, Windows on x86 (with the help of Interix), HP-UX on PARISC and ia64.

OpenRC runscript already support prefix-installed daemons, during the Summer of Code 2012 work will be done to implement full secondary/session daemon behavior to complete the overall feature set provided by Prefix.

[OpenRC/Prefix](https://wiki.gentoo.org/wiki/OpenRC/Prefix), a tutorial for trying it out.

### Hotplug

OpenRC can be triggered by external events, such as new hardware from udev. This is what the configuration file says about hotplugged services:

**`/etc/rc.conf`**

**rc\_hotplug**

```
# rc_hotplug controls which services we allow to be hotplugged.
# A hotplugged service is one started by a dynamic dev manager when a matching
# hardware device is found.
# Hotplugged services appear in the "hotplugged" runlevel.
# If rc_hotplug is set to any value, we compare the name of this service
# to every pattern in the value, from left to right, and we allow the
# service to be hotplugged if it matches a pattern, or if it matches no
# patterns. Patterns can include shell wildcards.
# To disable services from being hotplugged, prefix patterns with "!".
# If rc_hotplug is not set or is empty, all hotplugging is disabled.
# Example - rc_hotplug="net.wlan !net.*"
# This allows net.wlan and any service not matching net.* to be hotplugged.
# Example - rc_hotplug="!net.*"
# This allows services that do not match "net.*" to be hotplugged.
```
### CGroups support

OpenRC starting with version 0.12 has extended cgroups support. See [OpenRC/CGroups](https://wiki.gentoo.org/wiki/OpenRC/CGroups) for details. Since OpenRC 0.51, unified cgroups (v2) is enabled by default.

### Chroot support

`root #``mkdir -p /lib64/rc/init.d``root #````
ln -s /lib64/rc/init.d /run/openrc
```
`root #````
touch /run/openrc/softlevel
```
`root #````
emerge --oneshot sys-apps/openrc
```
**`/etc/rc.conf`**

**OpenRC config file**

```
rc_sys="prefix"
rc_controller_cgroups="NO"
rc_depend_strict="NO"
rc_need="!net !dev !udev-mount !sysfs !checkfs !fsck !netmount !logger !clock !modules"
```
The system may report the following message upon attempting to start a service:

\* WARNING: \<service> is already starting

This may be fixed by issuing the following command:

`root #``rc-update --update`
### User services

Starting with version 0.60, OpenRC has support for user services, which are modeled after similar functionality in systemd. User services are services associated with a user account that generally run only while the corresponding user is logged in, as processes whose effective user is equal to that user. If multiple users are logged in at the same time, all their configured user services run simultaneously.

The system services, on the other hand, are services associated with "the system", run during the machine's entire uptime —that is, are started during the boot sequence and stopped only during the shutdown sequence—, as processes whose effective user is either equal to root or to a special "system user" (like daemon).

OpenRC user services use service scripts with the same structure and syntax as that of system services. Like system services, except if they are needed dependencies of other services, user services are started automatically only if their scripts are added to a runlevel. A different set of runlevels are associated with system services and with each login user account. The wording *user runlevel* will be used to refer to a runlevel from the set associated with a specific user account.

A service with a script named service can be added to a user runlevel runlevel of the current user's account with:

`user $````
rc-update --user add service runlevel
```
Similarly, a service with a script named service can be removed from a user runlevel runlevel of the current user's account with:

`user $````
rc-update --user del service runlevel
```
Script service must be present in one the directories [searched for user services](https://wiki.gentoo.org#Files). If no runlevel is specified in the command, the service is added to the current runlevel.

All OpenRC commands (rc-update, rc-service, rc-status, etc.) have a `--user` option (short form `-U`) to act on OpenRC user services or runlevels instead of system ones. So, for example, services added to user runlevels of the current user's account can be shown with:

`user $``rc-update --user show -v````
                 dbus |      default         
             pipewire |      default         
       pipewire-pulse |                      
            ssh-agent |                      
          wireplumber |      default         
```
and the state of a user service of the current user can be shown with:

`user $``rc-service --user dbus status`
\* status: started

OpenRC user services require environment variable [XDG\_RUNTIME\_DIR](https://wiki.gentoo.org/wiki/XDG/Base_Directories) to be set during the login process, since OpenRC stores user services state in directory ${XDG\_RUNTIME\_DIR}/openrc. Thus, a mechanism for setting it is required. That could be [elogind](https://wiki.gentoo.org/wiki/Elogind), [sys-auth/pam\_xdg](https://packages.gentoo.org/packages/sys-auth/pam_xdg), or any other method of creating the directory and setting the environment variable, e.g. as described [here](https://wiki.gentoo.org/wiki/Configuring_a_system_without_elogind#Environment_variables).

Since the [stabilization of user services](https://www.gentoo.org/support/news-items/2025-09-04-openrc-user-services.html), services added to user runlevels are started automatically at user login, and stopped automatically when the user's last session terminates. This features uses [Pluggable Authentication Modules (PAM)](https://wiki.gentoo.org/wiki/PAM) functionality. No user action is required other than adding wanted services to user runlevels.

For the implementation of user services, OpenRC provides a PAM module, pam\_openrc.so, the openrc-run program, and a system service named `user`. Different instances of this service are started for each logged in user, each of which spawns an openrc-run process.

openrc-user opens another PAM session for the user, which will last as long as any given session using the pam\_openrc.so module is active. It loads the PAM stack specified by its configuration file, /etc/pam.d/openrc-user, which is used for that new session, then enters the user boot runlevel, and finally the configured user default runlevel —which is default unless the user changed it—.

When the user logs out, and that login session was the user's last one, pam\_openrc.so stops the user's `user` service instance, which kills the corresponding openrc-user process. When openrc-user is killed, it enters the user shutdown runlevel, and then exits.

User service scripts are run using the shell configured for the user's account in /etc/passwd, with option `-c`. openrc-user populates directory ${XDG\_CONFIG\_HOME}/rc/runlevels should it not exist.

Automatic start of OpenRC user services on login can be disabled globally through /etc/rc.conf:

**`/etc/rc.conf`**

```
# Set to "NO" if you don't want pam_openrc autostarting user services. This
# effectively disables the pam module, without the need of removing it from
# the pam configuration files.
rc_autostart_user="NO"
```
This will also prevent the `user` system service instance from starting.

#### Lingering

In the context of systemd, *lingering* is a mechanism that always ensures the presence of a user session for a specific user. A session is created on system startup, and this session is guaranteed to persist until shutdown, regardless of whether the user is logged in or not. Other properties of the service (e.g. processes with effective user equal to the logged in user) remain the same. Thus, services run on behalf of this user are preserved throughout the entire uptime of the system. While lingering itself is implemented by the session daemon (systemd-logind in case of systemd), there are methods to achieve similar results on an OpenRC-based system.

To make user services of user larry start during the boot sequence, and stop during the shutdown sequence, regardless of whether larry is logged in or not, create a symbolic link named user.larry in the directory of scripts for system services, pointing to file user, and add service `user.larry` to the default runlevel:

`root #````
ln -s user /etc/init.d/user.larry
```
`root #````
rc-update add user.larry default
```

#### KDE compatibility

As at OpenRC 0.62.10, using OpenRC user services with KDE requires some configuration changes.

Firstly, [KDE](https://wiki.gentoo.org/wiki/KDE)/[Qt](https://wiki.gentoo.org/wiki/Qt) fails to find the socket for the [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) session bus created by the `dbus` user service at ${XDG\_RUNTIME\_DIR}/bus. (Note that this is distinct from the `dbus` *system* service, which provides a D-Bus *system* bus.)

As a result, the following script is needed, in either /etc/profile.d/ or the appropriate startup file for the user's shell (e.g. \~/.bash\_profile):

Secondly, Gentoo bundles an autostart desktop file for PipeWire, /etc/xdg/autostart/pipewire.desktop, which conflicts with PipeWire user services ([bug #964059](https://bugs.gentoo.org/show_bug.cgi?id=964059)).

To disable the conflicting autostart file, add `Hidden=true` to it. It's also possible to copy pipewire.desktop to ${XDG\_CONFIG\_HOME}/autostart/ to create a local override for only the current user (where `XDG_CONFIG_HOME` is \~/.config/ by default).

Once this is done, PipeWire OpenRC user services can be enabled as decribed [here](https://wiki.gentoo.org/wiki/PipeWire#User_services:_OpenRC).

After logging out and logging back in, all services should now be running and able to be used by KDE.


#### Logging

To enable logging, add the following lines to ${XDG\_CONFIG\_HOME}/rc/rc.conf, creating that file if it doesn't already exist:

**`${XDG_CONFIG_HOME}/rc/rc.conf`**

```
rc_logger="YES"
rc_log_path="<location>/rc.log"
```
where `<location>` should be replaced by a literal directory path, e.g. /home/larry/logs/.

## systemd compatibility

### logind

Some setups require systemd-logind. [Elogind](https://wiki.gentoo.org/wiki/Elogind) can be a suitable replacement as a standalone logind running with OpenRC.

### tmpfiles.d

systemd has a special tmpfiles.d file syntax for managing temporary files. [sys-apps/systemd-utils\[tmpfiles\]](https://packages.gentoo.org/packages/sys-apps/systemd-utils) is a standalone provider for OpenRC systems.

Both can also be used to manage volatile entries in /sys or /proc.

### User services

By providing suitable service scripts, [user services](https://wiki.gentoo.org#User_services) can be used for the support of packages that provide service user unit files on OpenRC systems. That is, service unit files installed in /usr/lib/systemd/user, which are used by systemd when running as a per-user instance (i.e. non PID 1 process).

## Troubleshooting

### Respawning crashed services

OpenRC can return state of services to runlevel setting state, to provide stateful init scripts and automatic respawning.

To respawn crashed services from the default runlevel, run openrc: crashed services will be started and any manually-run services will be stopped. To keep manually-started services running, run openrc --no-stop or openrc -n, for short.

By default openrc will attempt to *start* crashed services, not to *restart* them. This can be controlled by the rc\_crashed\_stop (default NO) and rc\_crashed\_start (default YES) options in /etc/rc.conf.

### Manually recovering crashed services

When a process crashes while starting, an error or warning message will be printed when trying to start, stop, or show the status of a service. For example, when using the `docker` service:

`root #``rc-service docker status`
\* status: crashed

`root #``rc-service docker start`
\* WARNING: docker has already been started

`root #``rc-service docker stop`
\* Caching service dependencies ...                                                                                                  \[ ok \]
 \* Stopping docker ...
 \* Failed to stop docker                                                                                                             \[ !! \]
 \* ERROR: docker failed to stop

To remedy this situation, zap the service:

`root #````
rc-service docker zap
```
### User services are not starting on user login

This is a checklist for the case when user services don't start when a user logs in.

Check that user services have not been globally disabled:

`user $``grep rc_autostart_user /etc/rc.conf`
#rc\_autostart\_user="YES"

The assignment to `rc_autostart_user` must be absent or commented, or `rc_autostart_user` must be set to YES.

Check that user runlevels are not empty:

`user $``rc-status --user --all`
Runlevel: boot
Runlevel: shutdown
Runlevel: default
Dynamic Runlevel: hotplugged
Dynamic Runlevel: needed/wanted
Dynamic Runlevel: manual

If the output looks like the above, user runlevels are empty.

Check that loading of the pam\_openrc.so PAM module is enabled:

`user $``grep pam_openrc /etc/pam.d/system-login`
-session	optional	pam\_openrc.so

This line must be present and not commented.

Check that the symbolic link to the `user` service script for the logged in user exists, and that the corresponding instance is running:

`user $``$ ls -l /run/openrc/init.d`
total 0
lrwxrwxrwx 1 root root 16 Oct 29 12:00 user.larry -> /etc/init.d/user

`user $``rc-service user.larry status`
\* status: started

Check that the openrc-user process for the logged in user and its supervise-daemon supervisor are running:

`user $``$ ps -e f | grep openrc-user`
1742 ?        S      0:00 supervise-daemon user.larry --start ...
 1743 ?        Ss     0:00  \\\_ /usr/libexec/rc/bin/openrc-user larry
 ...

## See also

- [OpenRC/Advanced](https://wiki.gentoo.org/wiki/OpenRC/Advanced) — advanced topic about configuring and using [OpenRC]
- [OpenRC/CGroups](https://wiki.gentoo.org/wiki/OpenRC/CGroups) — OpenRC includes support for [cgroups](https://wiki.gentoo.org/wiki/Cgroups).
- [OpenRC/openrc-init](https://wiki.gentoo.org/wiki/OpenRC/openrc-init) — OpenRC's own init system
- [OpenRC/Prefix](https://wiki.gentoo.org/wiki/OpenRC/Prefix) — The following guideline applies to a Gentoo [Prefix](https://wiki.gentoo.org/wiki/Prefix) on RHEL-5.6 amd64 and on Debian 6.0 amd64, for other setups it should be similar.
- [OpenRC/Stacked runlevel](https://wiki.gentoo.org/wiki/OpenRC/Stacked_runlevel) — a tutorial for setting up complicated networking with the help of stacked runlevel.
- [OpenRC/supervise-daemon](https://wiki.gentoo.org/wiki/OpenRC/supervise-daemon) — OpenRC's daemon supervisor
- [OpenRC/Users](https://wiki.gentoo.org/wiki/OpenRC/Users) — an (incomplete) list of distributions and operating systems using OpenRC.
- [/etc/conf.d](https://wiki.gentoo.org/wiki//etc/conf.d) — generally contains initscript configuration files
- [/etc/local.d](https://wiki.gentoo.org/wiki//etc/local.d) — **/etc/local.d/** can contain small programs or light scripts to be run when the local service is started or stopped.
- [Gentoo AMD64 Handbook - Initscript system](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Initscripts)
- The main OpenRC configuration file, /etc/rc.conf, contains extensive comments documenting OpenRC configuration.

## External resources

### OpenRC documentation

OpenRC has its own useful documentation, maintained by the OpenRC developers themselves. Be aware that Gentoo does not use all of OpenRC's functionality by default:

- [README](https://github.com/OpenRC/openrc/blob/master/README.md)
- [User Guide](https://github.com/OpenRC/openrc/blob/master/user-guide.md)
- [OpenRC init process guide](https://github.com/OpenRC/openrc/blob/master/init-guide.md) - How to [use OpenRC's own init system](https://wiki.gentoo.org/wiki/OpenRC/openrc-init) (not used by default in Gentoo).
- [agetty guide](https://github.com/OpenRC/openrc/blob/master/agetty-guide.md) - Setting up the agetty service in OpenRC (By default, ttys are spawned with [sysvinit](https://wiki.gentoo.org/wiki/Sysvinit#Gentoo.27s_sysvinit_setup) in Gentoo).
- [Using runit with OpenRC](https://github.com/OpenRC/openrc/blob/master/runit-guide.md)
- [Using S6 with OpenRC](https://github.com/OpenRC/openrc/blob/master/s6-guide.md)
- [Using supervise-daemon](https://github.com/OpenRC/openrc/blob/master/supervise-daemon-guide.md) - [Optional process supervision](https://wiki.gentoo.org/wiki/OpenRC/supervise-daemon).
- [Using BusyBox with OpenRC](https://github.com/OpenRC/openrc/blob/master/BUSYBOX.md)
- [Service script writing guide](https://github.com/OpenRC/openrc/blob/master/service-script-guide.md) - For developers or packagers.
- [NEWS](https://github.com/OpenRC/openrc/blob/master/NEWS.md) - Important information about each release.
- [HISTORY](https://github.com/OpenRC/openrc/blob/master/HISTORY.md) - History of how OpenRC came to be.

#### Man pages

- [openrc(8)](https://manpages.debian.org/testing/openrc/openrc.8.en.html)- [openrc-run(8)](https://manpages.debian.org/testing/openrc/openrc-run.8.en.html)- [rc-service(8)](https://manpages.debian.org/testing/openrc/rc-service.8.en.html)- [rc-status(8)](https://manpages.debian.org/testing/openrc/rc-status.8.en.html)- [rc-update(8)](https://manpages.debian.org/testing/openrc/rc-update.8.en.html)
