<!-- source: https://wiki.gentoo.org/wiki/Mako | group: Gentoo Wiki (Main) | wiki-title: Mako -->
---
title: Mako
url: https://wiki.gentoo.org/wiki/Mako
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-11"
fingerprint: d44272529c87b8ee
license: CC BY-SA 4.0
---

# Mako

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Mako** is a lightweight replacement for the notification daemons provided by most desktop environments. It implements the [FreeDesktop Notifications Specification](https://specifications.freedesktop.org/notification-spec/latest/).

## Installation

### USE flags


| [+icons](https://packages.gentoo.org/useflags/+icons) | Enable support for icons | 
| [elogind](https://packages.gentoo.org/useflags/elogind) | Enable session tracking via sys-auth/elogind | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

`root #``emerge --ask gui-apps/mako`
### Configuration

Mako is highly configurable; refer to the [mako(5)](https://man.archlinux.org/man/mako.5.en) [man page for details about the configuration file.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

For example, to configure Mako to use [gui-apps/wofi](https://packages.gentoo.org/packages/gui-apps/wofi) to present a list of options when the notification requires a user response, and the user right-clicks on the notification:

**`~/.config/mako/config`**

```
[actionable=true]
on-button-right=exec makoctl menu -n "${id}" wofi_run.sh dmenu
```
where `${id}` is the shell variable `id`, which will contain the ID of the notification, and wofi\_run.sh is a simple shell script, e.g.:

**`wofi_run.sh`**

```
#!/bin/sh
wofi --width=400 --height=260 --hide-scroll --show="${1}"
```
To get notification content, such as the subject or message, use [makoctl(1)](https://man.archlinux.org/man/makoctl.1.en) [and](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [jq(1)](https://man.archlinux.org/man/jq.1.en)[. For example, to send the message body to a webhook service like ntfy.sh:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

**`~/.config/mako/config`**

```
=exec curl -d "$(makoctl list | jq -r '.data|..|select(.id?.data=='$id')|.body|.data')" https://ntfy.sh/examplewebhook
```
To configure Mako to present notification messages with urgency 'critical' in the center of the screen with a red background:

**`~/.config/mako/config`**

To configure Mako to handle notifications from a specific application in a specific way:

**`~/.config/mako/config`**

### Usage

Mako will run automatically when a notification is emitted via D-Bus activation, so in most cases there is no need to explicitly start it up. A [running session bus](https://wiki.gentoo.org/wiki/D-Bus#The_session_bus) is needed in order to use Mako.

Mako can be started from your GUI's startup file, e.g. \~/.config/sway/config:

**`~/.config/sway/config`**

Mako can be controlled from the command line via [makoctl(1)](https://manpages.debian.org/bookworm/mako-notifier/makoctl.1.en.html). For example, to reload the configuration file:

`user $``makoctl reload`
To show again the most recent expired notification:

`user $``makoctl restore`
To view the history of expired notificactions:

`user $``makoctl history`
## See also

- [dunst](https://wiki.gentoo.org/wiki/Dunst) - a lightweight replacement for the notification daemons provided by most desktop environments, usable under both X and Wayland.
