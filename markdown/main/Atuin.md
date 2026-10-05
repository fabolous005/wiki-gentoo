<!-- source: https://wiki.gentoo.org/wiki/Atuin | group: Gentoo Wiki (Main) | wiki-title: Atuin -->
---
title: Atuin
url: https://wiki.gentoo.org/wiki/Atuin
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-13"
fingerprint: e6585c5e3f8f58ec
license: CC BY-SA 4.0
---

# Atuin

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Atuin** is a [shell](https://wiki.gentoo.org/wiki/Shell) history manager supporting encrypted synchronization. It replaces shell command history records with a SQLite database providing greater flexibility for searching and synchronization. The standard `Ctrlr`-bound history overview gets superseded by an advanced TUI.

## Installation

### USE flags


| [+client](https://packages.gentoo.org/useflags/+client) | Enable the autin client | 
| [+daemon](https://packages.gentoo.org/useflags/+daemon) | Enable the autin background daemon on the client | 
| [+sync](https://packages.gentoo.org/useflags/+sync) | Enable the server-sync feature in the autin client | 
| [ai](https://packages.gentoo.org/useflags/ai) | Enable the autin AI bash helper | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [server](https://packages.gentoo.org/useflags/server) | Enable the autin server | 
| [system-sqlite](https://packages.gentoo.org/useflags/system-sqlite) | Use the system SQLite instead of the bundled one. WARNING: enabling this has a negative performance impact (https://bugs.gentoo.org/959120) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask app-shells/atuin`
If using Bash, note that either [ble.sh](https://github.com/akinomyoga/ble.sh) or [bash-preexec](https://github.com/rcaloras/bash-preexec) is required for capturing commands. See the [Atuin docs](https://docs.atuin.sh/cli/guide/installation/#installing-the-shell-plugin) for details.

## Configuration

It is possible to customize Atuin behavior, such as configuring filtering or search modes.

### Files

- \~/.config/atuin/config.toml - Local (per user) configuration file.

### Service

Atuin package comes with the background daemon functionality, which helps with lowering latency of database writes, supported by default. The daemon can be started manually via atuin daemon.

#### config

To make atuin aware of the daemon, add following lines to the config file:

**`~/.config/atuin/config.toml`**

```
[daemon]
enabled = true
systemd_socket = true
```
#### systemd

To enable the daemon functionality run:

`user $``systemctl enable --now --user atuin-daemon.socket`
## Usage

### Invocation

Simply hit `Ctrlr` to start browsing shell history.

## Troubleshooting

There is the atuin doctor utility dedicated to perform self-diagnosis of Atuin and its environment.
