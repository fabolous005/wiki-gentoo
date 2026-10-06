<!-- source: https://wiki.gentoo.org/wiki/Ghostty | group: Gentoo Wiki (Main) | wiki-title: Ghostty -->
---
title: Ghostty
url: https://wiki.gentoo.org/wiki/Ghostty
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-05-31"
fingerprint: "2dc5f45b8ab538dd"
license: CC BY-SA 4.0
---

# Ghostty

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Ghostty** is a [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) available for Linux and macOS that aims for high performance and a wide feature-set.

Ghostty was first publicly released in December 2024, as *version 1.0.0*.

## Installation

### USE flags


### USE flags for
            [x11-terms/ghostty](https://packages.gentoo.org/packages/x11-terms/ghostty)
            
            Fast, feature-rich, and cross-platform terminal emulator

### Emerge

Install [x11-terms/ghostty](https://packages.gentoo.org/packages/x11-terms/ghostty):

`root #``emerge --ask x11-terms/ghostty`
## Configuration

### Files

- $XDG\_CONFIG\_HOME/ghostty/config - main configuration file
- $HOME/.config/ghostty/config - main configuration file if the `XDG_CONFIG_HOME` [environment variable](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/EnvVar) does not exist

### Choose themes

Ghostty has [built-in themes](https://github.com/mbadolato/iTerm2-Color-Schemes/tree/master/ghostty) that can be previewed using the +list-themes action:

`user $``ghostty +list-themes`
Set the chosen theme by editing the Ghostty config file:

**`$XDG_CONFIG_HOME/ghostty/config`**

```
theme = tokyonight
```
#### Custom themes

Ghostty also supports custom themes. Themes must adhere to [Ghostty's format](https://ghostty.org/docs/features/theme#authoring-a-custom-theme) and be placed in the $XDG\_CONFIG\_HOME/ghostty/themes/ directory.

Here is an example theme converted from [Manjaro konsole Breath](https://gitlab.manjaro.org/artwork/themes/breath/-/blob/master/konsole/Breath.colorscheme):

**`$XDG_CONFIG_HOME/ghostty/themes/Breath`**

```
palette = 0=#1e2229 
palette = 1=#ed1515
palette = 2=#448539
palette = 3=#f67400
palette = 4=#1d99f3
palette = 5=#9b59b6
palette = 6=#1abc9c
palette = 7=#fcfcfc
palette = 8=#7f8c8d
palette = 9=#c0392b
palette = 10=#55a649
palette = 11=#fdbc4b
palette = 12=#3daee9
palette = 13=#8e44ad
palette = 14=#16a085
palette = 15=#ffffff
background = #1e2229
foreground = #17a88b
cursor-color = #17a88b
selection-background = #17a88b
selection-foreground = #1e2229
```
Once a custom theme file is added, edit the Ghostty config to use it:

**`$XDG_CONFIG_HOME/ghostty/config`**

```
theme = Breath
```
## Usage

### Invocation

For a listing of invocation options:

`user $``ghostty --help`
Usage: ghostty \[+action\] \[options\]
Run the Ghostty terminal emulator or a specific helper action.
If no \`+action\` is specified, run the Ghostty terminal emulator.
All configuration keys are available as command line options.
To specify a configuration key, use the \`--\<key>=\<value>\` syntax
where key and value are the same format you'd put into a configuration
file. For example, \`--font-size=12\` or \`--font-family="Fira Code"\`.
To see a list of all available configuration options, please see
the \`src/config/Config.zig\` file. A future update will allow seeing
the list of configuration options from the command line.
A special command line argument \`-e \<command>\` can be used to run
the specific command inside the terminal emulator. For example,
\`ghostty -e top\` will run the \`top\` command inside the terminal.
On macOS, launching the terminal emulator from the CLI is not
supported and only actions are supported.
Available actions:
  +version
  +help
  +list-fonts
  +list-keybinds
  +list-themes
  +list-colors
  +list-actions
  +show-config
  +validate-config
  +crash-report
  +show-face
Specify \`+\<action> --help\` to see the help for a specific action,
where \`\<action>\` is one of actions listed below.)

### Querying current setup

#### Show config

Pass +show-config to Ghostty to display the current configuration:

`user $``ghostty +show-config`
font-family = Cascadia Code NF
font-family-bold = Cascadia Code NF
font-family-italic = Cascadia Code NF
font-family-bold-italic = Cascadia Code NF
font-size = 11
command = /bin/zsh
click-repeat-interval = 500
auto-update-channel = stable

#### List keybinds

Pass +list-keybinds to Ghostty to display the current keybinds:

`user $``ghostty +list-keybinds`
super + ctrl  + shift + up             resize\_split:up,10
super + ctrl  + shift + equal          equalize\_splits
super + ctrl  + shift + left           resize\_split:left,10
super + ctrl  + shift + down           resize\_split:down,10
super + ctrl  + shift + right          resize\_split:right,10
ctrl  + alt   + shift + j              write\_scrollback\_file:open
super + ctrl  + right\_bracket          goto\_split:next
super + ctrl  + left\_bracket           goto\_split:previous
ctrl  + alt   + up                     goto\_split:top
ctrl  + alt   + left                   goto\_split:left
ctrl  + alt   + down                   goto\_split:bottom
ctrl  + alt   + right                  goto\_split:right
ctrl  + shift + v                      paste\_from\_clipboard
ctrl  + shift + a                      select\_all
ctrl  + shift + o                      new\_split:right
ctrl  + shift + c                      copy\_to\_clipboard
ctrl  + shift + q                      quit
ctrl  + shift + n                      new\_window
ctrl  + shift + page\_down              jump\_to\_prompt:1
ctrl  + shift + comma                  reload\_config
ctrl  + shift + left                   previous\_tab
ctrl  + shift + w                      close\_surface
ctrl  + shift + j                      write\_scrollback\_file:paste
ctrl  + shift + right                  next\_tab
ctrl  + shift + page\_up                jump\_to\_prompt:-1
ctrl  + shift + t                      new\_tab
ctrl  + shift + tab                    previous\_tab
ctrl  + shift + e                      new\_split:down
ctrl  + shift + enter                  toggle\_split\_zoom
ctrl  + shift + i                      inspector:toggle
alt   + five                           goto\_tab:5
alt   + eight                          goto\_tab:8
alt   + three                          goto\_tab:3
alt   + nine                           goto\_tab:9
alt   + two                            goto\_tab:2
alt   + four                           goto\_tab:4
alt   + f4                             close\_window
alt   + one                            goto\_tab:1
alt   + six                            goto\_tab:6
alt   + seven                          goto\_tab:7
ctrl  + comma                          open\_config
ctrl  + page\_down                      next\_tab
ctrl  + equal                          increase\_font\_size:1
ctrl  + minus                          decrease\_font\_size:1
ctrl  + zero                           reset\_font\_size
ctrl  + enter                          toggle\_fullscreen
ctrl  + page\_up                        previous\_tab
ctrl  + tab                            next\_tab
ctrl  + plus                           increase\_font\_size:1
shift + insert                         paste\_from\_selection
shift + up                             adjust\_selection:up
shift + left                           adjust\_selection:left
shift + page\_up                        scroll\_page\_up
shift + end                            scroll\_to\_bottom
shift + right                          adjust\_selection:right
shift + page\_down                      scroll\_page\_down
shift + down                           adjust\_selection:down
shift + home                           scroll\_to\_top

## Troubleshooting

### Title appears abrupt in system's window manager

If GTK's title appears abrupt in system's window manager, turn off the [adwaita](https://packages.gentoo.org/useflags/adwaita) [USE flag firstly:](https://wiki.gentoo.org/wiki/USE_flag)

**`/etc/portage/package.use`**

```
x11-terms/ghostty -adwaita
```
Then disable the GTK title:

**`$XDG_CONFIG_HOME/ghostty/config`**

```
gtk-titlebar = false
```
## See also

- [Terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) — emulates a video terminal within another display architecture (e.g. in [X](https://wiki.gentoo.org/wiki/X_server)).
