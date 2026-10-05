<!-- source: https://wiki.gentoo.org/wiki/Unrtf | group: Gentoo Wiki (Main) | wiki-title: Unrtf -->
---
title: unrtf
url: https://wiki.gentoo.org/wiki/Unrtf
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-02-23"
fingerprint: "361813537b775e90"
license: CC BY-SA 4.0
---

# unrtf

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**unrtf** is a program for displaying legacy Rich Text Format .rtf documents as HTML or plain text. In previous decades before the rise of XML-based office file formats, RTF was used as a "lowest common denominator" format that allowed for document sharing between otherwise incompatible word processors.

As with simular tools such as [antiword](https://wiki.gentoo.org/wiki/Antiword), unrtf is often used in combination with standard Linux tools such as [tail](https://wiki.gentoo.org/wiki/Coreutils#tail) and [grep](https://wiki.gentoo.org/wiki/Grep). Additionally, it has the ability to extract embedded images from documents and save them to disk.

## Installation

### Emerge

`root #``emerge --ask app-text/unrtf`
### USE flags


## Configuration

### Files

- /usr/share/unrtf/ the default location of configuration files for unrtf.

## Usage

### Invocation

`user $``unrtf --help`
Usage: unrtf \[--version\] \[--verbose\] \[--help\] \[--nopict|-n\] \[--noremap\] \[-P config\_search\_path\] \[--html\] \[--text\] \[--vt\] \[--latex\] \[--rtf\] \[-t \<file\_with\_tags>)\] \<filename>

The output of `--help` is a bit spartan, but the relevant section of the unrtf man file explains the options as follows:

- `--noremap` disables character set conversion (currently only works for 8-bit character sets).
- `--html` selects HTML output (default).
- `--rtf` selects RTF output. The resulting output will often be much smaller than the input.
- `--text` selects plain ASCII text output.
- `--vt` selects text output with VT100 escape codes.
- `--latex` selects output of a LaTeX document.
- `--verbose` prints additional information.
- `--quiet` suppress output of leading comments
- `--version` prints the program version.
- `-t` *tags\_file* specifies the tags output configuration file to be used. The command "unrtf -t html" is functionally identical to "unrtf --html". The configuration files are a simple format. To change the behavior of unrtf, a local copy of a system configuration file can be be made and edited. The most complete configuration file and hence the best starting point is /usr/share/unrtf/html.conf.
- `-P` *config\_search\_path* specifies the directories in which the configuration file for the specified format will be sought. The path can be provided as a single directory or a list of colon separated directories. The default is /usr/share/unrtf where distributed output configuration files are installed.

## Removal

`root #``emerge --ask --depclean --verbose app-text/unrtf`
