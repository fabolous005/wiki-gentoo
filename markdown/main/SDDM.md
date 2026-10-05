<!-- source: https://wiki.gentoo.org/wiki/SDDM | group: Gentoo Wiki (Main) | wiki-title: SDDM -->
---
title: SDDM
url: https://wiki.gentoo.org/wiki/SDDM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-21"
fingerprint: "8464735978b7c9dc"
license: CC BY-SA 4.0
---

# SDDM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**S**imple **D**esktop **D**isplay **M**anager (SDDM) is a modern [display manager](https://wiki.gentoo.org/wiki/Display_manager) that supports both the [X server](https://wiki.gentoo.org/wiki/X_server) and the [Wayland](https://wiki.gentoo.org/wiki/Wayland) protocol.

## Installation

### USE flags


| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [+elogind](https://packages.gentoo.org/useflags/+elogind) | Enable session tracking via sys-auth/elogind | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

Install [x11-misc/sddm](https://packages.gentoo.org/packages/x11-misc/sddm):

`root #``emerge --ask x11-misc/sddm`
If there are performance issues, it might help to add the sddm user to the video group:

`root #``usermod -a -G video sddm`
## Configuration

### Files

SDDM has two configuration locations: the "system configuration" directory, /usr/share/sddm/sddm.conf.d/, and the "local configuration" directory, /etc/sddm.conf.d/, which is used to override specific options, as described by [sddm.conf(5)](https://man.archlinux.org/man/sddm.conf.5.en)[. Changes should not be made to the files in the system configuration directory.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

KDE Plasma writes changed options to /etc/sddm.conf.d/.

Refer to the [sddm.conf(5)](https://man.archlinux.org/man/sddm.conf.5.en) [man page for details on the file format and the available options.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)


#### override.conf and Xsetup files

Some configuration cases require some commands to be executed before starting the [X server](https://wiki.gentoo.org/wiki/X_server) and showing the greeter screen. For these cases, use a custom Xsetup file.

Create an SDDM local configuration directory if it does not exist:

`root #``mkdir -p /etc/sddm.conf.d`
Then create and edit the file /etc/sddm.conf.d/override.conf, with the following lines in it:

**`/etc/sddm.conf.d/override.conf`**

```
[X11]
DisplayCommand=/etc/sddm/scripts/Xsetup
```
Next create the directory /etc/sddm/scripts and the file /etc/sddm/scripts/Xsetup:

`root #````
mkdir -p /etc/sddm/scripts
```
`root #````
touch /etc/sddm/scripts/Xsetup
```
`root #````
chmod a+x /etc/sddm/scripts/Xsetup
```
**`/etc/sddm/scripts/Xsetup`**

```
#!/bin/sh
# Xsetup - run as root before the login dialog appears
 
# Add various commands here for execution on SDDM start.
```

#### override.conf and X11 rootless mode

By default SDDM runs in X11 as the root user. This may be considered a security risk.

Since [x11-misc/sddm](https://packages.gentoo.org/packages/x11-misc/sddm) version 0.20 it is possible to run SDDM in X11 rootless mode instead.

The display server configuration must be overridden using /etc/sddm.conf.d/override.conf, as described in the [override.conf and Xsetup files](https://wiki.gentoo.org/wiki/SDDM#override.conf_file) section above. To do so, add:

**`/etc/sddm.conf.d/override.conf`**

```
[General]
DisplayServer=x11-user
```

#### override.conf and Wayland mode

Since [x11-misc/sddm](https://packages.gentoo.org/packages/x11-misc/sddm) version 0.20 it is possible to run SDDM in Wayland mode.

##### Weston

SDDM supports several Wayland compositors. This first example uses SDDMs default compositor, Weston, for systems where [kde-plasma/plasma-desktop](https://packages.gentoo.org/packages/kde-plasma/plasma-desktop) is not used. First install [dev-libs/weston](https://packages.gentoo.org/packages/dev-libs/weston) with the [kiosk](https://packages.gentoo.org/useflags/kiosk) [USE flag enabled.](https://wiki.gentoo.org/wiki/USE_flag)

The display server configuration must be overridden using /etc/sddm.conf.d/override.conf, as described in the [override.conf and Xsetup files](https://wiki.gentoo.org/wiki/SDDM#override.conf_file) section above. To do so, add:

**`/etc/sddm.conf.d/override.conf`**

```
[General]
DisplayServer=wayland
```
Additionally, it is possible to add a `[Wayland]` section to that file, e.g.:

**`/etc/sddm.conf.d/override.conf`**

```
# ...
[Wayland]
CompositorCommand=weston --shell=kiosk
```
##### Kwin

Users of systems using [kde-plasma/plasma-desktop](https://packages.gentoo.org/packages/kde-plasma/plasma-desktop) may want to use Kwin as the compositor instead. To do so, override by defaults via /etc/sddm.conf.d/override.conf, as described in the "[override.conf and Xsetup files](https://wiki.gentoo.org/wiki/SDDM#override.conf_file)" section above:

**`/etc/sddm.conf.d/override.conf`**

```
[General]
DisplayServer=wayland
GreeterEnvironment=QT_WAYLAND_SHELL_INTEGRATION=layer-shell
 
[Wayland]
CompositorCommand=kwin_wayland --drm --no-lockscreen --no-global-shortcuts --locale1
```
#### elogind (OpenRC) workarounds

Running SDDM in Wayland mode on an OpenRC/elogind system requires additional configuration. The main challenge is that elogind does not always set XDG\_RUNTIME\_DIR for sessions started by SDDM's Wayland helper chain.

##### seatd

SDDM's Wayland compositor (typically Weston) needs a seat manager. While elogind provides seat management for X11 sessions, Weston uses libseat which works best with the seatd daemon.

Enable the server USE flag:

**`/etc/portage/package.use/seatd`**

Rebuild and start the service:

`root #````
emerge --oneshot sys-auth/seatd
```
`root #````
rc-update add seatd default
```
`root #````
rc-service seatd start
```
Add the display manager user and the login user to the seat group:

`root #````
usermod -aG seat sddm
```
`root #````
usermod -aG seat <username>
```
##### Weston kiosk shell

The kiosk USE flag must be enabled for Weston:

**`/etc/portage/package.use/weston`**

`root #``emerge --oneshot dev-libs/weston`
##### Fixing XDG\_RUNTIME\_DIR for the greeter

SDDM starts the Wayland greeter via sddm-helper-start-wayland, which launches Weston and then the Qt6 greeter inside it. On elogind systems, XDG\_RUNTIME\_DIR is not set for the sddm user in this context, causing Weston to fail silently.

Back up the original binary and create a wrapper:

`root #``mv /usr/libexec/sddm-helper-start-wayland /usr/libexec/sddm-helper-start-wayland.real`
**`/usr/libexec/sddm-helper-start-wayland`**

```
#!/bin/sh
if [ -z "$XDG_RUNTIME_DIR" ]; then
    export XDG_RUNTIME_DIR="/run/user/$(id -u)"
    mkdir -p "$XDG_RUNTIME_DIR"
    chmod 700 "$XDG_RUNTIME_DIR"
fi
export SEATD_SOCK=/run/seatd.sock
exec /usr/libexec/sddm-helper-start-wayland.real "$@"
```
`root #``chmod +x /usr/libexec/sddm-helper-start-wayland`
##### Fixing XDG\_RUNTIME\_DIR for the user session

The same issue affects the user session. SDDM's wayland-session script starts a login shell which sources /etc/profile. Create a fallback:

**`/etc/profile.d/xdg-runtime.sh`**

```
if [ -z "$XDG_RUNTIME_DIR" ]; then
    export XDG_RUNTIME_DIR="/run/user/$(id -u)"
    if [ ! -d "$XDG_RUNTIME_DIR" ]; then
        mkdir -p "$XDG_RUNTIME_DIR"
        chmod 700 "$XDG_RUNTIME_DIR"
    fi
fi
```
Export the seatd socket path globally:

**`/etc/profile.d/seatd.sh`**

```
export SEATD_SOCK=/run/seatd.sock
```
##### Troubleshooting

| Symptom | Cause | Fix | 
|---|---|---|
| Greeter appears and disappears instantly | Multiple possible causes | See below | 
| No backend was able to open a seat | seatd not running or sddm user not in seat group | Start seatd, add sddm to seat group | 
| Failed to load module: .../wayland-backend.so | Missing --backend=drm in CompositorCommand | Add --backend=drm to weston command | 
| Failed to load module: .../kiosk-shell.so | Missing kiosk USE flag on weston | Enable kiosk USE flag, rebuild weston | 
| XDG\_RUNTIME\_DIR is not set | elogind not setting runtime dir | Apply the wrapper scripts (see above) | 
| Could not connect to socket /run/seatd.sock: Permission denied | User not in seat group | `root #``usermod -aG seat <username>` | 
| Desktop flashes briefly then returns to greeter | XDG\_RUNTIME\_DIR unset for user session | Create /etc/profile.d/xdg-runtime.sh | 

##### Technical background

On systemd, logind automatically creates /run/user/\<uid> and sets XDG\_RUNTIME\_DIR for every PAM session. On OpenRC with elogind, this works for most login paths but fails for SDDM's Wayland helper chain because sddm-helper launches sddm-helper-start-wayland (for the greeter) or wayland-session (for the user session) in a way that elogind's PAM module does not propagate the variable into the child process environment.

##### Summary of required changes

| File | Purpose | 
|---|---|
| /etc/portage/package.use/seatd | sys-auth/seatd server | 
| /etc/portage/package.use/weston | dev-libs/weston kiosk | 
| /usr/libexec/sddm-helper-start-wayland | Wrapper setting XDG\_RUNTIME\_DIR + SEATD\_SOCK | 
| /etc/profile.d/xdg-runtime.sh | Fallback XDG\_RUNTIME\_DIR for user sessions | 
| /etc/profile.d/seatd.sh | Export SEATD\_SOCK globally | 
| User + sddm in seat group | Required for seatd socket access | 


### Keymap

To select the correct keymap on the login screen, add the command setxkbmap to the /etc/sddm/scripts/Xsetup file, as described in the "[override.conf and Xsetup files](https://wiki.gentoo.org/wiki/SDDM#override.conf_file)" section above:

**`/etc/sddm/scripts/Xsetup`**

```
#!/bin/sh
# Xsetup - run as root before the login dialog appears
 
# ...
 
# First keymap will be gb
setxkbmap gb,us
```
The first country code is the default.

### Multimonitor configuration

The greeter location can be set by [xrandr](https://wiki.gentoo.org/wiki/Xrandr) configuration in /etc/sddm/scripts/Xsetup, as described in the "[override.conf and Xsetup files](https://wiki.gentoo.org/wiki/SDDM#override.conf_file)" section above. Firstly, install xrandr:

`root #``emerge --ask x11-apps/xrandr`
Then use xrandr to list connected monitors:

`user $``xrandr | grep -w connected`
DP-2 connected 2160x3840+0+0 left (normal left inverted right x axis y axis) 597mm x 336mm
DP-4 connected primary 3840x2160+2160+0 (normal left inverted right x axis y axis) 697mm x 392mm

The above example lists two connected monitors: primary (`DP-4`) and secondary (`DP-2`), meaning the following lines should be added to the [/etc/sddm/scripts/Xsetup](https://wiki.gentoo.org/wiki/SDDM#override.conf_file) file:

**`/etc/sddm/scripts/Xsetup`**

```
#!/bin/sh
# Xsetup - run as root before the login dialog appears
 
# ...
 
# First, set DP-4 as primary
xrandr --output DP-4 --auto --primary
# Second, DP-2 will be placed on left of DP-4 and rotated by 270 degree clockwise.
xrandr --output DP-2 --left-of DP-4 --rotate left --noprimary
```
After reboot SDDM will show the greeter in `DP-4` monitor.

### Service

#### OpenRC

If [gui-libs/display-manager-init](https://packages.gentoo.org/packages/gui-libs/display-manager-init) is not present, emerge it with:

`root #``emerge --ask gui-libs/display-manager-init`
The configuration file should be modified to use SDDM:

**`/etc/conf.d/display-manager`**

**Set SDDM as the display manager**

```
CHECKVT=7
DISPLAYMANAGER="sddm"
```
In its default configuration, SDDM likely won't start without [elogind](https://wiki.gentoo.org/wiki/Elogind). Enable and start it using:

`root #``rc-update add elogind boot``root #``rc-service elogind start`
To start the chosen display manager on boot, add the *display-manager* to the system's *default* runlevel:

`root #``rc-update add display-manager default`
To start the *display-manager* immediately, run:

`root #``rc-service display-manager start`
#### systemd

SDDM might not start properly if a machine-id hasn't been generated. To ensure the system has a machine-id:

`root #``if [ ! -f /etc/machine-id ]; then systemd-machine-id-setup; fi`
To start SDDM on boot:

`root #``systemctl enable sddm.service`
To start SDDM now:

`root #``systemctl start sddm.service`
### Plasma

Graphical configuration is integrated in [Plasma](https://wiki.gentoo.org/wiki/KDE) system settings by installing [kde-plasma/sddm-kcm](https://packages.gentoo.org/packages/kde-plasma/sddm-kcm):

`root #``emerge --ask kde-plasma/sddm-kcm`
## Troubleshooting

### Long load time before SDDM shows the greeter

A low entropy pool can cause long SDDM load time - see [upstream bug report](https://github.com/sddm/sddm/issues/1036). If using systemd, the graphical target is reached and then everything appears to hang. Moving the mouse or using the keyboard will make the SDDM greeter launch (faster).

Solve the problem by using, for example, the [sys-apps/haveged](https://packages.gentoo.org/packages/sys-apps/haveged) package to increase the entropy pool or by enabling `RANDOM_TRUST_CPU` kernel config option [with a recent-enough CPU](https://en.wikipedia.org/wiki/RdRand).

### Permission denied errors in Xorg.log

The [X server](https://wiki.gentoo.org/wiki/X_server) will not start and permission denied errors (such as the following) are present in the Xorg.log log file:

**`/var/log/Xorg.log`**

It is likely the sddm user has not been added to the video group. Running the following command should fix the problem:

`root #``usermod -a -G video sddm`
SDDM only displays buttons if the functionality evaluates to being available. This can depend on several factors. For OpenRC systems using [elogind](https://wiki.gentoo.org/wiki/Elogind), this can be caused by the service not running when the display manager is initialized. To make sure it is running, just add elogind to the boot run level.

`root #``rc-update add elogind boot`
### Missing users

If the login screen is missing some user(s) to choose from, this might be caused by /etc/sddm.conf - standard `MinimumUid` is 1000 and some existing users may have lower uids.

**`/etc/sddm.conf`**

```
[Users]
MaximumUid=60000
MinimumUid=1000
```
### SDDM service starts but yields a black screen

On [NVIDIA](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) cards, SDDM appears to require DRM enabled and operational according to [this closed bug report](https://bugs.gentoo.org/show_bug.cgi?id=697634).

Ensure Direct Rendering Manager is enabled in the kernel. If it isn't, enable it, rebuild the kernel, and re-emerge [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers).

**Enable Direct Rendering Manager**

Ensure the nvidia-drm module is configured to load at startup and `modeset` option enabled:

**`/etc/modules-load.d/nvidia-drm.conf`**

Alternatively, `modeset` option can be enabled on the [kernel command line](https://wiki.gentoo.org/wiki/Kernel#Kernel_command-line_parameters) with `nvidia-drm.modeset=1`. This can be set in the [bootloader](https://wiki.gentoo.org/wiki/Bootloader) or embedded in the kernel configuration.

## See also

- [LightDM](https://wiki.gentoo.org/wiki/LightDM)
- [SLiM](https://wiki.gentoo.org/wiki/SLiM) — a desktop-independent graphical [display manager](https://wiki.gentoo.org/wiki/Display_manager).
