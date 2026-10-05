<!-- source: https://wiki.gentoo.org/wiki/Ranger | group: Gentoo Wiki (Main) | wiki-title: Ranger -->
---
title: Ranger
url: https://wiki.gentoo.org/wiki/Ranger
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-09-15"
fingerprint: bf8b9e1d53b51aa3
license: CC BY-SA 4.0
---

# Ranger

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**ranger** is a console file manager with VI key bindings. It provides a minimalistic and nice curses interface with a view on the directory hierarchy.

## Installation

### USE flags


### Emerge

`root #``emerge --ask app-misc/ranger`
### Additional software

Ranger can be extended with additional tools, this section will discuss some possibilities of note.

#### Dragon

[Dragon](https://github.com/mwh/dragon) is a drag and drop functionality for X/Wayland, that can be used in conjunction with Ranger.

Dragon is not currently present in the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository), however it is available at the [GURU](https://wiki.gentoo.org/wiki/Project:GURU) [overlay](https://wiki.gentoo.org/wiki/Ebuild_repository).

In a pinch, Dragon can be installed by compiling from the git repo, to a user's home. First, create a directory to hold the source if one does not yet exist, clone the git repository, and enter the source directory:

`user $````
mkdir --parents ~/.local/src
```
`user $````
cd ~/.local/src
```
`user $````
git clone https://github.com/mwh/dragon.git
```
`user $````
cd dragon
```
Install Dragon, prefixing to a directory in the user's home dir:

`user $``make prefix="~/.local" install`
The \~/.local/bin directory can be added to the `PATH` [environment variable](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/EnvVar#User_specific), to allow invocation of dragon from the command line - and in scripts - without specifying the full path to the executable.

To use Dragon in Ranger, add to the configuration files:

**`/.config/ranger/rc.conf`**

**`~/.config/ranger/rifle.conf`**

It should now be possible to press `Ctrl`+`d` in ranger to open a GUI popup with the selected folder, which can be used to drag and drop.

## Configuration

### Files

All configuration files can be copied to \~/.config/ranger/ using the following command:

`user $``ranger --copy-config=all`
### Image Preview on Wayland

Ueberzugpp serves as a drop in replacement for ueberzug, adding wayland support.

**`~/.config/ranger/rc.conf`**

If low quality pixel image is previewed, [change the layer output to kitty](https://github.com/jstkdng/ueberzugpp/issues/81#issuecomment-1623923211) even if kitty is not installed.

**`~/.config/ueberzugpp/config.json`**

## Usage

### Invocation

`user $``ranger --help````
                                                                                                                                                                                                                                     Usage: ranger [options] [path]
Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -d, --debug           activate debug mode
  -c, --clean           don't touch/require any config files.
  --logfile=file        log file to use, '-' for stderr
  --cachedir=dir        change the cache directory. (/home/chr/.cache/ranger)
  -r dir, --confdir=dir
                        change the configuration directory.
                        (/home/chr/.config/ranger)
  --datadir=dir         change the data directory.
                        (/home/chr/.local/share/ranger)
  --copy-config=which   copy the default configs to the local config
                        directory. Possible values: all, rc, rifle, commands,
                        commands_full, scope
  --choosefile=OUTFILE  Makes ranger act like a file chooser. When opening a
                        file, it will quit and write the name of the selected
                        file to OUTFILE.
  --choosefiles=OUTFILE
                        Makes ranger act like a file chooser for multiple
                        files at once. When opening a file, it will quit and
                        write the name of all selected files to OUTFILE.
  --choosedir=OUTFILE   Makes ranger act like a directory chooser. When ranger
                        quits, it will write the name of the last visited
                        directory to OUTFILE
  --selectfile=filepath
                        Open ranger with supplied file selected.
  --show-only-dirs      Show only directories, no files or links
  --list-unused-keys    List common keys which are not bound to any action.
  --list-tagged-files=tag
                        List all files which are tagged with the given tag,
                        default: *
  --profile             Print statistics of CPU usage on exit.
  --cmd=COMMAND         Execute COMMAND after the configuration has been read.
                        Use this option multiple times to run multiple
                        commands.
```
## External resources

- [LF](https://github.com/gokcehan/lf) - a terminal file manager inspired by ranger, written in Go.
- [app-misc/nnn](https://packages.gentoo.org/packages/app-misc/nnn) - the "missing terminal file browser" for X.
- [Yazi](https://github.com/sxyazi/yazi) - a blazing fast terminal file manager inspired by ranger, written in Rust. It is available at the [GURU](https://wiki.gentoo.org/wiki/Project:GURU) [overlay](https://wiki.gentoo.org/wiki/Ebuild_repository).
