<!-- source: https://wiki.gentoo.org/wiki/Fdupes | group: Gentoo Wiki (Main) | wiki-title: Fdupes -->
---
title: fdupes
url: https://wiki.gentoo.org/wiki/Fdupes
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-15"
fingerprint: "9604ba6b0ea79a21"
license: CC BY-SA 4.0
---

# fdupes

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**fdupes** is a tool for identifying duplicate files across a set of directories. It works by scanning the specified directories for files, running md5sum on those files, then running a byte-by-byte comparison on the files. It can work in tandem with [duperemove](https://wiki.gentoo.org/wiki/Duperemove), a [deduplication](https://wiki.gentoo.org/wiki/Deduplication) tool (not only for [btrfs](https://wiki.gentoo.org/wiki/Btrfs)).

## Installation

### Emerge

`root #``emerge --ask app-misc/fdupes`
## Configuration

fdupes has no configuration options other than optional command-line parameters.

## Usage

### Invocation

`user $``fdupes --help````
Usage: fdupes [options] DIRECTORY...
 
 -r --recurse     	for every directory given follow subdirectories
                  	encountered within
 -R --recurse:    	for each directory given after this option follow
                  	subdirectories encountered within
 -s --symlinks    	follow symlinks
 -H --hardlinks   	normally, when two or more files point to the same
                  	disk area they are treated as non-duplicates; this
                  	option will change this behavior
 -n --noempty     	exclude zero-length files from consideration
 -f --omitfirst   	omit the first file in each set of matches
 -1 --sameline    	list each set of matches on a single line
 -S --size        	show size of duplicate files
 -m --summarize   	summarize dupe information
 -q --quiet       	hide progress indicator
 -d --delete      	prompt user for files to preserve and delete all
                  	others; important: under particular circumstances,
                  	data may be lost when using this option together
                  	with -s or --symlinks, or when specifying a
                  	particular directory more than once; refer to the
                  	fdupes documentation for additional information
 -N --noprompt    	together with --delete, preserve the first file in
                  	each set of duplicates and delete the rest without
                  	prompting the user
 -v --version     	display fdupes version
 -h --help        	display this help message
```
### Find duplicate files recursively

To find duplicate files in target directories recursively the following command could be used:

`user $``fdupes --recurse --size /path/to/dir/one /path/to/dir/two`
Most of the time, however, it is wise to redirect the output of the fdupes command to a file:

`user $``fdupes --recurse --size /path/to/dir/one /path/to/dir/two >> /tmp/fdupes_file_list.txt`
Creating a file is a wise and efficient idea, especially when a large amount of files are being compared. It is much easier to look through a large file list with a text editor rather than attempting to parse the list via scroll back in a terminal buffer.

### Find and delete files recursively

Users are strongly cautioned to run one of the above command(s) *before* running one of the next commands. This is done in order to verify the output is as expected. Do the operation right the first time; the fewer mistakes the better! After output is satisfactory, the following command can be used to delete all but the *first* occurrence of the file. Be sure to list the directories in the order of precedence so that the correct files are preserved. I.e. to keep all the files in home directory, listing the home directory last will make it show up first in the list.

The following command uses the `--noprompt` (`-N`) and `--delete` (`-d`) options to delete all but the first duplicate found in a file list (created using a previous command) without prompting the user:

`user $``fdupes --noprompt --delete --recurse /path/to/dir/one /path/to/dir/two`
## Removal

### Unmerge

No special files need to removed. Uninstall FDUPES via:

`root #``emerge --ask --depclean --verbose app-misc/fdupes`
## See also

- [Duperemove](https://wiki.gentoo.org/wiki/Duperemove) — a [btrfs](https://wiki.gentoo.org/wiki/Btrfs) and [XFS](https://wiki.gentoo.org/wiki/XFS) tool for finding duplicated extents and submitting them to the kernel for [deduplication](https://wiki.gentoo.org/wiki/Deduplication)
