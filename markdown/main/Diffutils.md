<!-- source: https://wiki.gentoo.org/wiki/Diffutils | group: Gentoo Wiki (Main) | wiki-title: Diffutils -->
---
title: Diffutils
url: https://wiki.gentoo.org/wiki/Diffutils
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-05"
fingerprint: "373eba18dba0ce29"
license: CC BY-SA 4.0
---

# Diffutils

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Diffutils** is a package that contains several utilities for identifying the differences in two different files.

## Installation

### USE flags


### Emerge

`root #``emerge --ask sys-apps/diffutils`
## Usage

The two files used in the examples are:

**`one`**

**`two`**

### cmp

cmp is used to compare two files byte by byte and reports the first byte difference.

#### Comparing two files

Finding where the first byte difference is similar to using diff:

`user $``cmp one two`
one two differ: byte 1, line 1

To print the bytes that differ, use `-b`:

`user $``cmp one two`
one two differ: byte 1, line 1 is 110 H 107 G

### diff

diff compares two files line by line

#### Basic comparisons

The most basic way to check the differences in two files is by using diff.

`user $``diff one two`
1c1
\< Hello, World!
---
> Goodbye, World!
4c4
\< Another Line
---
> Different Line

What this says is the file on the left (one) has "Hello, World!" while the file on the right (two) has "Goodbye, World".

#### Columns

To show the differences in column format to compare side-by-side, use the `-y` argument.

`user $``diff -y one two`
Hello, World!	| Goodbye, World!
Second Line	  Second Line
Gentoo Linux	  Gentoo Linux
Another Line	| Different Line

### diff3

diff3 compares three different files line-by-line. This will use the file output from the sdiff section in its examples.

#### Comparing three different files

Comparing files with this is similar to using diff:

`user $``diff3 one two output`
====2
1:1c
3:1c
  Hello, World!
2:1c
  Goodbye, World!
====1
1:4c
  Another Line
2:4c
3:4c
  Different Line

### sdiff

sdiff is similar to using diff's `-y` argument to display columns

#### Displaying the difference

`user $``sdiff one two`
Hello, World!	| Goodbye, World!
Second Line	  Second Line
Gentoo Linux	  Gentoo Linux
Another Line	| Different Line

#### Merging the differences

To go line-by-line and pick the differences to include in an output file, use the `-o` argument:

`user $``sdiff one two -o output`
Hello, World!	| Goodbye, World!
%?
ed:	Edit then use both versions, each decorated with a header.
eb:	Edit then use both versions.
el or e1:	Edit then use the left version.
er or e2:	Edit then use the right version.
e:	Discard both versions then edit a new one.
l or 1:	Use the left version.
r or 2:	Use the right version.
s:	Silently include common lines.
v:	Verbosely include common lines.
q:	Quit.
%l
Second Line	  Second Line
Gentoo Linux	  Gentoo Linux
Another Line	| Different Line
%r

This should now be in the output file:

**`output`**
