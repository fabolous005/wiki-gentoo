<!-- source: https://wiki.gentoo.org/wiki/Efivarfs | group: Gentoo Wiki (Main) | wiki-title: Efivarfs -->
---
title: efivarfs
url: https://wiki.gentoo.org/wiki/Efivarfs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-07"
fingerprint: cc9ff9b2a9b71e64
license: CC BY-SA 4.0
---

# efivarfs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

The efivarfs is a filesystem in the Linux [kernel](https://wiki.gentoo.org/wiki/Kernel) that enables users to create, delete, and modify [(U)EFI](https://wiki.gentoo.org/wiki/UEFI) variables. efivarfs is typically (and automatically) mounted to /sys/firmware/efi/efivars; if it needs to be mounted manually the following command can be used:

`root #``mount -t efivarfs none /sys/firmware/efi/efivars`
### Introduction

efivarfs was created to address the shortcomings of using entries in [sysfs](https://wiki.gentoo.org/wiki/Sysfs) to maintain EFI variables: the old sysfs EFI variables code only supported variables of up to 1024 bytes. This was originally a limitation in version 0.99 of the EFI specification which was was removed before any full releases<sup>[\[1\]](https://wiki.gentoo.org#cite_note-Kernel_Docs-1)</sup>.

### Kernel

`CONFIG_EFIVAR_FS` support needs to be enabled:

**Enable EFI Variable filesystem support**

```
Device Drivers  --->
  Firmware Drivers  --->
    EFI (Extensible Firmware Interface) Support --->
      [ ] Disable EFI runtime services support by default 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_EFI_DISABLE_RUNTIME</code> to find this item.
File systems  --->
  Pseudo filesystems  --->
    <*> EFI Variable filesystem [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_EFIVAR_FS</code> to find this item.
## Troubleshooting

### EFI-CSM: BIOS mode

On [x86](https://en.wikipedia.org/wiki/x86) UEFI replaced the legacy [BIOS](https://wiki.gentoo.org/wiki/BIOS), to enable backwards compatibility during the transitional period, UEFI on x86 included a BIOS emulation, called *Compatibility Support Module (CSM)*. When EFI-CSM is activated and in use, it will behave like a legacy BIOS, including hiding UEFI facilities from the operating system.

In most cases is a safe assumption that a computer or laptop manufactured after 2020 is a pure UEFI system that cannot be in BIOS mode; as an additional point of interest when Secure Boot is enabled EFI-CSM is automatically deactivated.

All (U)EFI functions can be disabled with the kernel parameter `efi=noruntime`, or activated with `efi=runtime`. A kernel booted without EFI runtime functions will not be able to alter any EFI settings and variables, including the boot configuration.

## See also

- [Efibootmgr](https://wiki.gentoo.org/wiki/Efibootmgr) — a tool for managing [UEFI](https://wiki.gentoo.org/wiki/UEFI) boot entries.
