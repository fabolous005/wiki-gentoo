<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2021/Ideas | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2021/Ideas -->
---
title: Google Summer of Code/2021/Ideas
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2021/Ideas
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-05-29"
fingerprint: "3719ba515d052378"
license: CC BY-SA 4.0
---

# Google Summer of Code/2021/Ideas

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![GSoC 2021 logo GSoC 2021 logo](https://wiki.gentoo.org/images/c/ce/Gsoc2016-sun-160x160.png)

Want to spend your summer contributing full-time to Gentoo, and get paid for it? Gentoo is in its 10th year in the Google Summer of Code. In the past, **most of our successful students have become Gentoo developers**, so your chances of becoming one are very good if you're accepted into this program.

**Most ideas listed here have a contact person associated with them. Please get in touch with them earlier rather than later to develop your idea into a complete application.** You can find many of them on Libera.Chat's IRC network under the same username. If there is no contact information, please join the [gentoo-soc mailing list](https://www.gentoo.org/get-involved/mailing-lists/) or [#gentoo-soc on the Libera.Chat IRC network](irc://irc.gentoo.org/gentoo-soc), and we will work with you to find a mentor and discuss your idea.

**You don't have to apply for one of these ideas! You can come up with your own**, and as long as it fits into Gentoo, we'll be happy to work with you to develop it. Remember, your project needs to have deliverables in less than 3 months of work in most cases. Be ambitious but not too ambitious ;)

## Students, please read this first

We have a **custom application template** that we will ask you to fill out. Here it is:

**Congratulations on applying for a project with Gentoo!** To improve your chances of succeeding with this project, we want to make sure you're sufficiently prepared to invest a full summer's worth of time on it. **In addition to the usual application, there are 2 specific actions and 2 pieces of info we would like to see from you:**

- **Use the tools that you will use in your project to make changes to code** (e.g., source code management \[SCM\] software such as CVS, Subversion, or git). Please use the same SCM as you will use for your project to check out one of our [repositories](https://www.gentoo.org/get-involved/get-code/), make a change to it, and post that change as a [patch](<https://en.wikipedia.org/wiki/Patch_(Unix)>) on a mailing list or [bug](https://bugs.gentoo.org/). Please fix a real bug reported in [Bugzilla](https://bugs.gentoo.org/) to show that you can use the tools to make a meaningful change. Your contact in Gentoo can help you determine which SCM and repository you should use for this as well as a good bug to fix. If your idea doesn't have a contact, please get in touch with us on the [gentoo-soc mailing list](https://www.gentoo.org/get-involved/mailing-lists/) or in real-time [on IRC](irc://irc.gentoo.org/gentoo-soc). Once you've made your change, link to it from your application.

- **Participate in our development community.** Please make a post to one of our [mailing lists](https://www.gentoo.org/get-involved/mailing-lists/) and link to it from your application ([archives.gentoo.org](https://archives.gentoo.org) holds past postings). The gentoo-soc list would be a good starting point, if you aren't subscribed to any others already. The best posts would be an introduction of the project you're applying for and a little background about you, to introduce yourself to the community and get some broader input about your project.

- **Give us your contact info and working hours.** Please provide your email address, home mailing address, and phone number. This is a requirement and provides for accountability on both your side and ours. Also, please tell us what hours you will be working and responsive to contact via email and IRC; these should sum to at least 35 hours a week.

These actions are things you will do extremely commonly as an open-source developer, and they really aren't that hard, so don't let them hold you back! The remainder of the application is free-form. **Please read our [application guidelines](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2021/Application_Guidelines) and [Google's FAQ](https://developers.google.com/open-source/gsoc/faq) to complete it.** Good luck!

## Ideas



The big data infrastructures are mostly built on the Java virtual machine ecosystem, most notably in Java and Scala.

Nevertheless, Java has not been adopted smoothly into GNU/Linux distributions.  The packaging of Java software are considered difficult by the GNU/Linux community (e.g. [Debian](https://wiki.debian.org/Java/Packaging), [Archlinux](https://wiki.archlinux.org/index.php/Java_package_guidelines), [Fedora](https://fedoraproject.org/wiki/Java)).  At the same time, the Java community has its own set of repositories like [maven](https://maven.apache.org), functionally similar to packages in GNU/Linux distributions.

The [Gentoo Java Project](https://wiki.gentoo.org/wiki/Project:Java) has done a good job laying out the framework of the Java ecosystem in Gentoo. At the same time, there are still thousands of useful Java packages to be packaged and maintained.  Last year, Zongyu Zhang has developed the Maven ebuild generator and published the [spark overlay](https://github.com/6-6-6/spark-overlay), make spark available for Gentoo users.  This project will build upon that, to design and set up a test framework for the generated ebuilds, handle kotlin and scala packages, and add [h2o](https://mvnrepository.com/artifact/ai.h2o/h2o-core) big data analysis platform into the overlay.



| Contacts | Required Skills | 
|---|---|
|  |  | 

## External resources

Gentoo is running a number of core services for both users and developers, many of which are bespoke for the community. Gentoo developers are constantly working on improving these services to deliver an enhanced experience for all Gentoo users and developers. This year, you have the chance to be part of a major overhaul of these services. That is, we have been rewriting a lot of these services and you can help with finally getting these rewrites into production to modernize and improve our infrastructure. The goal at end of the project is to have the old services fully replaced in production, to enhances the services we offer for all Gentoo users and developers.

In detail, this involves:

- **Gentoo mail archives ([https://archives.gentoo.org](https://archives.gentoo.org)):** Last year, we've created a complete rewrite of our mail archive ([source code](https://gitweb.gentoo.org/sites/archives.git/)). Mostly notably, this rewrite   provides the possibility of searching for mails using full text search. This rewrite is nearly ready for production. Your job would be to finish the work on the ingress of new mails as well as getting the new solution into production - meaning you will understand and fixing bugs that arise, once the rewrite is in production.
- **Gentoo GLSAMaker ([https://glsamaker.gentoo.org](https://glsamaker.gentoo.org), internal use only):** Last year, we've also created a complete rewrite of our GLSAMaker ([source code](https://gitweb.gentoo.org/proj/glsamaker.git/)). The GLSAMaker is a tool for Gentoo's Security Team to create and publish Gentoo Linux Security Advisories (GLSAs). The new rewrite is still missing some features compared to the old GLSAMaker. Part of your responsibility here is implementing these missing features. Also you will be working with the security team to clarify which features might be useful to enhance their workflow. Finally, the goal here is also to get the new rewrite into production as well. Again, this means understanding and fixing bugs that arise, once the rewrite is in production.
- **Gentoo Packages Database ([https://packages.gentoo.org](https://packages.gentoo.org)):** Finally, we have also created a rewrite of our packages database last year ([source code](https://gitweb.gentoo.org/sites/soko.git/)). This rewrite is already running live in production. Recently, we've been starting to add more functionality to let the user customize the information based on their preferences. These changes are, however, not working perfectly well currently. You've got the chance to enhance the user experience by reworking the customization features. Part of this will also include communicating with the packages.gentoo.org userbase to see which features are needed here.
- **Gentoo Recruiting Platform:** In former times we have been using our own recruiting platform ([source code](https://gitweb.gentoo.org/proj/recruiting-webapp.git/)) for recruiting new Gentoo developers. Actually this is an old Google Summer of Code project. This project is currently not used in production anymore. Reviving this project will help recruiting more developers in future, which in turn will help the whole community. So the goal here is to get the recruiting platform working in production again.
- **Containerization of our existing services:** We've already been starting to containerize a lot of our services. However, there is still work needed to create gentoo-based container images for some of our services. Part of this effort is to create container images for services that are not fully containerized yet, e.g. the mail archive or improve existing container images which are not using Gentoo-based images yet.


After the end of the project, you've got the chance to continue helping to maintain the services as an external maintainer in case you are interested. 
Alternatively, you can take even more responsibility by becoming an official Gentoo developer and take care of the services this way. 
Of course, the existing base of developers will help you with any questions that arise throughout.

All in all, you've got the chance to become a major part of the modernization and enhancement of our core services for users and developers. These projects touch many of the core services that Gentoo developers and users interact with to both keep the distribution running and use it daily.

Last but not least, most of our services are nowadays written in Go. So ideally speaking you should be either familiar with the language or don't mind to dig into it while working on the services. Don't get scared by this though, if you are motivated and ready to learn something new, you are good to go. Please, don't hesitate to reach out in case of any questions.



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
