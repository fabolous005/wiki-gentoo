<!-- source: https://wiki.gentoo.org/wiki/Configuring_a_system_without_elogind | group: Gentoo Wiki (Main) | wiki-title: Configuring a system without elogind -->
---
title: Configuring a system without elogind
url: https://wiki.gentoo.org/wiki/Configuring_a_system_without_elogind
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-19"
fingerprint: "88dffa1a7b257158"
license: CC BY-SA 4.0
---

# Configuring a system without elogind

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Various programs require a certain environment in order to provide their functionality, including setting particular environment variables. In particular, some software requires 'seat management' in order to mediate access to hardware shared by multiple system users (e.g. the display, input devices) without needing root access.

On systemd-based systems, logind ([systemd-logind.service(8)](https://man.archlinux.org/man/systemd-logind.service.8.en)[) is one of the programs used to set up the environment. On OpenRC-based systems, where logind isn't available,](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [elogind](https://wiki.gentoo.org/wiki/Elogind) ([sys-auth/elogind](https://packages.gentoo.org/packages/sys-auth/elogind), [elogind(8)](https://manpages.debian.org/testing/elogind/elogind.8.en.html)) can be used instead. However, some users might not wish to use elogind; this page describes the options available.

### seatd

In some instances, [seatd](https://wiki.gentoo.org/wiki/Seatd), provided by the [sys-auth/seatd](https://packages.gentoo.org/packages/sys-auth/seatd) package, might be a suitable replacement for the seat-management functionality provided by elogind. Refer to the linked wiki page and the [seatd(1)](https://man.archlinux.org/man/seatd.1.en) [man page for further information.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

### Environment variables

An important environment variable configured by elogind is `XDG_RUNTIME_DIR`, described on the [XDG/Base\_Directories](https://wiki.gentoo.org/wiki/XDG/Base_Directories) page:

The base directory relative to which user-specific non-essential runtime files and other file objects (such as sockets, named pipes, ...) should be stored. The directory MUST be owned by the user, who MUST be the only one having read and write access to it; its permissions MUST be 0700.


If not set by elogind, this variable needs to be manually set via the appropriate shell configuration file, e.g. \~/.bash\_login:

**`~/.bash_login`**

```
if test -z "${XDG_RUNTIME_DIR}"; then
    export XDG_RUNTIME_DIR=$(mktemp -d "${UID}-runtime-dir.XXX")
fi
```
However, some applications assume `XDG_RUNTIME_DIR` is set to /run/user/${UID}, e.g. gpg assumes this when deciding where to place various sockets. Additionally, the directory should ideally be placed in a directory on a tmpfs mount (like /run). So it may be wiser to use /run/user/${UID} like so:

**`~/.bash_login`**

```
if test -z "${XDG_RUNTIME_DIR}"; then
    export XDG_RUNTIME_DIR=/run/user/${UID}
fi
if test -d "${XDG_RUNTIME_DIR}"; then
    perms="$(stat -c '%a %u' "${XDG_RUNTIME_DIR}")"
    if [[ "${perms}" != "700 ${UID}" ]]; then
        export -n XDG_RUNTIME_DIR
        echo "WARNING! XDG_RUNTIME_DIR has incorrect permissions"
    fi
else
    mkdir -p "${XDG_RUNTIME_DIR}"
    chmod 0700 "${XDG_RUNTIME_DIR}"
fi
```
However, since /run/user might not exist and /run is owned by root, the above will fail due to permissions. One possibility is to create /etc/local.d/create-runuser.start with contents along the lines of:

**`/etc/local.d/create-runuser.start`**

```
#!/bin/sh
mkdir -p /run/user
chmod 1777 /run/user
```
Other environment variables that might also need to be manually set include `XDG_CACHE_HOME`, `XDG_CONFIG_HOME`, `XDG_DATA_HOME`, and `XDG_STATE_HOME`:

**`~/.bash_login`**

```
export XDG_CACHE_HOME="${HOME}/.cache"
export XDG_CONFIG_HOME="${HOME}/.config"
export XDG_DATA_HOME="${HOME}/.local/share"
export XDG_STATE_HOME="${HOME}/.local/state"
```
