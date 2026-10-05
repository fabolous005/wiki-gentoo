<!-- source: https://wiki.gentoo.org/wiki/Terminator | group: Gentoo Wiki (Main) | wiki-title: Terminator -->
---
title: Terminator
url: https://wiki.gentoo.org/wiki/Terminator
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-11-20"
fingerprint: df0e787a0091ab0d
license: CC BY-SA 4.0
---

# Terminator

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Terminator** (aka GNOME Terminator) is a python-based [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) allowing multiple terminals in the same window.

## Installation

### USE flags


### Emerge

`root #``emerge --ask x11-terms/terminator`
## Usage

### Invocation

Terminator will generally be launched from an on-screen menu, or keyboard shortcut, in a user's graphical environment.

Terminator may, if needed, be launched from a shell under X11.

`user $``terminator --help````
Usage: terminator [options]
Options:
  -h, --help            show this help message and exit
  -v, --version         Display program version
  -m, --maximise        Maximise the window
  -M, --maximize        Maximise the window
  -f, --fullscreen      Make the window fill the screen
  -b, --borderless      Disable window borders
  -H, --hidden          Hide the window at startup
  -T FORCEDTITLE, --title=FORCEDTITLE
                        Specify a title for the window
  --geometry=GEOMETRY   Set the preferred size and position of the window(see
                        X man page)
  -e COMMAND, --command=COMMAND
                        Specify a command to execute inside the terminal
  -g CONFIG, --config=CONFIG
                        Specify a config file
  -j CONFIGJSON, --config-json=CONFIGJSON
                        Specify a partial config json file
  -x, --execute         Use the rest of the command line as a command to
                        execute inside the terminal, and its arguments
  --working-directory=DIR
                        Set the working directory
  -i FORCEDICON, --icon=FORCEDICON
                        Set a custom icon for the window (by file or name)
  -r ROLE, --role=ROLE  Set a custom WM_WINDOW_ROLE property on the window
  -l LAYOUT, --layout=LAYOUT
                        Launch with the given layout
  -s, --select-layout   Select a layout from a list
  -p PROFILE, --profile=PROFILE
                        Use a different profile as the default
  -u, --no-dbus         Disable DBus
  -d, --debug           Enable debugging information (twice for debug server)
  --debug-classes=DEBUG_CLASSES
                        Comma separated list of classes to limit debugging to
  --debug-methods=DEBUG_METHODS
                        Comma separated list of methods to limit debugging to
  --new-tab             If Terminator is already running, just open a new tab
  --unhide              If Terminator is already running, just unhide all
                        hidden windows
```
