<!-- source: https://wiki.gentoo.org/wiki/WezTerm | group: Gentoo Wiki (Main) | wiki-title: WezTerm -->
---
title: WezTerm
url: https://wiki.gentoo.org/wiki/WezTerm
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-25"
fingerprint: c484197a53b7ebcf
license: CC BY-SA 4.0
---

# WezTerm

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**WezTerm** is a powerful cross-platform terminal emulator and multiplexer implemented in Rust

## Installation

### USE flags


### Emerge

`root #``emerge --ask x11-terms/wezterm`
### Configuration

### Files

WezTerm does not automatically create or install a configuration file, but it will search for one in the following locations:

- $XDG\_CONFIG\_HOME/.config/wezterm/wezterm.lua
- $HOME/.config/wezterm/wezterm.lua
- $XDG\_CONFIG\_HOME/wezterm/wezterm.lua
- $HOME/wezterm/wezterm.lua
- $HOME/.wezterm.lua

The default configuration can be created in a users' home directory with the following commands:

`user $````
mkdir --parents ~/.config/wezterm
```
Paste the following Lua code in wezterm.lua

**`~/.config/wezterm/wezterm.lua`**

**Default config**

```
-- Pull in the wezterm API
local wezterm = require 'wezterm'
-- This will hold the configuration.
local config = wezterm.config_builder()
-- This is where configuration choices are applied.
-- For example, changing the color scheme:
-- config.color_scheme = 'AdventureTime'
-- and finally, return the configuration to wezterm
return config
```
### Font configuration

One can run the following command and copy the desired font name:

`user $``fc-list -f '%{family}\n' | awk '!x[$0]++'`
Changing the default font by editing the config file.

**`~/.config/wezterm/wezterm.lua`**

**Default config**

```
config.font = wezterm.font 'hack'
-- It's possible to specify some parameters to influence the font selection;
-- for example, this selects a Bold, Italic font variant.
config.font =
  wezterm.font('hack', { weight = 'Bold', italic = true })
```
This will change the font to one provided by [media-fonts/hack](https://packages.gentoo.org/packages/media-fonts/hack), given that the package is installed.

### Colors configuration

#### WezTerm's bundled colorschemes

**`~/.config/wezterm/wezterm.lua`**

**Bundled**

```
-- Setting the colorscheme to Catppuccin Mocha
config.color_scheme = 'catppuccin-mocha'
```
#### Defining colors

**`~/.config/wezterm/wezterm.lua`**

**color schemes**

```
config.colors = {
  -- The default text color
  foreground = '#...',
  -- The default background color
  background = '#...',
  -- Overrides the cell background color when the current cell is occupied by the
  -- cursor and the cursor style is set to Block
  cursor_bg = '#...',
  -- Overrides the text color when the current cell is occupied by the cursor
  cursor_fg = '#...',
  -- Specifies the border color of the cursor when the cursor style is set to Block,
  -- or the color of the vertical or horizontal bar when the cursor style is set to
  -- Bar or Underline.
  cursor_border = '#...',
  -- the foreground color of selected text
  selection_fg = '#...',
  -- the background color of selected text
  selection_bg = '#...',
  -- The color of the scrollbar "thumb"; the portion that represents the current viewport
  scrollbar_thumb = '#...',
  -- The color of the split lines between panes
  split = '#...',
  ansi = {
    'black',
    'maroon',
    'green',
    'olive',
    'navy',
    'purple',
    'teal',
    'silver',
  },
  brights = {
    'grey',
    'red',
    'lime',
    'yellow',
    'blue',
    'fuchsia',
    'aqua',
    'white',
  },
```
### Transparent background

**`~/.config/wezterm/wezterm.lua`**

**transparent background configure**

```
# value range is 0 ~ 1
config.window_background_opacity = 1.0
```
### Tabs

WezTerm does support tabs, here are keybinds how to use them:

**New tab**: `Ctrl`+`Shift`+`T`

**Close tab**: `Ctrl`+`Shift`+`W`

**Switch between tabs**ː `Ctrl`+`Shift`+`1`-`9`

### Usage

### Invocation

WezTerm will generally be launched from an on screen menu or keyboard shortcut in a user's graphical environment.

WezTerm may be launched from a shell under X11 or Wayland.

`user $``wezterm --help````
Wez's Terminal Emulator
http://github.com/wez/wezterm
Usage: wezterm [OPTIONS] [COMMAND]
Commands:
  start                  Start the GUI, optionally running an alternative program [aliases: -e]
  ssh                    Establish an ssh session
  serial                 Open a serial port
  connect                Connect to wezterm multiplexer
  ls-fonts               Display information about fonts
  show-keys              Show key assignments
  cli                    Interact with experimental mux server
  imgcat                 Output an image to the terminal
  set-working-directory  Advise the terminal of the current working directory by emitting an OSC 7
                             escape sequence
  record                 Record a terminal session as an asciicast
  replay                 Replay an asciicast terminal session
  shell-completion       Generate shell completion information
  help                   Print this message or the help of the given subcommand(s)
Options:
  -n, --skip-config                Skip loading wezterm.lua
      --config-file <CONFIG_FILE>  Specify the configuration file to use, overrides the normal
                                   configuration file resolution
      --config <name=value>        Override specific configuration values
  -h, --help                       Print help
  -V, --version                    Print version
```
