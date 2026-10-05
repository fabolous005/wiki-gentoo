<!-- source: https://wiki.gentoo.org/wiki/Replace_a_Linux_installation_with_Gentoo_in-place | group: Gentoo Wiki (Main) | wiki-title: Replace a Linux installation with Gentoo in-place -->
---
title: Replace a Linux installation with Gentoo in-place
url: https://wiki.gentoo.org/wiki/Replace_a_Linux_installation_with_Gentoo_in-place
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2014-07-26"
fingerprint: "904db927ce832b24"
license: CC BY-SA 4.0
---

# Replace a Linux installation with Gentoo in-place

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This document gives an idea of the steps that need to be taken to replace a running linux system with Gentoo. Based on [Jordan Ritter's article on replacing RHEL9 with Gentoo](http://www.darkridge.com/~jpr5/doc/rh-gentoo.html).

### Prepare the system

We're going to need bzip2 for the stage3.

`root #````
apt-get install bzip2
```
We're also about to overwrite fstab, so save it to /tmp.

`root #````
cp /etc/fstab /tmp/fstab
```
Mark all present files as if they were modified at the beginning of time (which was in 1970) so that we can remove them later on. Modprobe is temporarily removed as it might potentially slow down this proccess.

`root #````
mv /sbin/modprobe /sbin/.modprobe
```
`root #````
find / -print0 | xargs -0 touch -d "1970-01-01 00:00 +0000" -m
```
`root #````
mv /sbin/.modprobe /sbin/modprobe
```
### Overwrite root with Gentoo

Extract the latest stage3 tarball over the current system.

`root #````
tar xjpf /tmp/stage3* -C /
```
Gentoo's /lib and /usr/lib are symlinks and tar if tar warned you about this, you need to fix them.

`root #````
mv /lib     /lib_     && ln -s /lib64     /lib
```
`root #````
mv /usr/lib /usr/lib_ && ln -s /usr/lib64 /usr/lib
```
Recover fstab from /tmp as it has been overwritten by tar. You can also copy it from /etc/mtab.

`root #````
cp /tmp/fstab /etc/fstab
```
### Bring the Gentoo system to a bootable state

You will want to be able to log in to your new system even if network/sshd fails, so set a root password.

`root #````
passwd
```
Enable your network interfaces.

`root #````
cd /etc/init.d
```
`root #````
ln -s net.lo net.eth0
```
`root #````
vim /etc/conf.d/net
```
`root #````
rc-update add net.eth0
```
Prevent any network device renaming surprises.

`root #````
ln -s /dev/null /etc/udev/rules.d/80-net-name-slot.rules
```
Fetch dhcpcd if you're going to be using DHCP. Otherwise, set up your interfaces manually.

`root #````
emerge-webrsync
```
`root #````
emerge dhcpcd
```
Set the timezone.

`root #````
TIMEZONE=Continent/City
```
`root #````
cp /usr/share/zoneinfo/$TIMEZONE /etc/localtime
```
`root #````
echo $TIMEZONE > /etc/timezone
```
Enable sshd for remote access

`root #````
rc-update add sshd
```
Feel free to test the system and finally reboot. You might have to use -f.

`root #````
reboot
```
### Remove anything that was left behind by the previous OS

If everything is now working as expected, go ahead and delete all the old files. /home and /root are spared.

`root #````
rm -rf /lib_ /usr/lib_
```
`root #````
find /bin /sbin /etc /lib /usr /var -newermt "1970-01-01" ! -newermt "1970-01-02" -print0 | xargs -0 rm -rf
```
