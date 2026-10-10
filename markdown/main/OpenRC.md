<!-- source: https://wiki.gentoo.org/wiki/OpenRC | group: Gentoo Wiki (Main) | wiki-title: OpenRC -->
---
title: OpenRC
url: https://wiki.gentoo.org/wiki/OpenRC
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-09"
fingerprint: b618bb590ba38a84
license: CC BY-SA 4.0
---

# OpenRC

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[checking over the content](https://wiki.gentoo.org/index.php?title=OpenRC&action=edit)(

[how to get started](https://wiki.gentoo.org/wiki/Gentoo_Wiki:Contributor%27s_guide)).

**Resources**

**Article status**

- Review for accuracy & test.
- Complete to cover most basic usage under Gentoo.
- Rework for precision, readability, concision...
- Section [BusyBox Integration](https://wiki.gentoo.org/wiki/OpenRC#BusyBox_integration) needs testing and documenting (or removing).

**OpenRC** is a dependency-based [init system](https://wiki.gentoo.org/wiki/Init_system) for Unix-like systems that maintains compatibility with the system-provided init system, normally located in /sbin/init.

OpenRC will start necessary system services in the correct order at boot, manage them while the system is in use, and stop them at shutdown. It can manage daemons installed from the Gentoo repository, can optionally supervise the processes it launches, and has the possibility to start processes in parallel - when possible - to shorten boot time.

Gentoo officially supports both OpenRC and [systemd](https://wiki.gentoo.org/wiki/Systemd), but [other init systems are available](https://wiki.gentoo.org/wiki/Comparison_of_init_systems).

OpenRC was developed for Gentoo, but is designed to be used in [other Linux distributions and BSD systems](https://wiki.gentoo.org/wiki/OpenRC/Users). By default, OpenRC is invoked by [sysvinit](https://wiki.gentoo.org/wiki/Sysvinit), on Gentoo.

Daemons installed from outside the Gentoo repository, e.g. software downloaded as source code and compiled manually, may sometimes need to be adapted to work with OpenRC (which is sometimes trivial).

## Implementation

OpenRC does not require large, fundamental, changes to the traditional Unix-like system. OpenRC integrates with other system software as a component of a modular and flexible system. It is designed to be fast, lightweight, easily configurable, and adaptable. OpenRC has only a few basic dependencies, on core system components.

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

- /etc/rc.conf
- The global OpenRC configuration file.
- **Includes extensive comments documenting OpenRC configuration.**
- /etc/conf.d
- Contains configuration files for individual initscripts.

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

This works best when "nonetwork" is a [stacked runlevel](https://wiki.gentoo.org/wiki/OpenRC#Stacked_runlevels) in both the "default" and "office" runlevels, and non-network services (e.g. the display manager) are added to the "nonetwork" runlevel only:

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

User services are services that run as the specific user they belong to. OpenRC has support for user services, starting with version 0.60.

OpenRC user services require `XDG_RUNTIME_DIR` to be set, since user services store state in ${XDG\_RUNTIME\_DIR}/openrc/; thus, a mechanism for setting `XDG_RUNTIME_DIR` is required. That could be [elogind](https://wiki.gentoo.org/wiki/Elogind), [sys-auth/pam\_xdg](https://packages.gentoo.org/packages/sys-auth/pam_xdg), the shell's configuration file(s), or any other method of creating the directory and setting the environment variable, e.g. as described [here](https://wiki.gentoo.org/wiki/Configuring_a_system_without_elogind#Environment_variables).

While system OpenRC service scripts are loaded from /etc/init.d/, scripts for user services are loaded from /etc/user/init.d/.

Configurations for particular user services are located in /etc/user/conf.d/. Configuration options defined in ${XDG\_CONFIG\_HOME}/rc/conf.d/ override options set in /etc/user/conf.d/, and options set in ${XDG\_CONFIG\_HOME}/rc/rc.conf override those in /etc/rc.conf. If `XDG_CONFIG_HOME` is unset, OpenRC uses \~/.config as a default value.

#### Lingering

In the context of systemd, lingering is a mechanism ensuring the presence of a user session for a specific user. A session is created on system startup and this session is guaranteed to persist until shutdown, even if the user logs in or out. Thus, services run on behalf of this user are preserved throughout the entire runtime of the system. While lingering itself is implemented by the session daemon (`logind` in case of systemd), there are methods to achieve the similar results on an OpenRC-based system.

Although [sys-auth/elogind](https://packages.gentoo.org/packages/sys-auth/elogind) has an `--enable-linger <user>` option that should work with the PAM-based auto-start, it is recommended to enable the [OpenRC user-specific session](https://wiki.gentoo.org#User_service_start_boot) that achieves the same effect.

#### Service start on boot (lingering)

To enable per-user services for a user (`<user>` is the name of the user), create a symlink /etc/init.d/user.\<user> pointing to /etc/init.d/user. This service starts an OpenRC user session that then handles all services enabled for the user.

`root #````
ln -s /etc/init.d/user /etc/init.d/user.<user>
```
`root #````
rc-update add user.<user>
```
Enable a service for a user with:

`user $````
rc-update --user add <service>
```
The service to enable must be present in /etc/user/init.d. All OpenRC commands (`rc-update`, `rc-service`, `rc-status` etc.) have a `--user` (`-U`) flag to act on the OpenRC user session instead of the system session.

#### Service start on login (no lingering)

The OpenRC session is only active while the user is logged in (see [#lingering](https://wiki.gentoo.org#lingering)). As soon as the user logs out, the session is terminated.

##### PAM-based auto-start

The provided `pam_openrc.so` can automatically start openrc on login. It's loaded from /etc/pam.d/system-login, and dynamically starts /etc/init.d/user script, multiplexed for the user logging in, which launches the `openrc-user` daemon.

`openrc-user` opens another pam session for the user, which will last as long as any given session using pam\_openrc is active. It loads /etc/pam.d/openrc-user as the pam stack for the new session, then proceeds to launch the `default` runlevel for the user via calling the user's login shell with `-c` as an argument, and creating ${XDG\_CONFIG\_HOME}/rc/runlevels/default should it not exist.

In order to make use of the PAM-based auto-start, `XDG_RUNTIME_DIR` must be set during the login process, either by a pam module or shell rc file.


##### KDE compatibility

As at OpenRC 0.62.10, using OpenRC user services with KDE requires some configuration changes.

Firstly, [KDE](https://wiki.gentoo.org/wiki/KDE)/[Qt](https://wiki.gentoo.org/wiki/Qt) fails to find the socket for the [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) session bus created by the `dbus` user service at ${XDG\_RUNTIME\_DIR}/bus. (Note that this is distinct from the `dbus` *system* service, which provides a D-Bus *system* bus.)

As a result, the following script is needed, in either /etc/profile.d/ or the appropriate configuration file for the user's shell (e.g. \~/.bash\_profile):

Secondly, Gentoo bundles an autostart desktop file for PipeWire which conflicts with the PipeWire user services ([bug #964059](https://bugs.gentoo.org/show_bug.cgi?id=964059)); refer also to [PipeWire](https://wiki.gentoo.org/wiki/PipeWire#User_Services)).

To disable the conflicting autostart file, add `Hidden=true` to it. It's also possible to copy pipewire.desktop to ${XDG\_CONFIG\_HOME}/autostart/ to create a local override for only the current user (where `XDG_CONFIG_HOME` is \~/.config/ by default).

Once this is done, the `dbus`, `pipewire`, `pipewire-pulse` and `wireplumber` services can be enabled via:

`user $``rc-update --user add <service> default`
where `<service>` should be replaced by the name of each service in turn.

After logging out and logging back in, all services should now be running and able to be used by KDE.


#### Logging

To enable logging, add the following lines to ${XDG\_CONFIG\_HOME}/rc/rc.conf, creating that file if it doesn't already exist:

**`${XDG_CONFIG_HOME}/rc/rc.conf`**

```
rc_logger="YES"
rc_log_path="<location>/rc.log"
```
where `<location>` should be replaced by a literal directory path, e.g. /home/larry/logs/.

## System integration

### systemd compatibility

#### logind

Some setups require systemd-logind. [Elogind](https://wiki.gentoo.org/wiki/Elogind) can be a suitable replacement as a standalone logind running with OpenRC.

#### tmpfiles.d

systemd has a special tmpfiles.d file syntax for managing temporary files. [sys-apps/systemd-utils\[tmpfiles\]](https://packages.gentoo.org/packages/sys-apps/systemd-utils) is a standalone provider for OpenRC systems.

Both can also be used to manage volatile entries in /sys or /proc.

### udev and mdev

[udev](https://wiki.gentoo.org/wiki/Udev) and [mdev](https://wiki.gentoo.org/wiki/Mdev) are systems available on Gentoo to manage /dev. [eudev](https://wiki.gentoo.org/wiki/Eudev) used to be available as well, but has been removed. udev is available for OpenRC via the [sys-apps/systemd-utils\[udev\]](https://packages.gentoo.org/packages/sys-apps/systemd-utils) package, however should be able to work with both on Gentoo.

Older Gentoo installs used udev as the main [virtual/udev](https://packages.gentoo.org/packages/virtual/udev) provider. Based on [bug #575718](https://bugs.gentoo.org/show_bug.cgi?id=575718) this was changed to eudev, but it was changed back to udev in [bug #807193](https://bugs.gentoo.org/show_bug.cgi?id=807193). However, the rc service is still /etc/init.d/udev.

See [mdev](https://wiki.gentoo.org/wiki/Mdev), for possible use, e.g. for embedded systems.

### BusyBox integration

N.B. This section lacks information on how to actually use BusyBox with OpenRC.

Please note that there are currently many BusyBox applets that are incompatible with OpenRC, see [bug #529086](https://bugs.gentoo.org/show_bug.cgi?id=529086) for details. **Be warned that using OpenRC with BusyBox may require some work to set up.** BusyBox is more adapted to embedded use, see previous section about mdev.

[BusyBox](https://wiki.gentoo.org/wiki/Busybox) can be used to replace most of the userspace utilities needed by OpenRC (init, shell, awk, and other POSIX tools), by using a complete BusyBox as the shell for OpenRC [\[1\]](https://github.com/OpenRC/openrc/blob/master/BUSYBOX.md), all the calls that normally would cause a fork/exec would be spared, improving the overall speed.

The SysV-init /etc/inittab file provided by Gentoo is not compatible with the BusyBox init. Here is an example inittab compatible with BusyBox:

**`/etc/inittab`**

**Example inittab compatible with BusyBox init**

```
::sysinit:/sbin/openrc sysinit
::wait:/sbin/openrc boot
::wait:/sbin/openrc
```
BusyBox provides a number of applets that could be used to replace third party software like acpid or dhcp/dhcpcd.

See the OpenRC documentation on [using BusyBox with OpenRC](https://github.com/OpenRC/openrc/blob/master/BUSYBOX.md).

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
## See also

- [OpenRC/CGroups](https://wiki.gentoo.org/wiki/OpenRC/CGroups) — OpenRC includes support for [cgroups](https://wiki.gentoo.org/wiki/Cgroups).
- [OpenRC/openrc-init](https://wiki.gentoo.org/wiki/OpenRC/openrc-init) — OpenRC's own init system
- [OpenRC/Prefix](https://wiki.gentoo.org/wiki/OpenRC/Prefix) — The following guideline applies to a Gentoo [Prefix](https://wiki.gentoo.org/wiki/Prefix) on RHEL-5.6 amd64 and on Debian 6.0 amd64, for other setups it should be similar.
- [OpenRC/Stacked runlevel](https://wiki.gentoo.org/wiki/OpenRC/Stacked_runlevel) — a tutorial for setting up complicated networking with the help of stacked runlevel.
- [OpenRC/supervise-daemon](https://wiki.gentoo.org/wiki/OpenRC/supervise-daemon) — OpenRC's daemon supervisor
- [OpenRC/Users](https://wiki.gentoo.org/wiki/OpenRC/Users) — an (incomplete) list of distributions and operating systems using OpenRC.
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
