<!-- source: https://wiki.gentoo.org/wiki/Frotz | group: Gentoo Wiki (Main) | wiki-title: Frotz -->
---
title: frotz
url: https://wiki.gentoo.org/wiki/Frotz
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-01-04"
fingerprint: d62a2b3cc7a7abc9
license: CC BY-SA 4.0
---

# frotz

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**frotz** is a Z-Machine emulator and Z-code interpreter for text-based interactive fiction games such as [Zork](https://en.wikipedia.org/wiki/Zork). The virtual machine is written in [C](https://wiki.gentoo.org/wiki/C) and is intended to be highly portable across operating systems. Development originated on [MS-DOS](https://en.wikipedia.org/wiki/MS-DOS) in 1995. Soon thereafter, it was ported to Unix-like operating systems.

### Inner Workings

A Z-Machine implements a byte-code interpreter which is a compiled form of ZIL (Zork implementation language), closely related to the MDL (pronounced "muddle") dialect of [Lisp](https://wiki.gentoo.org/wiki/Lisp). Internally the virtual machine itself, simply called Z, has a 16-bit word-size but its program counter size is not entirely fixed and varies by Z-machine version.

Different versions of the Z-Machine specification have different capabilities. The initial design goal was simply to be able to implement Zork inside of a VM that could be easily ported to as many different first generation home computers as possible without modifying the underlying game. Early versions of the Z-machine had an internal program counter that could support up to 128kB of address space but only supported text assets. Whereas, later versions had an internal program counter that could support up to 512kB of address space and handle simple graphics and sound. In order to overcome the limitations of the home computers of the era, some games were split into multi-part files; handy of the underlying game's needs exceeded the RAM limits of the underlying hardware.

By modern convention, Z-Machine programs end in .z1, .z2, .z3, .z4, .z5, .z6, .z7, and .z8 according to their Z-Machine version. Rarely, the much older .dat extension is used.

## Installation

### USE flags


### USE flags for
            [games-engines/frotz](https://packages.gentoo.org/packages/games-engines/frotz)
            
            Interpreter for Z-code based text games

### Emerge

Optional sound support exists but most Z-machine games don't actually use it. The easiest way to install frotz is to install it without sound support:

**`/etc/portage/package.use/frotz`**

```
games-engines/frotz modplug -sound -sdl
```
`root #``emerge --ask games-engines/frotz`


## Configuration

### Environment variables

- `ZCODE_PATH` — The default location of game files.
- `INFOCOM_PATH` — deprecated: use `ZCODE_PATH`.

### Files

- /etc/frotz.conf — global (system wide) configuration file.
- $HOME/.frotzrc — local (per user) configuration file.
- /usr/share/doc/frotz-\* — example configuration files.

## Usage

### Invocation

`user $``frotz -help`
FROTZ V2.53 - Curses interface.  Audio output disabled.
An interpreter for all Infocom and other Z-Machine games.
Syntax: frotz \[options\] story-file \[blorb file\]
  -a   watch attribute setting    	 -o   watch object movement
  -A   watch attribute testing    	 -O   watch object locating
  -b \<colorname> background color 	 -p   plain ASCII output only
  -c # context lines              	 -P   alter piracy opcode
  -d   disable color              	 -q   quiet (disable sound effects)
  -e   enable sound               	 -r # right margin
  -E \<style> emphasis style       	 -R \<path> restricted read/write
  -f \<colorname> foreground color 	 -s # random number seed value
  -F   Force color mode           	 -S # transcript width
  -h # text height                	 -t   set Tandy bit
  -i   ignore fatal errors        	 -u # slots for multiple undo
  -I # interpreter number         	 -v   show version information
  -l # left margin                	 -w # text width
  -L \<file> load this save file   	 -x   expand abbreviations g/x/z
  -m   enable mouse support       	 -Z # error checking (see below)
Error checking: 0 none, 1 first only (default), 2 all, 3 exit after any error.
For more options and explanations, please read the manual page.

## Troubleshooting

### Resizing the terminal window does not cause the text to reflow

The Z-Machine captures the character resolution of the terminal when the frotz is first launched, such as 80×25. Thereafter, this is assumed to be the correct resolution even after a game is resumed from a saved state. This is less a bug and more an artifact of the era in which the Z-Machine was created: at the time terminals had fixed character resolutions that did not change. This was a reasonable assumption when the Z-Machine was created but it has not aged well in the era of graphical interfaces and resizable terminal windows.

There is no "fix." The issue is known upstream. The work-around is to set your terminal window geometry as you wish to have it for the duration of a game.

### Frotz puts dot files directly into my home directory

Unfortunately, frotz does not support the [XDG Base Directory specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html); it was developed long before the specification existed. While it is still actively maintained this feature has not yet been added.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose games-engines/frotz`
