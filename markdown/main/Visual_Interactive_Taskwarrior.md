<!-- source: https://wiki.gentoo.org/wiki/Visual_Interactive_Taskwarrior | group: Gentoo Wiki (Main) | wiki-title: Visual Interactive Taskwarrior -->
---
title: Visual Interactive Taskwarrior
url: https://wiki.gentoo.org/wiki/Visual_Interactive_Taskwarrior
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-09-06"
fingerprint: bedd9ffc85a772e6
license: CC BY-SA 4.0
---

# Visual Interactive Taskwarrior

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Visual Interactive Taskwarrior** (**vit**) is an curses-based front-end for [Taskwarrior](https://wiki.gentoo.org/wiki/Taskwarrior) with [vim](https://wiki.gentoo.org/wiki/Vim)-like keybindings. Like vim, vit has extensive internal online help documentation. While intended to be intuitive to vim users vit is highly configurable and alternative key bindings entirely possible.

## Installation

### USE flags


### Emerge

`root #``emerge --ask app-misc/vit`
### Additional software

As the name *Visual Interactive Taskwarrior* suggests, Taskwarrior (task) is required for vit to function.

## Configuration

### Environment variables

- `$VIT_DIR` - specifies the location of the vit configuration directory.

### Files

The `$VIT_DIR` can force a configuration directory location to a location other than the default or an XDG directory.

- \~/.vit/config.ini - the default location for the vit configuration file directory.
- \<any valid xdg dir>/vit/config.ini - the XDG compliant configuration file location.

The default config.ini is heavily commented and intended to be studied by the end user for maximum customization.

## Usage

### Invocation

`user $``vit --help`
vit --help
usage: vit \[options\] \[report\] \[filters\]
VIT (Visual Interactive Taskwarrior)
options:
  -h, --help      show this help message and exit
  -v, --version   show program's version number and exit
  --list-actions  list all available actions
  --list-pids     list all pids found in pid\_dir, if configured
VIT (Visual Interactive Taskwarrior) is a lightweight, curses-based front end for
Taskwarrior that provides a convenient way to quickly navigate and process tasks.
VIT allows you to interact with tasks in a Vi-intuitive way.
A goal of VIT is to allow you to customize the way in which you use Taskwarrior's
core commands as well as to provide a framework for easily dispatching external
commands.
While VIT is running, type :help followed by enter to review basic command/navigation actions.
See https://github.com/vit-project/vit for more information.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-misc/vit`
## See also

- [Taskwarrior](https://wiki.gentoo.org/wiki/Taskwarrior) — to-do list manager for the command line
- [Timewarrior](https://wiki.gentoo.org/wiki/Timewarrior) — a time management tool for the terminal.
