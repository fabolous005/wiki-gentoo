<!-- source: https://wiki.gentoo.org/wiki/Asus_Ascent_GX10 | group: Gentoo Wiki (Main) | wiki-title: Asus Ascent GX10 -->
---
title: Asus Ascent GX10
url: https://wiki.gentoo.org/wiki/Asus_Ascent_GX10
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-24"
fingerprint: "6f0f0954d5e739c4"
license: CC BY-SA 4.0
---

# Asus Ascent GX10

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Short info on how to get Gentoo running on the Asus Ascent GX10

The current (as of 2026-06-29) install ISO does not boot. Looks like the USB-Drivers are not loaded correctly. I get dropped into a dracut shell, the USB keyboard does not work, so I assume the USB stick is not found as well, so no boot.

Booting with a NixOs installation image works:
[https://github.com/nix-community/nixos-images/](https://github.com/nix-community/nixos-images/)
The image for aarch64

From there, follow the x86\_64 handbook. small differences from the handbook:

**`/etc/portage/make.conf`**

**enable flags**

```
/etc/portage/make.conf:
MAKEOPTS="-j21"
ACCEPT_KEYWORDS="~arm64"
CUDA_TARGETS_SM="121"
CUDA_ARCH_BIN="12.1"
```
I used the kernel config from the pre-installed device but with the current active gentoo-sources/kernel 7.1.1 as of writing. I had issue when using dracut/initrd, so I modified the config to include the needed drivers (nvme) and filesystem (xfs) directly into the kernel, not as a modules and there it was, first boot.

**Enabling build in drivers for nvme and filesystem**

```
Device Drivers --->
  NVME Support --->
    <*> NVM Express block device
    <*> NVMe multipath support
    <*> NVMe hardware monitoring
    <*> NVMe Target support
      <*> NVMe Target Passthrough support
      <*> NVMe loopback device support
File Systems --->
  <*> XFS filesystem support
    <*> Support deprecated V4 (crc=0) format
    <*> Support deprecated case-insensitive ascii (ascii-ci=1) format
    <*> XFS Quota support
    <*> XFS POSIX ACL support
    <*> XFS Realtime subvolume support
```
Resume usage like any other gentoo installation.
