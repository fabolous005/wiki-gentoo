<!-- source: https://wiki.gentoo.org/wiki/Swap | group: Gentoo Wiki (Main) | wiki-title: Swap -->
---
title: Swap
url: https://wiki.gentoo.org/wiki/Swap
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-27"
fingerprint: bc206c1b2de7a5d9
license: CC BY-SA 4.0
---

# Swap

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

In the Linux/Unix world, the term **swap** is generally used as a synonym for [memory paging](https://en.wikipedia.org/wiki/Memory_paging). Swap refers to both the act of moving memory pages between memory and a secondary storage.

Linux can use a combination of swap areas - multiple swap devices and/or swap files together. It is also possible to assign different priorities to swap areas.

However, swap space may not be necessary at all depending on the requirements for the system in question. For example, a laptop that [suspends to disk](https://wiki.gentoo.org/wiki/Suspend_and_hibernate) (hibernation) requires all pages in memory to be stored to disk, so swap is necessary in this case. Server systems equipped with large amount of memory running at a constant load might not require swap at all. For further details, see the dedicated [Knowledge Base](https://wiki.gentoo.org/wiki/Knowledge_Base:Is_swap_space_really_necessary) article.

The Gentoo Handbook recommends, as part of the installation process, a [helpful rule of thumb table](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks#What_about_swap_space.3F) if a user is unsure how much swap is needed. Swap can be created and activated at any time though if a mistake is made.

## Creation

### Swap Partition

Presuming /dev/sda2 is the partition available to be used for swap, first format the partition as swap.

`root #``mkswap /dev/sda2`
### Swap file

In order to work around the more rigid constraints of disk partitions, an alternative is to use an on-disk file as swap. Files can be located *inside* disk partitions. This allows flexibility to resize or move the swap space as necessary to meet the demands of the system without having to [repartition](https://wiki.gentoo.org/wiki/Partition) a disk.

Begin by allocating a new file to be used for the backing store of the swapfile. The size of this file will be the size of the swap space. Standard utilities can be used for this purpose such as fallocate from [sys-apps/util-linux](https://packages.gentoo.org/packages/sys-apps/util-linux):

`root #``fallocate -l 12GiB swapfile`
Then, restrict permissions on the file to root access only.

`root #``chmod 600 swapfile`
Then initialize the swapfile:

`root #``mkswap swapfile`
## Activation

Before swap can be used, it must be enabled with the swapon command. For swap partitions:

`root #``swapon /dev/sdb`
or, in case of a swap file:

`root #``swapon swapfile`
While manually activating the swap with swapon is fine, there are several methods to automate this process.

Typically swap is added to fstab so that it can be activated along with other file systems during the init process. To do this append a line (adjusting as necessary) to fstab:

**`/etc/fstab`**

Alternatively, on systemd it's possible to setup a swap unit<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>:

**`/etc/systemd/system/swapfile.swap`**

```
[Unit]
Description=Activate /swapfile
 
[Swap]
# Path to the swap file or device
What=/swapfile
# Optional: Set priority for the swap space
Priority=10
 
[Install]
# This tells systemd to start this unit as part of the main swap target
WantedBy=swap.target
```
Then activate the unit:

`root #``systemctl enable swapfile.swap --now`
The final method for automatically enabling swap is via the Discoverable Partitions Specification<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>; by setting the GUID Partition Table (GPT) UUID to the correct value (`0657fd6d-a4ab-43c4-84e5-0933c84b4f4f` aka `SD_GPT_SWAP`. If using Discoverable Partitions, all swap partitions on the disk containing the root partition are automatically enabled. This functionality is init-dependent, but is known to be supported by systemd.

## Advanced Swapping

### Encrypted swap with hibernate option

Assuming the desired goal is a LUKS encrypted swap partition with the ability to still be able to perform [hibernate](https://wiki.gentoo.org/wiki/Suspend_and_hibernate) (a.k.a. suspend to disk), the encrypted swap partition needs a known LUKS-key (keyfile, password, etc.). Early in the boot process, the user will be asked to enter the password. The kernel decides whether to regularly boot the system, or to load a hibernated RAM image from swap if the system was hibernated in the previous power state.

The creation of a LUKS encrypted swap partition is not different from any other LUKS encrypted partition, described in [creating an encrypted storage platform](https://wiki.gentoo.org/wiki/Dm-crypt#Creating_an_encrypted_storage_platform). Use mkswap instead of mkfs.\* to the LUKS encrypted partition to create SWAP. The UUID (i.e., `01b37ea8-74a5-4526-85c7-9fdf6dad34cb`), created when formatting as swap, is needed for the next step:

Tell the bootloader (GRUB) where to resume from if hibernated:

**`/etc/default/grub`**

In addition, (when using OpenRC with dracut, systemd is presumed to be now used for this section of the guide) dracut will need to be told the major and minor number of the associated LUKS device, so it can create an appropriate initrd:

`root #``lsblk`
..
└─sdb1                                          8:22   0    32G  0 part   
 └─luks-cc166689-4246-41ae-86e8-84705b81ecc2  253:1    0    32G  0 crypt \[SWAP\]

`root #````
echo 253:1 > /sys/power/resume
```
Finally, the initrd needs to be updated with these changes:

`root #````
dracut --force
```
Do not forget the regular configuration of swap in /etc/fstab and restart the system afterwards.

**`/etc/fstab`**

#### Using full disk encryption

When already using [full disk encryption](https://wiki.gentoo.org/wiki/Dm-crypt_full_disk_encryption#Dracut) with LUKS, a decryption password prompt will appear twice when booting; once to enter the LUKS-key for the system's root partition (as before) and once to decrypt the swap partition. It is possible to just decrypt the root partition and then use a LUKS keyfile, stored on the just now decrypted root partition, to decrypt the swap partition automatically. Details and an easy example of how to do this within /etc/default/grub, can be found on [kernel.org](https://mirrors.edge.kernel.org/pub/linux/utils/boot/dracut/dracut.html#_crypto_luks_key_on_removable_device_support).

### Encrypted swap file

It is best practice to encrypt swap files.

#### Creation

To create a 2 GiB encrypted swap file in the /opt directory, run:

`root #````
cd /opt
```
`root #````
fallocate -l 2GiB swapfile
```
`root #````
chmod 600 swapfile
```
`root #````
cryptsetup --type plain --key-file /dev/urandom open swapfile cryptswap
```
`root #````
mkswap /dev/mapper/cryptswap
```
`root #````
swapon /dev/mapper/cryptswap
```
`root #``swapon --show`
**`/etc/fstab`**

#### Activation (systemd)

**`/etc/crypttab`**

#### Activation (OpenRC)

**`/etc/conf.d/dmcrypt`**

Enable the *dmcrypt* service:

`root #``rc-update add dmcrypt boot`
If the dm-crypt daemon fails to create the swap at boot, for example, with the following error:

mkswap: unable to erase bootbits sectors

the swap can be created manually. Disable the dm-crypt daemon and revert the changes made to /etc/fstab and /etc/conf.d/dmcrypt.

Create the shell script `01-swap.start` in [/etc/local.d](https://wiki.gentoo.org/wiki//etc/local.d):

**`/etc/local.d/01-swap.start`**

The cipher, key size and sector size of the underlying LUKS device can be displayed by executing the following command:

`root #``cryptsetup status /dev/mapper/<decrypted_partition>`
Next, create `01-swap.stop`:

**`/etc/local.d/01-swap.stop`**

Finally, make the scripts executable:

`root #``chmod +x /etc/local.d/01-swap.start``root #``chmod +x /etc/local.d/01-swap.stop`
The swap will be created after a reboot.

### OpenRC configuration

When using swap files which are not on the root filesystem, the service ordering in OpenRC should be changed via /etc/conf.d/swap:

**`/etc/conf.d/swap`**

```
# If you are only using local swap partitions, you should not change
# this file. Otherwise, you need to uncomment the below rc_before line
# followed by the appropriate rc_need line.
rc_before="!localmount"
#
# If you are using swap files stored on local file systems, uncomment
# this line.
rc_need="localmount"
#
# If you are using swap files stored on network file systems or swap
# partitions stored on network block devices such as iSCSI, uncomment
# this line.
#rc_need="netmount"
```
## Performance tuning

### Prioritization

It is possible to prioritize different swap areas by assigning priority (an integer from 0 to 32767). Higher-priority swap areas are used first. Lower-priority areas are used after exhausting the higher ones. Areas having the same priority are used in a round-robin fashion<sup>[\[6\]](https://wiki.gentoo.org#cite_note-6)</sup>.

The priority can be used for systems using a combination of fast ([ZRAM](https://wiki.gentoo.org/wiki/Zram) or [NVMe](https://wiki.gentoo.org/wiki/NVMe)-based devices) and slow swap areas ([HDD](https://wiki.gentoo.org/wiki/HDD)-based devices) to prioritize the former before the latter ones.

For example, prioritizing a fast swap device /dev/nvme0n1 before a regular swap file /swapfile using /etc/fstab:

**`/etc/fstab`**

**Swap prioritization example**

### Swappiness

Kernel allows tuning of the swap usage via [sysctl](https://wiki.gentoo.org/wiki/Sysctl) parameters allowing to adapt the swapping for various workloads.

The `vm.swappiness` parameter controls the ratio between kernel willingness to reclaim file-backed ("memory cached" parts files) and anonymous memory pages (application heap and stack) back to their respective backing storage. The value range on recent kernels is 0-200, while the default is 60<sup>[\[7\]](https://wiki.gentoo.org#cite_note-7)</sup>. Values lower than the default represent a preference of keeping the application-related anonymous memory pages in memory at the expense of file-backed pages and vice versa.

The current value can be displayed by:

`user $``sysctl vm.swappiness`
vm.swappiness = 60

The desired swappiness value can be persistently set via:

**`/etc/sysctl.d/90-swappiness.conf`**

**Reduced system swappiness**

## See also

- [Filesystem](https://wiki.gentoo.org/wiki/Filesystem) — a means to organize data to be retained after a program terminates.
- [Zram](https://wiki.gentoo.org/wiki/Zram) — a [Linux kernel](https://wiki.gentoo.org/wiki/Kernel) feature and set of userspace tools for creating compressible RAM-based block devices.
- [Zswap](https://wiki.gentoo.org/wiki/Zswap) — a lightweight compressed cache for swap pages.

## External resources

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [Swapfile — BTRFS  documentation](https://btrfs.readthedocs.io/en/latest/Swapfile.html)
2. [↑](https://wiki.gentoo.org#cite_ref-2) [Swapfile — BTRFS documentation, version 6.1 update](https://btrfs.readthedocs.io/en/latest/Swapfile.html#:~:text=Since%20version%206%2E1%20it%E2%80%99s%20possible%20to%20create%20the%20swapfile%20in%20a%20single%20command%20%28except%20the%20activation%29%3A)
3. [↑](https://wiki.gentoo.org#cite_ref-3) [Systemd Documentation - systemd.swap](https://www.freedesktop.org/software/systemd/man/latest/systemd.swap.html).
4. [↑](https://wiki.gentoo.org#cite_ref-4) [Discoverable Partitions Specification](https://uapi-group.org/specifications/specs/discoverable_partitions_specification/).
5. [↑](https://wiki.gentoo.org#cite_ref-5) [https://gitlab.com/cryptsetup/cryptsetup/-/blob/main/FAQ.md](https://gitlab.com/cryptsetup/cryptsetup/-/blob/main/FAQ.md)
6. [↑](https://wiki.gentoo.org#cite_ref-6) [swapon(2) - Linux man page](https://linux.die.net/man/2/swapon), die.net. Retrieved on: October 23, 2022
7. [↑](https://wiki.gentoo.org#cite_ref-7) [Documentation for /proc/sys/vm/ — The Linux Kernel  documentation](https://docs.kernel.org/admin-guide/sysctl/vm.html), kernel.org. Retrieved on October 23, 2022
