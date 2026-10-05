<!-- source: https://wiki.gentoo.org/wiki/Tmux/plugins/tmux-mem-cpu-load | group: Gentoo Wiki (Main) | wiki-title: Tmux/plugins/tmux-mem-cpu-load -->
---
title: Tmux/plugins/tmux-mem-cpu-load
url: https://wiki.gentoo.org/wiki/Tmux/plugins/tmux-mem-cpu-load
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-06-08"
fingerprint: b69b7053c6b76bf8
license: CC BY-SA 4.0
---

# Tmux/plugins/tmux-mem-cpu-load

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

tmux-mem-cpu-load is a small program designed for system monitoring in the status line of [tmux](https://wiki.gentoo.org/wiki/Tmux).

## Installation

### USE flags

There are currently no USE flags for this program.

### Emerge

For user convenience the package has been included in official Gentoo repositories:

## Configuration

### tmux integration

For proper integration, tmux's configuration file will need to be modified to include references to tmux-mem-cpu-load. If a configuration file does not exist, which is the default for new tmux installs, the main article has information [on how to obtain one](https://wiki.gentoo.org/wiki/Tmux#Package_examples).

Once the configuration file has been set add tmux-mem-cpu-load references. The following example will use a configuration file based on [the Wiki's example](https://wiki.gentoo.org/wiki/Tmux#Wiki_example) that has been copied to a user's home directory using the following command:

`user $``bzcat /usr/share/doc/tmux-2.0/examples/h-boetes.conf.bz2 > ~/.tmux.conf`
**`~/.tmux.conf`**

**Adding tmux-mem-cpu-load references**

## Usage

`user $``tmux-mem-cpu-load -h````
The interval and graph lines options are now specified with flags.
tmux-mem-cpu-load v3.4.0
Usage: tmux-mem-cpu-load [OPTIONS]
Available options:
-h, --help
         Prints this help message
-c, --colors
        Use tmux colors in output
-p, --powerline-left
        Use powerline left symbols throughout the output, enables --colors
-q, --powerline-right
        Use powerline right symbols throughout the output, enables --colors
-i <value>, --interval <value>
        Set tmux status refresh interval in seconds. Default: 1 second
-g <value>, --graph-lines <value>
        Set how many lines should be drawn in a graph. Default: 10
-m <value>, --mem-mode <value>
        Set memory display mode. 0: Default, 1: Free memory, 2: Usage percent.
-t <value>, --cpu-mode <value>
        Set cpu % display mode. 0: Default max 100%, 1: Max 100% * number of threads.
-a <value>, --averages-count <value>
        Set how many load-averages should be drawn. Default: 3
```
tmux-mem-cpu-load can be called independently for stand-alone operation or for use as a system status monitor in other programs. In order for the monitor to persist in the terminal, call it using the watch utility:

`user $``watch tmux-mem-cpu-load`
## See also

- [tmux-resurrect](https://wiki.gentoo.org/wiki/Tmux/plugins/resurrect) - A plugin that persists tmux sessions across system restarts.
