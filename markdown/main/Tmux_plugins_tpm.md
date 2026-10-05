<!-- source: https://wiki.gentoo.org/wiki/Tmux/plugins/tpm | group: Gentoo Wiki (Main) | wiki-title: Tmux/plugins/tpm -->
---
title: Tmux/plugins/tpm
url: https://wiki.gentoo.org/wiki/Tmux/plugins/tpm
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-08"
fingerprint: b2f0921e0c76bf12
license: CC BY-SA 4.0
---

# Tmux/plugins/tpm

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

TPM (**T**mux **P**lugin **M**anager) manages tmux plugins in an automated manner. It is used to install and load [tmux](https://wiki.gentoo.org/wiki/Tmux) plugins.

## Installation

1\. Clone TPM:

`user $````
mkdir -p ~/.tmux/plugins
```
`user $````
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```
2\. At the bottom of \~/.tmux.conf place the following (or $XDG\_CONFIG\_HOME/tmux/tmux.conf works too.):

**`.tmux.conf`**

**Config**

```
# List of plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
# Other examples:
# set -g @plugin 'github_username/plugin_name'
# set -g @plugin 'git@github.com/user/plugin'
# set -g @plugin 'git@bitbucket.com/user/plugin'
# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run -b '~/.tmux/plugins/tpm/tpm'
```
3\. Reload TMUX environment so TPM is sourced (if tmux is already running.):

`user $``tmux source ~/.tmux.conf`
## Usage

### Install plugins

1. Add new plugin to \~/.tmux.conf with `set -g @plugin '...'`
2. Press `prefix` + `I` (capital I, as in **i**nstall) to fetch the plugin.

The plugin will cloned to \~/.tmux/plugins/ and sourced.

### Uninstall plugins

1. Remove (or comment out) plugins from the list.
2. Press `prefix` + `Alt` + `u` (lowercase U as in **u**ninstall) to remove the plugin.

All the plugins are installed to \~/.tmux/plugins/ so alternatively you can find plugin directory there and remove it.

### Key bindings

- `prefix` + `I`: Install new plugins from GitHub or any other git repository. And Refreshes TMUX environment.
- `prefix` + `U`: Update plugins.
- `prefix` + `Alt` + `u`: Remove/uninstall plugins not on the plugin list.

### More plugins

For more plugins, check [tmux-plugins](https://github.com/tmux-plugins)

## Removal

Remove files:

`user $``rm -r ~/.tmux/plugins/tpm`
Then clear TPM settings in \~/.tmux.conf.
