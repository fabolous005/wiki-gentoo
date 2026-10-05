<!-- source: https://wiki.gentoo.org/wiki/Dos2unix | group: Gentoo Wiki (Main) | wiki-title: Dos2unix -->
---
title: dos2unix
url: https://wiki.gentoo.org/wiki/Dos2unix
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-06-10"
fingerprint: "55efe13bb5c7929a"
license: CC BY-SA 4.0
---

# dos2unix

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


dos2unix is a tool to convert text files from DOS line endings (carriage return + line feed) to Unix line endings (line feed). It is also capable of conversion between UTF-16 to UTF-8. Invoking the unix2dos command can be used to convert *from* Unix *to* DOS. This tool comes in handy when sharing files between Windows and Linux machines.

## Installation

`root #``emerge --ask app-text/dos2unix`
## Usage

To convert a file that has DOS line endings to Unix format:

`user $``dos2unix my_file.txt`
dos2unix: converting file my\_file.txt to Unix format...

To convert a file that has Unix line endings to DOS format:

`user $``unix2dos my_file.txt`
unix2dos: converting file my\_file.txt to DOS format...

## External resources

- man dos2unix
