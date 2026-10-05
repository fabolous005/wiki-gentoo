<!-- source: https://wiki.gentoo.org/wiki/Detox | group: Gentoo Wiki (Main) | wiki-title: Detox -->
---
title: Detox
url: https://wiki.gentoo.org/wiki/Detox
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-13"
fingerprint: ee9574792bf3bb9d
license: CC BY-SA 4.0
---

# Detox

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

detox is a utility that safely removes spaces and strange characters from filenames and directories.

## Installation

### USE flags


### Emerge

`root #``emerge --ask app-misc/detox`
## Configuration

### Files

- /etc/detoxrc - Detox's global configuration file. Specify an alternate configuration file using the `-f` option at the command line.
- \~/.detoxrc - Detox's local (per user) configuration file. Specify an alternate configuration file using the `-f` option at the command line.
- /usr/share/detox/iso8859\_1.tbl - Detox's default ISO 8859-1 translation table. Copy this file into user space in the event that it needs modification. Specify an alternate 'sample' file using the `-s` option at the command line.
- /usr/share/detox/unicode.tbl - Detox's default Unicode (UTF-8) translation table. Copy this file into user space in the event that it needs modification. Specify an alternate 'sample' file using the `-s` option at the command line.

## Usage

### Invocation

`user $``detox --help````
usage: detox [-hLnrvV] [-f configfile] [-s sequence] [--dry-run] [--special]
          file [file ...]
        -f configfile   choose which config file to use
        -h --help       this message
        -L              list available sequences and exit
                        with -v ... dump sequence contents
        -n --dry-run    do a dry run (don't actually do anything)
        -r              be recursive (descend into subdirectories)
        --remove-trailing (deprecated)
                        remove trailing _ and - before a period
        -s sequence     choose which sequence to detox with
        --special       work on links and special files
        -v              be verbose
        -V              show the current version
```
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-misc/detox`
