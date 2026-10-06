<!-- source: https://wiki.gentoo.org/wiki/COSMIC | group: Gentoo Wiki (Main) | wiki-title: COSMIC -->
---
title: COSMIC
url: https://wiki.gentoo.org/wiki/COSMIC
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-22"
fingerprint: c978b253188967e7
license: CC BY-SA 4.0
---

# COSMIC

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**COSMIC** is a desktop environment built by System76 in Rust.


## Installation

On [musl](https://wiki.gentoo.org/wiki/Musl) profiles, build [Rust](https://wiki.gentoo.org/wiki/Rust#source) ([dev-lang/rust](https://packages.gentoo.org/packages/dev-lang/rust)) from source before attempting installation.


### Overlay

fsvm88's [cosmic-overlay](https://github.com/fsvm88/cosmic-overlay) provides all necessary packages.

`root #````
eselect repository add cosmic-overlay git https://github.com/fsvm88/cosmic-overlay.git
```
`root #````
emaint sync -r cosmic-overlay
```

### Unmasking unstable ebuilds

For the latest tagged release, [accept](https://wiki.gentoo.org/wiki/ACCEPT_KEYWORDS) unstable ebuilds:

**`/etc/portage/package.accept_keywords/cosmic-de`**

**Unmasking unstable ebuilds**

```
cosmic-base/*
cosmic-de/*
```

### Unmasking live ebuilds

To try out the latest commits from the `master` branch, [accept](https://wiki.gentoo.org/wiki/ACCEPT_KEYWORDS) live ebuilds:

**`/etc/portage/package.accept_keywords/cosmic-de`**

**Unmasking unstable ebuilds**

```
# Live ebuilds are masked via "missing" keywords
cosmic-base/* **
cosmic-de/* **
```

### Emerge

After unmasking the ebuilds, COSMIC can be installed by emerging the following packages:

`root #``emerge --ask cosmic-meta pop-theme-meta`

## Configuration

The most convenient method of login management while using COSMIC is to use a display manager such as [greetd](https://wiki.gentoo.org/wiki/Greetd). If a display manager is not already in use, install greetd:

`root #``emerge --ask gui-libs/greetd`
The following assumes greetd is being used. Users of [SDDM](https://wiki.gentoo.org/wiki/SDDM) should refer to [the "SDDM" section](https://wiki.gentoo.org/wiki/COSMIC#SDDM).

### systemd

Enable the `greetd`, `cosmic-greeter`, `upower`, and `acpid` services.

`root #``systemctl enable greetd.service cosmic-greeter.service cosmic-greeter-daemon.service upower.service acpid.socket`
### OpenRC

In addition to greetd, [gui-libs/display-manager-init](https://packages.gentoo.org/packages/gui-libs/display-manager-init) and [elogind](https://wiki.gentoo.org/wiki/Elogind) should also be installed:

`root #``emerge --ask gui-libs/display-manager-init gui-libs/greetd sys-auth/elogind`
Configure greetd as a [display manager](https://wiki.gentoo.org/wiki/Display_manager):

**`/etc/conf.d/display-manager`**

**greetd with cosmic-greeter**

```
CHECKVT=7
DISPLAYMANAGER="greetd"
```
Add the relevant OpenRC services to the relevant runlevels:

`root #``rc-update add elogind boot``root #``rc-update add display-manager default`
Configure greetd to run cosmic-greeter as the frontend, which must be run as the `cosmic-greeter` user.

**`/etc/greetd/config.toml`**

**greetd with cosmic-greeter**

```
[terminal]
vt = 7
[default_session]
command = "/usr/bin/dbus-run-session /usr/bin/cosmic-comp /usr/bin/cosmic-greeter >>/var/log/cosmic.log 2>&1"
user = "cosmic-greeter"
```
Finally, reboot the machine.

Optionally, logging can be configured to use the system logger directly, rather than writing to a separate file:

**`/etc/greetd/config.toml`**

**greetd with cosmic-greeter**

```
[terminal]
vt = 7
[default_session]
command = "/usr/bin/dbus-run-session /usr/bin/cosmic-comp /usr/bin/cosmic-greeter 2>&1 | /usr/bin/logger -t cosmic-greeter"
user = "cosmic-greeter"
```
Additionally, if auto-login is desired:

**`/etc/greetd/config.toml`**

**greetd autologin to COSMIC**

```
[terminal]
vt = 7
[default_session]
command = "/usr/bin/dbus-run-session env XDG_SESSION_TYPE=wayland XDG_CURRENT_DESKTOP=COSMIC /usr/bin/start-cosmic 2>&1 | /usr/bin/logger -t cosmic-greeter"
user = "your-username"
```

### SDDM

To make COSMIC launch from SDDM's session list, create or edit /usr/share/wayland-sessions/cosmic.desktop:

**`/usr/share/wayland-sessions/cosmic.desktop`**

**COSMIC session for SDDM**

```
[Desktop Entry]
Name=COSMIC
Comment=System76 COSMIC Desktop
Exec=dbus-run-session env XDG_SESSION_TYPE=wayland XDG_CURRENT_DESKTOP=COSMIC start-cosmic
Type=Application
DesktopNames=COSMIC
```
If auto-login is desired:

**`/etc/sddm.conf.d/10-autologin.conf`**

**SDDM autologin to COSMIC**

```
[Autologin]
User=your-username
Session=cosmic.desktop
```

### Locales

In the COSMIC ecosystem, locales are managed via the COSMIC "initial setup" and "settings" applications.


#### systemd

On [systemd](https://wiki.gentoo.org/wiki/Systemd) systems, the required localed daemon should be running already.


#### OpenRC

To manage locales on [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) systems, the `openrc-settingsd` service must be used; this service provides the `org.freedesktop.locale1` D-Bus interface.

Add the `openrc-settingsd` service to the `default` runlevel, and start it:

`root #``rc-update add openrc-settingsd default``root #``rc-service openrc-settingsd start`

## Troubleshooting


### COSMIC session does not start

If the COSMIC session does not start, first check if it starts successfully via a TTY:

`root #````
dbus-run-session -- env XDG_SESSION_TYPE=wayland XDG_CURRENT_DESKTOP=COSMIC start-cosmic
```

#### Check D-Bus services

Inside a running COSMIC session, verify that System76 [D-Bus](https://wiki.gentoo.org/wiki/Dbus) services are available:

`root #````
dbus-send --session --dest=org.freedesktop.DBus --type=method_call --print-reply /org/freedesktop/DBus org.freedesktop.DBus.ListNames | sed -n 's/.*string "\(com\.system76[^"]*\)".*/\1/p'
```
The output should list multiple `com.system76` names, e.g. `com.system76.CosmicSettingsDaemon`.


#### Check session environment

Ensure the session has the `XDG_CURRENT_DESKTOP` and `XDG_SESSION_TYPE` variables set correctly:

`root #````
env | grep -E 'XDG_CURRENT_DESKTOP|XDG_SESSION_TYPE|XDG_RUNTIME_DIR|DBUS_SESSION_BUS_ADDRESS'
```
The value of `XDG_CURRENT_DESKTOP` should be `COSMIC`, and the value of `XDG_SESSION_TYPE` should be `wayland`.


#### Verify services are running

The `dbus` service, which provides a [D-Bus](https://wiki.gentoo.org/wiki/Dbus) *system* bus (as distinct from a *session* bus started via e.g. dbus-run-session), and the login/session manager service, e.g. `elogind`on OpenRC and `systemd-logind` on systemd, must be running for COSMIC to start properly.

On OpenRC systems:

`root #``rc-service dbus status``root #``rc-service elogind status`
On systemd systems:

`root #``systemctl status dbus``root #``systemctl status systemd-logind`

#### Check greeter user runtime directory

If using greetd, confirm the `cosmic-greeter` user has a runtime directory:

`root #``ls -ld /run/user/$(id -u cosmic-greeter)`
If that directory is missing, verify that /etc/pam.d/greetd contains:

**`/etc/pam.d/greetd`**

**PAM config for greetd with elogind**

```
auth      include  system-login
account   include  system-login
password  include  system-login
session   include  system-login
session   required pam_elogind.so
```

#### Examine logs

If COSMIC immediately returns to the greeter, check the logs:

`root #````
tail -n 200 /var/log/messages | grep -Ei 'cosmic-greeter|cosmic-comp|elogind|dbus|permission|denied'
```

## See also

- [List of software for Wayland](https://wiki.gentoo.org/wiki/List_of_software_for_Wayland) — various desktop related packages for Wayland
- [Hyprland](https://wiki.gentoo.org/wiki/Hyprland) — an open-source [Wayland compositor](https://wiki.gentoo.org/wiki/Wayland_compositor) written in C++.
- [Plasma](https://wiki.gentoo.org/wiki/Plasma) — a free software community, producing a wide range of applications including the popular Plasma desktop environment.
- [Gnome](https://wiki.gentoo.org/wiki/Gnome) — a feature-rich desktop environment provided by the [GNOME project](https://www.gnome.org).
