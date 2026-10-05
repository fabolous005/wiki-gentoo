<!-- source: https://wiki.gentoo.org/wiki/SELinux/LSM | group: Gentoo Wiki (Main) | wiki-title: SELinux/LSM -->
---
title: SELinux/LSM
url: https://wiki.gentoo.org/wiki/SELinux/LSM
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2014-05-12"
fingerprint: d5bf193f9a9ee780
license: CC BY-SA 4.0
---

# SELinux/LSM

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

SELinux uses the [Linux Security Modules (LSM)](https://en.wikipedia.org/wiki/Linux_Security_Modules) as the implementation to handle enforcement within the Linux kernel. All actions taken on the system which invokes Linux kernel calls (such as system calls) are also passed through LSM, and SELinux adds LSM hooks so that SELinux too can participate in deciding if a call is to be allowed or not.

## Resources

- [Implementing SELinux as a Linux Security Module (pdf)](http://www.nsa.gov/research/_files/publications/implementing_selinux.pdf)
- [LSM Overview](http://www.nsa.gov/research/_files/selinux/papers/module/x45.shtml) in the SELinux paper published by NSA research
