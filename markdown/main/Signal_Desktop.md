<!-- source: https://wiki.gentoo.org/wiki/Signal_Desktop | group: Gentoo Wiki (Main) | wiki-title: Signal Desktop -->
---
title: Signal Desktop
url: https://wiki.gentoo.org/wiki/Signal_Desktop
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-05"
fingerprint: b22ef49e2771d1dc
license: CC BY-SA 4.0
---

# Signal Desktop

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Signal Desktop** is a messaging application geared towards privacy. It is endorsed by Edward Snowden.

## Installation

### USE flags


### Emerge

Install [net-im/signal-desktop-bin](https://packages.gentoo.org/packages/net-im/signal-desktop-bin):

`root #``emerge --ask net-im/signal-desktop-bin`
## Usage

### Language Selection and Spell Checking

Signal respects the `LANGUAGE` environment variable, which takes a colon-separated list of locale names.
All entries will be used for spell checking. Additionally, the first entry defines the UI language, overriding any setting in preferences.
The debug log, accessible via View > Debug Log, lists all supported locales.

The following snippet launches Signal with an English user interface and spell checking in English and German:

`user $``LANGUAGE=en_US:de_DE signal-desktop`
### System tray

Use one of these commands to open Signal in focus and remain in the system tray, when you close it, or start minimized in the tray:

`user $``signal-desktop --use-tray-icon``user $``signal-desktop --start-in-tray`
## Troubleshooting

It is possible to get the following error when starting signal-desktop if /tmp has been mounted with the `noexec` mount option:

Uncaught error or unhandled promise rejection: Error: /tmp/.org.chromium.Chromium.xxxxxx: failed to map segment from shared object

To solve the issue remount /tmp without the execution restriction:

`root #``mount -o remount,exec /tmp`
In order to make the change persist across reboots, it will also be needed to remove the option from /etc/fstab.

### Unable to add attachments due to File Chooser not opening

If selecting the "Add attachment" icon (the 'paperclip' icon) has no effect, and doesn't open a File Chooser dialog, ensure that a [xdg-desktop-portal](https://wiki.gentoo.org/wiki/Xdg-desktop-portal) frontend implementing the FileChooser interface is installed (e.g. [sys-apps/xdg-desktop-portal-gtk](https://packages.gentoo.org/packages/sys-apps/xdg-desktop-portal-gtk)) and that xdg-desktop-portal is configured to use it, via $XDG\_CONFIG\_DIR/xdg-desktop-portal/portals.conf:

**`~/$XDG_CONFIG_DIR/xdg-desktop-portal/portals.conf`**

## Removal

Deselect Signal Desktop by issuing:

`root #``emerge --deselect net-im/signal-desktop-bin`
