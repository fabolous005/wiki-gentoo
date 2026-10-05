<!-- source: https://wiki.gentoo.org/wiki/PCManFM | group: Gentoo Wiki (Main) | wiki-title: PCManFM -->
---
title: PCManFM
url: https://wiki.gentoo.org/wiki/PCManFM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-01"
fingerprint: "3406f23908b569e4"
license: CC BY-SA 4.0
---

# PCManFM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Article status**

- #1 Review KDE style section
- #2 Review desktop icon instructions

**PCMan F**ile **M**anager (PCManFM) is a powerful yet lightweight file manager application, default file manager of [LXDE](https://wiki.gentoo.org/wiki/LXDE). Written by Hong Jen Yee.

## Installation

### USE flags


### Emerge

PCManFM can be easily installed via emerge:

`root #``emerge --ask x11-misc/pcmanfm`
## Configuration

### Openbox integration

To have PCManFM start (on a per-user basis) when using Openbox as a window manager add it to the user's \~/.config/openbox/autostart file.

**`~/.config/openbox/autostart`**

**Starting PCManFM with Openbox**

```
 --desktop &
```
### Adding desktop icons

Generally speaking, desktop icons for installed applications can be found in the /usr/share/applications/ folder. In order to add an application link to the Desktop simply copy the appropriate shortcut from the /usr/share/applications/ directory to the user's desktop directory. Follow the syntax in the example below by substituting `application.desktop` for the application of choice:

`user $``cp /usr/share/applications/application.desktop ~/Desktop/`
If the copied link for the application does not show up on the desktop try refreshing the session by logging out and logging back in.

### File templates

Like [GNOME](https://wiki.gentoo.org/wiki/GNOME)'s Nautilus and [KDE](https://wiki.gentoo.org/wiki/KDE)'s Dolphin file managers, PCManFS (version 1.2.0 and up) makes use of context menu file templates for quick file creation. PCManFS supports *both* the KDE style and GNOME style of template creation. Users can decide which method to use. For Gnome users or new users in general the quickest and most straight forward option would be to skip to [GNOME style](https://wiki.gentoo.org/wiki/PCManFM#GNOME_style). For transitioning KDE users the following [KDE style](https://wiki.gentoo.org/wiki/PCManFM#KDE_style) section should be helpful.

#### KDE style

To create new file templates using the KDE style .desktop files must be defined in the proper locations. Global templates should be placed in /usr/share/templates while local (per-user) templates should be placed in \~/.local/share/templates If these folders do not exist they will need to be created.

For global templates:

`root #``mkdir -p /usr/share/templates/.source`
For local templates:

`user $``mkdir -p ~/.local/share/templates`
The contents of the .desktop files should follow this scheme:

**`/usr/share/templates/text.desktop`**

```
[Desktop Entry]
Comment=<Comment on template>
Icon=<Icon name for template>
Name=<Name for template>
Type=Link
URL=<path to the template file>
```
If the above folder(s) did not exist before PCManFM was running, then the folder(s) will be ignored until PCManFM is restarted. Either log out and log back in or use `kill` to end the PCManFM process.

`user $``killall pcmanfm`
After a refresh the newly created template files can be accessed via the context menu's (right-click) Create New entry.

#### GNOME style

To use the GNOME style of templates make a folder titled \~/Templates in the user's home directory:

`user $``mkdir ~/Templates`
Fill the \~/Templates folder with desired template files. In order to work as most users expect, the template files must contain the data they are supposed to represent. For example, to have bash.sh, text.txt, and a document.odt as file templates create one of each *type* of file in the \~/Templates directory. This process involves saving a file of each type with the associated program. A template of an empty text file should be created with a text editor, the bash script file should contain a line starting with `#!/bin/bash`, and an empty open document text file should be created and then saved with an open document text editor ([LibreOffice](https://wiki.gentoo.org/wiki/LibreOffice) or [OpenOffice](https://wiki.gentoo.org/index.php?title=OpenOffice&action=edit&redlink=1)).

`user $``ls ~/Templates`
bash.sh  file.txt  document.odt

If the \~/Templates folder did not exist before PCManFM was running, then the folder will be ignored until PCManFM is restarted. Either log out and log back in or use `kill` to end the PCManFM process.

`user $``killall pcmanfm`
After a refresh the newly created template files can be accessed via the context menu's (right-click) Create New entry.

[x11-misc/xdg-user-dirs](https://packages.gentoo.org/packages/x11-misc/xdg-user-dirs) can configure the location of \~/Templates:

`user $``xdg-user-dirs-update --set TEMPLATES .config/Templates`
## Usage

### Invocation

`user $``pcmanfm --help`
Usage:
  pcmanfm \[OPTION...\] \[FILE1, FILE2,...\]  
Help Options:
  -h, --help                   Show help options
  --help-all                   Show all help options
  --help-gtk                   Show GTK+ Options
Application Options:
  -p, --profile=PROFILE        Name of configuration profile
  -d, --daemon-mode            Run PCManFM as a daemon
  --no-desktop                 No function. Just to be compatible with nautilus
  --desktop                    Launch desktop manager
  --desktop-off                Turn off desktop manager if it's running
  --desktop-pref               Open desktop preference dialog
  --one-screen                 Use --desktop option only for one screen
  -w, --set-wallpaper=FILE     Set desktop wallpaper from image FILE
  --wallpaper-mode=MODE        Set mode of desktop wallpaper. MODE=(color|stretch|fit|crop|center|tile|screen)
  --show-pref=N                Open Preferences dialog on the page N
  -n, --new-win                Open new window
  -f, --find-files             Open a Find Files window
  --role=ROLE                  Window role for usage by window manager
  --display=DISPLAY            X display to use

## See also

- [File managers](https://wiki.gentoo.org/wiki/File_managers) — a computer program that allows for the manipulation of files and directories on a computer's [filesystem](https://wiki.gentoo.org/wiki/Filesystem).
