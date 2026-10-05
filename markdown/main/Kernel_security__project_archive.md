<!-- source: https://wiki.gentoo.org/wiki/Kernel_security_(project_archive) | group: Gentoo Wiki (Main) | wiki-title: Kernel security (project archive) -->
---
title: Kernel security (project archive)
url: https://wiki.gentoo.org/wiki/Kernel_security_(project_archive)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-11-26"
fingerprint: "752b914c4a3373f"
license: CC BY-SA 4.0
---

# Kernel security (project archive)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**


The Gentoo security audit project handled patching the Linux kernel sources and informing users about global kernel security status. The aim of the project was also to audit Gentoo kernel's for potential flaws.

## Kernel sources

### Supported kernel sources

| Kernel source | Security liaison | 
|---|---|
| gentoo-sources | [Gentoo Kernel project](https://wiki.gentoo.org/wiki/Project:Kernel) | 
| gentoo-kernel, gentoo-kernel-bin | [Distribution Kernel project](https://wiki.gentoo.org/wiki/Project:Distribution_Kernel) | 

### Unsupported Kernel sources

| Kernel source | Security liaison | 
|---|---|
| git-sources |  [Mike Pagano (mpagano)](https://wiki.gentoo.org/wiki/User:Mpagano)  | 
| mips-sources |  [Joshua Kinard (kumba)](https://wiki.gentoo.org/wiki/User:Kumba)  | 
| pf-sources |  [Joonas Niilola (juippis)](https://wiki.gentoo.org/wiki/User:Juippis)  | 
| raspberrypi-sources |  [Sam James (sam)](https://wiki.gentoo.org/wiki/User:Sam)  | 
| rt-sources |  [Arisu Tachibana (Alicef)](https://wiki.gentoo.org/wiki/User:Alicef)  | 
| vanilla-sources |  [Agostino Sarubbo (ago)](https://wiki.gentoo.org/wiki/User:Ago) , [Gentoo Kernel project](https://wiki.gentoo.org/wiki/Project:Kernel) | 

### Making a new kernel source

Adding a new kernel source into the main Gentoo repository is not recommended by the Gentoo Kernel Security project unless it is a kernel source that could be used by a wide number of users. Please end consideration here and simply use an overlay to distribute custom or one-off kernel sources.

If you do believe that it is, you must be willing to become the security maintainer. Being the security maintainer for a kernel source means being willing to devote a significant amount of time to closing security bugs for that kernel source. Additionally, you must take care that your kernel source never falls into hard masking. If it does, your kernel source will automatically lose Gentoo Security support, and may be subject to removal from the repository.
