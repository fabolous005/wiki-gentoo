<!-- source: https://wiki.gentoo.org/wiki/SELinux/Tutorials | group: Gentoo Wiki (Main) | wiki-title: SELinux/Tutorials -->
---
title: SELinux/Tutorials
url: https://wiki.gentoo.org/wiki/SELinux/Tutorials
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-06-23"
fingerprint: "2dff8d29811a4f80"
license: CC BY-SA 4.0
---

# SELinux/Tutorials

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[SELinux](https://wiki.gentoo.org/wiki/SELinux) is sometimes seen as a daunting additional security measure on a Linux system. And it probably is, since it requires the users to have some non-basic knowledge of both Linux and SELinux. This series of tutorials attempts to teach the basics of how to work with and configure SELinux.

Throughout the tutorials, we will assume you have access to a SELinux enabled system. This can be a RedHat Enterprise Linux (6 or higher) system, a Fedora system, CentOS, Gentoo Hardened, and etc. If you can get it to boot, you can even use the selinuxnode (experimental) SELinux-enabled live environment (KVM/Qemu guest) offered through Gentoo's mirrors (in the experimental/amd64/qemu-selinux location).

Within each tutorial, we will try to guide you through new vocabulary used by SELinux, changes compared to a regular Linux system, and more. At the end of each tutorial, you will find a **What you need to remember** part. This is a quick reference of what the tutorial is about, and might help you in the future to remember some stuff without having to read the entire tutorial again.

So, let's get started.

## Introduction to SELinux

This first set of tutorials are an introduction to SELinux. They cover basic SELinux stuff and do not focus on Gentoo specifics (or at least not too much), so they are reusable for other SELinux-enabled distributions as well.

### Customizing SELinux policies

This set of tutorials focuses on customizing SELinux policies. It focuses on SELinux policy development from an operational point of view, starting with simple small policy enhancements and incrementally increasing the amount of features (and perhaps complexity?) used therein.
