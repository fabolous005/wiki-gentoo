<!-- source: https://wiki.gentoo.org/wiki/Filesystem_in_Userspace | group: Gentoo Wiki (Main) | wiki-title: Filesystem in Userspace -->
---
title: Filesystem in Userspace
url: https://wiki.gentoo.org/wiki/Filesystem_in_Userspace
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-22"
fingerprint: aa42f51cccc439ed
license: CC BY-SA 4.0
---

# Filesystem in Userspace

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**F**ilesystem in **U**ser**s**pac**e** (FUSE) provides a way for users to mount file systems without needing special permissions (mounting in Linux is generally reserved to those with administrative privileges).

## Installation

### Kernel

KERNEL **Enable support for FUSE**

```
File systems  --->
    <*> FUSE (Filesystem in Userspace) support
```
### USE flags


### USE flags for
            [sys-fs/fuse](https://packages.gentoo.org/packages/sys-fs/fuse)
            
            An interface for filesystems implemented in userspace

| [+suid](https://packages.gentoo.org/useflags/+suid) | Enable setuid root program(s) | 
| [examples](https://packages.gentoo.org/useflags/examples) | Install examples, usually source code | 
| [io-uring](https://packages.gentoo.org/useflags/io-uring) | Enable the use of io\_uring for efficient asynchronous IO and system requests | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [systemtap](https://packages.gentoo.org/useflags/systemtap) | Enable SystemTap/DTrace tracing | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

As with most file systems, after building support for the file system into the kernel be sure to install the user space tools:

`root #``emerge --ask sys-fs/fuse`
## Configuration

### Files

The following configuration files are available for FUSE:

- /etc/fuse.conf

There are two configuration variables available in the fuse.conf file:

- `mount_max` - Sets the maximum number of FUSE mounts allowed to non-root users (defaults to 1000 if unset).
- `user_allow_other` - Allows non-root users to specify the `allow_other` or `allow_root` mount options. This is disabled for security reasons.

## Usage

### Invocation

`user $``fusermount3 -h`
fusermount3: \[options\] mountpoint
Options:
 -h                 print help
 -V                 print version
 -o opt\[,opt...\]   mount options
 -u                 unmount
 -q                 quiet
 -z                 lazy unmount

### Mounting filesystems

Use the fusermount3 command:

`user $``fusermount3 /path/to/mountpoint`
### Unmounting filesystems

Filesystems can be unmounted using either the umount *or* the fusermount3 command:

`user $``fusermount3 -u /path/to/mountpoint`
## Removal

`root #``emerge --ask --depclean --verbose sys-fs/fuse`
## Troubleshooting

### AppImages

You may see an error message like the one below when executing an AppImage.

`user $``./app.appimage`
AppImages require FUSE to run.
You might still be able to extract the contents of this AppImage
if you run it with the --appimage-extract option.
See https://github.com/AppImage/AppImageKit/wiki/FUSE
for more information

[sys-fs/fuse](https://packages.gentoo.org/packages/sys-fs/fuse) provides fusermount3 but older AppImages may need `sys-fs/fuse:0`.

`root #``emerge --ask sys-fs/fuse:0`
## See also

- [Filesystem](https://wiki.gentoo.org/wiki/Filesystem) — a means to organize data to be retained after a program terminates.

## External resources

- [Writing a FUSE Filesystem: a Tutorial](https://www.cs.nmsu.edu/~pfeiffer/fuse-tutorial/)
- [FUSE](https://wiki.archlinux.org/index.php/FUSE) (Arch Linux Wiki)
