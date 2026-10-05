<!-- source: https://wiki.gentoo.org/wiki/Zellij | group: Gentoo Wiki (Main) | wiki-title: Zellij -->
---
title: Zellij
url: https://wiki.gentoo.org/wiki/Zellij
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-12"
fingerprint: "76b493e18d856fbb"
license: CC BY-SA 4.0
---

# Zellij

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Zellij** is a feature-rich terminal multiplexer and text-based window manager written in Rust.

## Installation

### USE flags


| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [man](https://packages.gentoo.org/useflags/man) | Build and install man pages | 
| [system-sqlite](https://packages.gentoo.org/useflags/system-sqlite) | Use the system-wide dev-db/sqlite instead of the bundled one | 

### Emerge

`root #``emerge --ask app-misc/zellij`
## Configuration

The default configuration can be created by issuing:

`user $````
mkdir ~/.config/zellij
```
`user $````
zellij setup --dump-config > ~/.config/zellij
```
## Core concepts: Session, attach and detach

A reader who is already familiar with these concepts can skip this section, being identical to [GNU Screen](https://wiki.gentoo.org/wiki/GNU_Screen) and [tmux](https://wiki.gentoo.org/wiki/Tmux).

A **session** is roughly speaking the entirety of the panes, tabs and the programms running inside of zellij. One can **detach** a session, sending the current session into background and returning the control of the terminal. The opposite is called **attach**.

A user runs zellij in a terminal (i.e. in a terminal emulator like xterm or a virtual terminal). Call it simply *the terminal*. Notice programs inside zellij don't access the terminal directly; its stdin, stdout and stderr are the ones created by zellij.

When a user detaches a zellij session, zellij gives back the control of the terminal to the original process, like bare shell. Still the session is "run in background"—the zellij server remembers the session information, and retains the standard streams of the programs running inside the session.

When a user attaches to one of sessions detached beforehand, the terminal begins to be controlled by zellij, and the previous session is restored, i.e. the panes and the tabs come back. Of course the new terminal can be different from the previous one.

## Usage

#### General

- `Ctrl`+`p` = Enters Pane mode (to create and manage panes).
- `Ctrl`+`t` = Enters Tab mode (to create and manage tabs).
- `Ctrl`+`s` = Enters Scroll mode (to navigate the scrollback buffer).
- `Ctrl`+`o` = Enters Session mode (to detach from the current session).
- `Ctrl`+`q` = Quits Zellij and all its sessions.
- `Alt`+`n` = Creates a new pane (without needing to enter Pane mode).
- `Alt`+`arrows` = Moves focus between panes.

#### Managing Tabs

After pressing `Ctrl`+`t`:

- `n` = Creates a new tab.
- `]` or `l` = Goes to the next tab.
- `[` or `h` = Goes to the previous tab.
- `1-9` = Selects tab 1 through 9.
- `r` = Renames the current tab.
- `x` = Closes the current tab.
- `s` = Synchronizes all panes in the current tab (input in one pane is replicated to all others).

#### Managing Panes

After pressing `Ctrl`+`p`:

- `d` = Creates a new pane below the current one.
- `n` = Creates a new pane to the right of the current one.
- `arrows` or `h`, `j`, `k`, `l` = Selects the next pane in the specified direction.
- `x` = Closes the current pane.
- `f` = Toggles the current pane to fullscreen mode.
- `w` = Creates a new pane in floating mode.
- `c` = Renames the current pane.
- `p` = Change focus.

#### Scroll and Copy Mode

After pressing `Ctrl`+`s`:

- `k` or `Up Arrow` = Scrolls one line up.
- `j` or `Down Arrow` = Scrolls one line down.
- `e` = Edits the scrollback buffer in the system's default text editor (defined by the $EDITOR environment variable).

### Session control

#### Start session

To start a new Zellij session with a default name, run:

`user $``zellij`
To give the session a specific name on startup, use the --session argument:

`user $``zellij --session portage`
#### Attach/Detach

It is possible to detach from a Zellij session, leaving it running in the background. To do this, enter Session mode with `Ctrl`+`o` and press `d`.
To view all running sessions, use the command:

`user $``zellij list-sessions`
To re-attach to a running session, use the attach command (or its shorthand a):

`user $``zellij attach portage`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-misc/zellij`
## See also

- [Tmux](https://wiki.gentoo.org/wiki/Tmux) — a program that enables a number of terminals (or windows), each running a separate program, to be created, accessed, and controlled from a single screen or terminal window.
- [Other terminal multiplexers](https://wiki.gentoo.org/wiki/Recommended_tools#Terminal_multiplexers)
