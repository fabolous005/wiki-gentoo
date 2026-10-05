<!-- source: https://wiki.gentoo.org/wiki/Zip | group: Gentoo Wiki (Main) | wiki-title: Zip -->
---
title: Zip
url: https://wiki.gentoo.org/wiki/Zip
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-09-20"
fingerprint: d619ba3a67a31991
license: CC BY-SA 4.0
---

# Zip

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Zip** provides classic ZIP [compression](https://wiki.gentoo.org/wiki/Data_compression). It is handy for cross-platform compatibility with Microsoft operating systems. Newer versions of Zip include support for Unicode and encryption. These can be enabled or disabled at build-time on Gentoo systems by their respective [USE flags](https://wiki.gentoo.org#USE_flags) (see below).

The equal and opposite program to Zip is [UnZip](https://wiki.gentoo.org/wiki/UnZip), which is included in a separate package ([app-arch/unzip](https://packages.gentoo.org/packages/app-arch/unzip)).

## Installation

### USE flags


Zip must be built with the `crypt` USE flag in order to support encryption. Also supports the `unicode` USE flag.

### Emerge

After adjusting USE flags:

`root #``emerge --ask app-arch/zip`
Optionally emerge [app-arch/unzip](https://packages.gentoo.org/packages/app-arch/unzip) for UnZip capabilities (see the [UnZip](https://wiki.gentoo.org/wiki/UnZip) article for more information).

## Removal

`root #``emerge --ask --depclean app-arch/zip`
## Configuration

### Environment variables

Zip does not have any configuration *files*, however its operation is modifiable by the following environment variables:

- `ZIPOPT` - Can be used to set any option for the zip command.
- `ZIP` - Does the exact same thing as the `ZIPOPT` variable (directly above).
- `Zip$Options` - For RISC OS use. Does the exact same thing as the `ZIPOPT` variable (directly above).
- `Zip$Exts` - For RISC OS use. Contains  extensions separated by a `:` (colon) that will cause native filenames with one of the specified extensions to be added to the ZIP file with basename and extension swapped.
- `ZIP_OPTS` - For VMS use. Does the exact same thing as the `ZIPOPT` variable.

## Usage

ZIP a file Creates the archive data.zip and puts all the files in the current directory in it in compressed form, type:

Note: No need to add .zip extension or suffix as it is added automatically by zip command.

`user $``zip data *` To zip-up an entire directory (including all subdirectories), type the following command:

`user $``zip -r data *`
It is possible to zip a file and save the ZIP file in another directory by indicating the new destination path with the name of the ZIP file at the end.

`user $``zip test/zipfolder/file10.zip file10` You can also try -9 option for best compressionː

`user $``zip -9 -r backupfile mydata`
Update one file or more files of the compressed archive, suppose we have compressed an archive then, modified a file. There is a possibility to add the update file to the compressed archive with the zip -u command.

`user $``zip -u backfile.zip foo boo`
You can replace (freshen) an existing entry in the ZIP archive only if it has been modified more recently than the version already in the ZIP archive. Unlike the update option, this will not add files that are not already in the ZIP archive.

`user $``zip -f foo.zip`
### Invocation

#### zip

`user $``zip --help`
Copyright (c) 1990-2008 Info-ZIP - Type 'zip "-L"' for software license.
Zip 3.0 (July 5th 2008). Usage:
zip \[-options\] \[-b path\] \[-t mmddyyyy\] \[-n suffixes\] \[zipfile list\] \[-xi list\]
  The default action is to add or replace zipfile entries from list, which
  can include the special name - to compress standard input.
  If zipfile and list are omitted, zip compresses stdin to stdout.
  -f   freshen: only changed files  -u   update: only changed or new files
  -d   delete entries in zipfile    -m   move into zipfile (delete OS files)
  -r   recurse into directories     -j   junk (don't record) directory names
  -0   store only                   -l   convert LF to CR LF (-ll CR LF to LF)
  -1   compress faster              -9   compress better
  -q   quiet operation              -v   verbose operation/print version info
  -c   add one-line comments        -z   add zipfile comment
  -@   read names from stdin        -o   make zipfile as old as latest entry
  -x   exclude the following names  -i   include only the following names
  -F   fix zipfile (-FF try harder) -D   do not add directory entries
  -A   adjust self-extracting exe   -J   junk zipfile prefix (unzipsfx)
  -T   test zipfile integrity       -X   eXclude eXtra file attributes
  -y   store symbolic links as the link instead of the referenced file
  -e   encrypt                      -n   don't compress these suffixes
  -h2  show more help

#### zipcloak

`user $``zipcloak --help`
ZipCloak 3.0 (July 5th 2008)
Usage:  zipcloak \[-dq\] \[-b path\] zipfile
  the default action is to encrypt all unencrypted entries in the zip file
  -d  --decrypt      decrypt encrypted entries (copy if given wrong password)
  -b  --temp-path    use "path" for the temporary zip file
  -O  --output-file  write output to new zip file
  -q  --quiet        quiet operation, suppress some informational messages
  -h  --help         show this help
  -v  --version      show version info
  -L  --license      show software license

#### zipnote

`user $``zipnote -h````
Copyright (c) 1990-2008 Info-ZIP - Type 'zipnote "-L"' for software license.
ZipNote 3.0 (July 5th 2008)
Usage:  zipnote [-w] [-q] [-b path] zipfile
  the default action is to write the comments in zipfile to stdout
  -w   write the zipfile comments from stdin
  -b   use "path" for the temporary zip file
  -q   quieter operation, suppress some informational messages
  -h   show this help    -v   show version info    -L   show software license
Example:
     zipnote foo.zip > foo.tmp
     ed foo.tmp
     ... then you edit the comments, save, and exit ...
     zipnote -w foo.zip < foo.tmp
  "@ name" can be followed by an "@=newname" line to change the name
```
#### zipsplit

`user $``zipsplit -h`
Copyright (c) 1990-2008 Info-ZIP - Type 'zipsplit "-L"' for software license.
ZipSplit 3.0 (July 5th 2008)
Usage:  zipsplit \[-tipqs\] \[-n size\] \[-r room\] \[-b path\] zipfile
  -t   report how many files it will take, but don't make them
  -i   make index (zipsplit.idx) and count its size against first zip file
  -n   make zip files no larger than "size" (default = 36000)
  -r   leave room for "room" bytes on the first disk (default = 0)
  -b   use "path" for the output zip files
  -q   quieter operation, suppress some informational messages
  -p   pause between output zip files
  -s   do a sequential split even if it takes more zip files
  -h   show this help    -v   show version info    -L   show software license

## See also

- [Data compression](https://wiki.gentoo.org/wiki/Data_compression) — a list of some of the **compression and file-archiver utilities** available in Gentoo Linux
- [UnZip](https://wiki.gentoo.org/wiki/UnZip) — provides [decompression](https://wiki.gentoo.org/wiki/Data_compression) for classic zip formats.
