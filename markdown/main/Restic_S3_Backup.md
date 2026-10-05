<!-- source: https://wiki.gentoo.org/wiki/Restic/S3_Backup | group: Gentoo Wiki (Main) | wiki-title: Restic/S3 Backup -->
---
title: Restic/S3 Backup
url: https://wiki.gentoo.org/wiki/Restic/S3_Backup
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-10-24"
fingerprint: bf01920d8ca52ae5
license: CC BY-SA 4.0
---

# Restic/S3 Backup

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the process of installing and configuring Restic to backup to an S3-compatible storage provider such as Backblaze B2, Wasabi, or Minio.

## Requirements

- Credentials for an S3-compatible storage provider, such as Backblaze B2, Wasabi, or Minio

## Installation

Install [app-backup/restic](https://packages.gentoo.org/packages/app-backup/restic):

`root #``emerge --ask app-backup/restic`
## Configuration

Restic can use any S3-compatible storage provider as a storage backend.

### Credentials

Create a file at /etc/restic/restic.env with the following contents:

**`/etc/restic/restic.env`**

**restic.env**

```
export AWS_ACCESS_KEY_ID=<ACCESS_KEY_GOES_HERE>
export AWS_SECRET_ACCESS_KEY=<SECRET_ACCESS_KEY_GOES_HERE>
```
Any time that restic is invoked the contents of this file must be read into the environment so that credentials are available to the tool. This can be done by sourcing the file before invoking restic:

`root #``. /etc/restic/restic.env`
A list of Restic environment variables is maintained [here](https://wiki.gentoo.org/wiki/Restic#Environment_variables), any of these may be used to configure the behaviour of the tool. As an example, `RESTIC_PASSWORD_FILE` can be used to specify a file containing the password for the repository, while `RESTIC_REPOSITORY` can store the location of the repository.

### Initialising a Repository

S3 Path-style URLs are expected by restic e.g. `s3.us-west-2.amazonaws.com/bucket_name`. Virtual-host-style URLs (`bucket_name.s3.us-west-2.amazonaws.com`), where the bucket name is part of the hostname, are not supported. These must be converted to path-style URLs instead.

Initialise the repository. If the bucket in question does not already exist (and the credentials provided have the appropriate privileges it), it will be created automatically.

`root #``restic -r s3:s3.us-east-005.backblazeb2.com/larry-nas-backup init`
enter password for new repository:
enter password again:
created restic repository eefee03bbd at s3:s3.us-east-005.backblazeb2.com/larry-nas-backup
Please note that knowledge of your password is required to access the repository.
Losing your password means that your data is irrecoverably lost.



### Backing up Files

The simplest invocation of a backup command is as follows:

`root #``restic -r s3:s3.us-east-005.backblazeb2.com/larry-nas-backup --verbose backup /data/@homes`
open repository
enter password for repository:
repository eefee03bbd opened (version 2, compression level auto)
lock repository
no parent snapshot found, will read all files
load index files
start scan on \[/data/@homes\]
start backup on \[/data/@homes\]
scan finished in 2.545s: 6290 files, 21.695 GiB
\[5:42\] 4.55%  2696 files 1010.249 MiB, total 6290 files 21.695 GiB, 0 errors ETA 2:47:32

As there is no built-in daemon / timer support, automating backups on a schedule is left as an exercise to the reader. Systemd timers or Cron jobs are both suitable options.
