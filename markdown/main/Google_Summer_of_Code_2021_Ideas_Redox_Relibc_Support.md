<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2021/Ideas/Redox_Relibc_Support | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2021/Ideas/Redox Relibc Support -->
---
title: Google Summer of Code/2021/Ideas/Redox Relibc Support
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2021/Ideas/Redox_Relibc_Support
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-03-17"
fingerprint: "159d9e7a4e437dbc"
license: CC BY-SA 4.0
---

# Google Summer of Code/2021/Ideas/Redox Relibc Support

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[Redox](https://www.redox-os.org/) is a brand new operating system written in [rust](https://rust-lang.org), it provides a [libc](https://gitlab.redox-os.org/redox-os/relibc) implementation that works both on Linux and on the Redox kernel.

This project aims to be able to have a **x86\_64-unknown-linux-relibc** stage3 produced and pave the way to have portage available to the Redox operating system itself, last year most of the **relibc** bugs had been addressed and relibc had been able to build a working GNU toolchain.

Main Tasks:

- Make a *relibc* ebuild that can be part of the main portage.
- Make sure base system could be built on top of it
  - Confirm that Gentoo toolchain utilities still build, e.g. building Python3 and Bash
  - Confirm that alternative toolchain could build (LLVM, clang)
  - Report any issues to both Gentoo and Redox
  - Submit Merge Requests fixes/improvements upstream to Redox GitLab
- Build a stage3 with the current *relibc* and the toolchain.
- Have a gentoo-prefix running on Redox

In order to apply on top of the normal Gentoo requirement of fixing an open issue and/or send a pull request, it is required to propose a fix or an improvement on the [redox gitlab](https://gitlab.redox-os.org/).




| Contacts | Required Skills | 
|---|---|
|  |  |
