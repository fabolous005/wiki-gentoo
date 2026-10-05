<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2020/Ideas/eclean-kernel_rewrite_or_fixes | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2020/Ideas/eclean-kernel rewrite or fixes -->
---
title: Google Summer of Code/2020/Ideas/eclean-kernel rewrite or fixes
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2020/Ideas/eclean-kernel_rewrite_or_fixes
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-01-25"
fingerprint: b520257731a43b10
license: CC BY-SA 4.0
---

# Google Summer of Code/2020/Ideas/eclean-kernel rewrite or fixes

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[eclean-kernel](https://github.com/mgorny/eclean-kernel) is a tool to clean up old kernel versions and related files. However, the original tool turned out to be quite buggy and making too many assumptions. I've started a redesign into modular [eclean-kernel2](https://github.com/mgorny/eclean-kernel2) but it was never finished or feature-complete, and choice of C++ was probably a big mistake.

The idea is to either update or rewrite eclean-kernel, fix all the major bugs, extend support to more architectures and support more systems (bootloaders, /boot layouts). My suggestion would be to take eclean-kernel and start modifying it to match the design of eclean-kernel2 while keeping it in Python. However, it's up to the student to decide whether to start from scratch or on top of one of the existing projects, whether to use Python or something else and what final design to use.

This project includes some research on topics which might lack proper documentation and require actual testing and/or reading code. This particularly involves kernel file format for different architectures (eclean-kernel needs to get kernel version string), /boot layouts, bootloaders.



| Contacts | Required Skills | 
|---|---|
|  |  |
