<!-- source: https://wiki.gentoo.org/wiki/Disk_quotas | group: Gentoo Wiki (Main) | wiki-title: Disk quotas -->
---
title: Disk quotas
url: https://wiki.gentoo.org/wiki/Disk_quotas
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-22"
fingerprint: a481b938a8d7a9a0
license: CC BY-SA 4.0
---

# Disk quotas

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Disk quotas** can be implemented by a system administrator as a way to manage restrictions on storage for users or groups of users.

Disk quotas allow the administrator to balance storage resources so that it is possible to manage finite capacity in a suitable way. An example of disk quotas in use would be, for example, a web hosting provider that allocates a certain amount of disk storage space per customer, or to limit certain users from consuming the full resources of the file-system, thereby preventing the file-system filling up and starving other users or the system of storage. There are two types of restrictions that can be put in place, one is inode which controls the number of files/directories, and block which can restrict based on storage blocks (size). When set limits are reached, it is possible to notify the system administrator or user consuming the resources in question, informing them to take appropriate action.

## Installation

### Kernel

Kernel support is required for disk quotas, if support for journaled quotas is required, the option "Quota formats vfsv0 and vfsv1 support" also needs to be enabled.

**Enable support for disk quotas**

```
File systems  --->
  [*] Quota support
  [*] Report quota messages through netlink interface
  [ ] Print quota warnings to console (OBSOLETE)
  ...
  <*> Quota format vfsv0 and vfsv1 support
```
### USE flags


### USE flags for
            [sys-fs/quota](https://packages.gentoo.org/packages/sys-fs/quota)
            
            Linux quota tools

| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [netlink](https://packages.gentoo.org/useflags/netlink) | Compile daemon receiving quota messages via netlink | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [rpc](https://packages.gentoo.org/useflags/rpc) | Enable quota interaction via RPC | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [tcpd](https://packages.gentoo.org/useflags/tcpd) | Add support for TCP wrappers | 

### Emerge

`root #``emerge --ask sys-fs/quota`
## Configuration

### fstab

To enable quotas, some configuration is required by editing /etc/fstab and enabling usrquota and/or grpquota in the options for each file-system upon which quotas need to be managed. For journaled quota support, add the mount options jqfmt. Additional information regarding /etc/fstab and filesystem (quota) options can be found by referring to the [mount(8)](https://man.archlinux.org/man/mount.8.en) [man page. Quotas are set and edited by the quota tools which manage configuration files located in the root of each file-system named aquota.user, and aquota.group for new quota version formats and quota.user, quota.group for older formats.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`root #``$EDITOR /etc/fstab`
**`/etc/fstab`**

```
# <DEV>    <MNTPOINT> <FSTYPE> <MNTOPTIONS>                     <DUMP> <PASS>
# example line for just usrquota support
/dev/sda8  /mnt/eg1   ext4     noatime,usrquota                      0 2
# example line for grpquota 
/dev/sda9  /mnt/eg2   ext4     noatime,grpquota                      0 2
# example line for usrquota, grpquota and journaled quota support
/dev/sda10 /home      ext4     noatime,usrquota,grpquota,jqfmt=vfsv0 0 2
```
After /etc/fstab has been edited accordingly, the file-systems need to be remounted. Repeat the following mount command example below for every file-system that has been edited and enabled for quotas.

`root #``mount -o remount /home`
### Group quotas (local system)

It is possible to apply quotas to groups of users, if this is desired a group has to be present on the system to assign the quota to. This creates a new group quotagroup and adds two users to the group, assuming there are already users alice and bob. For creating new users and groups please refer to [Complete\_Handbook/Configuring\_the\_system#User\_account](https://wiki.gentoo.org/wiki/Complete_Handbook/Configuring_the_system#User_account) and [Knowledge\_Base:Adding\_a\_user\_to\_a\_group](https://wiki.gentoo.org/wiki/Knowledge_Base:Adding_a_user_to_a_group).

`root #````
groupadd quotagroup
```
`root #````
gpasswd --add alice quotagroup
```
`root #````
gpasswd --add bob quotagroup
```
### Creating the quota files

After /etc/fstab has been edited, the quota files need to be created with the [quotacheck(8)](https://man.archlinux.org/man/quotacheck.8.en) [command and the](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) `-c` or `--create-files` option. The following command creates the quota files as necessary for user and group quotas on the /home filesystem.

`root #````
quotacheck --user --group --create-files /home
```
### Enabling quotas

Gentoo provides an init script to run quotacheck and initialize quotas when the system starts. The file /etc/conf.d/quota can be edited and customized to provide different parameters to the [quotacheck(8)](https://man.archlinux.org/man/quotacheck.8.en) [and](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [quotaon(8)](https://man.archlinux.org/man/quotaon.8.en) [commands if necessary.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`root #``$EDITOR /etc/conf.d/quota`
**`/etc/conf.d/quota`**

```
# /etc/conf.d/quota: config file for /etc/init.d/quota
# Note: if your console is hooked up to a serial terminal,
# you prob want to drop the '-v' from the OPTS vars below.
# Run quotacheck ?
RUN_QUOTACHECK="yes"
# Options for quotacheck
QUOTACHECK_OPTS="-avug"
# Options for quotaon
QUOTAON_OPTS="-avug"
# Options for quotaoff
QUOTAOFF_OPTS="-avug"
```
After doing any customization to the /etc/conf.d/quota file, it's time to start the quota system. The following commands start the quota init script and adds it to the runlevel to persist when the system reboots.

`root #````
/etc/init.d/quota start
```
`root #````
rc-update add quota default
```
### Checking if quotas are enabled

To make sure whether quotas are enabled or not, the following command can be executed.

`root #``quotaon --print-state --all`
group quota on /home (/dev/sda10) is on
user quota on /home (/dev/sda10) is on

### Environment variables

?

### Files

/etc/quotagrpadmins /etc/quotatab

## Usage

The following procedures describe startup and general usage of the tools to administer quotas.

### Invocation

The Linux quota tools contain several commands to create and manage filesystem quotas. The following are some basic examples of putting the tools to use.

#### Init scripts

Gentoo provides an init script to check quotas and turn quotas on and off. Using the Gentoo init scripts are preferred over using quotaon / quotaoff manually unless it's known what is really being done.

Starting quotas through the Gentoo init script

`root #````
/etc/init.d/quota start
```
Stopping quotas through the Gentoo init script

`root #````
/etc/init.d/quota stop
```
#### repquota

Displaying summaries of quotas for filesystems

`root #``repquota -a`
#### setquota

Setting disk quotas on the command line is accomplished with the [setquota(8)](https://man.archlinux.org/man/setquota.8.en) [command. From the man page the usage states \<block-softlimit> \<block-hardlimit> \<inode-softlimit> \<inode-hardlimit> as parameters to be passed when setting quotas, these need to be balanced according to requirements. To unset a quota limit the value can be set at 0 (zero).](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

To calculate the amount of blocks per GB a command similar to the following can be used, replace 32 with the required GB for the quotas to be set if it's required to limit based on block (size) usage.

`user $``awk '/BLOCK_SIZE[[:blank:]]/{print 32 * $3 ** 2}' /usr/include/sys/mount.h`
Once the inode and block values have been decided, a quota can now be set. The following command sets a soft limit of 30GB, a hard limit of 32GB and removes inode limits for user alice on the /home file-system.

`root #``setquota -u alice 31457280 33554432 0 0 /home`
#### edquota

Editing user or group quotas is accomplished with the [edquota(8)](https://man.archlinux.org/man/edquota.8.en) [command, the user or group to be edited, and -a for all filesystems or specify the filesystem with the -f option. The following command will edit the quotas for user bob on the /home filesystem.
The set](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) `EDITOR` will be opened to begin editing quotas.

`root #``edquota -f /home bob`
Disk quotas for user bob (uid 1006):
  Filesystem    blocks       soft       hard     inodes     soft     hard
  /dev/sda10        28          0          0         11        0        0

Changing the values allows placing soft or hard limits on the amount of blocks or inodes for user bob, exiting the `EDITOR` and saving the file will apply the specified quotas.

#### quotastats

Querying quota statistics

`root #``quotastats`
#### quotaon & quotaoff

Turning on filesystem quotas for all filesystems

`root #``quotaon -a`
Turning off filesystem quotas for all filesystems

`root #``quotaoff -a`
#### quota

Displaying users disk usage and limits is accomplished with the [quota(1)](https://man.archlinux.org/man/quota.1.en) [command.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``quota`
## Removal

### Turning off quotas

The quotas for each filesystem should be turned off before removal and removed from the runlevel.

`root #````
/etc/init.d/quota stop
```
`root #````
rc-update del quota default
```
### Removing configuration

To clean up the system if quotas are not required any more perform the following steps

Edit /etc/fstab and remove quota options for filesystems

Remove the aquota.user, and aquota.group or quota.user, quota.group configuration files in the root of the filesystem.

### Unmerge

`root #``emerge --ask --depclean --verbose sys-fs/quota`
## See also

- [Ext2](https://wiki.gentoo.org/wiki/Ext2) - ext2 filesystem.
- [Ext3](https://wiki.gentoo.org/wiki/Ext3) - ext3 filesystem with journaling support.
- [Ext4](https://wiki.gentoo.org/wiki/Ext4) - Extended file system version 4.
- [XFS](https://wiki.gentoo.org/wiki/XFS) - A high-performance journaling filesystem that also provides a quota system.
- [JFS](https://wiki.gentoo.org/wiki/JFS) - 64-bit journaling filesystem created by IBM.
- [Centralized\_authentication\_using\_OpenLDAP](https://wiki.gentoo.org/wiki/Centralized_authentication_using_OpenLDAP) - Quotas can be used with LDAP
