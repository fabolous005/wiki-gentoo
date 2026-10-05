<!-- source: https://wiki.gentoo.org/wiki/MIPS/Hardware_Requirements | group: Gentoo Wiki (Main) | wiki-title: MIPS/Hardware Requirements -->
---
title: MIPS/Hardware Requirements
url: https://wiki.gentoo.org/wiki/MIPS/Hardware_Requirements
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: "2d6cb81f62b229cc"
license: CC BY-SA 4.0
---

# MIPS/Hardware Requirements

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This document provides an overview of the status of various MIPS-based systems for installing Gentoo Linux. If you find errors or omissions please email the maintainer or an editor.

## Silicon Graphics Systems

### Stable

The following systems are known to be very stable overall. Just about all the core hardware is supported in these systems, and there are very few outstanding stability issues at this time.

#### <u>IP22: Indy, Indigo2 and Challenge S</u>

- **Processor Options:**
  - *(on Indy and Challenge S)* R4000, R4400, R4600, R5000
  - *(on Indigo 2)* R4000, R4400

- **Kernel Architecture Support:**
  - 32-bit: *Stable*
  - 64-bit: *Should work*

- **Notes:**
  1. At present, only the Newport (XL) graphics framebuffer is supported in Linux. Therefore on systems with other framebuffers, you will need to use serial console.
  2. The Challenge S has its RJ-45 connector on a daughter board (Mezz) linked with an unsupported SCSI adapter, a WD33C95A. In order for this network connector to function, this SCSI bus needs to be activated, however it is not yet supported by Linux. As a result of this, the Challenge S can only get network connectivity via its AUI connector.
  3. Indigo2 EISA Support isn't very well tested and may not work with all hardware. Hardware that it is known to function with is listed, and as more compatible hardware is discovered, it will be added. Please note that hardware requiring DMA support is currently not functional. This includes EISA/ISA-based soundcards. EISA Video cards requiring an x86-compatible BIOS are also not functional.

#### <u>IP32: O2</u>

- **Processor Options:**
  - **Stable:** R5000, RM5200, RM7000
  - **Unsupported:** R10000, R12000

- **Kernel Architecture Support:** *64-bit, Stable*

### Experimental/Unstable

The systems listed here are known to run Linux, however in many cases, there are known stability issues and major gaps in the hardware support. In short, it will be a rough and bumpy ride.

#### <u>IP27: Origin 200/2000 and Onyx2</u>

- **Processor Options:** R10000, R12000, R14000
- **Kernel Architecture Support:** *64-bit, Experimental/Buggy, SMP-capable, NUMAlink capability not tested in the last several years*

- **Notes:**
  1. InfiniteReality (Kona) graphics boards in Onyx2 systems are not currently supported. Patches welcome!
  2. Support is in mainline & linux-mips kernels, but appears to be very flakey, especially with the current in-tree IOC3 driver.  Use [sys-kernel/mips-sources](https://packages.gentoo.org/packages/sys-kernel/mips-sources) and the IOC3 metadriver for more stable operation of these systems until the metadriver code is accepted upstream.
  3. CONFIG\_TRANSPARENT\_HUGEPAGE does not work reliably on this platform. Random SIGSEGV or SIGBUS events will happen. Do not use it unless you are trying to debug the problem. Possibly an issue with the current MIPS TLB code.
  4. There is a yet-to-be determined bug in current kernels that causes IP27-class hardware to lock up after a period of time.  The lock up can be triggered sooner by performing heavy disk I/O (such as a bonnie++ run).  A possible cause may be a VM\_BUG\_ON\_PAGE() in [mm/page\_alloc.c:1490](http://git.linux-mips.org/cgit/ralf/linux.git/tree/mm/page_alloc.c#n1490), in move\_freepages().  Why or how this BUG() variant is getting triggered is currently unknown.

#### <u>IP28: Indigo2 Impact R10000</u>

- **Processor Options:** R10000
- **Kernel Architecture Support:** *64-bit, Experimental*

- **Notes:**
  1. IP28 Indigo2 Impact support (classified as an Indigo2 system w/ an R10000 Processor) is *very experimental* . Most of the work was done by Peter Fuerst.  The Indigo2 IP28 system suffers from an issue known as *Speculative Execution* , a feature of the R10000 Processor that is problematic on machines that are *Non-Cache Coherent*, such as the IP28 Indigo2 and on R10000/R12000-based IP32 O2 systems. Details on the nature of Speculative Execution, and the issues it poses to the Indigo2 can be found at the following URLs:
    - MIPS R10000 Microprocessor User's Manual (See *PDF Pages 51-55* ) [https://web.archive.org/web/20051028113506/http://techpubs.sgi.com/library/manuals/2000/007-2490-001/pdf/007-2490-001.pdf](https://web.archive.org/web/20051028113506/http://techpubs.sgi.com/library/manuals/2000/007-2490-001/pdf/007-2490-001.pdf)
    - Post to NetBSD sgimips Mailing List on 29 Jun 2000 [http://mail-index.netbsd.org/port-sgimips/2000/06/29/0006.html](http://mail-index.netbsd.org/port-sgimips/2000/06/29/0006.html)

#### <u>IP30: Octane</u>

- **Processor Options:** R10000, R12000, R14000A
- **Kernel Architecture Support:** *64-bit, Stable, SMP-capable*

- **Notes:**
  1. Octane support is back as of sys-kernel/mips-sources-3.18.3.
  2. SMP support works again as of sys-kernel/mips-sources-4.1.0.
  3. Memory is limited to a maximum of 2GB. PCI-USB devices might not work with >1.7GB of RAM, but most external PCI will not work at all beyond 2GB. This is due to incorrect DMA configuration at the moment.
  4. The serial ports on the Octane default to 9600 8N1 in the ARCS PROM. Booted into Linux, the kernel is using a rudimentary driver that relies on constant polling of the serial port to send or receive data at a maximum rate of 38400 8N1. To fully support DMA and interrupt-signaled serial ports, the in-kernel SGI Altix driver needs work, as it can currently detect the serial ports, but not send/receive data.
  5. CONFIG\_TRANSPARENT\_HUGEPAGE does not work reliably on this platform. Random SIGSEGV or SIGBUS events will happen. Do not use it unless you are trying to debug the problem. Possibly an issue with the current MIPS TLB code.
  6. CONFIG\_SLUB is broken on IP30. It will lock up during boot due to an infinite number of messages consuming all available CPU time regarding duplicate entries in /sys/kernel/slab/\*.

### <u>SGI IP35 Hardware</u>

The IP35 hardware family comprises several SGI systems, the last of the SGI MIPS workstations/servers developed before SGI spun MIPS Technologies off and exited the MIPS market altogether:

- Fuel/IP34 (R14000A/R16000 CPU)
- Origin 3000/IP35 (R14000\[A\]/R16000\[A\] CPU)
- Origin 300/IP45 (R14000A CPU)
- Origin 350 & Tezro/IP53 (R16000A CPU)


Support for these systems is currently VERY experimental.  An effort to make Linux boot on them was restarted several months ago, and a few of these machines can boot using a single CPU and load a minimal root filesystem.  The following git tree is where development and support for these systems is currently focused: [http://git.linux-mips.org/cgit/nyef/linux-ip35/](http://git.linux-mips.org/cgit/nyef/linux-ip35/).

Those interested would be advised to join the linux-mips mailing list or join the #mipslinux IRC channel on Freenode to learn more about this class of systems.

### Unsupported systems

Just about all of these systems are totally unsupported at the kernel level making a full Linux system totally impossible. Little is known about a lot of the systems listed here therefore any support in the near future is highly unlikely. You may wish to keep an eye on the [Linux/MIPS website](http://www.linux-mips.org) for any news of ports either that, or start your own. :-)

1. IP12: Iris Indigo (R3000 CPU)
2. IP20: Iris Indigo (R4000 or R4400 CPU)
  - Both systems completely unsupported in Linux.
3. IP26: Indigo2 Power (R8000 CPU)
  - Has TLB code written (`arch/mips/mm/tlb-r8k.c`), but no cache-handling code.  Patches welcome!
  - Documentation:
  - [Images](http://dev.gentoo.org/~kumba/mips/pics/ip26/)

### Accessories

As with most systems, there is quite wide array of peripherals that one can obtain for Silicon Graphics systems. While a lot of these work in IRIX, Linux support is a bit touch-and-go on some of these items. The following table lists the support for each device.

| Stable Support | Experimental Support | Unsupported/Untested | 
|---|---|---|
|  |  |  | 

## Cobalt Servers

### Stable

The Cobalt servers are a set of machines all based around the QED RM523\[01\] CPU. They came in two varieties, the RaQ a rackmounted model, and the Qube, a small desktop model (1ft. cube). The big difference between these systems and the SGI systems above, is that these systems are *little endian* ( `mipsel` ) as opposed to *big endian* ( `mips` ).

These were also sold under an OEM arragement to other companies, examples of this are the Gateway Microserver (Qube 2) and the Seagate NasRaQ (RaQ 2).

#### <u>Qube/Raq 2800 (a.k.a Qube/Raq 2)</u>

- **Processor:** QED RM5231 @ 250MHz
- **Kernel Architecture Support:**
  - 32-bit: *Stable*
  - 64-bit: *Previously Experimental, current status is unknown*

### Unsupported

#### <u>Raq 2700 (a.k.a Raq 1)</u>

- **Processor:** QED RM5230 @ 150MHz
- **Kernel Architecture Support:**
  - 32-bit: *Untested In Gentoo*
  - 64-bit: *Untested In Gentoo*

#### <u>Qube 2700 (a.k.a Qube 1)</u>

The Qube 2700 was the first of this whole subarchitecture. Unfortunately, it lacks a serial port, making installation at present, damn near impossible. You may wish to have a look at [Jan Metzner's page](http://web.archive.org/web/20060716160139/http://metzner.org/projects/qube/) for more information.

- **Processor:** QED RM5230 @ 150MHz
- **Kernel Architecture Support:**
  - 32-bit: *Untested In Gentoo*
  - 64-bit: *Untested In Gentoo*
