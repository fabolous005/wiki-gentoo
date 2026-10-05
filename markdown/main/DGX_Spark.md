<!-- source: https://wiki.gentoo.org/wiki/DGX_Spark | group: Gentoo Wiki (Main) | wiki-title: DGX Spark -->
---
title: DGX Spark
url: https://wiki.gentoo.org/wiki/DGX_Spark
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-11"
fingerprint: cfb10c2e2a8ee52e
license: CC BY-SA 4.0
---

# DGX Spark

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**DGX Spark** is a compact aarch64 (arm64) AI development computer from NVIDIA, built around the GB10. It ships from the factory running "DGX OS", an Ubuntu-based distribution. This page documents installing Gentoo on it.

## Hardware

The output from the lspci -nn command:

`root #``lspci -nn`
0000:00:00.0 PCI bridge \[0604\]: NVIDIA Corporation GB10 GEN5 X4 PCIe host \[10de:22ce\] (rev 01)
0000:01:00.0 Ethernet controller \[0200\]: Mellanox Technologies MT2910 Family \[ConnectX-7\] \[15b3:1021\]
0000:01:00.1 Ethernet controller \[0200\]: Mellanox Technologies MT2910 Family \[ConnectX-7\] \[15b3:1021\]
0002:00:00.0 PCI bridge \[0604\]: NVIDIA Corporation GB10 GEN5 X4 PCIe host \[10de:22ce\] (rev 01)
0002:01:00.0 Ethernet controller \[0200\]: Mellanox Technologies MT2910 Family \[ConnectX-7\] \[15b3:1021\]
0002:01:00.1 Ethernet controller \[0200\]: Mellanox Technologies MT2910 Family \[ConnectX-7\] \[15b3:1021\]
0004:00:00.0 PCI bridge \[0604\]: NVIDIA Corporation GB10 GEN5 X4 PCIe host \[10de:22ce\] (rev 01)
0004:01:00.0 Non-Volatile memory controller \[0108\]: Samsung Electronics Co Ltd NVMe SSD 9100 PRO \[PM9E1\] \[144d:a810\]
0007:00:00.0 PCI bridge \[0604\]: NVIDIA Corporation GB10 GEN4 X1 PCIe host \[10de:22d0\] (rev 01)
0007:01:00.0 Ethernet controller \[0200\]: Realtek Semiconductor Co., Ltd. RTL8127 10GbE Controller \[10ec:8127\] (rev 05)
0009:00:00.0 PCI bridge \[0604\]: NVIDIA Corporation GB10 GEN4 X1 PCIe host \[10de:22d0\] (rev 01)
0009:01:00.0 Network controller \[0280\]: MEDIATEK Corp. MT7925 802.11be 160MHz 2x2 PCIe Wireless Network Adapter \[Filogic 360\] \[14c3:7925\]
000f:00:00.0 PCI bridge \[0604\]: NVIDIA Corporation GB20B PCI bridge \[10de:22d1\]
000f:01:00.0 VGA compatible controller \[0300\]: NVIDIA Corporation GB20B \[GB10\] \[10de:2e12\] (rev a1)

## Kernel requirement

GB10 is new enough that mainline Linux lacks its hardware-enablement patches. NVIDIA ships a patched Ubuntu kernel (the `linux-nvidia` flavour) as part of DGX OS; without those patches, USB (including HID) does not come up under a generic kernel, even though GRUB/UEFI had no trouble with the same USB hardware moments earlier.

This is corroborated by the community project [RageLtd/linux-dgx-spark](https://github.com/RageLtd/linux-dgx-spark) (an Arch Linux package), which states plainly that "a standard kernel lacks the necessary patches" and sources from "NVIDIA's Ubuntu kernel fork published on Launchpad".

GB10 needs these two parameters on every boot:

## Installation (chroot method from DGX OS)

This follows the same shape as the Handbook's "installing from an existing Linux system" method, using the stock DGX OS as the host environment.

At the beginning, you can keep DGX OS and install Gentoo into a second disk or resize/shrink the existing partitions first, until you are totally sure the installation can boot the Spark.

Follow the Handbook's [Preparing the disks](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Media) section as normal (identical on arm64): an EFI System Partition, swap, and a root filesystem.

Mount the root and boot partitions under `/mnt/gentoo` as usual before continuing.

### Install stage3 and chroot

Follow the regular [arm64 Handbook](https://wiki.gentoo.org/index.php?title=Handbook:ARM64&action=edit&redlink=1) steps to fetch and extract an official stage3, bind-mount `/proc /sys /dev /run`, and chroot in.

### Install the kernel

Add the public overlay that carries GB10-specific kernel packages:

Two options are provided, both under `sys-kernel/`:

- sys-kernel/dgx-spark-kernel-bin
- Repackages Ubuntu's *signed* `linux-image`/`linux-modules` packages directly from `ports.ubuntu.com` - this is the exact binary DGX OS itself boots and validates on this hardware. Zero compile time. Recommended for most users.
- sys-kernel/dgx-spark-kernel
- Assembles the same Ubuntu `linux-nvidia` source package (orig tarball plus the Ubuntu/NVIDIA Debian diff, fetched from Launchpad) as a normal buildable Gentoo kernel-sources package, with a known-good defconfig contributed by the RageLtd/linux-dgx-spark project. Configurable, but you compile it yourself.

`root #``emerge --ask sys-kernel/dgx-spark-kernel-bin`
### Bootloader

`grub-install` and a hand-written `grub.cfg` work as normal for arm64-efi; the only DGX Spark-specific requirement is including the kernel command line parameters from [#Required kernel command line parameters](https://wiki.gentoo.org#Required_kernel_command_line_parameters):

`root #``grub-install --target=arm64-efi --efi-directory=/boot`
## Networking

The onboard Wi-Fi is a MediaTek MT7925 (`mt7925e` driver).

## CPU tuning

The Cortex-X925 in this SoC reports a rich ARMv9 feature set. The subset that corresponds to actual Gentoo `CPU_FLAGS_ARM` values (used by e.g. `dev-libs/openssl`, `sys-libs/zlib-ng` for hardware-accelerated crypto/CRC code paths):

```
# /etc/portage/package.use/00cpu-flags
*/* CPU_FLAGS_ARM: aes crc32 pmull sha1 sha2 sha3 sm3 sm4 sve sve2
```
## See also

- [RageLtd/linux-dgx-spark](https://github.com/RageLtd/linux-dgx-spark) - the community project this page's kernel-sources package builds on
- [NVIDIA's own DGX Spark documentation](https://docs.nvidia.com/dgx/dgx-spark/)
