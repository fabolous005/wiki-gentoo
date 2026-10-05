<!-- source: https://wiki.gentoo.org/wiki/GUID_Partition_Table | group: Gentoo Wiki (Main) | wiki-title: GUID Partition Table -->
---
title: GUID Partition Table
url: https://wiki.gentoo.org/wiki/GUID_Partition_Table
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-07-06"
fingerprint: d53c583e0948a98b
license: CC BY-SA 4.0
---

# GUID Partition Table

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **GUID Partition Table** (GPT) is a [partitioning](https://wiki.gentoo.org/wiki/Partition) scheme widely adopted in contemporary computers to organize and manage data on storage devices. Originally developed by Intel for the Itanium platform "[IA64](https://wiki.gentoo.org/wiki/Project:IA64)", GPT was designed to overcome limitations of the traditional [Master Boot Record](https://wiki.gentoo.org/wiki/Master_Boot_Record) (MBR) scheme and offers improved flexibility and scalability in handling storage on modern computing systems. The system firmware [UEFI](https://wiki.gentoo.org/wiki/UEFI) demands GPT partitioning on bootable media.

Unlike the MBR, GPT employs a Globally Unique Identifier ([GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)) for each partition, allowing for a more extensive range of partitions and larger storage capacities. This modern approach is essential for accommodating the demands of today's data-intensive applications.

### Structure and Features

Each partition is assigned a unique GUID, ensuring a globally distinct identifier across all systems. This design eliminates the limitations imposed by the MBR's maximum partition size and number of partitions, providing a solution for modern storage needs.

One notable feature of GPT is its inclusion of a Protective MBR. This ensures compatibility with legacy systems that may not fully support GPT, preventing accidental data loss or corruption when accessed by older software.

### Compatibility and Adoption

GPT is the standard for disk partitioning on modern computers, being compatible with both [BIOS](https://wiki.gentoo.org/wiki/BIOS) (Basic Input/Output System) and [UEFI](https://wiki.gentoo.org/wiki/UEFI) (Unified Extensible Firmware Interface) systems; the transition to GPT has been gradual but is now nearly ubiquitous.

### Implementation in Operating Systems

Major operating systems, including Linux, macOS, and Windows, include robust GPT support.

## See also

- [Partition](https://wiki.gentoo.org/wiki/Partition) — a means of splitting a block device up to sub-regions.
