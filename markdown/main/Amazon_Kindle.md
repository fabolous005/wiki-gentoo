<!-- source: https://wiki.gentoo.org/wiki/Amazon_Kindle | group: Gentoo Wiki (Main) | wiki-title: Amazon Kindle -->
---
title: Amazon Kindle
url: https://wiki.gentoo.org/wiki/Amazon_Kindle
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-10"
fingerprint: "5e0909988cabb4fd"
license: CC BY-SA 4.0
---

# Amazon Kindle

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A quick document explaining how to use the **Amazon Kindle** with Gentoo.

## Kernel

To be able to mount your Kindle as an [external storage device](https://wiki.gentoo.org/wiki/Removable_media), you require the **VFAT** file system as well as support for DOS partition tables in your kernel.

**Enabling File System Options**

## Kindle DX / DX Graphite

### Mounting the Removable Storage Media

#### Mounting using AutoFS

Execute blkid ([sys-apps/util-linux](https://packages.gentoo.org/packages/sys-apps/util-linux)) with the Kindle device attached.

`root #``blkid`
Find the [UUID](https://wiki.gentoo.org/wiki/Removable_media#UUIDs_and_labels) from the blkid output and insert the following line into your /etc/autofs/auto.misc file, substituting *UUID* with your UUID:

**`/etc/autofs/auto.misc`**

Edit the /etc/conf.d/autofs file to your liking.  Make sure you uncomment the *MASTER\_MAP\_NAME="auto.master"* line if you use the /etc/autofs/auto.master file!

The following addition to the auto.master file can be used.  The *--ghost* option auto-unmounts after five seconds:

**`1/etc/autofs/auto.master`**

#### Mounting using udev

The following [udev](https://wiki.gentoo.org/wiki/Udev) rule will mount your Kindle using the Volume Name (ie. "Kindle") to (/media/Kindle). You then will need to execute the user scripts add.sh and remove.sh to tell the udev to add (mount) and remove (unmount) the device.  In turn, the /media/Kindle folder is automagically created and destroyed on mount and unmount.

**`/etc/udev/rules.d/11-media-by-label-auto-mount.rules`**

**`/home/user/bin/udev-add-all.sh`**

```
#!/bin/bash - 
#===============================================================================
#
#          FILE:  udev-add.sh
# 
#         USAGE:  ./udev-add.sh 
# 
#   DESCRIPTION: udev add removable media device 
# 
#        AUTHOR: Roger Zauner (rdz), rogerx (dot) oss (at) gmail (dot) com
#       CREATED: 04/10/2010 12:18:57 AM AKDT
#===============================================================================
set -o nounset                              # Treat unset variables as an error
#set -o xtrace                              # Enable trace debugging
# sd[b-z][0-9]
#udevadm trigger --action="add" --sysname-match="sdb1" --verbose
if [ $HOSTNAME = "localhost1.local" ]; then
    sudo /sbin/udevadm trigger --action="add" \
      --sysname-match="sd[c-z][0-9]" --verbose
#    sudo /sbin/udevadm trigger --action="add" \
#      --sysname-match="sr[0-9]" --verbose
  
  elif [ $HOSTNAME = "localhost2.local" ]; then
    sudo /sbin/udevadm trigger --action="add" \
      --sysname-match="sd[c-z][0-9]" --verbose
#    sudo /sbin/udevadm trigger --action="add" \
#      --sysname-match="sr[0-9]" --verbose
  
  elif [ $HOSTNAME = "localhost3.local" ]; then
    sudo /sbin/udevadm trigger --action="add" \
      --sysname-match="sd[b-z][0-9]" --verbose
#    sudo /sbin/udevadm trigger --action="add" \
#      --sysname-match="sr[0-9]" --verbose
fi
```
**`/home/user/bin/udev-remove.sh`**

```
#!/bin/bash - 
#===============================================================================
#
#          FILE:  udev-remove.sh
# 
#         USAGE:  ./udev-remove.sh 
# 
#   DESCRIPTION: udev remove removable media device 
# 
#        AUTHOR: Roger Zauner (rdz), rogerx (dot) oss (at) gmail (dot) com
#       CREATED: 04/10/2010 12:22:17 AM AKDT
#===============================================================================
set -o nounset                              # Treat unset variables as an error
#set -o xtrace                              # Enable trace debugging
# sd[b-z][0-9]
#udevadm trigger --action="remove" --sysname-match="sdb1" --verbose
if [ $HOSTNAME = "localhost1.local" ]; then
    sudo /sbin/udevadm trigger --action="remove" \
      --sysname-match="sd[c-z][0-9]" --verbose
#    sudo /sbin/udevadm trigger --action="remove" \
#      --sysname-match="sr[0-9]" --verbose
  
  elif [ $HOSTNAME = "localhost2.local" ]; then
    sudo /sbin/udevadm trigger --action="remove" \
      --sysname-match="sd[c-z][0-9]" --verbose
#    sudo /sbin/udevadm trigger --action="remove" \
#      --sysname-match="sr[0-9]" --verbose
  elif [ $HOSTNAME = "localhost3.local" ]; then
    sudo /sbin/udevadm trigger --action="remove" \
      --sysname-match="sd[b-z][0-9]" --verbose
#    sudo /sbin/udevadm trigger --action="remove" \
#      --sysname-match="sr[0-9]" --verbose
fi
```
The AutoFS method may be preferred, as it is cleaner and doesn't need running the above user scripts.  The main problem with the Kindle DX seems to be the DX's display stating the device is plugged in.  Even though this is true, you can still unplug the device without using the **eject** command and the device is not mounted! And, using the **eject** command at times would confuse the device and the Linux sub-system at times. So, just unplug the device after checking the device is no longer mounted via the **mount** command.  As such, much easier to just use AutoFS, as it can be configured to auto unmount it for you using the *--ghost* option.

## File Format Conversions

Simple utilities for converting documents can be used, as shown below.

### PDF File Format

Best format to read technical documents is PDF.

- You can create PDF's easily from printing them from Seamonkey or Firefox using the print to file method.
- Use ps2pdf utilities ([app-text/ghostscript-gpl](https://packages.gentoo.org/packages/app-text/ghostscript-gpl))

### MOBI File Format

The basis of this file format is HTML, but with the feature of DRM when required by publishers. Keep it simple, avoiding TABLE tag usage. (See Amazon's AmazonKindlePublishingGuidelines.pdf.)

Use [Vim](https://wiki.gentoo.org/wiki/Vim) to convert a ASCII/UTF8 text file to HTML.  Execute Vim's ":ToHtml" command and save, then use Amazon's kindlegen binary on the saved file. (KindleGen is Amazon's version of MobiGen.)

## Tips

- When possible, copy both the MOBI and PDF file formats to the device as both currently have separate pros and cons (e.g. O'Reilly publishes books into a multitude of formats).
- Because there are so many files freely available, create subfolders within the /mnt/kindle/documents folder even though the Kindle DX Graphite firmware doesn't display the folders, but will see the files within the folders. Then use the Kindle firmware to create a "Collection" with a similar name to the folders (e.g. Filesystem folder name /mnt/kindle/documents/programming-c, Collection Name "Programming - C").

## Free Books

- Gentoo Wiki articles can be printed to either PDF or HTML using the "Printable version" link, located to the bottom left within the "toolbox" of the viewable version of pages.
- Many C Programming books, specifications and manuals are already freely available in PDF format (e.g. [#C on Libera](https://www.iso-9899.info/wiki/Main_Page) lists many freely available recommended books).
- Many [FSF GNU Manuals](https://www.gnu.org/manual/) are also already available in HTML and PDF formats.
- Manual Pages (AKA manpages) can be easily converted to HTML, then MOBI using KindleGen (see references below).
- [TLDP](https://tldp.org/) hosts many HowTo's and documents in PDF and HTML formats.
- [Python](https://www.python.org/doc/) hosts its documentation in PDF, HTML and EPUB formats.

## Mobile Linux Websites

## See also

- [AutoFS](https://wiki.gentoo.org/wiki/AutoFS) — a program that uses the Linux [kernel](https://wiki.gentoo.org/wiki/Kernel) automounter to automatically [mount](https://wiki.gentoo.org/wiki/Mount) [filesystems](https://wiki.gentoo.org/wiki/Filesystem) on demand.

## External resources

- [Amazon Publishing Programs](https://www.amazon.com/gp/feature.html?ie=UTF8&docId=1000234621) KindleGen Program and AmazonKindlePublishingGuidelines.pdf
- [Amazon Kindle DXG and Linux](http://rogerx.freeshell.org/programming/kindledxg_and_linux.html)
- [Amazon Kindle - Converting .txt to .html to .mobi](http://rogerx.freeshell.org/programming/kindle-convert_txttomobi.html)
- [Amazon Kindle - Convert Linux Manual Pages to .mobi](http://rogerx.freeshell.org/programming/kindle-convert_linuxmantomobi.html)
- [Amazon Kindle - Strip](http://rogerx.freeshell.org/programming/kindle-strip.html)
