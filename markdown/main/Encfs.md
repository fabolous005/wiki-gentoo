<!-- source: https://wiki.gentoo.org/wiki/Encfs | group: Gentoo Wiki (Main) | wiki-title: Encfs -->
---
title: EncFS
url: https://wiki.gentoo.org/wiki/Encfs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2016-02-01"
fingerprint: f7d7968977c53acc
license: CC BY-SA 4.0
---

# EncFS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**EncFS** (**Enc**rypted **F**ile **S**ystem) is a userspace encryption method that creates an directory containing encrypted files with hashed filenames.

## Installation

### Kernel

**Enable FUSE**

### USE flags

*sys-fs/encfs*correct?

### Emerge

Install [sys-fs/encfs](https://packages.gentoo.org/packages/sys-fs/encfs):

`root #``emerge --ask sys-fs/encfs`
## Configuration

### Deployment

EncFS makes a hidden directory to store encrypted data then uses FUSE to mount a decrypted directory.

`user $``encfs ~/.encfs ~/encfs`
After running the command setup will ask if a .encfs directory should be made, press the `Y` key then press `Enter`. Another question will come up regarding setting up the encryption scheme, etc. Press `Enter` to accept defaults. Enter the password to the prompt twice to finish the set up process.

Once finished encrypted files can be fed to \~/encfs

### Locking

The fusermount command can be used to unmount FUSE file systems. To unmount the FUSE decrypted directory run:

`user $``fusermount -u ~/encfs`
Remember that the files located in \~/encfs will not be available while it is unmounted.

### Unlocking

To mount the decrypted FUSE directory use the encfs command:

`user $``encfs ~/.encfs ~/encfs`
Running this command will make encrypted files located in \~/.encfs available in the \~/encfs directory.

## Usage

### Examples

Running a simple ls on the \~/encfs directory while the decrypted FUSE file system is mounted will display all the files that were fed into the EncFS storage area:

`user $``ls ~/encfs`
foo bar

Running ls on the \~/.encfs directory will output hash encrypted filenames:

`user $``ls ~/.encfs`
OtyK9QEW3,1a1qnC5Um5tc-S  s9CN,iacV1bM5SxN1TOI8JB2

## External resources

[https://wiki.archlinux.org/index.php/EncFS](https://wiki.archlinux.org/index.php/EncFS) - EncFS on the Arch wiki.
