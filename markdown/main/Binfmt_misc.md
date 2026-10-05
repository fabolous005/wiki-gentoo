<!-- source: https://wiki.gentoo.org/wiki/Binfmt_misc | group: Gentoo Wiki (Main) | wiki-title: Binfmt misc -->
---
title: Binfmt misc
url: https://wiki.gentoo.org/wiki/Binfmt_misc
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-13"
fingerprint: ab612054a552fed6
license: CC BY-SA 4.0
---

# Binfmt misc

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Kernel Support for miscellaneous Binary Formats (**binfmt\_misc**) allows to invoke almost every program by simply typing its name in the shell.

## Installation

### Kernel

Enable miscellaneous binary formats with `CONFIG_BINFMT_MISC=m` or `CONFIG_BINFMT_MISC=y` in the kernel's .config file.

**Enable CONFIG\_BINFMT\_MISC**

## Configuration

### Binary format handlers

Binary format handlers look rather confusing at first, but they can be simply broken down as: `:name:type:offset:magic:mask:interpreter:flags`

With each field representing, in order of appearance:

- `name` being the name of the binary format, a new `/proc` file will be created with this name at /proc/sys/fs/binfmt\_misc
- `type` being either `E` or `M`
  - `E` means the executable file format is identified by its file extension
  - `M` means the format is identified by the `magic` number and `mask`
- `offset` represents the offset of the magic/mask in the file in bytes, the default is 0
- `magic` is a byte sequence that binfmt matches for to know what files to pass through to the interpreter
- `mask` specifies which *bits* of `mask` must match, and which are ignored
- `interpreter` is a program that is to be run with the matching file as an argument
- `flags` is a field that controls different aspects of interpreter when it is invoked
  - `P` - preserve-argv\[0\]: adds an argument to the argument vector to preserve the original argv\[0\]
  - `O` - open-binary: opens the file for reading and pass its descriptor as an argument rather than the full path
  - `C` - credentials: by default, binfmt\_misc will determine the credentials and security of the new process according to the interpreter. Using this flag will be determined according to the binary. (Implies the `O` flag)
  - `F` - fix binary: currently, the binary is spawned lazily when the misc format file is invoked, however, this does not work very well in the face of mount namespaces and changroots so `F` allows the binary to be opened always once it is installed, regardless of environment changes.

Some restrictions apply with binfmt:

- The whole register string may not exceed 1920 characters
- The magic must reside in the first 128 bytes of the file
- The interpreter string may not exceed 127 characters

QEMU ships with

- /usr/share/qemu/binfmt.d/qemu.conf a list of handers and
- /etc/init.d/qemu-binfmt an init-script that shows how to register them.

The scope of binfmt\_misc is not limited to Linux binaries. Other file types can be registered too, e.g.:

- DOS/Windows PE `:DOSWin:M::MZ::/etc/eselect/wine/bin/wine:`
- .Net `:CLR:M::MZ::/usr/bin/mono:`

## Usage

You can control binfmt\_misc through these files:

- /proc/sys/fs/binfmt\_misc/status: disable/enable/remove binfmt\_misc
- /proc/sys/fs/binfmt\_misc/register: register an interpreter
- /proc/sys/fs/binfmt\_misc/${NAME}: disable/enable/remove one interpreter

You can enable/disable binfmt\_misc or one binary type by echoing

- `0` (to disable)
- `1` (to enable)
- `-1` (to remove)

to

- /proc/sys/fs/binfmt\_misc/status for all interpreters
- /proc/sys/fs/binfmt\_misc/${NAME} for one binary type

Catting these files tells you the current status.

### Register an interpreter

First, mount the **binfmt\_misc** handler if it is not already mounted, then register the format with the kernel via the [procfs](https://wiki.gentoo.org/wiki/Procfs):

`root #````
[ -d /proc/sys/fs/binfmt_misc ] || modprobe binfmt_misc
```
`root #````
[ -f /proc/sys/fs/binfmt_misc/register ] || mount binfmt_misc -t binfmt_misc /proc/sys/fs/binfmt_misc
```
#### Manually

To register a handler, pipe its format to /proc/sys/fs/binfmt\_misc/register, for example:

`root #``echo ':arm:M::\x7fELF\x01\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x02\x00\x28\x00:\xff\xff\xff\xff\xff\xff\xff\x00\xff\xff\xff\xff\xff\xff\xff\xff\xfe\xff\xff\xff:/usr/bin/qemu-arm:' > /proc/sys/fs/binfmt_misc/register`
#### Automatic

Various interpreters ship with init scripts
