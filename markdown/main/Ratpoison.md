<!-- source: https://wiki.gentoo.org/wiki/Ratpoison | group: Gentoo Wiki (Main) | wiki-title: Ratpoison -->
---
title: ratpoison
url: https://wiki.gentoo.org/wiki/Ratpoison
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-07-14"
fingerprint: "1e6c1e5d148d59cf"
license: CC BY-SA 4.0
---

# ratpoison

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**ratpoison** is a tiling [window manager](https://wiki.gentoo.org/wiki/Window_manager) modeled after `[screen](https://wiki.gentoo.org/wiki/Screen)`. The main philosophy behind ratpoison is to manage window without using a mouse (what its name reflects). Written in C, it is extremely lightweight and fast.

## Installation

### USE flags


| [+history](https://packages.gentoo.org/useflags/+history) | Use sys-libs/readline for history handling | 
| [+xft](https://packages.gentoo.org/useflags/+xft) | Build with support for XFT font renderer (x11-libs/libXft) | 
| [+xrandr](https://packages.gentoo.org/useflags/+xrandr) | Enable support for XRandR | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [emacs](https://packages.gentoo.org/useflags/emacs) | Add support for GNU Emacs | 
| [sloppy](https://packages.gentoo.org/useflags/sloppy) | Install sloppy, a focus-follows-mouse implementation for ratpoison | 

### Emerge

Install [x11-wm/ratpoison](https://packages.gentoo.org/packages/x11-wm/ratpoison):

`root #``emerge --ask x11-wm/ratpoison`
## Configuration

### Starting

Edit \~/.xinitrc in the user's home directory by adding the following line:

**`~/.xinitrc`**

```
exec /usr/bin/ratpoison
```
### Startup file

Being a simple window manager, ratpoison does not need much out-of-the-box configuration. Customizable settings can be adjusted to each user's needs by editing ratpoison's start up file.

On start up ratpoison runs commands found in the \~/.ratpoisonrc file. This file contains key bindings and programs that need to be run with ratpoison. Here is an example of a \~/.ratpoisonrc file:

**`~/.ratpoisonrc`**

## Usage

Since ratpoison is modeled after `screen`, users accustomed to `screen` will easily manage to use it. Each command begins with a `Ctrl`-`t` (abbreviated C-t from now on), and is followed by one other keystroke. The simplest way to get to know the commands is to press C-t ? This will open a help window containing the most common key bindings.

Most commonly used keys:

| Keystroke | Description | 
|---|---|
| C-t C-c | Execute xterm | 
| C-t ! | Spawn a shell executing shell command, usually an application, such as C-t ! firefox `Enter` | 
| C-t k | Close the current window | 
| C-t b | Banish the rat cursor to the lower right corner of the screen. The next step would be to unplug the rat from the computer altogether. | 
| C-t C-t | Switch to the window that was last accessed but is not currently visible | 
| C-t s | Split the current frame into upper frame and a lower frame. By default, split in halves. | 
| C-t S | Split the current frame into left frame and a right frame. | 
| C-t r | Resize the current frame interactively by pressing `Up` and `Down` keys. Hit `Enter` when finished. | 
| C-t R | Remove the current frame and extend some frames around to fill the remaining gap. | 
| C-t :quit | Run before exiting ratpoison | 
| C-t a | Output current data and time | 
| C-t C-g | Do nothing and that successfully | 

## Tips

By default, ratpoison only has one workspace. Add the following line to the \~/.ratpoisonrc file in order to create six workspaces:

**`~/.ratpoisonrc`**

```
exec /usr/bin/rpws init 6 -k
```
Switch between workspaces with `Alt`+`F1`, `Alt`+`F2`, etc.

## See also

- [Openbox](https://wiki.gentoo.org/wiki/Openbox) — a highly configurable stacking [window manager](https://wiki.gentoo.org/wiki/Window_manager) for [X11](https://wiki.gentoo.org/wiki/X11) with extensive standards support.
- [LXDE](https://wiki.gentoo.org/wiki/LXDE) - The Lightweight X11 Desktop Environment built off Openbox and a smart collection of lightweight applications.
- [Xfce](https://wiki.gentoo.org/wiki/Xfce)
