<!-- source: https://wiki.gentoo.org/wiki/Filesystem | group: Gentoo Wiki (Main) | wiki-title: Filesystem -->
---
title: Filesystem
url: https://wiki.gentoo.org/wiki/Filesystem
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-13"
fingerprint: bf887992f5950d21
license: CC BY-SA 4.0
---

# Filesystem

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**


A **filesystem** is a means to organize data to be retained after a program terminates. Filesystems provide procedures to store, retrieve, and update data, as well as to manage the available space on the device(s) which contain it.

Linux has a few dozen filesystems available, each with their own advantages and disadvantages when considering a particular use case.

## Filesystems

### Flash memory filesystems

The following flash memory filesystems are designed to be used on embedded flash memory known as [MTDs](https://en.wikipedia.org/wiki/Memory_Technology_Device); they are **not** intended to be used for USB based flash drives, SD cards, or other types of *removable* flash block devices.

| Name | Userspace package | Description | 
|---|---|---|
| [JFFS2](https://en.wikipedia.org/wiki/JFFS2) |  | Journalling Flash File System version 2. | 
| [YAFFS](https://en.wikipedia.org/wiki/YAFFS) | [sys-fs/yaffs2utils](https://packages.gentoo.org/packages/sys-fs/yaffs2utils) | Yet Another Flash File System. | 



### Disk filesystems

| Name | Userspace package | Description | 
|---|---|---|
| [bcachefs](https://wiki.gentoo.org/wiki/Bcachefs) | [sys-fs/bcachefs-tools](https://packages.gentoo.org/packages/sys-fs/bcachefs-tools) | A next generation, robust, high performance filesystem that supports native tiering, copy-on-write, compression, and encryption. | 
| [btrfs](https://wiki.gentoo.org/wiki/Btrfs) | [sys-fs/btrfs-progs](https://packages.gentoo.org/packages/sys-fs/btrfs-progs) | A copy-on-write B-tree file system (btrfs) with advanced features. | 
| [Cramfs](https://wiki.gentoo.org/wiki/Cramfs) | [sys-fs/cramfs](https://packages.gentoo.org/packages/sys-fs/cramfs) | A memory and space sensitive compressed filesystem that supports random reading. It avoids the block device layer and usefulness in tiny embedded systems with very tight memory constraints. | 
| [eCryptfs](https://wiki.gentoo.org/wiki/ECryptfs) | [sys-fs/ecryptfs-utils](https://packages.gentoo.org/packages/sys-fs/ecryptfs-utils) | The enterprise cryptographic filesystem for Linux. | 
| [efivarfs](https://wiki.gentoo.org/wiki/Efivarfs) |  | A (U)EFI variable filesystem <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> | 
| [exFAT](https://wiki.gentoo.org/wiki/ExFAT) | [sys-fs/exfatprogs](https://packages.gentoo.org/packages/sys-fs/exfatprogs) | Extensible File Allocation Table (exFAT) filesystem by Microsoft, natively supported since Linux 5.7 <sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> | 
| [ext4](https://wiki.gentoo.org/wiki/Ext4) | [sys-fs/e2fsprogs](https://packages.gentoo.org/packages/sys-fs/e2fsprogs) | The default, GPL licensed journaling filesystem for many Linux distributions. | 
| [F2FS](https://wiki.gentoo.org/wiki/F2FS) | [sys-fs/f2fs-tools](https://packages.gentoo.org/packages/sys-fs/f2fs-tools) | A Flash-Friendly File System (F2FS) created by Samsung for the Linux kernel. | 
| [FAT](https://wiki.gentoo.org/wiki/FAT) | [sys-fs/dosfstools](https://packages.gentoo.org/packages/sys-fs/dosfstools) | The File Allocation Table (FAT) filesystem. Originally created for use with Microsoft Windows. | 
| [GFS2](https://en.wikipedia.org/wiki/GFS2) |  | Global File System 2: A shared disk filesystem. Typically used in compute clusters. | 
| [HFS](https://wiki.gentoo.org/wiki/HFS) | [sys-fs/hfsutils](https://packages.gentoo.org/packages/sys-fs/hfsutils) | Hierarchical File System (HFS). Originally created for use with the Macintosh System Software, later renamed to Mac OS (Classic). | 
| [HFS+](https://wiki.gentoo.org/wiki/HFS%2B) | [sys-fs/hfsplusutils](https://packages.gentoo.org/packages/sys-fs/hfsplusutils) | The successor to HFS, introduced in Mac OS 8.1 and default filesystem for Mac OS X until macOS 10.12 Sierra. | 
| [JFS](https://wiki.gentoo.org/wiki/JFS) | [sys-fs/jfsutils](https://packages.gentoo.org/packages/sys-fs/jfsutils) | A GPL licensed, 64-bit Journaled File System (JFS) developed by IBM. <sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> | 
| [NILFS](https://wiki.gentoo.org/wiki/NILFS) | [sys-fs/nilfs-utils](https://packages.gentoo.org/packages/sys-fs/nilfs-utils) | A log-structured file system implementation for the Linux kernel. | 
| [NTFS](https://wiki.gentoo.org/wiki/NTFS) |  | Microsoft Windows' New Technology File System (NTFS) (Windows' default filesystem). | 
| [OCFS2](https://en.wikipedia.org/wiki/OCFS2) |  | Oracle Cluster File System version 2. | 
| [OverlayFS](https://wiki.gentoo.org/wiki/OverlayFS) |  | The only union-like filesystem built-in to the Linux kernel. | 
| [ReiserFS](https://wiki.gentoo.org/wiki/ReiserFS) | [sys-fs/reiserfsprogs](https://packages.gentoo.org/packages/sys-fs/reiserfsprogs) | Obsolete filesystem, removed from the kernel in 2025. | 
| [SquashFS](https://wiki.gentoo.org/wiki/SquashFS) | [sys-fs/squashfs-tools](https://packages.gentoo.org/packages/sys-fs/squashfs-tools), [sys-fs/squashfs-tools-ng](https://packages.gentoo.org/packages/sys-fs/squashfs-tools-ng) | A compressed, read-only file system for Linux <sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup> | 
| [UDF](https://en.wikipedia.org/wiki/Universal_Disk_Format) | [sys-fs/udftools](https://packages.gentoo.org/packages/sys-fs/udftools) | Universal Disk Format - needed for mounting some kind of .iso files | 
| [UFS](https://en.wikipedia.org/wiki/Unix_File_System) |  | The Unix File System (UFS) also called the Berkeley Fast File System. | 
| [XFS](https://wiki.gentoo.org/wiki/XFS) | [sys-fs/xfsprogs](https://packages.gentoo.org/packages/sys-fs/xfsprogs) | A GPL licensed, 64-bit journaling filesystem created by Silicon Graphics. <sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup> | 
| [ZFS](https://wiki.gentoo.org/wiki/ZFS) | [sys-fs/zfs](https://packages.gentoo.org/packages/sys-fs/zfs) | A CDDL (non-GPL compatible) licensed, copy-on-write filesystem created by Sun Microsystems <sup>[\[6\]](https://wiki.gentoo.org#cite_note-6)</sup>. | 

### Virtual filesystems

*Virtual* filesystems, also called *pseudo* filesystems, are for storing temporary data in memory while the system is running.

| Name | Userspace package | Description | 
|---|---|---|
| [debugfs](https://en.wikipedia.org/wiki/Debugfs) |  | Used for debugging purposes; primarily Linux kernel development. | 
| [procfs](https://wiki.gentoo.org/wiki/Procfs) |  | Used to output and change of system and process information. | 
| securityfs |  | Used by the TPM BIOS character driver, [AppArmor](https://wiki.gentoo.org/wiki/AppArmor) and IMA, an integrity provider.<sup>[\[7\]](https://wiki.gentoo.org#cite_note-7)</sup> | 
| [sysfs](https://wiki.gentoo.org/wiki/Sysfs) |  | Used to output information about and to configure devices and drivers. | 
| [tmpfs](https://wiki.gentoo.org/wiki/Tmpfs) |  | Used to store files in memory (RAM). | 
| devtmpfs |  | udev requires [devtmpfs](https://wiki.gentoo.org/wiki/Udev#Kernel) (Maintain a devtmpfs filesystem to mount at [/dev](https://wiki.gentoo.org/wiki//dev)) in the kernel. | 

### Network filesystems

| Name | Userspace package | Description | 
|---|---|---|
| [Ceph](https://wiki.gentoo.org/wiki/Ceph) | [sys-cluster/ceph](https://packages.gentoo.org/packages/sys-cluster/ceph) | A distributed object store and filesystem designed to provide excellent performance, reliability, and scalability. | 
| [GlusterFS](https://en.wikipedia.org/wiki/Gluster#GlusterFS) | [sys-cluster/glusterfs](https://packages.gentoo.org/packages/sys-cluster/glusterfs) | A powerful network/cluster filesystem. | 
| [NFS](https://wiki.gentoo.org/wiki/Nfs-utils) | [net-fs/nfs-utils](https://packages.gentoo.org/packages/net-fs/nfs-utils) | A common Linux network file system protocol. | 
| [Samba](https://wiki.gentoo.org/wiki/Samba) | [net-fs/samba](https://packages.gentoo.org/packages/net-fs/samba) | A re-implementation of the SMB/CIFS networking protocol. | 

### FUSE-based filesystems

| Name | Userspace package | Description | 
|---|---|---|
| [CurlFtpFS](https://wiki.gentoo.org/wiki/CurlFtpFS) | [net-fs/curlftpfs](https://packages.gentoo.org/packages/net-fs/curlftpfs) | File system for accessing FTP hosts based on FUSE. | 
| FuseISO | [sys-fs/fuseiso](https://packages.gentoo.org/packages/sys-fs/fuseiso) | FUSE module to mount ISO filesystem images. | 
| [MTPfs](https://wiki.gentoo.org/wiki/MTPfs) | [sys-fs/mtpfs](https://packages.gentoo.org/packages/sys-fs/mtpfs) | A FUSE filesystem providing access to Media Transfer Protocol (MTP) devices. | 
| [smbnetfs](https://wiki.gentoo.org/wiki/Smbnetfs) | [net-fs/smbnetfs](https://packages.gentoo.org/packages/net-fs/smbnetfs) | A FUSE filesystem for SMB shares. | 
| [SSHFS](https://wiki.gentoo.org/wiki/SSHFS) | [net-fs/sshfs](https://packages.gentoo.org/packages/net-fs/sshfs) | Implements FUSE to mount filesystems in user space. | 
| squashfuse | [sys-fs/squashfuse](https://packages.gentoo.org/packages/sys-fs/squashfuse) | Mount SquashFS archives using FUSE. | 

## Usage

### Mounting

Filesystems can be mounted in several ways:

- [mount](https://wiki.gentoo.org/wiki/Mount) - The command used to mount filesystems. Requires administrative privileges or entries in /etc/fstab.
- [/etc/fstab](https://wiki.gentoo.org/wiki//etc/fstab) - Contains descriptive information about the filesystems the system can mount.
- [Removable media](https://wiki.gentoo.org/wiki/Removable_media) - Mount on file demand.
- [Udevil](https://wiki.gentoo.org/wiki/Udevil) - A small auto-mount utility with little dependencies.
- [AutoFS](https://wiki.gentoo.org/wiki/AutoFS) - Automatic mount on file access.

## See also

- [Filesystem/Access Control List Guide](https://wiki.gentoo.org/wiki/Filesystem/Access_Control_List_Guide) — an additional security control feature for multiuser systems.
- [Filesystem/Security](https://wiki.gentoo.org/wiki/Filesystem/Security) — one of the basic means to harden a system.
- [Bcache](https://wiki.gentoo.org/wiki/Bcache) — a Linux [kernel](https://wiki.gentoo.org/wiki/Kernel) block layer cache.
- [Filesystem security](https://wiki.gentoo.org/wiki/Filesystem/Security) — one of the basic means to harden a system.
- [Filesystem in Userspace](https://wiki.gentoo.org/wiki/Filesystem_in_Userspace) — a way for users to mount file systems without needing special permissions
- [Filesystems in Handbook AMD64](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks#Filesystems)

## External resources

- [Linux Sea, by Sven Vermeulen, chapter about filesystems](http://swift.siphos.be/linux_sea/linuxfs.html#filesystems)
- [Bitrot and atomic COWs: Inside “next-gen” filesystems](https://arstechnica.com/information-technology/2014/01/bitrot-and-atomic-cows-inside-next-gen-filesystems/) (Ars Technica)
- [A Study of Linux File System Evolution](https://www.usenix.org/system/files/login/articles/03_lu_010-017_final.pdf) (PDF document from USENIX)
