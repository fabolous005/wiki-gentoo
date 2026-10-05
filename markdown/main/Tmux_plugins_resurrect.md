<!-- source: https://wiki.gentoo.org/wiki/Tmux/plugins/resurrect | group: Gentoo Wiki (Main) | wiki-title: Tmux/plugins/resurrect -->
---
title: Tmux/plugins/resurrect
url: https://wiki.gentoo.org/wiki/Tmux/plugins/resurrect
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-08"
fingerprint: f41d99580c87bfd9
license: CC BY-SA 4.0
---

# Tmux/plugins/resurrect

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

tmux-resurrect is a plugin that restore [tmux](https://wiki.gentoo.org/wiki/Tmux) environment after system restart.

## Installation

### Installation with [TPM](https://wiki.gentoo.org/wiki/Tmux/plugins/tpm)

1\. Add plugin to the list of TPM plugins in \~/.tmux.conf:

**`~/.tmux.conf`**

```
set -g @plugin 'tmux-plugins/tmux-resurrect'
```
2\. Hit `prefix` + `I` to Install.

### Manual installation

1\. Clone the repo:

`user $````
mkdir -p ~/.tmux/plugins
```
2\. Add the line to the bottom of .tmux.conf:

`user $````
run-shell ~/clone/path/resurrect.tmux
```
3\. Reload TMUX environment.

`user $````
tmux source-file ~/.tmux.conf
```
## Configuration

### Default configuration

Only a conservative list of programs is restored by default:

### Self configuration

**`~/.tmux.conf`**

**Configuration**

```
# Example restoring additional programs:
set -g @resurrect-processes 'ssh psql mysql sqlite3'
# Programs with arguments should be double quoted:
set -g @resurrect-processes 'some_program "git log"'
# Start with tilde to restore a program whose process contains target name:
set -g @resurrect-processes 'irb pry "~rails server" "~rails console"'
# Use -> to specify a command to be used when restoring a program (useful if the default restore command fails ):
set -g @resurrect-processes 'some_program "grunt->grunt development"'
# Don't restore any programs:
set -g @resurrect-processes 'false'
# Restore all programs (be careful with this!):
set -g @resurrect-processes ':all:'
```
## Usage

Default key binings:

- `prefix` + `Ctrl-s`: save.
- `prefix` + `Ctrl-r`: restore.
