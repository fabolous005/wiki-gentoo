<!-- source: https://wiki.gentoo.org/wiki/TestDisk | group: Gentoo Wiki (Main) | wiki-title: TestDisk -->
---
title: TestDisk
url: https://wiki.gentoo.org/wiki/TestDisk
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-26"
fingerprint: "1603da593ed70ba6"
license: CC BY-SA 4.0
---

# TestDisk

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

TestDisk is a free and open source, cross platform data recovery suite. TestDisk was written by Christophe Grenier in C and is capable of recovering files and partitions on FAT12, FAT16, FAT32, NTFS, and ext2 filesystems. The project continues to be actively maintained by Grenier.

## Installation

### USE flags


| [+sudo](https://packages.gentoo.org/useflags/+sudo) | Enable sudo helper integration | 
| [gui](https://packages.gentoo.org/useflags/gui) | Enable support for a graphical user interface | 
| [jpeg](https://packages.gentoo.org/useflags/jpeg) | Add JPEG image support | 
| [ntfs](https://packages.gentoo.org/useflags/ntfs) | Include the ability to read NTFS filesystems | 
| [reiserfs](https://packages.gentoo.org/useflags/reiserfs) | Include reiserfs reading ability | 
| [static](https://packages.gentoo.org/useflags/static) | !!do not set this during bootstrap!! Causes binaries to be statically linked instead of dynamically | 
| [zlib](https://packages.gentoo.org/useflags/zlib) | Add support for zlib compression | 

### Emerge

Install TestDisk:

`root #``emerge --ask app-admin/testdisk`
## Usage

Start TestDisk:

`user $``testdisk`
TestDisk can work with raw disks created with [dd](https://wiki.gentoo.org/wiki/Dd):

`user $``testdisk rawdisk.dd`
## PhotoRec

PhotoRec is a signature based recovery tool that is installed alongside TestDisk. PhotoRec scans partitions for file header information in order to find lost or deleted files. Since file header information is queried, PhotoRec can work on a variety of file systems and is capable of finding over [400 types of files](http://www.cgsecurity.org/wiki/File_Formats_Recovered_By_PhotoRec). Files found by PhotoRec do not have their original file name; recovered data is grouped into folders and labeled with a file extension matching the header information (ASCII files are labeled

- .txt, JPEG files named
- .jpeg, PNG files named
- .png, etc.). In order to find the specific file, the content of each file must be searched.

Start PhotoRec:

`user $``photorec`
## See also

- [Dd](https://wiki.gentoo.org/wiki/Dd) — a utility used to copy raw data from a source into sink, where source and sink can be a block device, file, or piped input/output.
- [Ddrescue](https://wiki.gentoo.org/wiki/Ddrescue) — a tool provided by GNU to retrieve data from failing (block) storage devices like disk drives, CDROMs, or memory sticks, etc.
