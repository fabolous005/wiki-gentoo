<!-- source: https://wiki.gentoo.org/wiki/Foot | group: Gentoo Wiki (Main) | wiki-title: Foot -->
---
title: foot
url: https://wiki.gentoo.org/wiki/Foot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-04"
fingerprint: "54c255491b1669cb"
license: CC BY-SA 4.0
---

# foot

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**Foot** is a minimalist terminal emulator for Wayland written in C.

## Installation

### USE flags


| [+grapheme-clustering](https://packages.gentoo.org/useflags/+grapheme-clustering) | Enable grapheme clustering support | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [utempter](https://packages.gentoo.org/useflags/utempter) | Enable utmp support via sys-libs/libutempter | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

`root #``emerge --ask gui-apps/foot`
## Configuration

Foot's user configuration is found at \~/.config/foot/foot.ini. The file and directory won't be created by default. A template can be found in /etc/xdg/foot/foot.ini or [from the upstream repository](https://codeberg.org/dnkl/foot/src/branch/master/foot.ini).

To create the directory for the foot config and copy the example:

`user $``mkdir ~/.config/foot``user $``cp /etc/xdg/foot/foot.ini .config/foot/foot.ini`
### Font/text configuration

Foot has several font configuration options, the `font` parameter can be used to set the terminal font. `font-bold`, `font-italic`, and `font-bold-italic` will automatically try to use variants of the primary font, but can be overridden. `letter-spacing`, `horizontal-letter-offset`, `vertical-letter-offset`, `underline-offset`, `box-drawings-uses-font-glyphs`, and `dpi-aware` options are also available.

**`~/.config/foot/foot.ini`**

**Set the font to 8pt Inconsolata**

```
font=Inconsolata:size=8
```
### Server mode configuration

Foot supports the operation of a daemon which can be used to reduce overhead. The server handles all wayland communication, VT parsing, and rendering. Additionally, fonts are cached in a single thread, reducing overall memory usage.

**`~/.config/foot/foot.ini`**

**Increase the number of worker threads to use for rendering**

Foot can be started as a server by running foot -s, this can be added to a window manager startup script to enable it upon login:

**`~/.config/sway/config`**

**Start Foot server with Sway if foot-client is the chosen terminal**

Once the server has been started, clients can be started with footclient.

### Scrollback configuration

The number of lines saved in the scrollback can be adjusted with:

**`~/.config/foot/config`**

**Increase the scrollback length to 16384**

**`~/.config/foot/config`**

**Show scrollback positions as a percentage**

### Color configuration

Colors can be specified with the `foreground`, `background`, `regular{0-7}`, and `bright{0-7}` parameters.

The order is `Black`, `Red`, `Green`, `Yellow`, `Blue`, `Magenta`, `Cyan`, `White`.

**`~/.config/foot/foot.ini`**

**Dark+**

```
[colors]
foreground=cccccc
background=1e1e1e
regular0=000000 # black
regular1=cd3131 # red
regular2=0dbc79 # green
regular3=e5e510 # yellow
regular4=2472c8 # blue
regular5=bc3fbc # magenta
regular6=11a8cd # cyan
regular7=e5e5e5 # white
bright0=666666 # bright black
bright1=f14c4c # bright red
bright2=23d18b # bright green
bright3=f5f543 # bright yellow
bright4=3b8eea # bright blue
bright5=d670d6 # bright magenta
bright6=29b8db # bright cyan
bright7=e5e5e5 # bright white
```
### Example Configuration

#### Minimal Configuration

**`~/.config/foot/foot.ini`**

**Foot minimal configuration**

```
font=Source Code Pro:size=10
initial-window-size-chars=190x60
```
This minimal configuration sets the font to *Source Code Pro* with a size of 10. The window will open with 60, 190 character long, lines. All configuration options can be found on foot's source repository.[\[2\]](https://wiki.gentoo.org#cite_note-2)

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose gui-apps/foot`
## Troubleshooting

### Terminal break after SSHing

If a SSH connection is made to a destination that does not have the foot *terminfo* files [\[1\]](https://codeberg.org/dnkl/foot/wiki#user-content-things-break-after-i-ssh-into-a-remote-machine).  This can be corrected in more than one way.

#### SSH with TERM set

This can be done multiple ways, by running export TERM=xterm-256color once logged in, or starting the SSH session like TERM=xterm-256color ssh

`user $``TERM=xterm-256color ssh larry@server`
#### Install terminfo files on destination

This is very distro-dependent, but this info should be part of the [sys-libs/ncurses](https://packages.gentoo.org/packages/sys-libs/ncurses):

`root #``emerge --ask sys-libs/ncurses`
## See also

- [List of software for Wayland](https://wiki.gentoo.org/wiki/List_of_software_for_Wayland) — various desktop related packages for Wayland
- [Terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) — emulates a video terminal within another display architecture (e.g. in [X](https://wiki.gentoo.org/wiki/X_server)).
- [Wayland](https://wiki.gentoo.org/wiki/Wayland) — a [communication protocol](https://en.wikipedia.org/wiki/communication_protocol) between a [display server](https://en.wikipedia.org/wiki/display_server) and its clients
