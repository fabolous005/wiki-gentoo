<!-- source: https://wiki.gentoo.org/wiki/Ideas | group: Gentoo Wiki (Main) | wiki-title: Ideas -->
---
title: Ideas
url: https://wiki.gentoo.org/wiki/Ideas
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-03"
fingerprint: "559d99c254236f76"
license: CC BY-SA 4.0
---

# Ideas

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a place for Gentoo developers to share information about their efforts to improve Gentoo among themselves and with others. This also includes ideas that they are unable to pursue in a reasonable period of time that would be best taken by someone else. Our hope is that this will foster greater collaboration and increase innovation in Gentoo.

Please keep these lists alphabetized. When you add an item, please include a description of what this is, a statement of how this benefits Gentoo and who is working on it. Feel free to list your name in things that need developers if you are interested, but are unable to work on this without collaborators. Preferably, please use the [idea template](https://wiki.gentoo.org/wiki/Ideas/Template).

## Under Development

- ~~[Clang system compiler support](https://wiki.gentoo.org/wiki/Ideas/Clang_system_compiler_support)~~ (this is more-or-less done, just the usual issues crop up on updates)
- ~~[Split boost](https://wiki.gentoo.org/wiki/Ideas/Split_boost)~~ (slotted Boost was attempted a long time ago and later dropped)
- ~~[ZFSOnLinux](https://wiki.gentoo.org/wiki/Ideas/ZFSOnLinux)~~ (long done, see [ZFS](https://wiki.gentoo.org/wiki/ZFS))

## Needs Developer

### Gentoo Minix port

Minix is a BSD-style operating system by Andrew S. Tanenbaum. It uses a microkernel architecture and it is designed with a strong emphasis on reliability. It is designed to be able to recover from hardware faults and it is also quite small.

Porting Gentoo to Minix will enable us to provide an operating system with the ability to recover from hardware faults. It could also serve as a research environment for future projects.

- looked at it, it is painful to get working. [666threesixes666](https://wiki.gentoo.org/wiki/User:666threesixes666) ([talk](https://wiki.gentoo.org/wiki/User_talk:666threesixes666)) 13:43, 11 September 2013 (UTC)
- The idea sounds awesome to me. It would also be interesting to look at HiStar or something related. [Fssirc](https://wiki.gentoo.org/wiki/User:Fssirc) ([talk](https://wiki.gentoo.org/wiki/User_talk:Fssirc)) 01:12, 14 November 2013 (UTC)
- Has anyone revisited this idea lately? This project sounds fascinating. [Rage](https://wiki.gentoo.org/wiki/User:Rage) ([talk](https://wiki.gentoo.org/wiki/User_talk:Rage)) 6:33, 25 June 2017 (UTC)

### Gentoo Hurd port

**This is now done**, see [Project:Hurd](https://wiki.gentoo.org/wiki/Project:Hurd).

micro mach kernel, instead of monolithic linux kernel. basically same idea as minix.

- [https://www.gnu.org/software/hurd/hurd.html](https://www.gnu.org/software/hurd/hurd.html)
- [https://en.wikipedia.org/wiki/Kernel\_%28computing%29#Microkernels](https://en.wikipedia.org/wiki/Kernel_%28computing%29#Microkernels)
