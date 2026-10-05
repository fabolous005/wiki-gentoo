<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2020/Ideas | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2020/Ideas -->
---
title: Google Summer of Code/2020/Ideas
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2020/Ideas
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-03-05"
fingerprint: "37b9b9501a27a3fc"
license: CC BY-SA 4.0
---

# Google Summer of Code/2020/Ideas

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![GSoC 2020 logo GSoC 2020 logo](https://wiki.gentoo.org/images/c/ce/Gsoc2016-sun-160x160.png)

Want to spend your summer contributing full-time to Gentoo, and get paid for it? Gentoo is in its 10th year in the Google Summer of Code. In the past, **most of our successful students have become Gentoo developers**, so your chances of becoming one are very good if you're accepted into this program.

**Most ideas listed here have a contact person associated with them. Please get in touch with them earlier rather than later to develop your idea into a complete application.** You can find many of them on Freenode's IRC network under the same username. If there is no contact information, please join the [gentoo-soc mailing list](https://www.gentoo.org/get-involved/mailing-lists/) or [#gentoo-soc on the Freenode IRC network](irc://irc.gentoo.org/gentoo-soc), and we will work with you to find a mentor and discuss your idea.

**You don't have to apply for one of these ideas! You can come up with your own**, and as long as it fits into Gentoo, we'll be happy to work with you to develop it. Remember, your project needs to have deliverables in less than 3 months of work in most cases. Be ambitious but not too ambitious ;)

## Students, please read this first

We have a **custom application template** that we will ask you to fill out. Here it is:

**Congratulations on applying for a project with Gentoo!** To improve your chances of succeeding with this project, we want to make sure you're sufficiently prepared to invest a full summer's worth of time on it. **In addition to the usual application, there are 2 specific actions and 2 pieces of info we would like to see from you:**

- **Use the tools that you will use in your project to make changes to code** (e.g., source code management \[SCM\] software such as CVS, Subversion, or git). Please use the same SCM as you will use for your project to check out one of our [repositories](https://www.gentoo.org/get-involved/get-code/), make a change to it, and post that change as a [patch](http://www.network-theory.co.uk/articles/patchintro.html) on a mailing list or [bug](https://bugs.gentoo.org/). Please fix a real bug reported in [Bugzilla](https://bugs.gentoo.org/) to show that you can use the tools to make a meaningful change. Your contact in Gentoo can help you determine which SCM and repository you should use for this as well as a good bug to fix. If your idea doesn't have a contact, please get in touch with us on the [gentoo-soc mailing list](https://www.gentoo.org/get-involved/mailing-lists/) or in real-time [on IRC](irc://irc.gentoo.org/gentoo-soc). Once you've made your change, link to it from your application.

- **Participate in our development community.** Please make a post to one of our [mailing lists](https://www.gentoo.org/get-involved/mailing-lists/) and link to it from your application ([archives.gentoo.org](https://archives.gentoo.org) holds past postings). The gentoo-soc list would be a good starting point, if you aren't subscribed to any others already. The best posts would be an introduction of the project you're applying for and a little background about you, to introduce yourself to the community and get some broader input about your project.

- **Give us your contact info and working hours.** Please provide your email address, home mailing address, and phone number. This is a requirement and provides for accountability on both your side and ours. Also, please tell us what hours you will be working and responsive to contact via email and IRC; these should sum to at least 35 hours a week.

These actions are things you will do extremely commonly as an open-source developer, and they really aren't that hard, so don't let them hold you back! The remainder of the application is free-form. **Please read our [application guidelines](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2020/Application_Guidelines) and [Google's FAQ](https://developers.google.com/open-source/gsoc/faq) to complete it.** Good luck!

## Ideas



The big data infrastructures are mostly built on the Java virtual machine ecosystem, most notably in Java and Scala.

Nevertheless, Java has not been adopted smoothly into GNU/Linux distributions.  The packaging of Java software are considered difficult by the GNU/Linux community (e.g. [Debian](https://wiki.debian.org/Java/Packaging), [Archlinux](https://wiki.archlinux.org/index.php/Java_package_guidelines), [Fedora](https://fedoraproject.org/wiki/Java)).  At the same time, the Java community has its own set of repositories like [maven](https://maven.apache.org), functionally similar to packages in GNU/Linux distributions.

The [Gentoo Java Project](https://wiki.gentoo.org/wiki/Project:Java) has done a good job laying out the framework of the Java ecosystem in Gentoo. At the same time, there are still thousands of useful Java packages to be packaged and maintained.  The project will parse the metadata of maven packages and automatically write ebuilds compatible with the Java build system used in Gentoo.  We are going to set up and maintain an automatically updated maven overlay every Gentoo user can use.  The overlay will at least contain [spark](http://spark.apache.org/) and [hadoop](http://hadoop.apache.org/).  We aim to make Gentoo an attractive choice for data scientists, Java developers, users and big data system administrators.

A preliminary tool, [java-ebuilder](https://github.com/gentoo/java-ebuilder) is available as [app-portage/java-ebuilder](https://packages.gentoo.org/packages/app-portage/java-ebuilder).  A proof-of-concept [overlay](https://github.com/heroxbd/maven-overlay) is also made.



| Contacts | Required Skills | 
|---|---|
|  |  | 

The Android custom ROM development has been based on cross-compilation and flashing whole partitions. This paradigm has been serving well for the embedded system developments. But as the performance of personal mobile devices boost, it becomes feasible and desirable to introduce package management like personal computers. In addition, with the introduction of Project Treble in Android 8.0 Oreo, device drivers are managed separately, opening more possibilities of operating system customization. Package manangement will make software installation and update reliable, reproducible, incremental and convienent.

This project aims to introduce Gentoo's prestiges package manager, portage, to manage Android software stack. Taking [KirenaHoro's work](https://jsteward.moe/gsoc-2018-final-report.html) as the starting point, you are going to develop a framework to drive the AOSP/LineageOS build system into a set of portage ebuilds.

In addition, this project targets an update of [Project:Android](https://wiki.gentoo.org/wiki/Project:Android) to work from within an Android app without rooting the device, to offer Gentoo to diverted audience and ease the adoption of GNU userlands on Android devices. For this part, a normal (unrooted, bootlocked phone) is sufficient. Bumping and upstreaming [gentooandroid.sf.net's](https://sourceforge.net/projects/gentooandroid/files/packages/packages/portage.diff/download) [patches](https://sourceforge.net/projects/gentooandroid/files/sys-app_portage-current-HEAD_patch/download) to bootstrap-prefix.sh and in $PORTDIR is a possible pathway. A suggested list of progressive steps would be to make bootstrap-prefix.sh work on a gentoo box, after removing : /usr/bin/python at step 1, /tmp at step 2, /etc/passwd at step 3. Step 4 would be to target Termux on vanilla Android that lacks /bin/sh, step 5 to take out /usr/bin/perl for LaTeX, step 6 /dev/tty for Xvfb.



| Contacts | Required Skills | 
|---|---|
|  |  | 

Gentoo portage is driving the build system of Google Chromebooks thanks to its flexibility and power. However, portage and other Gentoo tools are invisible to end-users because portage is only used to cross compile and build the filesystem image. The resulting image does not have portage or toolchain, and becomes a feature-limited ChromeOS compared to a standard Gentoo system. ChromiumOS, the community counterpart of ChromeOS, is an open operating system that is straightforward to build and hack. In this project, we will make a full-featured Gentoo-like ChromiumOS combining the innovative Chromebook experience with flexibility of portage and software packages from Gentoo.



| Contacts | Required Skills | 
|---|---|
|  |  | 

The old binary package format used by Gentoo has a few deficiencies (see [motivation on GLEP 78](https://www.gentoo.org/glep/glep-0078.html#motivation)).  The goal would be to implement support for a better format in Portage (alongside compatibility support for old binary packages).  This can either be the format suggested by [GLEP 78](https://www.gentoo.org/glep/glep-0078.html), Exherbo-alike format using .ebuild files or any other format designed by the student.



| Contacts | Required Skills | 
|---|---|
|  |  | 

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

[eclean-kernel](https://github.com/mgorny/eclean-kernel) is a tool to clean up old kernel versions and related files. However, the original tool turned out to be quite buggy and making too many assumptions. I've started a redesign into modular [eclean-kernel2](https://github.com/mgorny/eclean-kernel2) but it was never finished or feature-complete, and choice of C++ was probably a big mistake.

The idea is to either update or rewrite eclean-kernel, fix all the major bugs, extend support to more architectures and support more systems (bootloaders, /boot layouts). My suggestion would be to take eclean-kernel and start modifying it to match the design of eclean-kernel2 while keeping it in Python. However, it's up to the student to decide whether to start from scratch or on top of one of the existing projects, whether to use Python or something else and what final design to use.

This project includes some research on topics which might lack proper documentation and require actual testing and/or reading code. This particularly involves kernel file format for different architectures (eclean-kernel needs to get kernel version string), /boot layouts, bootloaders.



| Contacts | Required Skills | 
|---|---|
|  |  | 

Gentoo Kernel CI is the Gentoo Kernel sources Continuous Integration system.
Gentoo Kernel CI is checking all the files under sys-kernel and some kernel related eclass.
GkernelCI is based on buildbot and is currently checking only amd64 architecture.
We are recently moving to a docker based implementation and moving the testing to 
lava-docker system. There still are new testings and new architectures to be implemented. As we are in a GkernelCI moving phase there is many compatibility and implementation work to be addressed.

For see where you can help you can check the Gentoo Kernel CI issues list
[\[1\]](https://github.com/gentoo/Gentoo_kernelCI/issues)

GkernelCI can be found here [\[2\]](https://github.com/aliceinwire/GKernelCI)

The docker part is here [\[3\]](https://github.com/aliceinwire/GkernelCI_docker)

[Project:Gkernelci](https://wiki.gentoo.org/wiki/Project:Gkernelci)


| Contacts | Required Skills | 
|---|---|
|  |  | 

Gentoo sandbox is a cheap hack that aims to detect when ebuilds are accessing locations they aren't supposed to reach. It is implemented as LD\_PRELOAD library which generally makes it a bit of a hack. It is imperfect, sometimes requires hacks to stop breaking other software and sometimes need to be plain disabled.

I've proposed in the past to reimplement it as a overlay-alike filesystem using FUSE but never found time to work on it. The idea is rather simple — create a basic FUSE filesystem that wraps access to the root filesystem, add access control on top of it, add IPC to make it possible to edit access lists dynamically. Integrate everything into Portage, so it can be used in place of old sandbox.



| Contacts | Required Skills | 
|---|---|
|  |  | 

**Our best proposals, and a significant proportion of our total acceptances every year, come from student-initiated ideas** rather than those suggested by Gentoo developers. We highly encourage you to suggest your own idea based on what you think would make Gentoo a better distribution. If you do so, we **strongly recommend** you work with a potential mentor to develop your idea **before** proposing it formally. You can find a potential mentor by contacting alicef/lu\_zero/Bircoph or via discussion on the gentoo-soc mailing list or #gentoo-soc IRC channel.


| Contacts | Required Skills | 
|---|---|
|  |  |
