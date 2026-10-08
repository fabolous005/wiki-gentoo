<!-- source: https://wiki.gentoo.org/wiki/Ostree_rootfs | group: Gentoo Wiki (Main) | wiki-title: Ostree rootfs -->
---
title: Ostree rootfs
url: https://wiki.gentoo.org/wiki/Ostree_rootfs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-07"
fingerprint: "1b99d5eea21fde04"
license: CC BY-SA 4.0
---

# Ostree rootfs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[InfoBox stack template's documentation on the`11` parameter](https://wiki.gentoo.org/wiki/Template:InfoBox_stack/doc).



**ostree rootfs** is installation and management of gentoo installation with the aid of ostree

## Pre-installation filesystem arrangement

### Moving writable directories

Since /usr is versioned and read-only, writable directories and those which do not need versioning need to be moved to somewhere under /var and replaced with a symlink.

`root #``mv /usr/local /var/usrlocal && ln -sf ../var/usrlocal /usr/local`
Similarly /home too needs to be moved to /var/home. If /home is a mountpoint, re-mount it to the new directory.

### Portage state into system

Since system state like packages installed, slot version, etc. needs to be matched with the actual system at all times, this needs to be moved out of /var.

`root #``mv /var/db/pkg /usr/lib/portage/pkgdb && ln -sf ../../usr/lib/portage/pkgdb /var/db/pkg`
And also, world and surrounding files:

`root #``mv /var/lib/portage /usr/lib/portage/var && ln -sf ../../usr/lib/portage/var /var/lib/portage`
## Installation of ostree

The core package is:

`root #``emerge --ask dev-util/ostree`
It's the only tool required.

## Moving existing installation into ostree

The ostree repo:

`root #``ostree init --repo=/ostree/repo`
### Skip list

Certain directories are later managed differently, hence the skip-list:

**`/ostree-skiplist`**

```
/tmp/*
/var/*
/run/*
/dev/*
/sys/*
/proc/*
/ostree
/ostree-skiplist
/boot/*
```
(Yes the skiplist itself too is included)

### Pre-generating initramfs

TODO Initramfs need be in /lib/modules/$KVER/initramfs.img

### Putting files into ostree

(Existing system will remain intact)

`root #``ostree --repo=/osree/repo commit -b "${BRANCH}" -s "${SUBJECT}" -v --bootable --skip-list=/ostree-skiplist /`
`BRANCH` can be anything, like `gentoo/main`, `gentoo/systemd` or `hardened/systemd`. No requirements.

`SUBJECT` is a one-line description of the "commit".

### Deploy the OS (First)

TODO
- (Emits bootloader config)
- `/usr/lib/ostree/ostree-prepare-root` involved
- Repopulate /var after this
- Further modifications all count as updating

## Updating and installing packages

TODO: Updating system through ostree

Some TODO rough ideas beforehand:
- Most of the operations to be done in a separate mountns
- An overlayfs over the latest existing version takes in all changes and new files
- `ostree commit` to be run over that temporary overlayfs
- Once again deploy
- Cleanup
- Need to consider kernel updates, only one kernel at a time



## Caveats

## Tips

## Troubleshooting

### Issue 1

When X happens, Y is how to fix it.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose category/package`
