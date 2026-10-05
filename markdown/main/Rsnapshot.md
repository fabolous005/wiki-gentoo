<!-- source: https://wiki.gentoo.org/wiki/Rsnapshot | group: Gentoo Wiki (Main) | wiki-title: Rsnapshot -->
---
title: rsnapshot
url: https://wiki.gentoo.org/wiki/Rsnapshot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-03"
fingerprint: bc49451c41bf13a8
license: CC BY-SA 4.0
---

# rsnapshot

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


rsnapshot is an automated [backup](https://wiki.gentoo.org/wiki/Backup) tool based on the [rsync](https://en.wikipedia.org/wiki/Rsync) protocol and written in Perl. rsnapshot makes a specified number of incremental backups of specified file trees using [hard links](https://en.wikipedia.org/wiki/Hard_link) to save space on the backup medium.

The following backup scheme will rotate the backups on a daily, weekly, and monthly basis. That means, it will keep a daily snapshot for 7 days, a weekly snapshot for 4 weeks and a monthly snapshot for 12 month. Furthermore, it uses an extra partition for the backup which will be mounted only for the time of the backup process.

## Installation

### Emerge

Install [app-backup/rsnapshot](https://packages.gentoo.org/packages/app-backup/rsnapshot):

`root #``emerge --ask app-backup/rsnapshot`
## Configuration

### fstab

Assumed the backup partition is labeled *backup*, formatted with [ext4](https://wiki.gentoo.org/wiki/Ext4) and it should be mounted on /mnt/backup during backup: Add an entry like the following in [fstab](https://wiki.gentoo.org/wiki//etc/fstab):

**`/etc/fstab`**

The `noauto` option means that this backup filesystem will not be mounted by default.  The backup filesystem would normally be on an external device to be safe in the case of a device failure.

### Cron scripts

Create [cron](https://wiki.gentoo.org/wiki/Cron) scripts for the different backup intervals:

**`/etc/cron.daily/rsnapshot.daily`**

```
#!/bin/sh
echo "### RSNAPSHOT DAILY ###"
mount /mnt/backup && rsnapshot daily || echo "Backup failure"
umount /mnt/backup
echo ""
```
**`/etc/cron.weekly/rsnapshot.weekly`**

```
#!/bin/sh
echo "### RSNAPSHOT WEEKLY ###"
mount /mnt/backup && rsnapshot weekly || echo "Backup failure"
umount /mnt/backup
echo ""
```
**`/etc/cron.monthly/rsnapshot.monthly`**

```
#!/bin/sh
echo "### RSNAPSHOT MONTHLY ###"
mount /mnt/backup && rsnapshot monthly || echo "Backup failure"
umount /mnt/backup
echo ""
```
### rsnapshot configuration files

Set up the rsnapshot configuration file.

Default rsnapshot config file:

**`/etc/rsnapshot.conf`**

```
# Default config version
config_version	1.2
# So the hard disk is not polluted in case the backup filesystem is not available
no_create_root	1
# Standard settings
cmd_cp			/bin/cp
cmd_rm			/bin/rm
cmd_rsync		/usr/bin/rsync
link_dest		1
# For convenience, so that mount points can be taken as backup starting points
one_fs			1
# Store all backups in one directory per machine
# A useful alternative may be to create a separate directory for each interval
snapshot_root	/mnt/backup/
# increments, which are kept
retain	daily	7
retain	weekly	4
retain	monthly	12
# Backup folder(s)/files
backup	/path/to/something/	localhost/
backup	/var/			localhost/
# Exclude pattern (refer to --exclude-from from rsync man page)
exclude		/path/to/something/tmp/
```
In these files, the second argument of *backup* specifies a container directory for the backups, usually referring to the machine (in this case, *localhost*).  This can be changed to any name of your choosing. The final snapshots will be saved under /mnt/backup/{daily,weekly,monthly}.\[0-9\]\*/localhost/path/to/something/

### Using rsyncd on a trusted LAN

Following the [Home router Rsync server](https://wiki.gentoo.org/wiki/Home_router#Rsync_server) you can setup a rsync daemon on your source computer with a share like:

**`/etc/rsyncd.conf`**

```
[larry]
  path = /home/larry
  comment = Larry's home directory
  exclude = /foo
```
On the destination server, which runs rsnapshot, define a backup line in /etc/rsnapshot.conf like:

**`/etc/rsnapshot.conf`**

```
backup	larry@my_computer::larry	larry/
```
Pay attention to highly insecure mechanism without password checking or encrypted transfer.

## Usage

### Restoration

To restore the *localhost* backups specified above, we would use:

`root #````
mount /mnt/backup
```
`root #````
rsync -a /mnt/backup/localhost/monthly.0/localhost/. /mnt/myroot/
```
`root #````
rsync -a /mnt/backup/localhost/weekly.0/localhost/. /mnt/myroot/
```
`root #````
rsync -a /mnt/backup/localhost/daily.0/localhost/. /mnt/myroot/
```
where /mnt/myroot is the mount point of the fresh root filesystem. In the paths above *\*.0* refers to the latest increment.

## Possible improvements

It is also possible to make remote backups via **rsync** or **[SSH](https://en.wikipedia.org/wiki/Secure_Shell)** -- see the *rsnapshot* man page for details or [Advanced backup using rsnaphot](https://wiki.gentoo.org/wiki/Advanced_backup_using_rsnaphot).

### BTRFS snapshots

Those using btrfs can leverage its snapshotting abilities with rsnapshot. Walter Werther has a [guide](http://it.werther-web.de/2011/10/23/migrate-rsnapshot-based-backup-to-btrfs-snapshots/) on this.

## Complete system backup on a local drive

In that example, we will make a complete system backup and restoration using an USB drive.

### Configuration

In addition to what have been said, we must make a few other configuration changes.

Set up the rsnapshot configuration file:

**`/etc/rsnapshot.conf`**

```
# All snapshots will be stored under this root directory.
# Path to the backup media root directory:
snapshot_root	/media/MaxiTux/snapshots/
# Default rsync args. All rsync commands have at least these options set.
#
rsync_short_args	-aAHSX
rsync_long_args	--delete --numeric-ids --relative --delete-excluded
# The include and exclude parameters, if enabled, simply get passed directly
# to rsync. If you have multiple include/exclude patterns, put each one on a
# separate line. Please look up the --include and --exclude options in the
# rsync man page for more details on how to specify file name patterns.
exclude	/dev/*
exclude	/home/*/.thumbnails/*
#exclude	/home/*/.cache/google-chrome/*
exclude	/home/*/.local/share/Trash/*
exclude	/lost+found/*
exclude	/media/*
exclude	/mnt/*
exclude	/proc/*
exclude	/sys/*
exclude	/run/*
exclude	/tmp/*
exclude	/var/lib/dhcpcd/*
exclude	/var/lib/nfs/*
exclude	/resume_swap
# The include_file and exclude_file parameters, if enabled, simply get
# passed directly to rsync. Please look up the --include-from and
# --exclude-from options in the rsync man page for more details.
include_file	/var/lib/nfs/.keep_net-fs_nfs-utils-0
###############################
### BACKUP POINTS / SCRIPTS ###
###############################
backup	/		localhost/
```
### Restoration

To restore the *localhost* backups specified above, we would use:

`root #````
mount /media/MaxiTux
```
`root #````
rsync -a /media/MaxiTux/snapshots/daily.0/localhost/. /
```
## See also

- [Advanced backup using rsnaphot](https://wiki.gentoo.org/wiki/Advanced_backup_using_rsnaphot) — describes a advanced automated remote backup scheme using the tool rsnapshot as non-root user
