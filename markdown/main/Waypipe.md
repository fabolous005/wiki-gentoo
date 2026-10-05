<!-- source: https://wiki.gentoo.org/wiki/Waypipe | group: Gentoo Wiki (Main) | wiki-title: Waypipe -->
---
title: Waypipe
url: https://wiki.gentoo.org/wiki/Waypipe
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-03"
fingerprint: dec33a4c7bd21dd0
license: CC BY-SA 4.0
---

# Waypipe

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Waypipe** is a proxy for Wayland clients. It forwards Wayland messages and serializes changes to shared memory buffers over a single socket. This makes application forwarding similar to `ssh -X` feasible.

## Installation

### USE flags


| [dmabuf](https://packages.gentoo.org/useflags/dmabuf) | Use DMABUFs for data exchange and hardware decoding | 
| [ffmpeg](https://packages.gentoo.org/useflags/ffmpeg) | Link with ffmpeg to allow buffer displays using video streams | 
| [lz4](https://packages.gentoo.org/useflags/lz4) | Enable support for lz4 compression (as implemented in app-arch/lz4) | 
| [systemtap](https://packages.gentoo.org/useflags/systemtap) | Enable SystemTap/DTrace tracing | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [vaapi](https://packages.gentoo.org/useflags/vaapi) | Enable Video Acceleration API for hardware decoding | 
| [zstd](https://packages.gentoo.org/useflags/zstd) | Enable support for ZSTD compression | 

### Emerge

`root #``emerge --ask gui-apps/waypipe`
### Environment

To connect to a server with waypipe, the server needs `XDG_RUNTIME_DIR` set to an accessible directory.

However, for example when using sshd without pam, `XDG_RUNTIME_DIR` can end up unset, and the path it should point to (usually /run/user/$UID) won't be created by [elogind](https://wiki.gentoo.org/wiki/Elogind) or [systemd](https://wiki.gentoo.org/wiki/Systemd).

To ensure the proper environment, append this sh-code to your \~/.bashrc or equivalent login script of the target user on the server.

**`~/.bashrc`**

```
if [ -z "$XDG_RUNTIME_DIR" ]; then
    export XDG_RUNTIME_DIR="/run/user/$(id -u)"
    mkdir -p "$XDG_RUNTIME_DIR"
    chmod 700 "$XDG_RUNTIME_DIR"
fi
```
## Usage

Any GUI program can be started remotely through Waypipe, by prefixing an ssh command with `waypipe` and connecting to a server that has Waypipe installed:

`user $````
waypipe ssh user@::
Last login from ::1
```
`user $``kwrite`
For example to run [Sway](https://wiki.gentoo.org/wiki/Sway), use:

`user $``waypipe ssh user@127.0.0.1 sway`
Refer to the [waypipe(1)](https://man.archlinux.org/man/waypipe.1.en) [man page for detailed usage information.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)
