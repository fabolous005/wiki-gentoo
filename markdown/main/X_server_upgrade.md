<!-- source: https://wiki.gentoo.org/wiki/X_server/upgrade | group: Gentoo Wiki (Main) | wiki-title: X server/upgrade -->
---
title: X server/upgrade
url: https://wiki.gentoo.org/wiki/X_server/upgrade
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-11-19"
fingerprint: "67dba70a8d0e3814"
license: CC BY-SA 4.0
---

# X server/upgrade

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page lists critical changes that must be considerd before updating to avoid system breakage. Usual [X server](https://wiki.gentoo.org/wiki/X_server) updates require no special action.

## xorg-server 1.17

- [x11-drivers/xf86-video-modesetting](https://packages.gentoo.org/packages/x11-drivers/xf86-video-modesetting) was merged into xorg-server-1.17 and must be masked if present:

- `root #``echo "=x11-drivers/xf86-video-modesetting" > /etc/portage/package.mask/video-modesetting`

## xorg-server 1.13

- Rebuild all graphics and input drivers because the ABI changed:

- `root #``emerge --ask @x11-module-rebuild`

## xorg-server 1.12

- Rebuild all graphics and input drivers because the ABI changed:

- `root #``emerge --ask @x11-module-rebuild`

## xorg-server 1.11

- Rebuild all graphics and input drivers because the ABI changed:

- `root #``emerge --ask @x11-module-rebuild`

## xorg-server 1.10

- X.Org server no longer does autodetect devices using [x11-drivers/xf86-input-keyboard](https://packages.gentoo.org/packages/x11-drivers/xf86-input-keyboard) and [x11-drivers/xf86-input-mouse](https://packages.gentoo.org/packages/x11-drivers/xf86-input-mouse). For input devices to be able to be hotplugged, migrate settings to the [evdev](https://wiki.gentoo.org/wiki/Evdev) driver.

- Rebuild all graphics and input drivers because the ABI changed:

- `root #``emerge --ask @x11-module-rebuild`

## xorg-server 1.9

- X.Org server can detect input devices using [udev](https://wiki.gentoo.org/wiki/Udev), removing its HAL support. Users who used HAL before need to migrate to udev.

- Rebuild all graphics and input drivers because the ABI changed:

- `root #``emerge --ask @x11-module-rebuild`
