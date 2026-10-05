<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2014/Ideas | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2014/Ideas -->
---
title: Google Summer of Code/2014/Ideas
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2014/Ideas
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2015-02-16"
fingerprint: b7a998424a3583b8
license: CC BY-SA 4.0
---

# Google Summer of Code/2014/Ideas

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![GSoC 2014 logo GSoC 2014 logo](https://wiki.gentoo.org/images/thumb/b/b2/Gsoc-logo-2014.png/160px-Gsoc-logo-2014.png)

Want to spend your summer contributing full-time to Gentoo, and get paid for it? Gentoo is in its 9th year in the Google Summer of Code. In the past, **most of our successful students have become Gentoo developers**, so your chances of becoming one are very good if you're accepted into this program.

**Most ideas listed here have a contact person associated with them. Please get in touch with them earlier rather than later to develop your idea into a complete application.** You can find many of them on Freenode's IRC network under the same username. If there is no contact information, please join the [gentoo-soc mailing list](http://www.gentoo.org/main/en/lists.xml) or [#gentoo-soc on the Freenode IRC network](irc://irc.gentoo.org/gentoo-soc), and we will work with you to find a mentor and discuss your idea.

You don't have to apply for one of these ideas! You can come up with your own, and as long as it fits into Gentoo, we'll be happy to work with you to develop it. Remember, your project needs to have deliverables in less than 3 months of work in most cases. Be ambitious but not too ambitious ;)

# Read this first

We have a **custom application template** that we will ask you to fill out. Here it is:

**Congratulations on applying for a project with Gentoo!** To improve your chances of succeeding with this project, we want to make sure you're sufficiently prepared to invest a full summer's worth of time on it. **In addition to the usual application, there are 2 specific actions and 2 pieces of info we would like to see from you:**

- **Use the tools that you will use in your project to make changes to code** (e.g., source code management \[SCM\] software such as CVS, Subversion, or git). Please use the same SCM as you will use for your project to check out one of our [repositories](http://anoncvs.gentoo.org/) ([more repositories](http://git.overlays.gentoo.org/gitweb/)), make a change to it, and post that change as a [patch](http://www.network-theory.co.uk/articles/patchintro.html) on a mailing list or [bug](http://bugs.gentoo.org/). Please fix a real bug reported in [Bugzilla](http://bugs.gentoo.org/) to show that you can use the tools to make a meaningful change. Your contact in Gentoo can help you determine which SCM and repository you should use for this as well as a good bug to fix. If your idea doesn't have a contact, please get in touch with us on the [gentoo-soc mailing list](http://www.gentoo.org/main/en/lists.xml) or in real-time [on IRC](irc://irc.gentoo.org/gentoo-soc). Once you've made your change, link to it from your application.

- **Participate in our development community.** Please make a post to one of our [mailing lists](http://www.gentoo.org/main/en/lists.xml) and link to it from your application ([archives.gentoo.org](http://archives.gentoo.org) holds past postings). The gentoo-soc list would be a good starting point, if you aren't subscribed to any others already. The best posts would be an introduction of the project you're applying for and a little background about you, to introduce yourself to the community and get some broader input about your project.

- **Give us your contact info and working hours.** Please provide your email address, home mailing address, and phone number. This is a requirement and provides for accountability on both your side and ours. Also, please tell us what hours you will be working and responsive to contact via email and IRC; these should sum to at least 35 hours a week.

These actions are things you will do extremely commonly as an open-source developer, and they really aren't that hard, so don't let them hold you back! The remainder of the application is free-form. **Please read our [application guidelines](http://www.gentoo.org/proj/en/userrel/soc/applying.xml) and [Google's FAQ](http://www.google-melange.com/gsoc/document/show/gsoc_program/google/gsoc2014/help_page) to complete it.** Good luck!

# Ideas





#### Improvements to gentoolkit

Gentoolkit's euse application is in need of a rewrite from bash to python. This conversion would not be enough for a complete GSoC project on it's own, but could be extended with additional features, functions that could be shared with other gentoolkit utilities. There are other changes desired in equery, eclean, enalyze that can be done as well. Familiarity with gentoo and gentoolkit would be a plus in aiding the applicant to prepare a complete and thorough application, but not a necessity. For further information contact us to discuss your ideas and/or proposal.

For some additional gentoolkit info, see some irc chat I've posted that describes more things that can be done.
[gentoolkit-gsoc chat](http://dev.gentoo.org/~dolsen/gentoolkit-gsoc)



| Contacts | Required Skills | 
|---|---|
|  |  | 

Gentoo has a great opportunity to be amazing for use in public clouds like Amazon's AWS platform. For this to work well, we will need to integrate cloud support much more thoroughly into our release-engineering processes. For examples, [catalyst](http://www.gentoo.org/proj/en/releng/catalyst/) and related [releng](http://www.gentoo.org/proj/en/releng/) tools should produce and upload images to AWS, in addition to supporting tools like [Vagrant](http://www.vagrantup.com/) with pregenerated boxes (perhaps using [veewee](https://github.com/jedi4ever/veewee)).

This could be supplemented by improving Gentoo's support for [DevOps](https://en.wikipedia.org/wiki/DevOps)-style workflows. For example, we should make it extremely easy to set up [modern monitoring tools](http://dberkholz.com/2013/03/14/opportunities-for-gentoo/) like Graphite, to push development from local boxes to the cloud, to integrate with continuous integration testing tools like [Jenkins](http://jenkins-ci.org/) or [Travis](https://travis-ci.org/), and more.



| Contacts | Required Skills | 
|---|---|
|  |  | 

#### Log collector/analyzer for tinderbox build logs

Flameeyes's tinderbox has been using a custom, spit-and-chewingum log collector and analyzer written in Ruby for a while. A replacement is dearly needed.

The ideal software would be a daemon service that receives build logs from Portage, both for storage in full and for metadata extraction (package name, maintainers, type of failure if any, emerge --info output, ...), and then provide a (password-protected) web interface that allows to read the failing logs, and open bugs at Gentoo's Bugzilla.

Bonus points for:

- being completely standalone (current setup depends on Amazon's S3);
- attaching logs instead of just linking them (size limits could make it hard to achieve);
- searching for the list of already open bugs for the failing package;
- proper integration with Portage so that no extra software is required on the tinderbox side to push the logs;
- standard, HTTP-based protocol between the tinderbox and the collector, which can be proxied through Squid;
- being written in Python to reduce the language fragmentation (current setup is written in Shell and Ruby).

Further information on [Flameeyes's Weblog](https://flameeyes.blog/2013/02/28/gsoc-proposal-a-better-log-aggregator-analyzer/).



| Contacts | Required Skills | 
|---|---|
|  |  | 

Gentoo uses categories now. A package can only be in a single category, which is very limiting because generally things don't fit perfectly into one place without other possibilities. Tags could make it a lot easier to find packages they're looking for by doing Boolean searches like: kde AND mail. This project would add support for tags to Portage and would allow for backwards compatibility of categories as tags.



| Contacts | Required Skills | 
|---|---|
|  |  | 

This is a continuation of a project from a previous year. The aim is to complete the creation of a stable api for consumer app use. The api will be fully capable of performing merges, unmerges, getting dependency graphs, etc.. An additional task is to provide a c interface lib that connects to the python api functions and classes. This allows further use of the api for c, c++ based applications and portage frontends.




| Contacts | Required Skills | 
|---|---|
|  |  | 

The idea behind this potential GSoC project is to come up with a remote service (no webpages!) to estimate/calculate what the path of upgrade would be for an old Gentoo          installation. It is a known fact that once a Gentoo system is old enough, it can be problematic (or really difficult) to get it running on newer **@system** packages and       profiles. This project is made of various parts, such as an DSL for encoding the upgrade path (result of the calculation), coding the actual remote service, coding the algorithms for the estimation of the upgrade path, etc.



| Contacts | Required Skills | 
|---|---|
|  |  | 

Currently, bugs are usually bound to packages through naming the relevant package in Summary. Although this works, it is quite limited. It is unsuitable for obtaining the package in an automated way or adding long lists of relevant packages in a searchable manner.

The idea is to write a Bugzilla extension which would allow bindings bugs with actual package lists. Potential uses include:

- providing auto-completion for package names,
- ability to list multiple packages without polluting the summary field,
- ability to clearly obtain all bugs relevant to a package of choice,
- providing an easy way to assign bug to the package maintainers.

Additionally, a database for package metadata cache would need to be designed, and a post-sync hook would need to be written to update it.


| Contacts | Required Skills | 
|---|---|
|  |  | 

In the past year, what was the classical network system on Gentoo, known as *oldnet*, has been split to an independent package for improving maintenance, and is now known as *netifrc*.

*netifrc* provides a lot of advanced networking functionality (VLANs, bridging, bonding, pppoe, ipv6, ADSL, CCW and more), but is still dependant on OpenRC.
It would be nice for *netifrc* to remain a viable contender against other advanced network configuration systems, such as netctl.

For this project, you would implement a compatibility layer for *netifrc* usage of OpenRC functionality (this is started already as function.sh for systemd users in Gentoo), as well as provide systemd services to run each *net.$IFACE* service, via symlinks, similar to the existing net.lo to net.iface symlinks. This project would also encourage usage of *netifrc* OUTSIDE of Gentoo, so while any applicant should be prepared to start with *netifrc* on Gentoo, testing on a non-Gentoo distribution towards the end of the project should be included in any project proposal.





| Contacts | Required Skills | 
|---|---|
|  |  | 

OpenRC is the default init system in Gentoo and a viable alternative for various operating system and distribution.

The project aims to make OpenRC an even more compelling and easy to deploy init system.

Tasks:

- Better integrations with non-sysvinit systems such as busybox init and runit.
- Provide migration/importers to compatible init scripts (e.g. systemd units)
- Better integration with network management programs (e.g. connman)
- Improved support for service hotplugging




| Contacts | Required Skills | 
|---|---|
|  |  | 

One possible way of distributing software is to avoid the concept of binary packages and release full OS atomic updates remotely. One can create an extremely minimal system consisting of not much more than a kernel and would fetch needed files on-demand. It has great applications on the cloud for massive deployment of VM (CoreOS), easy maintenance netbook (ChromeOS), or science on the LHC (CernVM). One project could be to create automatically an extremely small Gentoo VM that would fetch needed files on boot, ala [μ-CernVM](http://cernvm.cern.ch/portal/release_3.1.1), depending on the profile.



| Contacts | Required Skills | 
|---|---|
|  |  | 

Many of the typical Gentoo arch tester duties consist of repetitive tasks which could really gain in being unsupervised. Package keywording, stabilization, QA bug filing... should really be done by machines, not humans. Some distributions have build farms, but Gentoo has many options to cover making the testing process quite intensive. One idea that every Gentoo developer wants is to incorporate continuous integration into packages, which could be part of the project. The idea could be taken further and distribute the automated tests to idle home users machines, leveraging Gentoo infrastructure. It could have great impacts on QA and accelerating the stabilization process.



| Contacts | Required Skills | 
|---|---|
|  |  | 

[Puppet](https://puppetlabs.com/) is a tool for [configuration management](https://en.wikipedia.org/wiki/Configuration_management) — in other words, maintaining Gentoo by defining infrastructure as code rather than manually editing config files. This is extremely powerful for scaling servers up, and it greatly increases the efficiency of sysadmins. It further makes it easy to track changes to configurations over time, maintain them in git, do rollbacks, and more. The [Puppet Portage module](https://github.com/gentoo/puppet-portage/) supports a bunch of Gentoo specific operations (more info can be found in its [README](https://github.com/gentoo/puppet-portage/blob/master/README.markdown) file). There are lots of issues and feature requests needed, that can be found in its [issue tracker](https://github.com/gentoo/puppet-portage/issues) in Github that should be implemented. This is an extension of the [Puppet Portage module](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas/Puppet_Portage_module) project from previous year.



| Contacts | Required Skills | 
|---|---|
|  |  | 

This project involves integrating an alternative standard C library called musl into Gentoo, both for cross compiling and native compiling.  Musl aims to be "lightweight, fast, simple, free, and strives to be correct in the sense of standards-conformance and safety." [\[2\]](http://www.musl-libc.org)  So far, stage4 tarballs have been built for amd64 [\[3\]](http://distfiles.gentoo.org/experimental/amd64/musl), i686 [\[4\]](http://distfiles.gentoo.org/experimental/x86/musl) and armv7a-hardfloat-eabi [\[5\]](http://distfiles.gentoo.org/experimental/arm/musl). These were initially built using cross compiling toolchians which themselves were built using crossdev [\[6\]](http://www.gentoo.org/proj/en/base/embedded/handbook/cross-compiler.xml?style=printable), but then were rebuilt on native hardware using home grown scripts [\[7\]](http://git.overlays.gentoo.org/gitweb/?p=proj/releng.git;a=tree;f=tools-musl), and not catalyst [\[8\]](https://www.gentoo.org/proj/en/releng/catalyst). Picking up from here, the next steps in the project are:

1. To build cross compiling toolschains for mips32r2-o32, mipsel3-o32 and mips64r2-n32 architectures/abis. This will involve properly intergrating musl with crossdev which currently has some bugs [\[9\]](https://bugs.gentoo.org/show_bug.cgi?id=498114).
2. To build stage 4 tarballs for the those architectures.  This involves, among other things, patching various packages that don't conform to strict POSIX which musl assumes [\[10\]](http://git.overlays.gentoo.org/gitweb/?p=proj/hardened-dev.git;a=shortlog;h=refs/heads/musl).
3. These stages need to be converted to stage3's, properly built using catalyst rather than using the ROOT=rootfs emerge -e @system technique which the homegrown scripts use.
4. If all goes well, the project can continue to porting over the hardened tool chain to those architectures [Project:Hardened](https://wiki.gentoo.org/wiki/Project:Hardened).



| Contacts | Required Skills | 
|---|---|
|  |  | 

**Clang is the main compiler of many operating systems and it is already working well on Linux, why not on Gentoo?**. This project aims in making clang the main compiler in a Gentoo distribution. This involves updating a number of tools (e.g. gcc-config), eclasses (toolchain), Standard C++ library support and Clang build system itself. This project requires more than high dedication and ability to interact with multiple parties.


| Contacts | Required Skills | 
|---|---|
|  |  | 

g-octave is a generator of ebuilds for octave-forge package, that does everything on its own. We have the g-sorcery framework now, that tries to encapsulate everything needed to create such ebuild generators easily. This project aims to port g-octave to g-sorcery framework, including implement the features that lacks on the framework but are used by g-octave.


| Contacts | Required Skills | 
|---|---|
|  |  |
