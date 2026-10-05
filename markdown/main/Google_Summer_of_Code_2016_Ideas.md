<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2016/Ideas | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2016/Ideas -->
---
title: Google Summer of Code/2016/Ideas
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2016/Ideas
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2016-03-07"
fingerprint: a6edb8424867bbf8
license: CC BY-SA 4.0
---

# Google Summer of Code/2016/Ideas

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![GSoC 2016 logo GSoC 2016 logo](https://wiki.gentoo.org/images/c/ce/Gsoc2016-sun-160x160.png)

Want to spend your summer contributing full-time to Gentoo, and get paid for it? Gentoo is in its 10th year in the Google Summer of Code. In the past, **most of our successful students have become Gentoo developers**, so your chances of becoming one are very good if you're accepted into this program.

**Most ideas listed here have a contact person associated with them. Please get in touch with them earlier rather than later to develop your idea into a complete application.** You can find many of them on Freenode's IRC network under the same username. If there is no contact information, please join the [gentoo-soc mailing list](https://www.gentoo.org/get-involved/mailing-lists/) or [#gentoo-soc on the Freenode IRC network](irc://irc.gentoo.org/gentoo-soc), and we will work with you to find a mentor and discuss your idea.

You don't have to apply for one of these ideas! You can come up with your own, and as long as it fits into Gentoo, we'll be happy to work with you to develop it. Remember, your project needs to have deliverables in less than 3 months of work in most cases. Be ambitious but not too ambitious ;)

## Students, please read this first

We have a **custom application template** that we will ask you to fill out. Here it is:

**Congratulations on applying for a project with Gentoo!** To improve your chances of succeeding with this project, we want to make sure you're sufficiently prepared to invest a full summer's worth of time on it. **In addition to the usual application, there are 2 specific actions and 2 pieces of info we would like to see from you:**

- **Use the tools that you will use in your project to make changes to code** (e.g., source code management \[SCM\] software such as CVS, Subversion, or git). Please use the same SCM as you will use for your project to check out one of our [repositories](https://www.gentoo.org/get-involved/get-code/), make a change to it, and post that change as a [patch](http://www.network-theory.co.uk/articles/patchintro.html) on a mailing list or [bug](https://bugs.gentoo.org/). Please fix a real bug reported in [Bugzilla](https://bugs.gentoo.org/) to show that you can use the tools to make a meaningful change. Your contact in Gentoo can help you determine which SCM and repository you should use for this as well as a good bug to fix. If your idea doesn't have a contact, please get in touch with us on the [gentoo-soc mailing list](https://www.gentoo.org/get-involved/mailing-lists/) or in real-time [on IRC](irc://irc.gentoo.org/gentoo-soc). Once you've made your change, link to it from your application.

- **Participate in our development community.** Please make a post to one of our [mailing lists](https://www.gentoo.org/get-involved/mailing-lists/) and link to it from your application ([archives.gentoo.org](https://archives.gentoo.org) holds past postings). The gentoo-soc list would be a good starting point, if you aren't subscribed to any others already. The best posts would be an introduction of the project you're applying for and a little background about you, to introduce yourself to the community and get some broader input about your project.

- **Give us your contact info and working hours.** Please provide your email address, home mailing address, and phone number. This is a requirement and provides for accountability on both your side and ours. Also, please tell us what hours you will be working and responsive to contact via email and IRC; these should sum to at least 35 hours a week.

These actions are things you will do extremely commonly as an open-source developer, and they really aren't that hard, so don't let them hold you back! The remainder of the application is free-form. **Please read our [application guidelines](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2016/Application_Guidelines) and [Google's FAQ](https://developers.google.com/open-source/gsoc/faq) to complete it.** Good luck!

## Ideas



**Clang is the main compiler of many operating systems and it is already working well on Linux, why not on Gentoo?** This project aims at making clang the main compiler in a Gentoo distribution. This involves updating a number of tools (e.g. gcc-config), eclasses (toolchain), Standard C++ library support and Clang build system itself. This project requires more than high dedication and ability to interact with multiple parties.



| Contacts | Required Skills | 
|---|---|
|  |  | 

[D2CC](https://bitbucket.org/mgorny/d2cc) was started as an attempt to fix shortcomings of distcc (and icecc). However, the author has quickly ran out of time and is unable to continue the work on it. This idea either implies continuing the project or starting from scratch.

The goal behind d2cc is to provide joblimit- & network-sandbox-friendly, dynamically scaling, zero-configuration distributed compilation solution for small local networks running multiple Gentoo hosts. The setup being as simple as starting the daemon on each host, and multicast being used to dynamically find other hosts.



| Contacts | Required Skills | 
|---|---|
|  |  | 

[Eudev](https://github.com/gentoo/eudev) is a standalone dynamic and persistent device naming support (aka userspace devfs) daemon that runs independently from the init system.    Eudev strives to remain init system and linux distribution neutral.  It is currently used as the devfs manager for more than a dozen different linux distros.  This project would be to fully document it's API's, and perform an audit on all the existing tests.  Create additional tests in areas of code not currently or inadequately covered.  Work to resolve any outstanding bugs.  Extend any of it's utility scripts as needed to further enhance it's ease of use and compatibility with hardware and software.



| Contacts | Required Skills | 
|---|---|
|  |  | 

If you're reading this, you're on the Gentoo Wiki. It has been the main documentation source for Gentoo for well over two years now. We have plenty of content, but need a few things added to the software, MediaWiki, to provide even better documentation. Here's a few things that need to be done:

- Create a document export pipeline for PDF and common e-reader formats for the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) that renders beautiful, print-quality files containing our most important piece of documentation whenever it is updated.
- Improve Handbook navigation on smartphones: Currently, we share the Handbook navigation on all screen sizes in the form of a large box floating to the right of the content. That doesn't work on small screens. Your task is to think up and implement a better replacement.
- Fork-a-page: We have quite a few content pages on the Wiki that are restricted. We'd like a way for contributors to fork pages that are read-only for them, implement improvements, and allow the privileged developers to merge them back in easily.
- Kernel configuration documentation: Currently, we manually copy-paste the output of menuconfig entries to visualize Kernel configuration changes people need to get certain things done. We can do better: This extension should be able to ingest a set of .config parameters (`CONFIG_FOO=M`) and render the location and description for a set of current kernel versions.

The above list is neither exhaustive nor compulsory. We'll define a good set of tasks depending on your interests and abilities. If you have other tasks that you think need doing, do let us know.



| Contacts | Required Skills | 
|---|---|
|  |  | 

Gentoo-gpg is to continue the work done in a previous years GSOC project "gentoo-keys".

The gentoo-keys project has a system for managing and distributing the Gentoo developers and release OpenPGP keys. This project will encompass several sub tasks.

1. Work with the infrastructure team for the completion and generation of needed scripts to utilize the gentoo-keys system. This will include automated scripts to update the gpg keys seeds and keyrings. Generate email reminders for keys nearing expiry. Generate automatic update scripts for user systems. Extend the various gentoo-keys library code and command line interfaces as needed.
2. Code a new Meta-Manifest system for the Gentoo ebuild tree.  See [GLEP 58](https://www.gentoo.org/glep/glep-0058.html) for details
3. Integrate gkeys library code into Gentoo's package management system, portage (could also include pkgcore). This includes meta-manifest handling changes as well their gpg verification. This can also include verification of git based ebuild tree commits.
4. Extending gkeys and gkeys-gen features.



| Contacts | Required Skills | 
|---|---|
|  |  | 

[oVirt](https://www.ovirt.org/) is a complete open sourced virtualization management platform working with KVM. ovirt-engine is the backend server that does the management, with client UI and RESTful API. Patches should be submitted to upstream oVirt projects, mainly ovirt-engine, to provide support to using Gentoo as operating system for the oVirt engine. This project includes the packaging of ovirt-engine itself and required dependencies.

Some initial packaging work:



| Contacts | Required Skills | 
|---|---|
|  |  | 

[oVirt](https://www.ovirt.org/) is a complete open sourced virtualization management platform working with KVM. The ovirt-guest-agent is the service that runs in the guests. Patches should be submitted to upstream oVirt projects, mainly ovirt-guest-agent, to provide support to using Gentoo as operating system for the guests. This project includes the packaging of ovirt-guest-agent itself and required dependencies.

Some relevant documentation from Debian port:



| Contacts | Required Skills | 
|---|---|
|  |  | 

[oVirt](https://www.ovirt.org/) is a complete open sourced virtualization management platform working with KVM. VDSM is the agent that runs on each oVirt managed host. Patches should be submitted to upstream oVirt projects, mainly VDSM, to provide support to using Gentoo as operating system for oVirt managed hosts. This project includes the packaging of VDSM itself and required dependencies.

Some relevant documentation from Debian port:



| Contacts | Required Skills | 
|---|---|
|  |  | 

OfflineIMAP is a python program included in Gentoo to synchronize emails via IMAP. Since some months, the **imapfw** project was started to become the **next generation tool** with a *new approach* and *extended features*.

There are various features that can be written for imapfw like:

- enable syncing of mail between a local Maildir and a remote IMAP server
- rewrite the offlineimap program with the framework
- start a new in-house IMAP library


A more [detailed page](http://offlineimap.org/gsoc/) dedicated to the GSoC can be found on the OfflineIMAP website.



| Contacts | Required Skills | 
|---|---|
|  |  | 

Consistently generate custom Linux kernel configurations from curated sources ([on github](https://github.com/Calchan/kernelconfig)).

Compiling a kernel is easy, configuring it not so much. Unless you have the time and skill to follow kernel development, it is best to leave that to a team of specialists. But where to download up to date configurations? What to do if you need to do the same changes over and over with each new version? Enter kernelconfig. It will automatically download a base configuration matching the kernel you want to compile from a curated source of your choice (Debian, Ubuntu, Fedora, Liquorix, etc...), customize it with a number of options which suit your taste and needs, apply macros to set a number of options in a smart way, and more.

kernelconfig is still in its infancy and needs a lot of love. The author has a lot of ideas to get you started, but yours are welcome too!



| Contacts | Required Skills | 
|---|---|
|  |  | 

Multiple projects such as portage-utils and [pkgcore](https://github.com/pkgcore/pkgcore) have their own C-based implementations of parsing package atoms and related support. At some point it would be nice if they all shared a common, well-tested implementation in the form of a library along with bindings for various languages (python currently being the most important). The main focuses here would be on efficiency, test coverage, and a well-defined API to support external tool development.



| Contacts | Required Skills | 
|---|---|
|  |  | 

Ensuring ebuilds are working before committing them is a mostly personal process.  A utility that allows one to quickly (one command) run a battery of installation and optionally runtime testing against an ebuild would streamline this process significantly.  The basic requirements are the same as any other test runner: isolated and repeatable.  This work can scope out or in as needed to fit within time constraints.


| Contacts | Required Skills | 
|---|---|
|  |  | 

OpenRC is the default init system in Gentoo and a viable alternative for various operating systems and distributions.

The project aims to make OpenRC an even more compelling and easy to deploy init system.

Tasks:

- Better integrations with non-sysvinit systems such as busybox init and runit.
- Provide migration/importers to compatible init scripts (e.g. systemd units)
- Better integration with network management programs (e.g. connman)
- Improved support for service hotplugging



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

Relevant discussions at:

- [https://archives.gentoo.org/gentoo-project/message/00e02ff494857599633e2bbc30520ca3](https://archives.gentoo.org/gentoo-project/message/00e02ff494857599633e2bbc30520ca3)
- [https://archives.gentoo.org/gentoo-dev/message/c9675f0359679007054b57de123c0bf3](https://archives.gentoo.org/gentoo-dev/message/c9675f0359679007054b57de123c0bf3)



| Contacts | Required Skills | 
|---|---|
|  |  | 

g-octave is a generator of ebuilds for octave-forge package, that does everything on its own. We have the g-sorcery framework now, that tries to encapsulate everything needed to create such ebuild generators easily. This project aims to port g-octave to g-sorcery framework, including implement the features that lacks on the framework but are used by g-octave.


| Contacts | Required Skills | 
|---|---|
|  |  | 

[Puppet](https://puppetlabs.com/) is a tool for [configuration management](https://en.wikipedia.org/wiki/Configuration_management) — in other words, maintaining Gentoo by defining infrastructure as code rather than manually editing config files. This is extremely powerful for scaling servers up, and it greatly increases the efficiency of sysadmins. It further makes it easy to track changes to configurations over time, maintain them in git, do rollbacks, and more. The [Puppet Portage module](https://github.com/gentoo/puppet-portage/) supports a bunch of Gentoo specific operations (more info can be found in its [README](https://github.com/gentoo/puppet-portage/blob/master/README.markdown) file). There are lots of issues and feature requests needed, that can be found in its [issue tracker](https://github.com/gentoo/puppet-portage/issues) in Github that should be implemented. This is an extension of the [Puppet Portage module](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas/Puppet_Portage_module) project from previous year.



| Contacts | Required Skills | 
|---|---|
|  |  | 

Come up with a remote service (no webpages!) to estimate/calculate what the path of upgrade would be for an old Gentoo installation. It is a known fact that once a Gentoo system is old enough, it can be problematic (or really difficult) to get it running on newer **@system** packages and profiles. This project is made of various parts, such as an DSL for encoding the upgrade path (result of the calculation), coding the actual remote service, coding the algorithms for the estimation of the upgrade path, etc...



| Contacts | Required Skills | 
|---|---|
|  |  | 

Gentoo uses categories now. A package can only be in a single category, which is very limiting because generally things don't fit perfectly into one place without other possibilities. Tags could make it a lot easier to find packages they're looking for by doing Boolean searches like: kde AND mail. This project would add support for tags to Portage and would allow for backwards compatibility of categories as tags.



| Contacts | Required Skills | 
|---|---|
|  |  | 

Gentoo has long used a common text file based layout to store information about currently installed packages on the system. For [pkgcore](https://github.com/pkgcore/pkgcore), it would be nice to move on to something more efficient and yet remain modular enough to support various tools that need access to the data. Leveraging a well-structured sqlite datastore would probably work very well along with the proper API support from pkgcore (and/or libebuild).



| Contacts | Required Skills | 
|---|---|
|  |  | 

Build an automated machine registry for automated server installations. This idea involves creating a Registry Server (no Web UI for the initial project) that would receive requests from clients embedded in Gentoo minimal install media (whatever the format).

The idea is when a server boots up, it registers automatically to the Registry Server and then, according to some of the data the server registers with and profiles associated with it (Type of machine, capabilities, model), it can either be automatically assigned a role (then tie it up with Puppet/Chef), or just be accessed for further configuration.

The project would involve writing:

- Registry
- Client code
- Client and Registry ebuild
- Scripts to add the client to the install media
- Tools to query the Registry (machines avail, add profile, add/update/del machine, etc).

The registry would maintain information about the state of machines, maybe even add handlers for monitoring systems etc.



| Contacts | Required Skills | 
|---|---|
|  |  | 

One good way of distributing software is to avoid the concept of binary packages and release full OS atomic updates remotely. One can create an extremely minimal system consisting of not much more than a kernel and would fetch needed files on-demand.

The idea of this project is to automate the building of minimal stateless Gentoo. This stateless Gentoo could be the base OS for large scale software deployment, but possibly desktop as well.

Several other Linux systems mainly, CoreOS and CernVM already offer such stateless minimal systems. (source of good inspiration for this project)



| Contacts | Required Skills | 
|---|---|
|  |  | 

Gentoo packages are tested manually by a team of developers, generally on-demand, then marked as stable. Lots of packages are behind the stabilization process. One idea would be to automate the process using continuous integration (CI). The work could involve: designing CI for Gentoo packages, scheduling and distributing CI stabilization jobs to users willing to give some CPU time, and updating the portage tree automatically.



| Contacts | Required Skills | 
|---|---|
|  |  | 

This should be a console configuration and build tool for customizing a given stage3 tarball with user-selected packages and USE flags, and rebuilding into a deployable stage4 rootfs.

Basic Features:

- Similar to "make menuconfig" for the kernel or buildroot
- Input fields for packages, USE flags, bootloader, and kernel sources
  - use dependency resolution to verify selection, update make.conf
  - bootloader/kernel options should include "none"
- Select input stage3 and output/logging destination, save/load configuration
- Automated download, chroot, build, archive results
- Support current autobuilds, experimental, hardened on all current arches
- Use ncurses/slang/other, include internationalization support



| Contacts | Required Skills | 
|---|---|
|  |  | 

**Our best proposals, and a significant proportion of our total acceptances every year, come from student-initiated ideas** rather than those suggested by Gentoo developers. We highly encourage you to suggest your own idea based on what you think would make Gentoo a better distribution. If you do so, we **strongly recommend** you work with a potential mentor to develop your idea **before** proposing it formally. You can find a potential mentor by contacting Denis/Rafael or via discussion on the gentoo-soc mailing list or #gentoo-soc IRC channel.


| Contacts | Required Skills | 
|---|---|
|  |  |
