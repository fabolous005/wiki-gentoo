<!-- source: https://wiki.gentoo.org/wiki/Rdiff-backup | group: Gentoo Wiki (Main) | wiki-title: Rdiff-backup -->
---
title: rdiff-backup
url: https://wiki.gentoo.org/wiki/Rdiff-backup
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-13"
fingerprint: b45290cc05a733da
license: CC BY-SA 4.0
---

# rdiff-backup

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


rdiff-backup is a GPL-licensed incremental backup utility based on librsync; it stores *changes* to files instead of entire duplications. This can greatly reduce storage requirements for backups. The resultant incremental data can be viewed and restored from as if it were whole file backups via [FUSE](https://wiki.gentoo.org/wiki/FUSE)-based rdiff-backup-fs.

## Installation

### USE flags


### Emerge

`root #``emerge --ask --verbose --tree rdiff-backup`
## Usage

### Backup

`user $``rdiff-backup path/to/source path/to/backup/destination`
To backup again, simply run the exact same command; each increment will be individually accessible.

### Restore

The cp command can simply be used to copy a file from a backup created with rdiff-backup.

See [rdiff website](http://rdiff-backup.nongnu.org/examples.html#restore) for more examples.

### cron

`user $``crontab -e`
