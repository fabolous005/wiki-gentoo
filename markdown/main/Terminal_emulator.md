<!-- source: https://wiki.gentoo.org/wiki/Terminal_emulator | group: Gentoo Wiki (Main) | wiki-title: Terminal emulator -->
---
title: Terminal emulator
url: https://wiki.gentoo.org/wiki/Terminal_emulator
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-12"
categories: ['x11-terms']
fingerprint: "7cdbdf7d62843b91"
license: CC BY-SA 4.0
---

# Terminal emulator

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A **terminal emulator**, **terminal application**, or **term**, (sometimes also referred to as a **tty**) emulates a video terminal within another display architecture (e.g. in [X](https://wiki.gentoo.org/wiki/X_server)). This usually takes the form of a window in which the user can enter commands and view output, or of a fullscreen **virtual console**.  A terminal emulator will generally start the [shell](https://wiki.gentoo.org/wiki/Shell) that is defined as the login shell for a given user. On Gentoo, the default shell is [bash](https://wiki.gentoo.org/wiki/Bash).

After booting, Gentoo will show either a login prompt on a [virtual console](https://wiki.gentoo.org/wiki/Terminal_emulator#Virtual_consoles_and_switching), by default, or a [display manager](https://wiki.gentoo.org/wiki/Display_manager) if one has been set up. See next section about virtual consoles and how to switch between them.

If an [X environment has been set up](https://wiki.gentoo.org/wiki/Xorg/Guide), there are many terminal emulator options available for the user to choose from - see software section.

For information about colors in terminal emulators, refer to the [Terminal emulator/Colors](https://wiki.gentoo.org/wiki/Terminal_emulator/Colors) page.

A **virtual console** (*VC*), aka **virtual terminal** (*VT*), allows for full-screen text-based interaction, via facilities provided directly by the kernel. Gentoo starts with six virtual consoles by default (this can be configured in [inittab](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Initscripts#Initscripts) or with [openrc-init](https://wiki.gentoo.org/wiki/OpenRC/openrc-init#Start_terminals_as_OpenRC_services)).

From a text virtual console, it is possible to access the other VCs by pressing the `Alt`+`F1` through `Alt`+`F6` keys on the keyboard. The `super` key ("*Windows*" key on some keyboards, other times the *Command* or "*Apple*" key) will toggle consoles.  To switch to the next or previous VC in numerical order, press `Alt`+`←` or `Alt`+`→`.

When using a X11-based environment served by the X.Org server, X11 a can be started in a virtual console from the shell or by using a [display manager](https://wiki.gentoo.org/wiki/Display_manager). When a display manager is used, an X11 session is traditionally shown on virtual console number 7. Keyboard shortcuts are used to switch between VCs, and the chvt command exists if needed.

From an X11 session, the Linux virtual consoles can be accessed with `Ctrl`+`Alt`+`F1` through `Ctrl`+`Alt`+`F6`.

If an X11 session was started from a session manager on virtual console number 7, return to it by pressing `Ctrl`+`Alt`+`F7`, otherwise return to a graphical X11 session by going back to the virtual console on which it was started.

## Available software

Popular terminal emulators include:

| Title | Package | Description | 
|---|---|---|
| [Alacritty](https://wiki.gentoo.org/wiki/Alacritty) | [x11-terms/alacritty](https://packages.gentoo.org/packages/x11-terms/alacritty) | GPU-accelerated terminal emulator. | 
| [Foot](https://wiki.gentoo.org/wiki/Foot) | [gui-apps/foot](https://packages.gentoo.org/packages/gui-apps/foot) | A fast, lightweight and minimalistic Wayland terminal emulator | 
| [Ghostty](https://wiki.gentoo.org/wiki/Ghostty) | [x11-terms/ghostty](https://packages.gentoo.org/packages/x11-terms/ghostty) | A fast and feature-rich terminal emulator with a native user interface. | 
| [Kitty](https://wiki.gentoo.org/wiki/Kitty) | [x11-terms/kitty](https://packages.gentoo.org/packages/x11-terms/kitty) | A modern, hackable, featureful, OpenGL-based terminal emulator written in [Python](https://wiki.gentoo.org/wiki/Python) and C. | 
| [Konsole](https://wiki.gentoo.org/wiki/Konsole) | [kde-apps/konsole](https://packages.gentoo.org/packages/kde-apps/konsole) | The default terminal emulator for [KDE](https://wiki.gentoo.org/wiki/KDE) / Plasma. | 
| [GNOME Terminal](https://help.gnome.org/users/gnome-terminal/stable/) | [x11-terms/gnome-terminal](https://packages.gentoo.org/packages/x11-terms/gnome-terminal) | The default terminal emulator for [GNOME](https://wiki.gentoo.org/wiki/GNOME). | 
| [Guake](https://github.com/Guake/guake) | [x11-terms/guake](https://packages.gentoo.org/packages/x11-terms/guake) | Drop-down terminal emulator for GNOME. | 
| [lxterminal(1)](https://man.archlinux.org/man/lxterminal.1.en) | [lxde-base/lxterminal](https://packages.gentoo.org/packages/lxde-base/lxterminal) | The standard terminal emulator of [LXDE](https://wiki.gentoo.org/wiki/LXDE). | 
| [rxvt-unicode](https://wiki.gentoo.org/wiki/Rxvt-unicode) | [x11-terms/rxvt-unicode](https://packages.gentoo.org/packages/x11-terms/rxvt-unicode) | Light in resource usage, fast, and more feature-rich (tabs, transparency, Unicode, etc.). | 
| [st](https://wiki.gentoo.org/wiki/St) | [x11-terms/st](https://packages.gentoo.org/packages/x11-terms/st) | Simple terminal implementation for X. | 
| [Terminator](https://wiki.gentoo.org/wiki/Terminator) | [x11-terms/terminator](https://packages.gentoo.org/packages/x11-terms/terminator) | A terminal emulator arranging multiple terminals in one window, Python based, for GNOME. | 
| [terminology](https://www.enlightenment.org/about-terminology.md) | [x11-terms/terminology](https://packages.gentoo.org/packages/x11-terms/terminology) | The default terminal emulator for [Enlightenment](https://wiki.gentoo.org/wiki/Enlightenment). | 
| [tilda](https://github.com/lanoxx/tilda) | [x11-terms/tilda](https://packages.gentoo.org/packages/x11-terms/tilda) | A drop down terminal, similar to the consoles found in first person shooters. | 
| [xfce4-terminal](https://docs.xfce.org/apps/terminal/start) | [x11-terms/xfce4-terminal](https://packages.gentoo.org/packages/x11-terms/xfce4-terminal) | The default terminal emulator for the [Xfce](https://wiki.gentoo.org/wiki/Xfce) desktop environment. | 
| [XTerm](https://wiki.gentoo.org/wiki/XTerm) | [x11-terms/xterm](https://packages.gentoo.org/packages/x11-terms/xterm) | The default terminal emulator for X.org. | 
| [Yakuake](https://apps.kde.org/yakuake/) | [kde-apps/yakuake](https://packages.gentoo.org/packages/kde-apps/yakuake) | Quake-style (drop-down) terminal emulator based on Konsole for [KDE](https://wiki.gentoo.org/wiki/KDE) / Plasma. | 
| [WezTerm](https://wezfurlong.org/wezterm/index.html) | [x11-terms/wezterm](https://packages.gentoo.org/packages/x11-terms/wezterm) | A GPU-accelerated cross-platform terminal emulator and multiplexer. | 

Additional terminal emulators can be found in the [x11-terms](https://packages.gentoo.org/categories/x11-terms) category.

## General usage

### Interrupt application

In many applications, pressing `Ctrl`+`c` will abort the application immediately by sending the [SIGINT](<https://en.wikipedia.org/wiki/Signal_(IPC)#SIGINT>) signal. See [Wikipedia article](https://en.wikipedia.org/wiki/Control-C#In_command-line_environments).

### Jobs

In many applications, pressing `Ctrl`+`z` will suspend the process and return to the shell. Then, the bg command can be used to continue the process in the background. This may be useful for example, for running a command in a shell while in text editor. In many shells, the fg command will return to the suspended task, and jobs will list current background jobs.

In many shells, appending an "&" symbol to a command will start the command directly in the background.

See [Wikipedia on Ctrl+z](https://en.wikipedia.org/wiki/Substitute_character#Other_uses), [Wikipedia on job control](<https://en.wikipedia.org/wiki/Job_control_(Unix)#Implementation>), [Bash docs](https://www.gnu.org/software/bash/manual/html_node/Job-Control-Basics.html), [fish docs](https://fishshell.com/docs/current/language.html#syntax-job-control), and
[zsh docs](https://zsh.sourceforge.io/Doc/Release/Jobs-_0026-Signals.html).

## Troubleshooting

Some actions can leave a terminal in a state unadapted to the normal display of text. For example, if binary information is output to the terminal (say with cat, or less), some of the data can be interpreted as control characters and modify the terminal's state. Another example would be a program crashing and leaving the terminal in an abnormal state.

This problem can usually be fixed by typing the reset command.

Some shells can be cleared and redrawn by pressing `Ctrl`+`l` (lower case L). Pressing `Ctrl`+`c` a few times can, in cases, help somewhat. Another option is to use the "stty sane" and "tput rs1" commands.

Sometimes, the visual feedback of typing characters may be affected, even though typing the reset command still works.

## See also

- [Key repeat rate](https://wiki.gentoo.org/wiki/Key_repeat_rate) — controls how quickly a key press on the keyboard will repeat if the key is pressed and held for a period of time.
- [Recommended tools](https://wiki.gentoo.org/wiki/Recommended_tools) — lists system-administration related tools recommended for use in a **[shell](https://wiki.gentoo.org/wiki/Shell) environment** ([terminal/console])
  - [Pagers](https://wiki.gentoo.org/wiki/Recommended_tools) — lists system-administration related tools recommended for use in a **[shell](https://wiki.gentoo.org/wiki/Shell) environment** ([terminal/console])
  - [Shell environment tools](https://wiki.gentoo.org/wiki/Recommended_tools) — lists system-administration related tools recommended for use in a **[shell](https://wiki.gentoo.org/wiki/Shell) environment** ([terminal/console])
  - [Terminal multiplexers](https://wiki.gentoo.org/wiki/Recommended_tools) — lists system-administration related tools recommended for use in a **[shell](https://wiki.gentoo.org/wiki/Shell) environment** ([terminal/console])
- [Shell](https://wiki.gentoo.org/wiki/Shell) — command-line interpreter that provides a text-based interface to users

## External resources

- [https://www.linusakesson.net/programming/tty/index.php](https://www.linusakesson.net/programming/tty/index.php) - Detailed history of the TTY.
