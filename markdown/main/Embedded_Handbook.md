<!-- source: https://wiki.gentoo.org/wiki/Embedded_Handbook | group: Gentoo Wiki (Main) | wiki-title: Embedded Handbook -->
---
title: Embedded Handbook
url: https://wiki.gentoo.org/wiki/Embedded_Handbook
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-06"
fingerprint: "3da27f0f07c62bf4"
license: CC BY-SA 4.0
---

# Embedded Handbook

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Gentoo Embedded Handbook is a collection of community maintained documents providing a consolidation of embedded and SoC knowledge for Gentoo. It aims to cover just about all aspects of getting Gentoo to run on a SoC - from theory, to design, to practice.

## General topics

Embedded fundamentals you need before playing with hardware. See individual parts below or the [all-in-one-page General topics article](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Full).

- [Introduction](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Introduction)
- An introduction into the world of embedded, cross-compilers, and dragons.
- [Compiling with QEMU user chroot](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Compiling_with_QEMU_user_chroot)
- How To compile with QEMU user.
- [Creating a cross-compiler](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Creating_a_cross-compiler)
- Build a cross-compiler on your machine!
- [Cross-compiling with Portage](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Cross-compiling_with_Portage)
- Leverage Portage as a cross-compiling package manager.
- [Cross-compiling the kernel](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Cross-compiling_the_kernel)
- Cross-compile a kernel for your system with flair!
- [Frequently asked questions](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Frequently_asked_questions)
- FAQs for Gentoo Embedded.

## Emulators

Software emulation of systems can often times be as good (if not better) than the real thing.

- [Qemu](https://wiki.gentoo.org/wiki/Embedded_Handbook/Emulators/Qemu)
- A generic and open source machine emulator and virtualizer for x86, x86\_64, arm, sparc, powerpc, mips, m68k (coldfire), and superh.
- [SkyEye](https://wiki.gentoo.org/index.php?title=Embedded_Handbook/Emulators/SkyEye&action=edit&redlink=1)
- ARM embedded hardware simulator.
- [Armulator](https://wiki.gentoo.org/wiki/Embedded_Handbook/Emulators/Armulator)
- Emulate armnommu/uClinux (no-mmu Linux) in GDB.
- [Softgun](https://wiki.gentoo.org/index.php?title=Embedded_Handbook/Emulators/Softgun&action=edit&redlink=1)
- ARM software emulator.
- [Hercules](https://wiki.gentoo.org/wiki/Embedded_Handbook/Emulators/Hercules)
- Hercules System/370, ESA/390 and zArchitecture Mainframe Emulator.

## Bootloaders

From the obscure to the obscene, we'll cover some of the common bootloaders out there and how to get your feet wet with them.

- [Das U-Boot](https://wiki.gentoo.org/wiki/Embedded_Handbook/Bootloaders/Das_U-Boot)
- The Universal Bootloader which supports every embedded architecture out there.
- [NeTTrom](https://wiki.gentoo.org/wiki/Embedded_Handbook/Bootloaders/NeTTrom)
- Simple bootloader on NetWinders.
- [RedBoot](https://wiki.gentoo.org/wiki/Embedded_Handbook/Bootloaders/RedBoot)
- Small bootloader based on eCos which supports every embedded architecture out there.
- [SH-LILO](https://wiki.gentoo.org/wiki/Embedded_Handbook/Bootloaders/SH-LILO)
- Port of LILO to SuperH which tends to be pretty common on that architecture.

## Boards

Some boards are fun while others can be a PITA; we'll cover many of the common gotchas with systems out there.

- [Hammer Board and Nail Board](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/Hammer_Board_and_Nail_Board)
- Little-endian armv4l board.
- [LANTank](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/LANTank)
- Little-endian SuperH based NAS (using internal IDE) from I-O Data.
- [NetWinder](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/NetWinder)
- Little-endian ARMv4 based network server from Rebel.
- [NSLU2](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/NSLU2)
- Big-endian arm based NAS (using external USB) from Linksys.
- [QNAP TurboStation 109/209/409](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/QNAP_TurboStation_109/209/409)
- Little-endian ARMv5TE NAS from QNAP.
- [Marvell Sheevaplug](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/Marvell_Sheevaplug)
- Little-endian ARMv5TE from Marvell.
- [ACME SYSTEMS Netus G20](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/ACME_SYSTEMS_Netus_G20)
- Netus G20 (ARMv5TE) from ACME SYSTEMS
- [Genesi Efika MX](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/Genesi_Efika_MX)
- Little-endian ARMv7-A from Genesi USA.
- [Pandaboard](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/Pandaboard)
- Little-endian ARMv7-A from pandaboard.org.
- [TrimSlice](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/TrimSlice)
- Little-endian ARMv7-A from Compulab/trimslice.com.
- [BeagleBone](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/BeagleBone)
- Little-endian ARMv7-A from Beagleboard.org.
- [BeagleBone Black](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/BeagleBone_Black)
- Little-endian ARMv7-A from Beagleboard.org.
- [NVIDIA Jetson TX2](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/NVIDIA_Jetson_TX2)
- Little-endian ARMv8-A from NVIDIA.
- [Intel Edison](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/Intel_Edison)
- Big-endian dual Atom and Quark from Intel.com.
- [NanoPI Neo 3 + R2S](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/NanoPI/Neo3)
- Rockchip RK3328 IoT device from NanoPI
- [Pine64 RockPro64](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/Pine64/RockPro64)
- Rockchip RK3399 Single Board Computer from Pine64
- [Pine64 QuartzPro64 development board](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/Pine64/QuartzPro64)
- Rockchip RK3588 Single Board Computer from Pine64
- [Mango Pi MQ-Pro](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/Mango_Pi_MQ-Pro)
- An (adorable) Allwinner D1(H)-based RISCVGCV SBC
- [StarFive VisionFive 2](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/StarFive_VisionFive_2)
- StarFive JH7110 SoC with a quad-core SiFive U74 RISC-V CPU running at 1.5 GHz and an Imagination BXE-4-32 GPU
