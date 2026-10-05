<!-- source: https://wiki.gentoo.org/wiki/Virtiofs | group: Gentoo Wiki (Main) | wiki-title: Virtiofs -->
---
title: Virtiofs
url: https://wiki.gentoo.org/wiki/Virtiofs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-25"
fingerprint: "5e9f5919ef925be8"
license: CC BY-SA 4.0
---

# Virtiofs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

virtiofs is a shared file system that lets virtual machines access a directory tree on the host.

## Installation

### USE flags


### Emerge

`root #``emerge --ask app-emulation/virtiofsd`
## Usage

Start virtiofsd with the relevant shared directory, and a path to be used as a socket:

`user $``SHARED_DIR=/home/user/share/``user $``SOCKET_PATH=/tmp/vhostqemu``user $``/usr/libexec/virtiofsd --shared-dir "${SHARED_DIR}" --socket-path "${SOCKET_PATH}" --cache auto`
### QEMU VMs

The guest VM needs to be running the Linux kernel, version 5.4 or later, with the `CONFIG_VIRTIO_FS` kernel configuration option enabled.

Start [QEMU](https://wiki.gentoo.org/wiki/QEMU) with your preferred options, plus the following:

```
-chardev socket,id=char0,path="${SOCKET_PATH}"
-device vhost-user-fs-pci,queue-size=1024,chardev=char0,tag=VHOST_TAG
-object memory-backend-file,id=mem,size=MEM_SIZE,mem-path=/dev/shm,share=on -numa node,memdev=mem
```
where `VHOST_TAG` is an arbitrary name (e.g. `myfs`) that will be used with a [mount(8)](https://man.archlinux.org/man/mount.8.en) [command inside the guest, and](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) `MEM_SIZE` is the memory size you specified with the `-m` option.

Once logged in on the guest as root, mount the virtiofs using the VHOST\_TAG specified when starting QEMU:

`root #``mount -t virtiofs myfs /mnt`
The contents of `SHARED_DIR` should now be available in the /mnt directory.
