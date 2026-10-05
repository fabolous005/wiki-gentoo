<!-- source: https://wiki.gentoo.org/wiki/OverlayFS | group: Gentoo Wiki (Main) | wiki-title: OverlayFS -->
---
title: OverlayFS
url: https://wiki.gentoo.org/wiki/OverlayFS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-15"
fingerprint: "5d61631be1fc5124"
license: CC BY-SA 4.0
---

# OverlayFS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Overlayfs** (**Overlay F**ile**s**ystem) is an in-kernel attempt at providing union file system capabilities on Linux. OverlayFS differs from other union filesystem implementations in that after a file is opened all operations go directly to the underlying, lower or upper, filesystems. This simplifies the implementation and allows native performance in these cases.[\[1\]](https://wiki.gentoo.org#cite_note-1)

The option to enable OverlayFS exists in Linux kernels 3.18 and higher.[\[2\]](https://wiki.gentoo.org#cite_note-2)

## Installation

### Kernel

**Enable OverlayFS (OVERLAY\_FS) support**

## Usage

Once enabled in the kernel OverlayFS can be controlled using the mount command.

`root #``mount -t overlay overlay -o lowerdir=``lowerdir`,upperdir=`upperdir`,workdir=`workdir mountpoint`
## Example

To mount an overlay filesystem using the following example of a structure on an ext4 base filesystem.

Create the following folder structure:

`user $``tree test_folder`
test\_folder
├── low
├── my\_overlay
└── up

On the folder *low*, create a file with a clear and recognizable name. Repeat the step on the folder *up* to get a structure similar to the following:

`user $``tree test_folder````
test_folder
├── low
│   └── low_file
├── my_overlay
└── up
    └── up_file
```
Having that tree, the following command will create an overlay structure with the *up* folder above the *low* folder and that structure will be on the *my\_overlay* folder.

`root #``mount -t overlay overlay -o lowerdir=/test_folder/low,upperdir=/test_folder/up,workdir=/test_folder/my_overlay /test_folder/my_overlay/`
After inspecting the tree structure of the test\_folder, this will be printed:

`user $``tree test_folder````
test_folder
├── low
│   └── low_file
├── my_overlay
│   ├── low_file
│   └── up_file
└── up
    └── up_file
```
A file can be created using the normal filesystem structure, like the following

`root #``touch my_overlay/my_overlay_file`
and will generate the following tree

`user $``tree test_folder````
test_folder
├── low
│   └── low_file
├── my_overlay
│   ├── low_file
│   ├── my_overlay_file
│   └── up_file
└── up
    ├── my_overlay_file
    └── up_file
```
The overlay working dir can be unmounted with the umount command

`root #``umount /test_folder/my_overlay/`
After unmounting the overlay folder, a new subfolder will appear on the directory where the operation was conducted

`user $``tree test_folder````
|
test_folder
├── low
│   └── low_file
├── my_overlay
│   └── work
└── up
    ├── my_overlay_file
    └── up_file
```
That folder will have the following properties

## See also

- [Aufs](https://wiki.gentoo.org/wiki/Aufs) — an advanced multi-layered unification filesystem.
- [SquashFS](https://wiki.gentoo.org/wiki/SquashFS) — an open source, read only, extremely compressible filesystem.
- [Wikipedia:UnionFS](https://en.wikipedia.org/wiki/UnionFS) — The *original* union filesystem.

## External resources

- [A LWN article written by Jonathan Corbet in June 2011 covering vises and virtues of OverlayFS](http://lwn.net/Articles/447650/)
- [An informative AskUbuntu.com thread](http://askubuntu.com/a/109441)
- [Overlay fs in the Linux git repository](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/Documentation/filesystems/overlayfs.rst)
