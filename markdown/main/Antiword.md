<!-- source: https://wiki.gentoo.org/wiki/Antiword | group: Gentoo Wiki (Main) | wiki-title: Antiword -->
---
title: antiword
url: https://wiki.gentoo.org/wiki/Antiword
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-05-23"
fingerprint: f5529b1873f6ce81
license: CC BY-SA 4.0
---

# antiword

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**antiword** is a program for displaying legacy Microsoft Word .doc documents in common use from MS Word 97 – 2007 as plain text. Typically, antiword is used to display MS Word documents in a terminal window often in combination with standard Linux tools such as [head](https://wiki.gentoo.org/wiki/Coreutils#head) and [grep](https://wiki.gentoo.org/wiki/Grep). Additionally, it has the ability to extract images from MS Word documents and save them to disk.

Despite the fact that development of antiword has stagnated and there have been no releases since 2005 it is nonetheless frequently used by other software packages to handle legacy MS Word documents. For example, the desktop publishing software Scribus uses antiword as an import filter for .doc documents and users of the text-based email client [mutt](https://wiki.gentoo.org/wiki/Mutt) frequently use it to display such documents in the terminal as well.

## Installation

### Emerge

`root #``emerge --ask app-text/antiword`
### USE flags


### Environment variables

- `ANTIWORDHOME` the location of antiword's configuration files.
- `COLUMNS` the width of output unless overridden by the `-w` option.

### Files

- /usr/share/antiword/ — Global (system wide) configuration files for mapping code pages to UTF-8 output.
- \~/.antiword — This file may contain a path which specifies an alternate location for code page map files; `$ANTIWORDHOME` is preferred.

## Removal

`root #``emerge --ask --depclean --verbose app-text/antiword`
## Usage

### Invocation

`user $``antiword -h`
Name: antiword
	Purpose: Display MS-Word files
	Author: (C) 1998-2005 Adri van Os
	Version: 0.37  (21 Oct 2005)
	Status: GNU General Public License
	Usage: antiword \[switches\] wordfile1 \[wordfile2 ...\]
	Switches: \[-f|-t|-a papersize|-p papersize|-x dtd\]\[-m mapping\]\[-w #\]\[-i #\]\[-Ls\]
		-f formatted text output
		-t text output (default)
		-a \<paper size name> Adobe PDF output
		-p \<paper size name> PostScript output
		  paper size like: a4, letter or legal
		-x \<dtd> XML output
		  like: db (DocBook)
		-m \<mapping> character mapping file
		-w \<width> in characters of text output
		-i \<level> image level (PostScript only)
		-L use landscape mode (PostScript only)
		-r Show removed text
		-s Show hidden (by Word) text

## Troubleshooting

### antiword cannot find its home directory

This is a known issue with antiword that comes up most often in hosted environments. The solution is to set `$ANTIWORDHOME` to the location of its configuration files, usually /usr/share/antiword.

## Limitations

- While quite usable, Antiword is far from complete and cannot render complex layouts or obscure formatting to a text terminal without compromise.
- antiword predates Microsoft Office XML formats and therefore lacks support for the .docx file format entirely.
- antiword cannot tell the difference between a file that cannot be read and a file that does not exist.
- antiword long predates the XDG Base Directory Specification so it lacks direct support for it.
