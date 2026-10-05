<!-- source: https://wiki.gentoo.org/wiki/Tmux | group: Gentoo Wiki (Main) | wiki-title: Tmux -->
---
title: tmux
url: https://wiki.gentoo.org/wiki/Tmux
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-17"
fingerprint: f4789a508f85adbd
license: CC BY-SA 4.0
---

# tmux

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


tmux (**t**erminal **mu**ltiple**x**er) is a program that enables a number of terminals (or windows), each running a separate program, to be created, accessed, and controlled from a single screen or terminal window. tmux may be detached from a screen and continue running in the background, then later reattached.[\[1\]](https://wiki.gentoo.org#cite_note-1)

Users familiar with [GNU Screen](https://wiki.gentoo.org/wiki/GNU_Screen) may find tmux as a suitable alternative.

## Installation

### USE flags


| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [jemalloc](https://packages.gentoo.org/useflags/jemalloc) | Use dev-libs/jemalloc for memory management | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [sixel](https://packages.gentoo.org/useflags/sixel) | Enable sixel support | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [utempter](https://packages.gentoo.org/useflags/utempter) | Include libutempter support | 
| [vim-syntax](https://packages.gentoo.org/useflags/vim-syntax) | Pulls in related vim syntax scripts | 

### Emerge

Install [app-misc/tmux](https://packages.gentoo.org/packages/app-misc/tmux):

`root #``emerge --ask app-misc/tmux`
## Configuration

Standard installation of tmux will not install a global (/etc/tmux.conf) or user (\~/.tmux.conf) configuration file. Examples are provided in the /usr/share/doc/tmux-`VERSION`/example\_tmux.conf file and can be copied to a user and system locations.

### Configuration Files

- /etc/tmux.conf — the system-wide configuration file.
- $XDG\_CONFIG\_HOME/tmux/tmux.conf — The user specific tmux configuration file.
- \~/.tmux.conf — The legacy single-user tmux configuration file location.

### Example config

**`~/.config/tmux/tmux.conf /etc/tmux.conf`**

To reload the configuration file from a terminal run:

`user $``tmux source-file ~/.tmux.conf`
Alternatively, modifications to the file can be loaded from within tmux via:

`:``source-file ~/.tmux.conf`
### Automatic connection

Running tmux with exec and the `-ADX` options will cause tmux to replace the current shell and create a session, or if one exists connect to it and both detach and exit the other client. This provides a very consistent way of working with remote sessions, and it can be run automatically:

Bash

`user $``test -n "$PS1" && test -z "$TMUX" -a -n "$SSH_TTY" && exec tmux new -ADX`
Fish

`user $``status is-interactive ; and test -z "$TMUX" -a -n "$SSH_TTY" ; and exec tmux new -ADX`
### Plugins

Tmux is extensible through the use of plugins. Please see the [Tmux Plugins subarticle](https://wiki.gentoo.org/wiki/Tmux/plugins) for more information, as well as for specific information regarding some more notable plugins.

## Usage

### Key bindings

tmux can be controlled from an attached client by using a key combination of a *prefix key stroke* (`Ctrl`+`b` by default) followed by a *command* key.

After pressing `Ctrl`+`b` the following key combinations can be used:

#### General

- `?` = List all key bindings.
- `d` = Detach the current client.
- `:` = Enter the tmux command prompt.

#### Creating and managing windows

- `c` = Create a new window
- `n` = Change to the next window.
- `p` = Change to the previous window.
- `l` = Move to the previously selected window.
- `0-9` = Select windows 0 to 9.
- `'` = Prompt for a window index to select. Then enter a number or title to switch to that window.
- `,` = Rename the current window.
- `w` = Choose the current window interactively.
- `:`, then type `list-windows enter` = Display the list of windows.

#### Creating and managing panes

- `"` = Split the current pane into two, top and bottom.
- `%` = Split the current pane into two, left and right.
- `o` = Select the next pane in the current window.
- `;` = Move to the previously active pane.
- `{` = Swap the current pane with the previous pane.
- `}` = Swap the current pane with the next pane.
- `Ctrl`+`o` = Rotate the panes in the current window forwards.
- `Alt`+`1` to `Alt`+`5` = Arrange panes in one of the five preset layouts: even-horizontal, even-vertical, main-horizontal, main-vertical, or tiled.
- `x` = Kill the current pane.
- `!` = Break the current pane out of the window.

#### Copy, paste, and scroll operations

The keys available depend on whether emacs (default) or vi mode is selected. The mode-keys option can be set in .tmux.conf for vi mode.

- `[` = Enter copy mode to copy text or view output history via the scrollback buffer. Once in copy mode, pressing `j` or `k` will move the cursor down or up lines respectively, while `{` and `}` will move down or up on in per-paragraph chunks.
- `]` = Paste the most recently copied buffer of text.
- `#` = List all paste buffers.
- `-` = Delete the most recently copied buffer of text.

### Session control

#### Start session

Once started tmux creates a socket for the session in /tmp/S-\<UID>/\<Session Name>

tmux can be started with the following command:

`user $``tmux`
Or, to give the session a name on start up, run:

`user $``tmux new-session -s portage`
#### Listing sessions

List tmux sessions to see existing session information:

`user $``tmux ls`
0: 1 windows (created Thu Apr  9 09:09:03 2015) \[180x65\] (attached)

When listing sessions the name of the session should appear as the first item in the session information line. It is possible to see from the output above the session was created without a name, hence the session is to be referenced as `0`.

Another way to list sessions is by typing out the long `list-sessions` argument.

`user $``tmux list-sessions`
0: 1 windows (created Thu Apr  9 09:09:03 2015) \[180x65\] (attached)

The exact same output as the previous list command is displayed.

#### Renaming a session

Simply using tmux to start a session will not provide the session with a nice, human readable name.

If the default session name is not descriptive enough (`0` does not tend to describe much), then a session can be renamed. Suppose Larry the cow started tmux without specifying a session name on start up. He begins working on compiling a new version of Portage, and wants to change the session name to reflect his current task. To change the session name he would first assume control of tmux by pressing the magic key stroke: `Ctrl`+`b`, then `:` which will drop focus into the tmux control line. By default the line should turn yellow. Once there he would issue:

`:``rename-session -t 0 portage`
Where `0` is the existing (default) session name and `portage` is the desired new name for the session. To rename when detached from a tmux session issue:

`user $``tmux rename-session -t 0 portage`
#### Resuming a session

After the session is detached, all the active terminals remain active and so do commands that did not finish yet. To resume a session use `attach -t <session_name>`.

`user $``tmux a -t portage`
Or use the long way of attaching to an existing session:

`user $``tmux attach -t portage`
#### Daemon-like operation

To start a command in a tmux session *without* attaching to the session (like a daemon) use `new-session -d` followed by the command to execute in quotes:

`user $``tmux new-session -d 'emerge -uDNvp @world'`
## See also

- [Screen](https://wiki.gentoo.org/wiki/Screen) — a program that enables the creation of multiple sessions and virtual terminals within a single terminal.
- [Other terminal multiplexers](https://wiki.gentoo.org/wiki/Recommended_tools#Terminal_multiplexers)

## External resources

- [Making tmux Pretty and Usable](https://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/) — A Guide to Customizing your `tmux.conf`.
- [Tmux Cheat Sheet & Quick Reference](https://tmuxcheatsheet.com/).
- [An excellent tmux primer](https://danielmiessler.com/study/tmux/).
- [The Tao of tmux](https://leanpub.com/the-tao-of-tmux/read), eBook by Tony Narlock, creator of [libtmux](https://libtmux.git-pull.com) and [tmuxp](https://tmuxp.git-pull.com).
- [Oh my tmux!](https://github.com/gpakosz/.tmux) To make things easier.
