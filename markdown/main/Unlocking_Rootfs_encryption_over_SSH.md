<!-- source: https://wiki.gentoo.org/wiki/Unlocking_Rootfs_encryption_over_SSH | group: Gentoo Wiki (Main) | wiki-title: Unlocking Rootfs encryption over SSH -->
---
title: Unlocking Rootfs encryption over SSH
url: https://wiki.gentoo.org/wiki/Unlocking_Rootfs_encryption_over_SSH
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-16"
fingerprint: "9e831679190fa3d3"
license: CC BY-SA 4.0
---

# Unlocking Rootfs encryption over SSH

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

If you have followed [Rootfs encryption](https://wiki.gentoo.org/wiki/Rootfs_encryption) and would like to unlock the root device remotely you can using [Dropbear](https://wiki.gentoo.org/wiki/Dropbear).

This document assumes you have configured encryption using [Dracut](https://wiki.gentoo.org/wiki/Rootfs_encryption#Dracut) with [Systemd](https://wiki.gentoo.org/wiki/Rootfs_encryption#Systemd) and booting using [Grub](https://wiki.gentoo.org/wiki/Grub).

## Installation

### Emerge

Install dropbear

`root #``emerge --ask net-misc/dropbear`
### Configure dropbear

Generate the dropbear server host keys

`root #``dropbear -R`
Edit /etc/dropbear/authorized\_keys with the SSH public key(s) you will use to access the machine.

### Dracut module

Create the module directory

`root #``mkdir /usr/lib/dracut/modules.d/50dropbear`
Create the script which starts dropbear replace 2222 with your port of choice.

**`/usr/lib/dracut/modules.d/50dropbear/dropbear-init.sh`**

```
#!/bin/sh 
echo "Starting Dropbear SSH server..." 
dropbear -E -s -j -k -p 2222 &
```
Create the script used to unlock the disks.

**`/usr/lib/dracut/modules.d/50dropbear/unlock.sh`**

```
#!/bin/sh
for f in $(systemctl list-units | awk '/systemd.*activating/ {print $2}')
do       
    systemctl start "$f"
done
```
Create the script which will configure the module.

**`/usr/lib/dracut/modules.d/50dropbear/module-setup.sh`**

```
#!/bin/sh 
                                                
check() {                      
    return 0
}
depends() {
    echo "network"
}
install() {
    inst dropbear
    inst /etc/dropbear/authorized_keys /root/.ssh/authorized_keys
    inst /etc/dropbear/dropbear_ecdsa_host_key /etc/dropbear/dropbear_ecdsa_host_key
    inst /etc/dropbear/dropbear_ed25519_host_key /etc/dropbear/dropbear_ed25519_host_key
    inst /etc/dropbear/dropbear_rsa_host_key /etc/dropbear/dropbear_rsa_host_key
    inst /usr/lib/dracut/modules.d/50dropbear/unlock.sh /bin/unlock
    inst_hook initqueue 50 "$moddir/dropbear-init.sh"
}
```
Allow executing the scripts

`root #``chmod u+x /usr/lib/dracut/modules.d/50dropbear/dropbear-init.sh /usr/lib/dracut/modules.d/50dropbear/unlock.sh /usr/lib/dracut/modules.d/50dropbear/module-setup.sh` Updated the initramfs

`root #``dracut --force`
### Grub

Edit /etc/default/grub and configure the network parameters, this assumes you've already added rd.luks.uuid

**`/etd/default/grub`**

Update the grub config

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
## Usage

SSH into the machine

`user $` `ssh -p 2222 root@xxx.xxx.xxx.xxx`
Then unlock the drive

`root #``unlock`
-sh-5.2# unlock 
🔐 Please enter passphrase for disk DISK (luks-fbb4fc25-3fa7-4ff7-aeca-b867be758f80): (press TAB for no echo)

Dropbear will automatically close the connection once the passphrase is accepted.
