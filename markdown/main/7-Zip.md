<!-- source: https://wiki.gentoo.org/wiki/7-Zip | group: Gentoo Wiki (Main) | wiki-title: 7-Zip -->
---
title: "7-Zip"
url: https://wiki.gentoo.org/wiki/7-Zip
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-19"
fingerprint: d59f3aca6581d9e1
license: CC BY-SA 4.0
---

# 7-Zip

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**7-Zip** is a file [archiver](https://wiki.gentoo.org/wiki/Data_compression).

## Installation

### USE flags


### USE flags for
            [app-arch/7zip](https://packages.gentoo.org/packages/app-arch/7zip)
            
            Free file archiver for extremely high compression

### Emerge

7-Zip can be installed by running:

`root #``emerge --ask app-arch/7zip`
## Usage

### Extraction

To extract all files from an archive, use either `e` or `x` in the following command:

`user $``7zz <e/x> <archive name>`
In the above command, `e` will simply just **e**xtract the archive, while `x` will e**x**tract the archive, but with full paths.

### Archiving

To add files and/or folders to an archive, use the following command:

`user $``7zz a <folder/file(s) name(s)>`
#### Making a password-protected archive

A password-protected archive can be created using the following flags:

- `-p` : Prompt's the user for a password

Optionally, archive header encryption can also be enabled using `-mhe=on`, which forces the `7z` format to be used.

## See also

- [Data compression](https://wiki.gentoo.org/wiki/Data_compression) — a list of some of the **compression and file-archiver utilities** available in Gentoo Linux
- [Tar](https://wiki.gentoo.org/wiki/Tar) — an [archiver](https://wiki.gentoo.org/wiki/Data_compression) tool that provides the ability to create tar archives, as well as various other kinds of manipulation.
- [Zip](https://wiki.gentoo.org/wiki/Zip) — provides classic ZIP [compression](https://wiki.gentoo.org/wiki/Data_compression)

## External resources

[https://linux.die.net/man/1/7z](https://linux.die.net/man/1/7z) - 7-zip Linux man-page
