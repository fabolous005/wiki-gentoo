<!-- source: https://wiki.gentoo.org/wiki/Euse | group: Gentoo Wiki (Main) | wiki-title: Euse -->
---
title: Euse
url: https://wiki.gentoo.org/wiki/Euse
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-13"
fingerprint: e2b714cfa6a9fb19
license: CC BY-SA 4.0
---

# Euse

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

euse provides functionality to set (disable/enable) and obtain information about [USE flags](https://wiki.gentoo.org/wiki/USE_flag) in [make.conf](https://wiki.gentoo.org/wiki/Make.conf), without having to edit the file directly. It is also used to get detailed information about USE flags like description, status of flags (enabled/disabled), type of flag (global/local), etc.

For more information on USE flags, please refer to [USE Flags](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/USE).

euse is part of the [gentoolkit](https://wiki.gentoo.org/wiki/Gentoolkit) package.

### Invocation

See euse --help for a complete listing of all invocation options:

`user $``euse --help`  ```
euse (0.5.1-r1)
 
Syntax: euse <option> [suboptions] [useflaglist]
 
Options: -h, --help           - show this message
         -V, --version        - show version information
         -i, --info           - show descriptions for the given useflags
         -I, --info-installed - show descriptions for the given useflags and
                                their current impact on the installed system
         -g, --global         - show only global use flags (suboption)
         -l, --local          - show only local use flags (suboption)
         -a, --active         - show currently active useflags and their origin
         -E, --enable         - enable the given useflags
         -D, --disable        - disable the given useflags
         -R, --remove         - remove all references to the given flags from
                                make.conf and package.use to revert to default
                                settings
         -P, --prune          - alias for --remove
         -p, --package        - used with -E, -D, and -R to apply to a
                                specific package only
 
Notes: euse currently works for global flags defined
       in make.globals, make.defaults, make.conf, use.force, and use.mask
       and local flags defined in package.use and individual package ebuilds.
       It might have issues with cascaded profiles. If multiple options are
       specified only the last one will be used.
```
### Viewing USE flags

The euse -a command shows the current active USE flags and and where they are activated.

There are 7 "columns" that euse uses to show whether a flag is set/unset and where the flag has been set. Upper case for set, lower case for unset:

- +/-: active or not
- "E": set in the **E**nvironment
- "C": set in make.**C**onf
- "D": set in make.**D**efaults
- "G": set in make.**G**lobals
- "F": set in use.force
- "m": flipped in use.mask

Full positive values would be `[+ECDGFm]`, full negative values would be `[-ecdgfM]`, full missing values would be `[-      ]`.

Example euse -a output (truncated):

`user $``euse -a`
\[...\]
flac                \[+  D   \] 
fortran             \[+  D   \] 
fuse                \[-      \] (fuse)
gbm                 \[-      \] (archive)
gdbm                \[+  D   \] 
gif                 \[+  D   \] 
gnutls              \[-      \] (gnutls)
gpm                 \[+  D   \] 
gtk                 \[+  D   \] 
gui                 \[+  D   \] 
hdri                \[+ C    \] 
heif                \[+ C    \] 
hpcups              \[+ C    \] 
hwaccel             \[-      \] (hwaccel)
iconv               \[+  D   \] 
\[...\]

Similarly the euse -a -g command is used to view *active* global USE flags. The euse -a -l command does the same for active local USE flags. `-g` and `-l` are sub-options to euse and need an option before them (like `-a`) to function correctly.

### Setting, and unsetting USE flags

euse is able to set, unset, or remove USE flags from make.conf. The commands used for this are euse -E flagname (enable a flag), euse -D flagname (disable a flag), and euse -P flagname (remove, or "prune", a flag).

#### Enabling a USE flag

Use the `-E` option to *enable* a USE flag.

`root #``euse -E 3dfx`
/etc/portage/make.conf was modified, a backup copy has been placed at /etc/portage/make.conf.euse\_backup

The /etc/portage/make.conf file looks like so after the command was run:

**`make.conf`**

**After enabling the 3dfx USE flag**

```
USE="alsa acpi apache2 -arts cups cdr crypt cscope -doc fbcon \
     firefox gd gif gimpprint gnome gpm gstreamer gtkhtml imlib \
     innodb -java javascript jpeg libg++ libwww mad mbox md5sum \
     mikmod mmx motif mpeg mpeg4 mysql ncurses nvidia \
     ogg odbc offensive opengl pam pdflib perl png python \
     quicktime readline sdl spell sse ssl svga tcltk tiff truetype usb \
     vanilla X xosd xv xvid x86 zlib 3dfx"
```
#### Disabling a USE flag

Use the `-D` option to *disable* a USE flag.

`root #``euse -D 3dfx`
/etc/portage/make.conf was modified, a backup copy has been placed at /etc/portage/make.conf.euse\_backup

The example /etc/portage/make.conf file, after the command:

**`make.conf`**

**After disabling the 3dfx USE flag**

```
USE="alsa acpi apache2 -arts cups cdr crypt cscope -doc fbcon \
     firefox gd gif gimpprint gnome gpm gstreamer gtkhtml imlib \
     innodb -java javascript jpeg libg++ libwww mad mbox md5sum \
     mikmod mmx motif mpeg mpeg4 mysql ncurses nvidia \
     ogg odbc offensive opengl pam pdflib perl png python \
     quicktime readline sdl spell sse ssl svga tcltk tiff truetype usb \
     vanilla X xosd xv xvid x86 zlib -3dfx"
```
#### Remove (prune) a USE flag

Use the `-P` (prune) option to *remove* a USE flag.

`root #``euse -P 3dfx`
The example /etc/portage/make.conf file, after the command:

**`make.conf`**

**After removing the 3dfx USE flag**

```
USE="alsa acpi apache2 -arts cups cdr crypt cscope -doc fbcon \
     firefox gd gif gimpprint gnome gpm gstreamer gtkhtml imlib \
     innodb -java javascript jpeg libg++ libwww mad mbox md5sum \
     mikmod mmx motif mpeg mpeg4 mysql ncurses nvidia \
     ogg odbc offensive opengl pam pdflib perl png python \
     quicktime readline sdl spell sse ssl svga tcltk tiff truetype usb \
     vanilla X xosd xv xvid x86 zlib"
```
## See also

- [Gentoolkit](https://wiki.gentoo.org/wiki/Gentoolkit) — a suite of tools to ease the administration of a Gentoo system, and [Portage](https://wiki.gentoo.org/wiki/Portage) in particular.
- [Useful\_Portage\_tools](https://wiki.gentoo.org/wiki/Useful_Portage_tools) — provides a list of Gentoo-specific system management tools, notably for [Portage](https://wiki.gentoo.org/wiki/Portage), available in the [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository).
