<!-- source: https://wiki.gentoo.org/wiki/Model-specific_registers | group: Gentoo Wiki (Main) | wiki-title: Model-specific registers -->
---
title: Model-specific registers
url: https://wiki.gentoo.org/wiki/Model-specific_registers
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-18"
fingerprint: "3fff9c1e5558624a"
license: CC BY-SA 4.0
---

# Model-specific registers

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[Model-specific registers](https://en.wikipedia.org/wiki/Model-specific_register) are [processor registers](https://en.wikipedia.org/wiki/Processor_register) in the x86 system architecture that can be used for debugging, monitoring, and toggling of features.

### Kernel

To enable access to MSRs in the kernel:

**menuconfig**

Note that enabling either of the kernel [lockdown](https://wiki.gentoo.org/wiki/Security_Handbook/Kernel_security/Kernel_Lockdown) modes requires disabling access to MSRs.
