<!-- source: https://wiki.gentoo.org/wiki/AMD_microcode | group: Gentoo Wiki (Main) | wiki-title: AMD microcode -->
---
title: AMD microcode
url: https://wiki.gentoo.org/wiki/AMD_microcode
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-24"
fingerprint: "2716d87d42b603a5"
license: CC BY-SA 4.0
---

# AMD microcode

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes updating the [microcode](https://wiki.gentoo.org/wiki/Microcode) for [AMD](https://wiki.gentoo.org/wiki/AMD) processors. In most cases the firmware of a system, i.e. the BIOS or UEFI, will contain updated processor microcode. It is recommended to always update the system firmware, because vendors will also incorporate new microcode provided by AMD with their BIOS updates (in the changelog this is often called "AGESA update"). It is however also possible to have the Linux kernel load the same updated microcode.

## Installation

### Kernel configuration

For the Linux kernel to support CPU microcode loading, the following must be enabled if a kernel lower than version 6.6 is used. With kernel version 6.6 this is automatically activated and can no longer be selected [\[1\]](https://wiki.gentoo.org#cite_note-1)<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>:

**Enable AMD microcode loading support up to kernel version 6.5**

```
Processor type and features  --->
    [*] CPU microcode loading support
    [*]   AMD microcode loading support
```
For the kernel to be able to load any firmware (including microcode), the *firmware loading facility* option must also be enabled:

**Enable firmware loading facility**

```
Device Drivers  --->
    Generic Driver Options 
        Firmware loader --->
        {*} Firmware loading facility
```
When microcode is available and the kernel is configured, it will update microcode automatically. In most modern configurations the root partition (where the /lib/firmware directory is located) will be mounted during the boot process. For this reason, to be able to update the microcode as soon as possible, it is also necessary to include the microcode firmware blobs either in the kernel image or the initrd/initramfs.

For further instructions, refer to the [Microcode](https://wiki.gentoo.org/wiki/Microcode) article.

### Firmware blob files installation

Microcode updates for AMD processors are provided by the [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package.

`root #``emerge --ask sys-kernel/linux-firmware`
### Microcode firmware files

There are different firmware files for different AMD CPU families. The CPU family identification of the current system can be obtained from /proc/cpuinfo:

`user $``grep -F -m 1 "cpu family" /proc/cpuinfo`
cpu family      : 22

In this example the CPU belongs to the AMD CPU family **22**.

The following table helps to identify the right firmware blob file for a given CPU family identificator:

| Firmware blob file | Decimal | Hexadecimal | Family name | Year | Examples | 
|---|---|---|---|---|---|
| amd-ucode/microcode\_amd.bin | **16** | `10h` | K10 | 2007 | Phenom, Phenom II, Athlon II | 
| amd-ucode/microcode\_amd.bin | **17** | `11h` | Turion | 2008 | Athlon X2, Sempron X2, Turion X2 | 
| amd-ucode/microcode\_amd.bin | **18** | `12h` | Llano, Fusion | 2009 | A- and E2-Series APU with Radeon HD graphics, Athlon II, Sempron X2 | 
| amd-ucode/microcode\_amd.bin | **20** | `14h` | Bobcat | 2011 | C- and E-Series APU with Radeon HD graphics | 
| amd-ucode/microcode\_amd\_fam15h.bin | **21** | `15h` | Bulldozer, Piledriver, Steamroller, Excavator | 2011 | FX series, A-Series APU with Radeon HD graphics, Opteron 6200/6300 | 
| amd-ucode/microcode\_amd\_fam16h.bin | **22** | `16h` | Jaguar, Puma | 2013 | A-series and E-Series APU with Radeon HD graphics | 
| amd-ucode/microcode\_amd\_fam17h.bin | **23** | `17h` | [Zen](<https://en.wikipedia.org/wiki/Zen_(microarchitecture)>), Zen+, [Zen 2](https://en.wikipedia.org/wiki/Zen_2) | 2017 | [Ryzen](https://wiki.gentoo.org/wiki/Ryzen) 1000-5000 series, Threadripper, EPYC 7xx1/7xx2 | 
| amd/amd\_sev\_fam17h\_model0xh.sbin | **23** | `17h` | EPYC 7xx1 <sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> (Zen 1) | 2017 | SEV (Secure Encrypted Virtualization) firmware update for models in the range `00h` to `0fh`<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup> | 
| amd/amd\_sev\_fam17h\_model3xh.sbin | **23** | `17h` | EPYC 7xx2 <sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup> (Zen 2) | 2019 | SEV firmware update for models in the range `30h` to `3fh`<sup>[\[6\]](https://wiki.gentoo.org#cite_note-6)</sup> | 
| amd-ucode/microcode\_amd\_fam19h.bin | **25** | `19h` | [Zen 3](https://en.wikipedia.org/wiki/Zen_3) and [Zen 4](https://en.wikipedia.org/wiki/Zen_4) | 2021 | Ryzen 5000 series (not all models), Ryzen 9 7900X & 7950X series, EPYC 7xx3 | 
| amd/amd\_sev\_fam19h\_model0xh.sbin | **25** | `19h` | EPYC 7xx3 <sup>[\[7\]](https://wiki.gentoo.org#cite_note-7)</sup> (Zen 3) | 2021 | SEV firmware update for models in the range `00h` to `0fh`<sup>[\[8\]](https://wiki.gentoo.org#cite_note-8)</sup> | 
| amd/amd\_sev\_fam1ah\_model0xh.sbin | **26** | `1ah` | EPYC 9005 <sup>[\[9\]](https://wiki.gentoo.org#cite_note-9)</sup> (Zen 5) | 2025 | SEV firmware update for models in the range `00h` to `0fh`<sup>[\[10\]](https://wiki.gentoo.org#cite_note-10)</sup> | 

### Supplying the microcode files to the kernel

It is possible to incorporate the microcode firmware blob files for all AMD processors in the Linux kernel, but it should be considered that this will enlarge the kernel image by the combined size of those files. For 6.6 and newer kernels, the menuconfig option is "Build named firmware blobs into kernel binary".

**All AMD firmware blobs in-kernel**

```
Device Drivers  --->
    Generic Driver Options 
        Firmware loader --->
        {*} Firmware loading facility
        (amd-ucode/microcode_amd.bin amd-ucode/microcode_amd_fam15h.bin amd-ucode/microcode_amd_fam16h.bin amd-ucode/microcode_amd_fam17h.bin amd/amd_sev_fam17h_model0xh.sbin amd/amd_sev_fam17h_model3xh.sbin amd-ucode/microcode_amd_fam19h.bin amd/amd_sev_fam19h_model0xh.sbin) External firmware blobs to build into the kernel binary
        (/lib/firmware) Firmware blobs root directory
```
For in-kernel microcode it may be preferable to only include the firmware blob file(s) specific to the identified CPU. For example, for the EPYC 7xx1 CPU family this would be `CONFIG_EXTRA_FIRMWARE="amd-ucode/microcode_amd_fam17h.bin amd/amd_sev_fam17h_model0xh.sbin"`.

Instead of in-kernel, the firmware blob files can also be included in an (additional) initrd/initramfs.

**Enable kernel support for loading microcode via initramfs/initrd**

```
General setup  --- >
    [*] Initial RAM filesystem and RAM disk (initramfs/initrd) support (BLK_DEV_INITRD)
```
This method has the advantage that only the specific initrd/initramfs must be rebuild in case of a firmware files update. If the `initramfs` USE flag is used with the [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package, microcode for all AMD processors will be saved in /boot/amd-uc.img, allowing it to be loaded alongside any other initrd/initramfs by a Linux bootloader such as [GRUB](https://wiki.gentoo.org/wiki/GRUB).

### Verification

Booting a capable kernel, the following line should be observed in case a microcode update was performed: `microcode: updated early`

In any case the output will show a `patch_level`. The following example shows a successful early microcode update using the Linux kernel and supplied microcode firmware blob files (in-kernel or from an initrd/initramfs):

`user $``dmesg | grep microcode`
\[    0.584603\] microcode: microcode: updated early to new patch\_level=0x06000832
\[    0.868036\] microcode: CPU0: patch\_level=0x06000832
\[    0.868116\] microcode: CPU1: patch\_level=0x06000832
\[    0.868196\] microcode: CPU2: patch\_level=0x06000832
\[    0.868277\] microcode: CPU3: patch\_level=0x06000832
\[    0.868360\] microcode: CPU4: patch\_level=0x06000832
\[    0.868451\] microcode: CPU5: patch\_level=0x06000832
\[    0.868538\] microcode: CPU6: patch\_level=0x06000832
\[    0.868619\] microcode: CPU7: patch\_level=0x06000832
\[    0.868718\] microcode: Microcode Update Driver: v2.01 \<tigran@aivazian.fsnet.co.uk>, Peter Oruba

You may verify this patch level against the patch level for your CPU model and stepping number (seen on lscpu) listed on the README in the amd-ucode folder. If the built-in patch is more recent (greater value for the patch level), no update will be applied nor seen on dmesg.

## Troubleshooting

### Newly installed firmware is not used after a reboot

The image file /boot/amd-uc.img is in a [file path recognized](https://wiki.gentoo.org/wiki/Microcode#Early_microcode_loading) by GRUB by default, but only after regenerating the grub configuration. This is done by the following command:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
Found linux image: /boot/vmlinuz-4.18.12
Found initrd image: /boot/amd-uc.img /boot/initramfs-4.18.12.img

The command output should show it found the /boot/amd-uc.img file.

### After installing updated microcode firmware files, the kernel still loads the previous microcode patch-level

After updating [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) that includes updated AMD processor microcode they will only be installed to /lib/firmware on the root directory. With the `initramfs` USE flag set the package will also update /boot/amd-uc.img with the new firmware files, whereas (if so configured) both the main initramfs/initrd as well as the kernel will still contain the previous files from [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware).

If the microcode was supplied in-kernel, [rebuilding](https://wiki.gentoo.org/wiki/Kernel/Configuration#Build) and installing the kernel as usual is needed to finally provide the new microcode firmware files to the kernel during the boot process. Likewise, if the microcode was included in the main initramfs/initrd, rebuilding it will be necessary to replace the previous with the new versions of the microcode firmware files.

If the patch-level still doesn't change after including the updated firmware files it is very likely that the microcode simply doesn't apply for the specific CPU.

## See also

- [Microcode](https://wiki.gentoo.org/wiki/Microcode) — describes various ways to update a CPU's microcode in Gentoo.
- [Intel microcode](https://wiki.gentoo.org/wiki/Intel_microcode) — describes the process of updating the [microcode](https://wiki.gentoo.org/wiki/Microcode) on Intel processors.

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=e6bcfdd75d53390a67f67237f4eafc77d9772056](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=e6bcfdd75d53390a67f67237f4eafc77d9772056)
2. [↑](https://wiki.gentoo.org#cite_ref-2) [https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=4d2b748305e96fb76202a0d1072a285b1500bff3](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=4d2b748305e96fb76202a0d1072a285b1500bff3)
3. [↑](https://wiki.gentoo.org#cite_ref-3) [Revision Guide for AMD Family 17h Models 00h-0Fh Processors](https://www.amd.com/system/files/TechDocs/55449_Fam_17h_M_00h-0Fh_Rev_Guide.pdf) (PDF, page 6)
4. [↑](https://wiki.gentoo.org#cite_ref-4) [\[PATCH\] linux-firmware: Update AMD SEV firmware](https://lkml.org/lkml/2019/6/18/492) (LKML, John Allen, commit pushed, 18 Jun 2019)
5. [↑](https://wiki.gentoo.org#cite_ref-5) [Revision Guide for AMD Family 17h Models 30h-3Fh Processors](https://developer.amd.com/wp-content/resources/56323-PUB_0.78.pdf) (PDF, page 6)
6. [↑](https://wiki.gentoo.org#cite_ref-6) [\[PATCH\] linux-firmware: Update AMD SEV firmware](https://lkml.org/lkml/2020/7/20/797) (LKML, Josh Boyer, signed-off, 20 Jul 2020)
7. [↑](https://wiki.gentoo.org#cite_ref-7) [Revision Guide for AMD Family 19h Models 00h-0Fh Processors](https://www.amd.com/system/files/TechDocs/56683-PUB-1.07.pdf) (PDF, page 8)
8. [↑](https://wiki.gentoo.org#cite_ref-8) 	[\[PATCH v2\] linux-firmware: Update AMD SEV firmware](https://lkml.org/lkml/2021/9/10/647) (LKML, John Allen, signed-off, 10 Sep 2021)
9. [↑](https://wiki.gentoo.org#cite_ref-9) [Revision Guide for AMD Family 1Ah Models 00h-0Fh Processors](https://www.amd.com/content/dam/amd/en/documents/processor-tech-docs/revision-guides/58251.pdf) (PDF, page 8)
10. [↑](https://wiki.gentoo.org#cite_ref-10) [linux-firmware: Update AMD SEV firmware](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/commit/amd/amd_sev_fam1ah_model0xh.sbin?id=3660cb7665df91e664b240c19c560f138d74f483) (John Allen, committed, 10 Feb 2025)
