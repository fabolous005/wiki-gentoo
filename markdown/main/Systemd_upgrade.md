<!-- source: https://wiki.gentoo.org/wiki/Systemd/upgrade | group: Gentoo Wiki (Main) | wiki-title: Systemd/upgrade -->
---
title: systemd/upgrade
url: https://wiki.gentoo.org/wiki/Systemd/upgrade
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-11-19"
fingerprint: "8122e2b73f4285d7"
license: CC BY-SA 4.0
---

# systemd/upgrade

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This page only lists changes, which must be considered before updating [Systemd](https://wiki.gentoo.org/wiki/Systemd) or risk system brakege. For regular updates, see [sys-apps/systemd's changelog](https://wiki.gentoo.org#See_also) or [upstream's NEWS file](https://wiki.gentoo.org#External_resources).

## systemd 203

The compatibility symlinks /bin/systemctl and /bin/systemd pointing to /usr/lib/systemd/systemd are deprecated. The ebuild checks whether the system was booted using compatibility symlinks and refuses to build the new version of systemd.

To upgrade systemd, update the [bootloader](https://wiki.gentoo.org/wiki/Bootloader) to use *init=/usr/lib/systemd/systemd* and reboot the system.

## systemd 200

See the [udev upgrade](https://wiki.gentoo.org/wiki/Udev/upgrade#udev_200) article.

## systemd 187

graphical.target now depends on the display-manager.service for starting a display manager like GDM, KDM, etc. . Create a new .service file, e.g. for KDM:

**`/etc/systemd/system/kdm.service`**

```
[Unit]
Description=KDM Display Manager
Conflicts=getty@tty1.service
After=systemd-user-sessions.service getty@tty1.service plymouth-quit.service
[Service]
ExecStart=/usr/bin/kdm -nodaemon
Restart=always
IgnoreSIGPIPE=no
[Install]
Alias=display-manager.service
```
Now enable the new service, e.g. for KDM:

`root #``systemctl enable kdm.service`
Afterwards starting KDM with systemd >=187 should work. Disable and delete the old .service file (e.g. kdm@.service).

For the rationale and more informations see the [Fedora 18 feature "Display Manager Infrastructure Rework"](https://fedoraproject.org/wiki/Features/DisplayManagerRework).
