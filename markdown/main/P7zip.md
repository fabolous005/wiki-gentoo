<!-- source: https://wiki.gentoo.org/wiki/P7zip | group: Gentoo Wiki (Main) | wiki-title: P7zip -->
---
title: p7zip
url: https://wiki.gentoo.org/wiki/P7zip
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-19"
fingerprint: df8fba1a60954bcd
license: CC BY-SA 4.0
---

# p7zip

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is

**archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

*Page archived as of**2026-07-19**.*

TLDR:

**Do not use this article!**




**p7zip** is a command-line port of [7-Zip](https://www.7-zip.org/) for POSIX compliant systems such as Unix, macOS, BeOS, and Amiga. Created by Igor Pavlov, the 7-Zip [compression](https://wiki.gentoo.org/wiki/Data_compression) type implements the LZMA compression algorithm, which is one of the highest compression ratios currently available. Since version 4.10 it supports little and big endian machines.<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>
p7zip has most of the additional compression methods from the windows only 7zip plugin [modern7z](https://www.tc4shell.com/en/7zip/modern7z/) ported and integrated in core. Additional compression methods available compared to 7zip are BROTLI, ZSTD, LZ5, LZ4, LIZARD and FLZMA2.

## Installation

### USE flags


| [+pch](https://packages.gentoo.org/useflags/+pch) | Enable precompiled header support for faster compilation at the expense of disk space and memory | 
| [natspec](https://packages.gentoo.org/useflags/natspec) | Use dev-libs/libnatspec to correctly decode non-ascii file names archived in Windows. | 
| [rar](https://packages.gentoo.org/useflags/rar) | Enable support for non-free rar decoder | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

p7zip can be installed by running:

`root #``emerge --ask app-arch/p7zip`
## Usage

### Invocation

There are three different ways to invoke the compression utility:

- 7z
- 7za
- 7zr

If compiled with the [`wxwidgets`](https://packages.gentoo.org/useflags/wxwidgets) USE flag it also provides a graphical interface via the following invocations:

- 7zG
- 7zFM

Also a wrapper included is for 7za:

- p7zip

### Extraction of files to current directory

To extract all files from an archive to the current directory without using directory names, use the following command:

`user $``7za e <archive name>`
Where `<archive name>` is to be replaced with the archive's name.

To extract with full paths, use the following command:

`user $``7za x <archive name>`
### Extraction to a new directory

To extract into a new directory, use the following command:

`user $``7za x -o<folder name> <archive name>`
Where `<folder name>` is the name of the new folder.

### Preserving file attributes

When using 7-zip on Gentoo or any other operating system that should preserve Unix file permissions, tar will need to be used in conjunction with 7z to archive or extract files.

Use the following command to archive directories of files, preserving Unix file permissions:

`user $``tar cf - <directory> | 7za a -si <directory>.tar.7z`
To extract:

`user $``7za x -so <directory>.tar.7z | tar xf -`
## See also

- [Data compression](https://wiki.gentoo.org/wiki/Data_compression) — a list of some of the **compression and file-archiver utilities** available in Gentoo Linux
- [7-Zip](https://wiki.gentoo.org/wiki/7-Zip) — a file [archiver](https://wiki.gentoo.org/wiki/Data_compression).
- [Tar](https://wiki.gentoo.org/wiki/Tar) — an [archiver](https://wiki.gentoo.org/wiki/Data_compression) tool that provides the ability to create tar archives, as well as various other kinds of manipulation.
- [Zip](https://wiki.gentoo.org/wiki/Zip) — provides classic ZIP [compression](https://wiki.gentoo.org/wiki/Data_compression)
