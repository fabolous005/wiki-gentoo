<!-- source: https://wiki.gentoo.org/wiki/Power_management/Hardware_Acceleration | group: Gentoo Wiki (Main) | wiki-title: Power management/Hardware Acceleration -->
---
title: Power management/Hardware Acceleration
url: https://wiki.gentoo.org/wiki/Power_management/Hardware_Acceleration
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-31"
fingerprint: c287789440aab944
license: CC BY-SA 4.0
---

# Power management/Hardware Acceleration

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the setup of [power management](https://wiki.gentoo.org/wiki/Power_management) of hardware acceleration devices.

"Hardware acceleration is the use of computer hardware, known as a hardware accelerator, to perform specific functions faster than can be done by software running on a general-purpose central processing unit (CPU)."<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> For example, we *could* run a desktop environment with the CPU (software rendering), but that would be slow and consume a lot of CPU resources and power; or we could run a desktop environment with a graphics card (hardware rendering). Using a dedicated piece of hardware designed for a specific task tends to complete said task more efficiently than the general-purpose CPU.

The example of using a graphics card to render graphics is an example of hardware acceleration that most people know, but there are many more hardware acceleration devices supported by the kernel; there are devices that support network, drive, encryption, and entropy acceleration.

Hardware acceleration devices:

- generally have better performance than the CPU.
- generally have better power efficiency than the CPU.
- take load off the CPU.

## Configuration

### Kernel

\[\*\] Networking support ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_NET\</code> to find this item.
  Networking options --->
    \[\*\] Receive packet steering [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_RPS\</code> to find this item.
    \[\*\] Hardware acceleration of RFS [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_RFS\_ACCEL\</code> to find this item.
Device Drivers --->
  Character devices --->
    \<M> Hardware Random Number Generator Core support ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_HW\_RANDOM\</code> to find this item.
      \<Enable all hardware acceleration options.>
    \<M> TPM Hardware Support ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_TCG\_TPM\</code> to find this item.
      \<Enable all hardware acceleration options.>
  \[\*\] Compute Acceleration Framework ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_DRM\_ACCEL\</code> to find this item.
    \<Enable all hardware acceleration options.>
\[\*\] Cryptographic API ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CRYPTO\</code> to find this item.
  \[\*\] Hardware crypto devices ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CRYPTO\_HW\</code> to find this item.
      \<Enable all hardware acceleration options.>
Library routines --->
  \[\*\] Enable optimized CRC implementations [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CRC\_OPTIMIZATIONS\</code> to find this item.
