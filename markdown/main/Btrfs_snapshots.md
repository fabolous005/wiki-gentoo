<!-- source: https://wiki.gentoo.org/wiki/Btrfs/snapshots | group: Gentoo Wiki (Main) | wiki-title: Btrfs/snapshots -->
---
title: Btrfs/snapshots
url: https://wiki.gentoo.org/wiki/Btrfs/snapshots
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-07-09"
fingerprint: "3f136b5824bf13b8"
license: CC BY-SA 4.0
---

# Btrfs/snapshots

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page proposes a script to **make automatic snapshots with [Btrfs](https://wiki.gentoo.org/wiki/Btrfs)** filesystem, using  btrfs subvolume list-new function to create snapshots only when files have changed, so as to create fewer snapshots.

**`/mnt/pool/snapshots/snapshot_home.sh`**

**Automatic incremental snapshots**

```
#!/bin/bash
d="$(date +%Y.%m.%d-%H.%M.%S)"
echo "Backup started at ${d}"
time="@GMT_${d}"
# Mountpoint
mount="/mnt/pool/volumes/home"
## symlink to most recent snapshot
symlink="/mnt/pool/snapshots/home"
old_snap="$(readlink "/mnt/pool/snapshots/home")"
## New snapshot name
new_snap=${symlink}"_${time}"
## Check for most recent generation ID for most recent snapshot.
## This is used when looking for changed files.
if [[ -d "${old_snap}" ]]; then
        # find-new outputs last generation ID when using a too high value is used for comparing.
        gen_id=$(/sbin/btrfs sub find-new "${old_snap}" 9999999|cut -d " " -f 4)
        # Count changed files.
        count="$(/sbin/btrfs subvolume find-new "${mount}" ${gen_id} | cut -d " " -f 17-1000 | sed '/^$/d'| wc -l)"
        /sbin/btrfs subvolume find-new "${mount}" ${gen_id} | cut -d " " -f 17-1000
        # Create a new snapshot if files have changed.
        if [[ ${count} -gt 0 ]]; then
                /sbin/btrfs subvolume snapshot -r "${mount}" "${new_snap}"
                ## Recreate a symlink to the new snapshot
                rm "${symlink}" && ln -s "${new_snap}" "${symlink}"
        else
                echo "No files changed, skipping creating of a new snapshot"
        fi
else
        echo Something went wrong. Check that symlink $symlink points to a real snapshot
fi
echo "Backup finished at $(date +%Y.%m.%d-%H.%M.%S)"
```
It is recommended to schedule the /mnt/pool/snapshots/snapshot\_home.sh with cron.

**`/etc/cron.hourly/autosnap.sh`**

**hourly cron script**

```
#!/bin/bash
/mnt/pool/snapshots/snapshot_home.sh >> /mnt/pool/snapshots/snapshot_home.log 2>&1
```
For more detailed information on btrfs subvolumes and snapshots see the [btrfs wiki](https://btrfs.wiki.kernel.org/index.php/SysadminGuide).

## See also

- [Btrfs/System Root Guide](https://wiki.gentoo.org/wiki/Btrfs/System_Root_Guide) - Use the Btrfs filesystem as a collection of subvolumes including one as a system root.
- [Btrfs native system root guide](https://wiki.gentoo.org/wiki/Btrfs/Native_System_Root_Guide) - An alternative guide on using a subvolume in a Btrfs filesystem as the system's root.
- [Btrbk](https://wiki.gentoo.org/wiki/Btrbk) — a tool for creating incremental snapshots and remote backups of [Btrfs](https://wiki.gentoo.org/wiki/Btrfs) subvolumes.
- [Snapper](https://wiki.gentoo.org/wiki/Snapper) — a command-line program to create and manage filesystem snapshots, allowing viewing or reversion of changes.
- [Samba shadow copies](https://wiki.gentoo.org/wiki/Samba_shadow_copies) - Using Samba to expose Shadow Copies as 'Previous Versions' to Windows clients.
- [Ext4](https://wiki.gentoo.org/wiki/Ext4) — an open source disk [filesystem](https://wiki.gentoo.org/wiki/Filesystem) and the most recent version of the extended series of filesystems.
- [ZFS](https://wiki.gentoo.org/wiki/ZFS) — a next generation [filesystem](https://wiki.gentoo.org/wiki/Filesystem) created by Matthew Ahrens and Jeff Bonwick.
