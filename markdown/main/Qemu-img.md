<!-- source: https://wiki.gentoo.org/wiki/Qemu-img | group: Gentoo Wiki (Main) | wiki-title: Qemu-img -->
---
title: qemu-img
url: https://wiki.gentoo.org/wiki/Qemu-img
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-02"
fingerprint: "5170f751cf08982"
license: CC BY-SA 4.0
---

# qemu-img

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

qemu-img is a QEMU disk image utility

The program is used to create, inspect, convert, resize and check disk image.

## Introduction

qemu-img creates, maintains, appends, and converts a block disk image for use with most any virtual machines.



### Disk image formats

Storages format (-f \<format-type>) options are:

| Format | Description | 
|---|---|
| bochs | Bochs disk image format. | 
| cloop | Linux compressed loop image format. | 
| dmg | Apple Disk Image format. | 
| parallels | Parallels disk image format. | 
| qcow | QEMU copy-on-write disk image format. | 
| qcow2 | QEMU copy-on-write disk image format, version 2. | 
| qed | QEMU Enhanced Disk image format. Obsolete. | 
| raw | Raw disk image format without image metadata. | 
| vdi | VirtualBox Disk Image format. | 
| vhdx | Microsoft Hyper-V Virtual Hard Disk format. | 
| vmdk | VMware Virtual Machine Disk format. | 
| vpc | Microsoft Virtual PC Virtual Hard Disk format. | 
| vvfat | Virtual VFAT filesystem image. | 

### Network and storage protocols

| Protocol | Description | 
|---|---|
| ftp | Accesses disk images over FTP. | 
| ftps | Accesses disk images over FTP with TLS. | 
| gluster | Accesses disk images stored on GlusterFS. | 
| http | Accesses disk images over HTTP. | 
| https | Accesses disk images over HTTPS. | 
| iscsi | Accesses remote SCSI devices over iSCSI. | 
| iser | Accesses iSCSI devices using iSER. | 
| nbd | Accesses images exported using the Network Block Device protocol. | 
| nfs | Accesses disk images stored on NFS. | 
| rbd | Accesses Ceph RADOS Block Device images. | 
| ssh | Accesses disk images on a remote SSH server. | 

### Host and local storage

| Driver | Description | 
|---|---|
| file | Accesses a local file as a block device. | 
| host\_cdrom | Accesses a host CD-ROM device. | 
| host\_device | Accesses a host block device. | 
| nvme | Accesses NVMe devices directly. | 

### Filters

| Filter | Description | 
|---|---|
| compress | Compresses data written through the block layer. | 
| copy-before-write | Copies data before it is overwritten. | 
| copy-on-read | Copies data from a backing image when it is read. | 
| null-aio | Discards block-device I/O using asynchronous I/O. | 
| null-co | Discards block-device I/O using coroutines. | 
| preallocate | Preallocates storage when extending a disk image. | 
| quorum | Provides redundant access to multiple block devices. | 
| snapshot-access | Provides access to an internal snapshot. | 
| throttle | Limits block-device I/O. | 

### Pseudo-formats

| Format | Description | 
|---|---|
| blkdebug | Injects errors into block-device operations for testing. | 
| blklogwrites | Logs block-device write operations. | 
| blkverify | Verifies block-device operations against a reference image. | 
| luks | Accesses LUKS-encrypted block devices. | 
| replication | Provides block-device replication support. | 

## Installation

See [QEMU Installation](https://wiki.gentoo.org/wiki/QEMU#Installation).

### USE flags

See [QEMU](https://wiki.gentoo.org/wiki/QEMU#USE_flags) for available USE flags.

### Emerge

`root #``emerge --ask app-emulation/qemu`
### Additional software

qemu-img is included with [QEMU](https://wiki.gentoo.org/wiki/QEMU) and does not require a separate
package.

## Configuration

qemu-img configuration is specified through command-line options, image-format options, and environment variables.

### Environment variables

A list of all environment variables that are read and checked by the qemu-img command:



| Variable | Description | Default | 
|---|---|---|
| `SDL_VIDEODRIVER` | Selects the SDL video driver, such as `x11` or `sdl`, when it cannot be determined automatically. | (not set) | 
| `LISTEN_FDNAMES` | Passes socket name descriptors for systemd socket activation. | (not set) | 
| `LISTEN_FDS` | Passes file descriptors (fds) for systemd socket activation. | Recommend (not set) for default fds 0. | 
| `LISTEN_PID` | Passes the process ID (PID) for systemd socket activation. | (not set) | 
| `QEMU_MODULE_DIR` | Specifies the directory to search for [QEMU](https://wiki.gentoo.org/wiki/QEMU) modules before the default module directory. | (not set) | 
| `QEMU_AUDIO_DRV` | Selects the QEMU audio driver. Supported values include `none`, `alsa`, `pa`, `sdl`, `oss`, `jack`, `spice`, and `wav`. | none | 
| `QEMU_PATH` | Specifies the path used by QEMU to locate resources. | (not set) | 
| `G_MESSAGES_DEBUG` | Enables additional diagnostic messages from the GLib library used by [GNOME](https://wiki.gentoo.org/wiki/GNOME). | (not set) | 
| `QEMU_STRACE` | Enables QEMU's system-call tracing facility; primarily applicable to BSD. | Disabled | 
| `XDG_RUNTIME_DIR` | Specifies the directory for user-specific runtime files and sockets, such as those used by the [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio) daemon. | (not set) | 

### Files

qemu-img does not use a configuration file such as /etc/qemu-img.cfg or \~/.qemu-img.

Files that may be read by the host operating system or accessed by qemu-img include:

| Path | Description | 
|---|---|
| /dev/cdrom | Host CD-ROM device. | 
| /dev/fdset | Set of file-descriptors used by QEMU. | 
| /dev/null | Null device. | 
| /dev/vfio/\<device-name> | VFIO device interface. | 
| /proc/\<PID>/cmdline | Command line of the specified process. | 
| /proc/self/exe | Executable associated with the current process. | 
| /proc/self/fd | File descriptors associated with the current process. | 
| /proc/sys/vm/overcommit\_memory | Linux kernel virtual-memory overcommit setting. | 
| /sys/bus/pci/devices/%s/iommu\_group | IOMMU group associated with a PCI device. | 

Files that may be created or written by qemu-img include:

| Path | Description | 
|---|---|
| /var/run/qemu/qemu-socket-\<TMPID> | QEMU socket created for communication with a QEMU block device. | 
| /var/tmp/ | Temporary files created during operation. | 

### User permissions

To use qemu-img as a non-root user, ensure each user has been added to the qemu group:

`root #``gpasswd -a <user> qemu`
See [qemu-img configuration](https://wiki.gentoo.org/wiki/QEMU#Configuration) for more setup on enabling user to use the qemu-img command.

## Usage

### Invocation

The qemu-img can be checked by running:

`user $``qemu-img --help````
qemu-img version 10.0.11 (Debian 1:10.0.11+ds-0+deb13u1)
Copyright (c) 2003-2025 Fabrice Bellard and the QEMU Project developers
QEMU disk image utility.  Usage:
  qemu-img [standard options] COMMAND [--help | command options]
Standard options:
  -h, --help
     display this help and exit
  -V, --version
     display version info and exit
  -T,--trace TRACE
     specify tracing options:
        [[enable=]<pattern>][,events=<file>][,file=<file>]
Recognized commands (run qemu-img COMMAND --help for command-specific help):
  amend - Update format-specific options of the image
  bench - Run simple image benchmark
  bitmap - Perform modifications of the persistent bitmap in the image
  check - Check basic image integrity
  commit - Commit image to its backing file
  compare - Check if two images have the same contents
  convert - Copy one image to another with optional format conversion
  create - Create and format new image file
  dd - Copy input to output with optional format conversion
  info - Display information about image
  map - Dump image metadata
  measure - Calculate file size requred for a new image
  rebase - Change backing file of the image
  resize - Resize the image to the new size
  snapshot - List or manipulate snapshots within image
Supported image formats:
  blkdebug blklogwrites blkverify bochs cloop compress copy-before-write
  copy-on-read dmg file ftp ftps gluster host_cdrom host_device http https
  io_uring iscsi iser luks nbd nfs null-aio null-co nvme nvme-io_uring
  parallels preallocate qcow qcow2 qed quorum raw rbd replication
  snapshot-access ssh throttle vdi vhdx virtio-blk-vfio-pci
  virtio-blk-vhost-user virtio-blk-vhost-vdpa vmdk vpc vvfat
See <https://qemu.org/contribute/report-a-bug> for how to report bugs.
More information on the QEMU project at <https://qemu.org>.
```
### Inspect an image

Display information about a disk image:

`root #``qemu-img info /dev/sda````
image: /dev/sda
file format: raw
virtual size: 466 GiB (500107862016 bytes)
disk size: 0 B
Child node '/file':
    filename: /dev/sda
    protocol type: host_device
    file length: 466 GiB (500107862016 bytes)
    disk size: 0 B
```
### Create an image

Create a 40 GiB qcow2 disk image:

`root #``qemu-img create -f qcow2 disk.qcow2 40G`
Formatting 'disk.qcow2', fmt=qcow2 cluster\_size=65536 extended\_l2=off compression\_type=zlib size=42949672960 lazy\_refcounts=off refcount\_bits=16

### Convert an image

Convert a raw disk image to qcow2:

`root #``qemu-img convert -f raw -O qcow2 disk.img disk.qcow2`
Convert a VMware disk image to qcow2:

`root #``qemu-img convert -f vmdk -O qcow2 vmware-disk.vmdk vmware-disk.qcow2`
### Resize an image

Increase an existing disk image by 10 GiB:

`root #``qemu-img resize disk.qcow2 +10G`


### Check an image

Check a qcow2 image for consistency:

`root #``qemu-img check disk.qcow2`
0 errors were found on the image.
326615/409600 = 79.74% allocated, 23.50% fragmented, 0.00% compressed clusters
Image end offset: 24816123904



### Compare images

Compare two disk images:

`root #``qemu-img compare disk1.qcow2 disk2.qcow2`


### Create a snapshot

qemu-img snapshot manages internal snapshots stored within a disk image.

Create an internal snapshot:

`root #``qemu-img snapshot -c snapshot-name disk.qcow2`

List internal snapshots:

`root #``qemu-img snapshot -l disk.qcow2`
Apply an internal snapshot:

`root #``qemu-img snapshot -a snapshot-name disk.qcow2`
Delete an internal snapshot:

`root #``qemu-img snapshot -d snapshot-name disk.qcow2`
## Caveats

### Disk image in use

qemu-img must not be used to modify a disk image that is in use by a running virtual machine or another process. Doing so may corrupt the disk image.

### Resizing

Before shrinking a disk image, reduce the filesystem and partition sizes from within the guest. Failure to do so may result in data loss.

### Backing files

An image using a backing file depends on the backing file remaining unchanged. An incorrect backing file or backing format may result in corrupted guest data.

### Image format

Image format features vary between formats. Features such as snapshots, compression, encryption, and backing files are not supported by every image format.

### Untrusted images

Disk images are untrusted input. Consider the security implications before using qemu-img with an untrusted disk image.

## Removal

Since qemu-img is part of [app-emulation/qemu](https://packages.gentoo.org/packages/app-emulation/qemu) removal is done by removing the qemu package (toolkit, library, and utilities):

`root #``emerge --ask --depclean --verbose app-emulation/qemu`
## See also

- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
- [QEMU/Front-ends](https://wiki.gentoo.org/wiki/QEMU/Front-ends) — provide graphical, terminal, web-based, or command-line interfaces for configuring, managing, or accessing QEMU virtual machines.
