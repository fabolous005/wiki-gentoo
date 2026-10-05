<!-- source: https://wiki.gentoo.org/wiki/HP_Proliant_Microserver | group: Gentoo Wiki (Main) | wiki-title: HP Proliant Microserver -->
---
title: HP Proliant Microserver
url: https://wiki.gentoo.org/wiki/HP_Proliant_Microserver
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-17"
fingerprint: a7f20cd4cfff08de
license: CC BY-SA 4.0
---

# HP Proliant Microserver

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The [HP ProLiant MicroServer](https://www.hpe.com/us/en/product-catalog/servers/proliant-servers.filters-facet_subbrand_url:ProLiant-MicroServer.html) is an unexpensive server with four cold-swappable SATA bays, an optical drive and an eSATA connector. Up to 8GB of RAM in two slots are supported.

## Hardware

### CPU

The latest model, HP article no. 664447-425 is an AMD Turion(tm) II Neo N40L Dual-Core Processor.

**Processor support**

It supports virtualization (`svm` flag):

`root #``egrep svm /proc/cpuinfo`
flags: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr\_opt pdpe1gb rdtscp lm 3dnowext 3dnow constant\_tsc rep\_good nopl nonstop\_tsc extd\_apicid pni monitor cx16 popcnt lahf\_lm cmp\_legacy svm extapic cr8\_legacy abm sse4a misalignsse 3dnowprefetch osvw ibs skinit wdt nodeid\_msr npt lbrv svm\_lock nrip\_save

The right processor architecture for CFLAGS is `-march=amdfam10`.

### SCSI device names at boot time

I only started this page to pass the tip [from the Gentoo forums](https://forums.gentoo.org/viewtopic-p-6919622.html#6919622):

`root #``sed -i -e "s~CONFIG_SATA_AHCI=y~CONFIG_SATA_AHCI=m~"  /usr/src/linux/.config`
