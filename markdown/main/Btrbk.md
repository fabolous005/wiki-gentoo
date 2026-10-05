<!-- source: https://wiki.gentoo.org/wiki/Btrbk | group: Gentoo Wiki (Main) | wiki-title: Btrbk -->
---
title: btrbk
url: https://wiki.gentoo.org/wiki/Btrbk
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-19"
fingerprint: be416f0df5ee128b
license: CC BY-SA 4.0
---

# btrbk

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**btrbk** is a tool for creating incremental snapshots and remote backups of [Btrfs](https://wiki.gentoo.org/wiki/Btrfs) subvolumes. It is used for simple backups to an external [hard drive](https://wiki.gentoo.org/wiki/HDD) as well as more complex scenarios, like a server pulling the backups from all computers in the network or just to make local snapshots to protect against accidental deletions.

## Terminology

btrbk has terms for the snapshots and backups it creates based on where they're stored and their intended purpose:

- *Snapshots* are locally (on the same filesystem) stored [Btrfs snapshots](https://wiki.gentoo.org/wiki/Btrfs#Snapshots)
- *Backups* are snapshots copied to a folder or over [SSH](https://wiki.gentoo.org/wiki/SSH)
- *Archives* are extra copies of backups.

## Installation

`root #``emerge --ask app-backup/btrbk`
## Configuration

To backup subvolumes etc, var/log, and var/lib to /media/backup/btrbk, with snapshots to .btrbk\_snapshots:

**`/etc/btrbk/btrbk.conf`**

**Basic configuration**

### Snapshotting subvolumes

**`/etc/btrbk/btrbk.conf`**

**Make snapshots of Larry's homedir.**

### Backing up subvolumes

#### Backing up the root subvolume

To backup the root subvolume (the subvolume mounted at /) with the relative path to the top-level subvolume, the subvolume with `subvolid=5`, it must be mounted:

**`/etc/fstab`**

**fstab example with the top-level subvolume mounted at /mnt/btr\_pool and*root* subvolume (named `@root` here) mounted at /.**

**`/etc/btrbk/btrbk.conf`**

**Backup the*root* subvolume to /media/backup/btrbk.**

#### Backing up standard subvolumes

**`/etc/btrbk/btrbk.conf`**

**Backup the*home* subvolume to /media/backup/home\_backups**

### Remote Backups

#### SSH configuration

##### Enable and Restrict Root Login

**`/etc/ssh/sshd_config`**

To restrict the IPs/IP ranges from where root can log in, use the `Match` keyword. Consult the man page for *sshd\_config* for details.

**`/etc/ssh/sshd_config`**

##### Generate keys

Root login should only be performed using keys, not passwords. To generate a new root SSH key, and install it on a target system:

`root #````
ssh-keygen -t ed25519 -f /etc/btrbk/id_ed25519
```
`root #````
ssh-copy-id -i /etc/btrbk/id_ed25519.pub root@backup.example.org
```
#### Backing up to another host using SSH

Backups can be made over SSH:

**`/etc/btrbk/btrbk.conf`**

**Backup homedirs to backup.example.org using SSH**

#### Pull backups from another host using SSH

This is an example configuration for multiple clients to backup onto a server:

**`/etc/btrbk/btrbk.conf`**

For more examples, take a look at the official documentation hyperlinked at the top right of this page.

## Usage

### Dry run

To do a verbose dry run:

`root #``btrbk --dry-run --verbose run`
### Run a full backup

To create snapshots and backup (if a target was configured), run:

`root #``btrbk run`
### Create snapshots

To only create snapshots even if a target is configured, run:

`root #``btrbk snapshot`
### Automation with cron

**`/etc/cron.hourly/btrbk-snapshot`**

**Local snapshots once an hour**

```
#!/bin/sh
exec /usr/bin/btrbk -q snapshot
```


**`/etc/cron.daily/btrbk-run`**

**Backup once a day**

```
#!/bin/sh
exec /usr/bin/btrbk -q run
```
