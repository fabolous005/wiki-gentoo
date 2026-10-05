<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2020/Ideas/Redox_Relibc_Support | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2020/Ideas/Redox Relibc Support -->
---
title: Google Summer of Code/2020/Ideas/Redox Relibc Support
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2020/Ideas/Redox_Relibc_Support
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-01-17"
fingerprint: "319e9c2a420b3bbc"
license: CC BY-SA 4.0
---

# Google Summer of Code/2020/Ideas/Redox Relibc Support

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[Redox](https://www.redox-os.org/) is a brand new operating system written in [rust](https://rust-lang.org), it provides a [libc](https://gitlab.redox-os.org/redox-os/relibc) implementation that works both on Linux and on the Redox kernel.

This project aims to be able to have a **x86\_64-unknown-linux-relibc** stage3 produced and pave the way to have portage available to the Redox operating system itself.

Main Tasks:

- Prepare a *relibc* ebuild.
- Make sure a the base system could be built on top of it
  - Check for errors when building the critical Gentoo toolchain utilities, e.g. building Python3 and Bash
  - Report any issues to both Gentoo and Redox
  - Submit Merge Requests fixes/improvements upstream to Redox GitLab
- Make sure a viable stage3 could be built

Bonus Tasks:

- Have a gentoo-prefix running on Redox

In order to apply on top of the normal Gentoo requirement of fixing an open issue and/or send a pull request, it is required to propose a fix or an improvement on the [redox gitlab](https://gitlab.redox-os.org/).




| Contacts | Required Skills | 
|---|---|
|  |  |
