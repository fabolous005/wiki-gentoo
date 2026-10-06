<!-- source: https://wiki.gentoo.org/wiki/Duperemove | group: Gentoo Wiki (Main) | wiki-title: Duperemove -->
---
title: Duperemove
url: https://wiki.gentoo.org/wiki/Duperemove
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-07-10"
fingerprint: "97062ef912a65761"
license: CC BY-SA 4.0
---

# Duperemove

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

Duperemove is a [btrfs](https://wiki.gentoo.org/wiki/Btrfs) and [XFS](https://wiki.gentoo.org/wiki/XFS) tool for finding duplicated extents and submitting them to the kernel for [deduplication](https://wiki.gentoo.org/wiki/Deduplication).

## Installation

### Emerge

`root #``emerge --ask sys-fs/duperemove`
## Usage

Detailed information can be seen by running man duperemove.

### Invocation

`root #``duperemove --help`
duperemove v0.11
Find duplicate extents and optionally dedupe them.
Basic usage: duperemove \[-r\] \[-d\] \[-h\] \[-v\] \[-A\] \[--hashfile=hashfile\] OBJECTS
"OBJECTS" is a list of files (or directories) which we
want to find duplicate extents in. If a directory is 
specified, all regular files inside of it will be scanned.
	\<switches>
	-r		Enable recursive dir traversal.
	-d		De-dupe the results (must run on a supported fs).
	--hashfile=FILE	Store hashes in this file.
	-A		Open files for dedupe in read-only mode.
	-h		Print numbers in human-readable format.
	-v		Print extra information (verbose).
	--help		Prints this help text.
Please see the duperemove(8) manpage for a complete list of options.

The following command shows how to deduplicate the /home filesystem; the hash file will be stored under the /root directory:

`root #``duperemove -rdh --hashfile=/root/home.hash /home`
### Reading a file list created with fdupes

By passing the `--fdupes` option, duperemove can work in conjunction with [fdupes](https://wiki.gentoo.org/wiki/Fdupes) in order to deduplicate a pre-calculated list of files. When in this mode, input will be accepted on stdin:

`root #``cat fdupes_list.txt | duperemove --fdupes`
This is handy when a list of duplicates has already been created so that disk-intensive deduplication job can be ran at a time when the system is not under heavy load.

It is also possible to deduplicate directly from fdupes (without creating a file list):

`root #``fdupes -r /path/to/filesystem/directory | duperemove --fdupes`
## See also

- [Deduplication](https://wiki.gentoo.org/wiki/Deduplication) — a mechanism for reducing the space taken by multiple identical copies of a file are stored on a [filesystem](https://wiki.gentoo.org/wiki/Filesystem)
- [fdupes](https://wiki.gentoo.org/wiki/Fdupes) — a tool for identifying duplicate files across a set of directories.
- [btrfs](https://wiki.gentoo.org/wiki/Btrfs) — a copy-on-write (CoW) [filesystem](https://wiki.gentoo.org/wiki/Filesystem) for Linux aimed at implementing advanced features while focusing on fault tolerance, repair, and easy administration.
- [XFS](https://wiki.gentoo.org/wiki/XFS) — a high-performance journaling [filesystem](https://wiki.gentoo.org/wiki/Filesystem)
