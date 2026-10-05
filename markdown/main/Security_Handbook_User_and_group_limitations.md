<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/User_and_group_limitations | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/User and group limitations -->
---
title: Security Handbook/User and group limitations
url: https://wiki.gentoo.org/wiki/Security_Handbook/User_and_group_limitations
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-22"
fingerprint: a0112c38afab2d84
license: CC BY-SA 4.0
---

# Security Handbook/User and group limitations

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This section provides detail on controlling the system's resource usage of users via limits and quotas.

Controlling system resource usage can be very effective when trying to prevent a local Denial of Service (DoS) or restricting the maximum allowed logins for a group or user. However, settings that are too strict will impede the system's behavior, so make sure each setting is sanity checked before implemented.

**`/etc/security/limits.conf`**

```
*    soft core 0
*    hard core 0
*    hard nproc 15
*    hard rss 10000
*    -    maxlogins 2
@dev hard core 100000
@dev soft nproc 20
@dev hard nproc 35
@dev -    maxlogins 10
```
Considering removing a user before setting `nproc` or `maxlogins` to `0`. The example above sets the group dev settings for processes, core file and maxlogins. The rest are set to the default values.

/etc/limits is very similar to the limit file found at /etc/security/limits.conf. The difference between the files is the format and that it only works on users or wild cards (not groups). Let's have a look at a sample configuration:

**`/etc/limits`**

```
   L2 C0 U15 R10000
kn L10 C100000 U35
```
Here we set the default settings and a specific setting for the user kn. Limits are part of the [sys-apps/shadow](https://packages.gentoo.org/packages/sys-apps/shadow) package. It is not necessary to set any limits in this file if the `pam` USE flag has been enabled in /etc/portage/make.conf.

Putting quotas on a file system restricts disk usage on a per-user or per-group basis. Quotas are enabled in the kernel and added to a mount point in /etc/fstab. The kernel option is enabled in the kernel configuration under File systems → Quota support Apply the following settings, rebuild the kernel and reboot using the new kernel.

Start by installing quotas with emerge sys-fs/quota. Then modify /etc/fstab and add `usrquota` and `grpquota` to the partitions to be restricted, like in the example below:

**`/etc/fstab`**

```
 /boot ext2 noauto,noatime 1 1
/dev/sda2 none swap sw 0 0
/dev/sda3 / ext3 notail,noatime 0 0
/dev/sda4 /tmp ext3 noatime,nodev,nosuid,noexec,usrquota,grpquota 0 0
/dev/sda5 /var ext3 noatime,nodev,usrquota,grpquota 0 0
/dev/sda6 /home ext3 noatime,nodev,nosuid,usrquota,grpquota 0 0
/dev/sda7 /usr ext3 notail,noatime,nodev,ro 0 0
/dev/cdroms/cdrom0 /mnt/cdrom iso9660 noauto,ro 0 0
proc /proc proc defaults 0 0
```
On every partition that have quotas enabled, create the quota files (aquota.user and aquota.group) and place them in the root of the partition:

`root #````
touch /tmp/aquota.user
```
`root #````
touch /tmp/aquota.group
```
`root #````
chmod 600 /tmp/aquota.user
```
`root #````
chmod 600 /tmp/aquota.group
```
This step has to be done on every partition where quotas are enabled.

On OpenRC systems, after adding and configuring the quota files, be sure to add the quota script to the boot run level.

Add quota to the boot runlevel:

`root #``rc-update add quota boot`
Quotas can be checked once a week by adding the following line to /etc/crontab:

**`/etc/crontab`**

```
0 3 * * 0 /usr/sbin/quotacheck -avug.
```
After rebooting, it is time to setup the quotas for users and groups. edquota -u kn will start the editor defined in `EDITOR` environment variable (default is nano) and allow editing quotas of the user kn. edquota -g will do the same thing for groups.

`edquota -u kn`
Quotas for user kn: /dev/sda4: blocks in use: 2594, limits (soft = 5000, hard = 6500)

inodes in use: 356, limits (soft = 1000, hard = 1500)
For more detail read man edquota or the [Quota mini howto](https://www.tldp.org/HOWTO/Quota.html).

If an organizational security policy states that users should change their password every other week, change the value of `PASS_MAX_DAYS` variable to `14` and `PASS_WARN_AGE` variable to `7`. It is recommended password aging be implemented since brute force methods can find any password, given enough time. Sysadmins are also encouraged to set `LOG_OK_LOGINS` variable to `yes`.

The access.conf file is also part of the [sys-libs/pam](https://packages.gentoo.org/packages/sys-libs/pam) package, which provides a login access control table. This table is used to control who can and cannot login based on user name, group name or host name. By default, all users on the system are allowed to login, so the file consists only of comments and examples. Whether securing a server or workstation, we recommend this file be secured so no one other the sysadmin has access to the console.

**`/etc/security/access.conf`**

```
-:ALL EXCEPT wheel sync:console
-:wheel:ALL EXCEPT LOCAL .gentoo.org
```
This will setup login access so members of the wheel group can login locally or from the gentoo.org domain. Maybe too paranoid, but better to be safe than sorry.
