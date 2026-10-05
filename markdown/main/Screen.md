<!-- source: https://wiki.gentoo.org/wiki/Screen | group: Gentoo Wiki (Main) | wiki-title: Screen -->
---
title: Screen
url: https://wiki.gentoo.org/wiki/Screen
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-08"
fingerprint: d6105b100b87a98f
license: CC BY-SA 4.0
---

# Screen

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**GNU Screen**, commonly referred to as screen, is a program that enables the creation of multiple sessions and virtual terminals within a single terminal. Among other features, it enables the preserved state of sessions, the detaching and reattaching of sessions, copy and paste functionality, and can be used for serial sessions.

## Installation

### USE flags


| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [multiuser](https://packages.gentoo.org/useflags/multiuser) | Enable multiuser support (by setting correct permissions) | 
| [nethack](https://packages.gentoo.org/useflags/nethack) | Express error messages in nethack style | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [utempter](https://packages.gentoo.org/useflags/utempter) | Enable support for sys-libs/libutempter | 

### Emerge

Install [app-misc/screen](https://packages.gentoo.org/packages/app-misc/screen) with the emerge command:

`root #``emerge --ask app-misc/screen`
## Configuration

The configuration files are located in /etc/screenrc and \~/.screenrc.

### Hardstatus customization

Customizing the status line, or hardstatus line in GNU Screen terminology is quite common. This can be done with:

**`~/.screenrc`**

This configuration results in a status line like, without the color:

`user $`
[ localhost ][ 0$ Calendar  1$ HTop  2$ startx  3$ Root  4$ bash  5-$ bash  (6*$bash) ][ Tue Nov 15  3:02 ]

### Enable 256 colors

In order for Screen to use 256 colors, two requisites must be met:

1. The "term screen-256color" line must be specified prior to any 'screen' definitions. Sessions must be restarted for these changes to apply
2. The application and terminal to be compiled with 256 color support.

**`~/.screenrc`**

**Enable 256 color support in screen**

### Increase the scrollback size

The scrollback size can be increased to 10000 lines with:

**`$HOME/.screenrc`**

## Usage

General usage of screen.

### Starting a Session

Screen can be started with the following command:

`user $``screen`
#### Named Session

The **-t** argument can be used to assign a title to the created screen session, the following creates one named *portage*:

`user $``screen -t portage`
#### Detached Sessions

The **-d -m** arguments can be used to start a screen session detached, running it in the background:

`user $``screen -d -m emerge -uDN @world`
This can be used in combination with the **-t** argument to name this session *portage* and start it in detached:

`user $``screen -t portage -d -m emerge -uDN @world`
### Starting a Serial Session

To start a session on the serial interface **/dev/ttyUSB0** with a baud rate of **1500000**:

`user $``screen /dev/ttyUSB0 1500000`
### Commands

In *Command Mode* (after pressing `Ctrl`+`a`) the following key combinations can be used:

#### General

- `?` - Show quick help information.
- `d` - Detach from the current session, all windows will continue to run in the background.

#### Creating and managing windows

- `c` - create another window
- `n` or `Space` - Switch to the next window.
- `p` or `Backspace` - Switch to the previous window.
- `0` - Switch to window 0 (similar for 1-9).
- `'`, then a number or title - switch to window with given number or title, used to reach windows numbered above 9.
- `Ctrl`+`a` - Switch to last used window.
- `Shift`+`a` - Set title for window.
- `"` - Display a list of windows, which can be navigated with arrow keys and selected with `Enter` or `Space`
- `w` - Display a non-interactive list of windows at the bottom of screen (see [Hardstatus Customization](https://wiki.gentoo.org#Hardstatus_Customization))

#### Copy, paste, and scroll operations

- `Esc` - Begin "copy mode", which allows scrolling and copying of window contents with following keys.
  - `Ctrl`+`B` - Scroll up in copy mode.
  - `Ctrl`+`F` - Scroll down in copy mode.
  - arrow keys - Move the cursor in copy mode.
  - `Space` or `Enter` - Begin or end copying.
- `]` = Paste the text copied in copy mode.

#### Logging and monitoring

- `Shift`+`h` = start/stop logging everything displayed on current window. log will be saved as a file named screenlog.X under the directory in which screen was run
- `h` = (disabled in default configuration) dump the current contents of the current window to a file named hardcopy.X under the directory in which screen was run
- `Shift`+`m` = start/stop monitoring current window for activity, any output produced by a program will cause an alert
- `_` = start/stop monitoring for inactivity (i.e. a long and output producing command finishes running)

### Listing Sessions

To view currently active screen sessions:

`user $````
screen -list
```
```
       30014.pts-1.larry-gentoo (Detached)
```
1 Socket in /tmp/screen/S-larry.
1 Socket in /tmp/screen/S-larry.

### Reattach to a session

To reattach to the session **30014**:

`user $``screen -r 30014`
### Multiuser

Initiate screen with a human readable session name:

`user $``screen -S multi`
All other clients to view (and act on) the active live terminal can join by running:

`user $``screen -x multi`
Then its like conference call style terminal.

### Session within session recursion

A Screen may be used within a Screen session. This occurs when connecting to a another host's Screen session from within a Screen session.

To signal the secondary (remote) Screen session with the Meta key, issue the key twice to signal the second (remote) Session.

| `Ctrl`+`a` | Normal meta key usage on local screen | 
| `Ctrl`+`aa` | Signals the second (remote) Session | 
| `Ctrl`+`aaa` | Signals the third (remote) Session | 
| `Ctrl`+`aaaa` | Signals the fourth (remote) Session, and so on | 

Some tend to remap the Meta key on the other terminals so they don't have to count. However, if using the console/terminal often, there tends to be a shortage of keys on the keyboard.

### Ending a Session

A screen session can be ended by sending the quit command to that session:

`user $``screen -X -S 30014 quit`
## Troubleshooting

### After quitting a text editor the contents of the edited file remain in the screen window

Add the following line to screen preferences file:

**`$HOME/.screenrc`**

### Programs with too much output make older window contents scrolled away

The scrollback size must be [increased](https://wiki.gentoo.org/wiki/Screen#Increase_the_scrollback_size).

### 256 color configuration does not work

## TTY auto-login with a GNU screen session

The following will allow the system to auto-login as a specific user, and automatically initiate a GNU Screen session on tty2. A good scenario for this: being off the network, secluded from society, and entering a password is a bit redundant. The following demonstrates using the BASH Shell.

MinGetty will provide autologin capability along without having to use a password:

`root #``emerge --ask net-dialup/mingetty`
Comment-out the second TTY (ie. c2, or terminal number of choosing) and add mingetty with autologin capability.

**`/etc/inittab`**

Replace the above "USER\_NAME" with desired login name!

Add a line within the boot loader's configuration file to specify loading the init #4 level. (Referencing the "scr2:4" level specified within the above inittab file.) Also, utilize the "nox" option as well. (As I can recall, the line ending with "4" specifies runlevel four in correspondence to the above inittab entry. And the nox avoids XDM starting(?) If something needs further documenting here for clarification, please do so!)

The following configuration states, use [framebuffer](https://wiki.gentoo.org/wiki/Framebuffer) at the specified screen size using nox and start init level 4.

**`/boot/grub/menu.lst`**

At this point, the system can now auto login on TTY2 with the default shell. Assuming the BASH shell, GNU Screen can be started automatically by adding the "screen" command to $HOME/.bash\_profile file. This file is executed automatically on first login. (Make sure to start any subsequent BASH shells using the "bash --no-profile" switch, including shells started within the $HOME/.screenrc files!)

**`$HOME/.bash_profile`**

```
# You don't need the "if/fi" statements. I only make mention to give you the idea of using a shared $HOME/.bash_profile.
# if [ $HOSTNAME = "localhost1.local" ]; then
    # Insert any $VARIABLES you want exported to the environment before GNU Screen is executed.
    # Insert any commands you want started on initial login before screen is executed.
    # vico/case checks if we're running from only a virtual tty
    vico="$(tty | grep -oE ....$)"
    case "$vico" in
        tty2) screen -wipe; ionice -c 2 -n 0 screen -RD ;;
    esac
    # Nothing further is executed after "screen -RD" unless you use "&"
# elif [ $HOSTNAME = "localhost2.local" ]; then
# fi
```
With a little further scripting within $HOME/.bash\_profile and a second entry within /etc/inittab, runlevel 5 may be specified for auto logging in and starting the preferred graphical desktop. (ie. XFCE4, GNOME, KDE, ...) And, while still spawning a GNU Screen session onto the desktop. But this is getting a little off topic of this GNU Screen Wiki.

## See also

- [Byob](https://www.byobu.org/) - Text-based window manager and terminal multiplexer.
- [Recommended\_tools#Terminal\_multiplexers](https://wiki.gentoo.org/wiki/Recommended_tools#Terminal_multiplexers) — other terminal multiplexers.
- [Tmux](https://wiki.gentoo.org/wiki/Tmux) — a program that enables a number of terminals (or windows), each running a separate program, to be created, accessed, and controlled from a single screen or terminal window.
