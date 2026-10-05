<!-- source: https://wiki.gentoo.org/wiki/Ddrescue | group: Gentoo Wiki (Main) | wiki-title: Ddrescue -->
---
title: ddrescue
url: https://wiki.gentoo.org/wiki/Ddrescue
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-04-29"
fingerprint: a4002e870d70c681
license: CC BY-SA 4.0
---

# ddrescue

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


ddrescue is a tool provided by GNU to retrieve data from failing (block) storage devices like disk drives, CDROMs, or memory sticks, etc. It uses a similar technique as [dd](https://wiki.gentoo.org/wiki/Dd) and copies block by block, but has an intelligent algorithm to recover failed data.

## Installation

### Emerge

Install [sys-fs/ddrescue](https://packages.gentoo.org/packages/sys-fs/ddrescue) using the following command:

`root #``emerge --ask sys-fs/ddrescue`
## Usage

The general purpose of ddrescue is to recover data from failing block devices. It can be also used as a general disk (or even file) copying tool with quite a good efficiency and a nice progress bar.

### Copy data

ddrescue can use block devices or files as input and output. The main difference to other programs used to copy files, is that the destination needs to be specified:

`user $``ddrescue /etc/portage/make.conf ~/make.conf`
### Rescue data

The strategy is to read and copy as much data as possible in the first round, since every access to the storage device could be the last one before it totally fails. Then try to copy the data off the failed/failing areas of the drive. Things to check before getting started:

- The exact device name of the drive to rescue and any other device involved.
- The health of the disk drive using [smartmontools](https://wiki.gentoo.org/wiki/Smartmontools).
- The drive should not be mounted during the process.
- Enough disk space for recovered data.
- Enough time, as the process may take a couple of hours.

### Disk to image

In this scenario the disk drive /dev/sdb is about to fail and we want to create an exact copy in the form of an image.

First, copy every block without read error and log the errors to /root/rescue.log.

`root #``ddrescue -f -n /dev/sdb /root/sdb_rescue.img /root/rescue.log`
- `-f`
- Force ddrescue to run even if the destination file already exists.
- `-n`
- Skip the splitting phase to avoid spending a lot of time trying to rescue the most difficult parts of a file.

Second, copy only the bad blocks and try 3 times to read from the source before giving up:

`root #``ddrescue -d -f -r3 /dev/sdb /root/sdb_rescue.img /root/rescue.log`
- `-d`
- Use direct access to the drive and skip any cache.
- `-rN`
- Try `N` times to rescue the block.

Now the image can be mounted as loop device and the file system checked for corruption.

### Disk to disk

In this scenario the disk drive /dev/sdb is about to fail and we want to create an exact copy on a new disk drive /dev/sdc, which should be at least the same size as the source drive.

First, copy every block without read error and log the errors to /root/rescue.log.

`root #``ddrescue -f -n /dev/sdb /dev/sdc /root/rescue.log`
Second, copy only the bad blocks and try 3 times to read from the source before giving up.

`root #``ddrescue -d -f -r3 /dev/sdb /dev/sdc /root/rescue.log`
Now the new drive could be mounted and the file system checked for corruption.

## See also

- [Dd](https://wiki.gentoo.org/wiki/Dd) — a utility used to copy raw data from a source into sink, where source and sink can be a block device, file, or piped input/output.
- [Dcfldd](https://wiki.gentoo.org/wiki/Dcfldd) — an enhanced [dd](https://wiki.gentoo.org/wiki/Dd) tool that includes additional features for forensics and security.
- [Pv](https://wiki.gentoo.org/wiki/Pv) — a command line tool to view verbose information about data streamed/piped *through* it.

## External resources

- [http://www.garloff.de/kurt/linux/ddrescue/](http://www.garloff.de/kurt/linux/ddrescue/) - A tutorial on ddrescue written by Kurt Garloff.
- [https://www.cyberciti.biz/tips/how-do-i-save-recover-data-from-crashed-disks-with-dd-and-ddrescue-command.html](https://www.cyberciti.biz/tips/how-do-i-save-recover-data-from-crashed-disks-with-dd-and-ddrescue-command.html)
- [http://forensicswiki.org/wiki/Ddrescue](http://forensicswiki.org/wiki/Ddrescue) - ddrescue on the Forensics Wiki.
