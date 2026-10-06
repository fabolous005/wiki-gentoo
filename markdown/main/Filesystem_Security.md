<!-- source: https://wiki.gentoo.org/wiki/Filesystem/Security | group: Gentoo Wiki (Main) | wiki-title: Filesystem/Security -->
---
title: Filesystem/Security
url: https://wiki.gentoo.org/wiki/Filesystem/Security
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-29"
fingerprint: b605191a26079385
license: CC BY-SA 4.0
---

# Filesystem/Security

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Article status**

A well-designed [file system](https://wiki.gentoo.org/wiki/Filesystem) is one of the basic means to harden a system. While there are numerous file systems in existence, this guide tries to remain agnostic and focus more on the hierarchy itself rather than an individual implementation.

## Partitioning

Partitioning is a key part of implementing security at the file system level:

- It limits the impact of disk failure
- It simplifies the process of creating backups
- It allows administrators to add restrictions such as [quotas](https://wiki.gentoo.org#Quotas) and read-only [permissions](https://wiki.gentoo.org#Permissions) more effectively

### File System Hierarchy

To better understand how to divide the file system across partitions and apply various restrictions, we need to understand a little about the function of the file system hierarchy and its major directories.

Unix-like operating systems, such as Gentoo, based on GNU/Linux, or OpenBSD, borrow their directory structure from the traditional Unix file system hierarchy. This hierarchy was designed in a time when many physical disks where needed to span the whole system. In modern times, with larger storage media being commonplace, average users need not worry about partitioning their file system hierarchy too much. On a server, however, we need to have finer-grained control over the system so we can manipulate it to our will.

Some of the more common directories include:

- **/** Pronounced as "root", this is the top level of the hierarchy. All other file systems are mounted somewhere below this one.
- **/root** Is the home directory of the root user. Typically email from daemons such as [cron](https://wiki.gentoo.org/wiki/Cron) will be sent here.
- **/boot** Typically holds the bootloader and its configuration, as well as kernel binaries.
- **/etc** On modern systems like GNU/Linux and FreeBSD, this holds system wide configuration information and is a good target for regular backups.
- **/bin** Essential system binaries  for use by all users are located here, including tools like grep, ls, and tar.
- **/sbin** Root-only system binaries are located here; for example, the initialization daemon and utilities to mount and create file systems.
- **/lib** System libraries are located here. On most 64bit systems /lib is usually just a symlink and separate **/lib32** and **/lib64** directories will exist.
- [**/dev**](https://wiki.gentoo.org/wiki//dev) Special [device files](https://wiki.gentoo.org/wiki/Device_file) are located here. This is one of the most important directories as its contents are how Unix-like systems interface with hardware from all but the lowest level: the drivers themselves.
- **/home** This directory is where the home directory of a typical system user goes. It usually isn't a good idea to have network shares mounted here as users' home directories.
- **/opt** Is a place for non-default software. A good generalization is: if the software didn't come from Portage or another Gentoo maintained source, it should probably go here. Troublesome software that doesn't cleanly follow the Unix File Hierarchy should be located here to avoid disturbing the rest of the file system.
- **/tmp** This is for caches and temporary files and is typically overwritten on a reboot. Often implemented using [tmpfs](https://wiki.gentoo.org/wiki/Tmpfs).
- **/var** This directory contains data files that change a lot, from system logs to PID files to the Portage world set.
- **/var/tmp** This is the temporary space within /var. It deserves its own listing because (by default) Portage uses this location as a build directory for packages.
- **/usr** This is where "shareable, read-only data" is kept. It holds data like binaries (/usr/bin), program data like man pages, fonts and icons (/usr/share) and documentation (/usr/share/doc) for user software, and shouldn't be writable by unprivileged users.
- **/proc** [Procfs](https://wiki.gentoo.org/wiki/Procfs) is a virtual file system that allows privileged users to monitor and modify kernel settings and configurations at run time. It has been superseded by [sysfs](https://wiki.gentoo.org/wiki/Sysfs).
- **/sys** Sysfs is a virtual file system used to interface with the kernel. It is like /proc, but in a format easier for programs to parse.

The original Unix filesystem hierarchy was designed to involve multiple disks. Using partitions, we can emulate this and segregate some of these directories for the aforementioned reasons of security and backup simplicity.

### Sizing

It is rather difficult to judge what size a partition should be. It takes a bit of experience but after thinking about the machine's intended purpose, one can usually get a reasonably good idea of sizing needs. For example, a computer used on a typical client machine would benefit from a large /home directory, while, on a server, /home should be considerably smaller.

### Mount options

On Unix-like systems mount points are typically defined in [/etc/fstab](https://wiki.gentoo.org/wiki//etc/fstab):

**`/etc/fstab`**

```
# <fs>                  <mountpoint>                   <type>          <opts>                                  <dump/pass>
/dev/sda1               /boot                           ext4            noauto,nouser,noatime,ro                   1 2
/dev/sda3               /                               ext4            noatime,nouser,ro                          0 1
/dev/sda2               none                            swap            sw                                         0 0
/dev/sda5               /usr                            ext4            nodev,nouser,noatime,ro                    0 3
/dev/sda6               /opt                            ext4            nodev,nouser,noatime,ro                    0 3
/dev/sda7               /var                            ext4            nodev,nouser,noexec                        0 3
/dev/sda8               /tmp                            ext4            nodev,nouser,noatime,noexec                0 3
/dev/sda9               /var/tmp                        ext4            nodev,nouser,noatime                       0 3
/dev/sda10              /var/cache/distfiles            ext4            nodev,nouser,noatime                       0 3
/dev/sda11              /var/cache/binpkgs              ext4            nodev,nouser,noatime,noexec                0 3
/dev/sda12              /home                           ext4            nodev,nouser,noatime,noexec                0 3
/dev/md0               /srv                             ext4            nodev,nouser,noatime,noexec                0 3
```
This is an example of a recently created /etc/fstab on a home server. In the fourth column, mount options are listed.

The options we are currently focused on include:

- **ro**/**rw**: Certain partitions, such as /usr and /boot, can be safely mounted with the **ro** flag which makes them read-only, as opposed to **rw**, or read/write. This lowers the impact of rogue software or user mistakes and eliminates the need to use fsck on the partition after a power interruption or system crash. It does, however, require a user to [remount those partitions](https://wiki.gentoo.org/wiki/Mount#Changing_mount_options) when they want to install programs or upgrade their kernel. For files that need to be written to such as /etc/resolv.conf, they can be moved to a different directory and a symlink can be created that points to them from their original location.
- **suid**/**nosuid**: **nosuid** disables the use of SUID and SGID bits in file permissions for the partition. This makes privilege escalation attacks harder but isn't recommended for /usr, as some commands vital for users such as sudo, passwd and chsh exist here.
- **dev**/**nodev**: Mounting a partition with the **nodev** flag disables the use of [device files](https://wiki.gentoo.org/wiki/Device_file) on that partition. These files, in anywhere but /dev, can let an attacker break out of a chroot and bypass other restrictions, as these device files interface with the hardware.
- **exec**/**noexec**: **noexec** makes all files on the partition ignore their execute bits. This is great for partitions like /tmp, but, on Gentoo, /var/tmp is used to extract distfiles and requires executable permissions. It is recommended, from a security standpoint, to have the **noexec** flag set on the /var partition, but on Gentoo this requires /var/tmp to be on a separate partition with the **noexec** flag not set.
- **user**(**s**)/**nouser**: **nouser** permits only root to mount the partition; this is the default. **user** allows ordinary users to mount the partition, with only the user that mounts the partition being allowed to unmount it. This implies **noexec**, **nosuid** and **nodev** unless explicitly overridden by subsequent options. **users** is like **user** but permits anyone to unmount a partition mounted by other users.
- **auto**/**noauto**: **auto** causes the partition to be mounted at boot. It is desired to mount /boot with the **noauto** flag because, in the event of a power outage or system crash, /boot will not be mounted and thus won't be uncleanly unmounted. This keeps the kernel image safe, along with other recovery tools such as a statically compiled busybox.
- **defaults**: This typically enables the following flags: **rw**, **suid**, **dev**, **exec**, **auto**, **nouser**, **async**.

## Chroot

What is chroot exactly? chroot comes as both a shell utility and a system call. What chroot does is quite simple: it changes the way programs see the root file system. For example, running the following command will make /mnt/foo the new / within the shell process that ran the chroot command. Essentially chroot is a primitive sandbox that is quick and easy to set up:

`root #``chroot /mnt/foo/`
Why would anyone want to do this to the file system? Traditionally, as a security measure, daemons have been run within chroots so that, if they are compromised, no damage can be done to the outside of this chroot. Nowadays, people use things like Linux kernel [cgroups](https://wiki.gentoo.org/wiki/Cgroups) or [containers](https://wiki.gentoo.org/wiki/Virtualization) that provide more complete sandboxing.

Nevertheless, chroot is quick, easy to set up, and often has configure script support in some more popular daemons such as [BIND](https://wiki.gentoo.org/wiki/BIND) and [Apache](https://wiki.gentoo.org/wiki/Apache). We can easily use chroot to provide a safe build environment. chrooting into an extracted Stage3 tarball offers the user an easy way to set up, strip down, and rebuild the environment to build and test new technologies without risking the destruction of their current workstation.

## File System check

*The second letter was originally different.* -Dennis Ritchie on fsck

fsck is a utility that can be run on a partition to check it for errors and automatically fix them. With modern journaled file systems, fsck is not only much quicker but typically has a higher success rate when recovering from corruption. There are quite a few options one could pass to the fsck command, including some gotchas, so reading over the [man page](https://wiki.gentoo.org/wiki/Man_page) is highly recommended. A basic fsck command might look like this:

`root #``fsck -y /dev/sda8`
This will run a consistency check on the 8th partition of the first SATA/SCSI disk and automatically repair any issues, if the file system supports that option.

Some important things to remember when using fsck:

- **Never** run fsck on a mounted partition
- lost+found is where orphaned files go
- rebooting after fscking is usually healthy

Filesystems are automatically checked on boot in most setups. The 5th column in an fstab entry contains 2 numbers; the second number, **fs passno** according to **fstab(5)**, has the order in which file systems should be checked during init. It is recommended to have the root partition as 1 and the rest of the partitions as 2. Setting this number to 0 disables filesystem checks on this partition. Virtual file systems and the swap partition can be left un-fscked.

## RAID

RAID technologies are pretty straightforward, take a few hard drives and string them together. RAIDs are defined by their *level*. For example RAID 0 is a system where two or more disks are combined so that there is additional storage and a disk I/O performance bonus. This documentation will discuss RAID level 5 and RAID level 6 which both offer some security.

### RAID controller

What ties a RAID system all together is known as a RAID controller. RAID controllers can exist in software or hardware. The primary difference is that hardware RAID controllers off load processing to the RAID controller itself rather then on the central processing unit, which in some high performance environments can be beneficial. However in home systems or small businesses, often a software RAID controller is enough.

Oftentimes a hardware RAID controller will have its own special configuration method so be sure to read the documentation provided. However in the Linux world a tool called mdadm is used for software RAID setups.

To install mdadm on Gentoo Linux:

`root #``emerge --ask sys-fs/mdadm`
### Creating a software RAID

Quickly before proceeding to the meat and potatoes, lets talk about the difference between the RAID levels of 5 and 6. RAID 5 requires a minimum of **three** physical disks, and is able to withstand the loss of **one** of those disks, but will take a performance hit until the failed disk is replaced and the RAID is synchronized. RAID 6 requires a minimum of **four** physical disks and can withstand the loss of **two disks**, and will only see a noticeable performance hit if two drives fail at the same time. Just like RAID 5, failed drives should be replaced and the RAID should be synchronized.

When deciding between RAID 5 and RAID 6, it is important to note that RAID 6 is favored. RAID 5 has a *write hole*, essentially under certain circumstances the RAID will fail to preserve data because a flaw in the RAID 5 algorithm. For that reason the following example will detail RAID 6.

First add kernel support for RAID systems:

**.config 3.2.11-hardened**

```
Device Drivers --->
    [*] Multiple devices driver support (RAID and LVM) --->
        <*> RAID support
        [*]   Autodetect RAID arrays during kernel boot
        <*>   RAID-4/RAID-5/RAID-6 mode
        <*> Device mapper support
        <*>   Mirror target
        <*> Zero target
```
Next the disks need to be partitioned, assume four disks exist: sda, sdb, sdc, and sdd. Use a favorite partitioning tool to format the drives and create empty partition tables on them. Then allocate all the space in to a single partition.

Finally we actually create the RAID:

`root #``mdadm --create /dev/md0 --level=6 --raid-devices=4 /dev/sda1 /dev/sdb1 /dev/sdc1 /dev/sdd1`
A new [device file](https://wiki.gentoo.org/wiki/Device_file) has been created, /dev/md0 which can be manipulated just like any other disk [device file](https://wiki.gentoo.org/wiki/Device_file), however md0 represents the RAID that was just created.

The array needs to do an initial sync which will take a while to finish. This will take upwards of several hours, depending on multiple factors including disk size. I was able to get relatively better performance by issuing the following command:

`root #``echo 8192 > /sys/block/md0/md/stripe_cache_size`
We aren't quite done yet. The mdadm tool uses a configuration file located at /etc/mdadm.conf and while this is an optional step, editing this configuration file is a recommended step.

Instead of manually filling out this file, lets be lazy and run a couple commands instead:

`root #````
echo "DEVICE /dev/sda1 /dev/sdb1 /dev/sdc1 /dev/sdd1 > /etc/mdadm.conf
```
`root #``mdadm --detail --scan >> /etc/mdadm.conf`
This configuration file serves two purposes, first it helps to document the RAID which is very important when it comes time to replace a disk. And second it helps the kernel activate the RAID during boot instead of a user doing it manually.

You are also encouraged to explore the monitor feature of mdadm, this sends mail warnings to administrators as issues with the RAID are encountered. Along with the monitor feature, a MAILADDR variable can be specified in /etc/mdadm.conf to give mdadm a mail address to send warnings to.

### Maintaining a software RAID

There are a few commands and kernel data structures with which administrators dealing with a software RAID should be familiar. To get details about the RAID run:

`root #``cat /proc/mdstat`
Here you will also see the progress of a RAID recovery or sync.

Occasionally, you will need to do a check for bad blocks on the RAID, reallocate the data stored on them, and then sync the RAID. To do this run:

`root #``echo check >> /sys/block/md0/md/sync_action`
To cleanly halt a RAID sync:

`root #``echo idle >> /sys/block/md0/md/sync_action`
For troubleshooting and configuration purposes, there is an option to simulate a disk failure:

`root #``mdadm /dev/md0 -f /dev/sda1`
#### Replacing failed disks

In our simulation, the disk at /dev/sda1 failed. To fix it, all we have to do is remove it from the RAID logically and readd it to the RAID. When real hardware fails in reality the process is similar, except the additional step of replacing the failed hardware.

First logically remove the drive from the RAID:

`root #``mdadm /dev/md0 -r /dev/sda1`
At this point, in a real failure situation we would need to swap the failed disk. Depending on the hardware configuration we might have to power the system down all the way to accomplish this.

To add a disk into the RAID as a replacement run this:

`root #``mdadm /dev/md0 -a /dev/sda1`
Once a new disk is added, the RAID should automatically begin rebuilding itself.

## Quotas

Quotas are an important tool for administrators of multi-user systems. They allow you to prevent a group, user, or process (through group and user restrictions) from filling up disk space. This is particularly useful on for example a server that hosts a network share.

Gentoo documentation already covers this topic in depth in their [user and group limitations](https://wiki.gentoo.org/wiki/Security_Handbook/User_and_group_limitations) guide.

## Encryption

Under construction...

## Permissions

Under construction...

### The traditional Unix way

In Unix, everything is a file. As such, permissions revolve around a series of file attributes. In a nutshell, these attributes relate to a read-bit, write-bit, executable-bit, setuid-bit, setgid-bit, sticky-bit, and information about owner, group, and world.

Example output of the ls command:

`user $``ls -la /etc/rsyncd.conf`
-rw-r--r-- 1 root root 405 Apr  1 14:49 /etc/rsyncd.conf

The very first column shows various information about the file in question, including the permissions set on it: the first dash indicates this is a file, not a directory (which would be marked by a *d*).  An *s* would mean it's a socket file, and an *l* would make it a link.  The next three spaces show whether the read-bit (r), write-bit (w), or executable-bit (x) is set for the owner of the file.  The next three denote the same permission bits, but for the group instead of the user.  The third group of three bits denote permissions for the world, or everyone else on the system.  This is the basic Unix permissions model.

In the third column, the user 'root' can be seen as the owner of the file, while the fourth column shows the group, also named 'root' in the example.

Looking into permissions deeper, the 'setuid' and 'setgid' bits are somewhat less commonly used, especially by the common user, also being considerably more dangerous if used incorrectly. If used correctly, setuid and setgid bits can make system management much easier by making permissions more flexible. Essentially, the setuid (set user id) bit, when activated, will allow the file to be executed with the permissions of its owner by anyone. Likewise, setgid (set group id) will allow any user to execute a file with the permissions of its group.

One way of making use of these, is for administrators to give users access to particular programs that would otherwise require the root password.  With the ls command, the *x* denoting user executable permission will be replaced by an *s* if the setuid-bit is set.  This means that in the group permission section, an *s* will replace the *x* if the setgid-bit is set.

The sticky-bit can be confusing: while traditionally, it had a very clear use, it has evolved over time and different Unix environments have different uses for it.  On Linux, the sticky bit can be set on directories, and as a consequence, any files under those directories can be deleted (or unlinked) only by the owner.  If the sticky-bit is set, a *t* will be displayed at the far right of the user-group-world permissions listing in the ls output.

#### Managing permission bits, bit wrangling

Being able to wrangle all these permissions bits is very important for the system administrator(s). The tools of the trade include: chmod, chown, chgrp, and umask.

Further still, understanding the hex values that activate and deactivate various bits is just as important:

- 0 - no permissions are set
- 1 - execute only
- 2 - write only
- 3 - write and execute
- 4 - read only
- 5 - read and execute
- 6 - read and write
- 7 - read, write, and execute

The setuid, setgid, and sticky bits are separated from the other permissions bits so their values can be repeated:

- 0 - clears any previously set bit in this category
- 1 - stickybit
- 2 - setgid
- 4 - setuid

One of the most useful tools for changing permissions is [chmod](https://linux.die.net/man/1/chmod), which simply modifies the state of the permission bits on a file or directory.  For example, to set a file as writeable by the owner, but only readable by everyone else, one would use the following command:

`user $``chmod 644 /path/to/file`
To add the setuid bit to that file, a fourth column is used. This is the column where setgid, setuid, and stickybit are specified:

`user $``chmod 4644 /path/to/file`
Another useful tool is [chown](https://linux.die.net/man/1/chown), which changes ownership of files and directories.  Its basic usage is fairly straightforward:

`user $``chown user:group /path/to/file`
Yet another useful tool, [chgrp](https://linux.die.net/man/1/chgrp), might not be that often used on workstations, but is more useful for servers and mainframes.  It allows one to to change only the group ownership of files and directories, while leaving the user ownership intact.

This might be useful for a shared directory:

`user $``chgrp group /path/to/file`
The umask shows the current default file creation bit mask, and allows for a different mask to be set so that all new files created in the current working directory will have those permissions bits activated by default. Because it is a mask, the way the permissions are represented is different from chmod:

- 0 - read, write, and execute
- 1 - read and write
- 2 - read and execute
- 3 - read only
- 4 - write and execute
- 5 - write only
- 6 - execute only
- 7 - no permissions are set

Example of setting a directory to have full permissions for the owner, but 'read only' for everyone else:

`user $````
cd /my/directory
```
`user $````
umask 033
```
### Access Control Lists

Consult the [dedicated guide](https://wiki.gentoo.org/wiki/Filesystem/Access_Control_List_Guide).

## Backups

Under construction... (lightly discuss, link to existing Gentoo wiki pages.)

## See also

- [/etc/fstab](https://wiki.gentoo.org/wiki//etc/fstab) — a configuration file that defines how and where the main [filesystems](https://wiki.gentoo.org/wiki/Filesystem) are to be mounted, especially at boot time.
- [Mount](https://wiki.gentoo.org/wiki/Mount) — the attaching of an additional [filesystem](https://wiki.gentoo.org/wiki/Filesystem) to the currently accessible filesystem of a computer.
- [Security\_Handbook/User\_and\_group\_limitations](https://wiki.gentoo.org/wiki/Security_Handbook/User_and_group_limitations) — provides detail on controlling the system's resource usage of users via limits and quotas.
