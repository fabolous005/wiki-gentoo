<!-- source: https://wiki.gentoo.org/wiki/CD/DVD/BD_writing | group: Gentoo Wiki (Main) | wiki-title: CD/DVD/BD writing -->
---
title: CD/DVD/BD writing
url: https://wiki.gentoo.org/wiki/CD/DVD/BD_writing
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-21"
fingerprint: ac851dbc22a49a9d
license: CC BY-SA 4.0
---

# CD/DVD/BD writing

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Article status**

- Describe how to write UDF images to CDs

This article describes how to **burn optical disks** on Gentoo from the [command line](https://wiki.gentoo.org/wiki/Terminal_emulator) with the [app-cdr/cdrtools](https://packages.gentoo.org/packages/app-cdr/cdrtools) or [app-cdr/dvd+rw-tools](https://packages.gentoo.org/packages/app-cdr/dvd+rw-tools) packages.

## Installation

## Kernel

Configure the kernel to support the filesystems necessary for reading and writing ISO disks.

**Enable ISO 9660 and UDF filesystem support**

```
File systems  --->
   CD-ROM/DVD Filesystems  --->
      <*> ISO 9660 CDROM file system support
      [*]   Microsoft Joliet CDROM extensions
      [*]   Transparent decompression extension
      <*> UDF file system support
```
### Emerge

Follow the [CDROM](https://wiki.gentoo.org/wiki/CDROM) page for hardware driver kernel configuration, along with including UDF write support.

Install the [app-cdr/cdrtools](https://packages.gentoo.org/packages/app-cdr/cdrtools) or [app-cdr/dvd+rw-tools](https://packages.gentoo.org/packages/app-cdr/dvd+rw-tools) packages, for writing CD/DVD/BD media:

`root #``emerge --ask app-cdr/cdrtools`
Or:

`root #``emerge --ask app-cdr/dvd+rw-tools`
For UDF writing, ensure included the above mentioned UDF kernel drivers and the following package:

`root #``emerge --ask sys-fs/udftools`
Best practice is to use read write (RW/RE) media for testing writing ISO9660/UDF [filesystem](https://wiki.gentoo.org/wiki/Filesystem) images.  If a command fails to work, or the hardware or media fails, you can try again.

## Usage

Usage for the ISO9660/UDF filesystem.

### Determine the size of media

First, find the maximum size the media can contain.

`user $``dvd+rw-mediainfo /dev/sr0`
Track Size:            24438784\*2KB

Or 24438784\*2KB = 48877568 KB for 50GB BD-R DL (Blu-ray dual layer) media.

`user $``truncate --size=48877568KB test.udf`
Or you can use the following with disabling defect management:

`user $``truncate --size=50GB ./test.udf`
### Create and populate filesystem

Create either a ISO9660 or a UDF filesystem. Microsoft Windows uses lvid for optical media title:

`user $``mkudffs --lvid="MY_VOLUME" --utf8 ./test.udf`
Mount the filesystem:

`user $``sudo mount -oloop,rw ./test.udf /mnt/tmp/`
Populate filesystem:

`user $``rsync -ax --delete /home/larry/Documents/ /mnt/tmp/`
Verify proper permissions are preserved:

`user $````
chown -R larry.larry /mnt/tmp
```
`user $````
chmod -R a+r /mnt/tmp
```
`user $````
chmod -R go-w /mnt/tmp
```
### Writing

#### CD-RW media

CD-RW media requires the packet device driver and starting the /etc/init.d/pktcdvd service and the following line within [/etc/fstab](https://wiki.gentoo.org/wiki//etc/fstab):

**`/etc/fstab`**

```
/dev/pktcdvd/0  /mnt/udfwrite  udf             user,noauto,noatime,utf8  0 0
```
#### DVD/Blu-ray (RW/RE) media

DVD-RW, DVD+RW, DVD-RAM, and Blu-ray Recordable Erasable (BD-RE) media can be easily written by simply mounting the media and writing to the media as a normal filesystem, as these devices and media allow random writing, versus CD-RW only allowing sequential writing.

The following simply automates writing a ISO9660 by piping to mkisofs, then writing.

`user $` `growisofs -dvd-compat -rock -V "TITLE" ./some/files /dev/sr0`
The commands mkisofs/growisofs provide the "-udf" option for writing a bridged/hybrid ISO9660/UDF filesystem. This option may waste disc space, upwards of 483,328 bytes or sectors 20-256. See mkisofs "-UDF" option.

Verify write session:

`user $` `diff -urN ./some/files /mnt/dvd/`
### Image writing

#### CD writing

##### ISO

`user $````
cdrecord -scanbus
```
`user $````
cdrecord -speed=40 dev=2,0,0 -eject -dao driveropts=burnfree test.iso
```
#### DVD/Blu-ray

##### BD defect management

By default growisofs uses defect management which requires 256MB extra space and incurs a penalty of \~50% reduced write speeds. This may be disabled via:

`root #``dvd+rw-format /dev/dvd -ssa=none`
Disabling comes with the tradeoff of making the disk more prone to failed burns, if used burns should be done at slowest supported speed and with new and high-quality disks. This option may be preferable for archivists used to generating their own parity data.

##### ISO

`user $``growisofs -Z /dev/sr0=test.iso`
##### UDF

##### UDF Direct Writing

For writing, modifying, removing files to/from UDF filesystem mounted DVD and Bluray media, users only require the common cp and rm tools.

Linux kernel requirements, compile without pktcdvd or blacklist the module for avoiding conflicts, include the usual SCSI related drivers for the optical drive and enable the UDF filesystem driver. Verify and/or recompile, reboot as needed.

First, insert a blank DVD/BD rewritable disc:

`root #``dvd+rw-format /dev/sr0``root #``mkudffs -l video /dev/sr0`
Then, mount the filesystem:

`root #``mount -t udf -o rw,noatime /dev/sr0 /mnt/dvd`
From here, the UDF filesystem mounted disc can be used as a normal writable mount.

Finally, verify any write operations:

`root #``diff -urN /your/file /mnt/dvd/your/file` If needed, monitor /var/log/messages for mounting/write operations.

If drive operations are excessively slow or delayed, use "dvd+rw-format -lead-out" for improving disc compatibility. Read dvd+rw-format author/maintainer's website notes. Also if possible, specifying the booktype via dvd+rw-booktype may help but is untested here as of writing this.

##### UDF Image Writing

An alternative command to write a UDF filesystem to the disk where the user is certain it will fit after factoring in some overhead for Defect Management (typically 24GB or less for single-layer):

`user $``growisofs -dvd-compat -Z /dev/sr0=test.udf`
If the above truncate command is used with 25GB/50GB specified for BD media, it is required to disable Defect Management to fit the image (else the burn will fail):

`user $``growisofs -use-the-force-luke=spare:none -dvd-compat -speed=4 -Z /dev/sr0=test.udf`
## See also

- [Blu-ray](https://wiki.gentoo.org/wiki/Blu-ray) — **Blu-ray** is the optical media successor to DVD
- [CDROM](https://wiki.gentoo.org/wiki/CDROM) — describes the setup of an internal optical drive like CD, DVD, and Blu-Ray drives
- [FAQ - How do I burn an ISO file?](https://wiki.gentoo.org/wiki/FAQ#How_do_I_burn_an_ISO_file.3F)
- [LiveUSB](https://wiki.gentoo.org/wiki/LiveUSB) — explains how to create a *Gentoo LiveUSB* or, in other words, how to emulate a **x86** or **amd64** Gentoo LiveCD using a USB drive.
- [Recommended GUI burners](https://wiki.gentoo.org/wiki/Recommended_applications#Optical_disk_burners)
