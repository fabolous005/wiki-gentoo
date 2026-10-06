<!-- source: https://wiki.gentoo.org/wiki/Automatic_login_to_virtual_console | group: Gentoo Wiki (Main) | wiki-title: Automatic login to virtual console -->
---
title: Automatic login to virtual console
url: https://wiki.gentoo.org/wiki/Automatic_login_to_virtual_console
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-11"
fingerprint: ec837f4791eb5c
license: CC BY-SA 4.0
---

# Automatic login to virtual console

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gentoo supports different init systems. Each init system requires their own solution for auto-login.

All involve passing `--autologin <username>` to the terminal handler called agetty, but how this is done differs per init system

## Automatic login for different init systems

### sysvinit

Edit /etc/inittab as follows:

FILE **`/etc/inittab`**

```
# TERMINALS
c1:12345:respawn:/sbin/agetty --autologin <username> --noclear 38400 tty1 linux
c2:2345:respawn:/sbin/agetty 38400 tty2 linux
c3:2345:respawn:/sbin/agetty 38400 tty3 linux
c4:2345:respawn:/sbin/agetty 38400 tty4 linux
c5:2345:respawn:/sbin/agetty 38400 tty5 linux
c6:2345:respawn:/sbin/agetty 38400 tty6 linux
```
### openrc-init

Start with the [openrc-init](https://wiki.gentoo.org/wiki/OpenRC/openrc-init) instructions, and adapt it for auto login on the first tty as follows:

Create a file /etc/conf.d/agetty.tty1:

FILE **`/etc/conf.d/agetty.tty1`**

```
agetty_options="--autologin <username> --noclear"
```
Then for the changes to take affect you can restart tty1 with the following command:

`root #``rc-service agetty.tty1 restart`
### systemd

FILE **`/etc/systemd/system/getty@tty1.service.d/override.conf`**

```
[Service]
Type=simple
ExecStart=
ExecStart=-/sbin/agetty --autologin <username> --noclear %I 38400 linux
```
## Continue with starting an X session

This is described at [X without Display Manager](https://wiki.gentoo.org/wiki/X_without_Display_Manager).
