<!-- source: https://wiki.gentoo.org/wiki/Synergy | group: Gentoo Wiki (Main) | wiki-title: Synergy -->
---
title: Synergy
url: https://wiki.gentoo.org/wiki/Synergy
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-07-07"
fingerprint: "89d07c48a9be5bd5"
license: CC BY-SA 4.0
---

# Synergy

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Synergy is open source, cross platform software used to manage multiple displays (running on an Xorg display server) with a single keyboard and mouse. It can be thought of as a KVM - but only supporting keyboard and mouse. As of 2023-07-06, Synergy does not support compositors running on the Wayland protocol.[\[1\]](https://wiki.gentoo.org#cite_note-1)

## Installation

### USE flags

*x11-misc/synergy*correct?

### Emerge

`root #``emerge --ask x11-misc/synergy`
## Configuration

### Files

- /etc/synergy.conf - Global (system wide) configuration file.
- \~/.synergy - Local (per user) configuration directory.

### Service

See man 1 synergys for Synergy service options.

Gentoo does not presently include OpenRC or systemd service scripts for Synergy.

### Encryption

The Synergy wiki upstream provides details for enabling encryption between the client/server connection(s).

In short, copy the bundled encryption plugin to the Synergy user's home directory:

`user $````
mkdir -p ~/.synergy/plugins
```
`user $````
cp /usr/lib64/synergy/plugins/libns.so ~/.synergy/plugins
```
Next, generate the certificate:

## Usage

There are a few executable files that are merged after the compilation:

- /usr/bin/syntool
- /usr/bin/synergyc - Used to connect to a Synergy mouse/keyboard sharing server.
- /usr/bin/synergys - Starts and controls the synergy mouse/keyboard sharing server. See man 1 synergys for options.
- /usr/bin/qsynergy - Graphical user interface for Synergy. This can be disabled via the `gui` USE flag.

## Troubleshooting

### Wayland support

No, Synergy is not presently supported in environments running compositors using the Wayland protocol as a backend. Follow upstream's [GitHub issue #4090](https://github.com/symless/synergy-core/issues/4090) to stay in the loop.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose x11-misc/synergy`
## See also

- [TigerVNC](https://wiki.gentoo.org/wiki/TigerVNC) — a client/server software package allowing remote network access to graphical desktops.

## External resources

- [https://github.com/symless/synergy-core/wiki](https://github.com/symless/synergy-core/wiki) - Upstream's project wiki.
- [https://brahma-dev.github.io/synergy-stable-builds/](https://brahma-dev.github.io/synergy-stable-builds/) - A third party site that hosts compiled binaries for Windows and OSX.
