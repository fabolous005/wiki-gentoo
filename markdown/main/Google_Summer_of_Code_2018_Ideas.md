<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2018/Ideas | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2018/Ideas -->
---
title: Google Summer of Code/2018/Ideas
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2018/Ideas
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-03-06"
fingerprint: "36b5f6505a27a5f8"
license: CC BY-SA 4.0
---

# Google Summer of Code/2018/Ideas

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![GSoC 2018 logo GSoC 2018 logo](https://wiki.gentoo.org/images/c/ce/Gsoc2016-sun-160x160.png)

Want to spend your summer contributing full-time to Gentoo, and get paid for it? Gentoo is in its 10th year in the Google Summer of Code. In the past, **most of our successful students have become Gentoo developers**, so your chances of becoming one are very good if you're accepted into this program.

**Most ideas listed here have a contact person associated with them. Please get in touch with them earlier rather than later to develop your idea into a complete application.** You can find many of them on Freenode's IRC network under the same username. If there is no contact information, please join the [gentoo-soc mailing list](https://www.gentoo.org/get-involved/mailing-lists/) or [#gentoo-soc on the Freenode IRC network](irc://irc.gentoo.org/gentoo-soc), and we will work with you to find a mentor and discuss your idea.

**You don't have to apply for one of these ideas! You can come up with your own**, and as long as it fits into Gentoo, we'll be happy to work with you to develop it. Remember, your project needs to have deliverables in less than 3 months of work in most cases. Be ambitious but not too ambitious ;)

## Students, please read this first

We have a **custom application template** that we will ask you to fill out. Here it is:

**Congratulations on applying for a project with Gentoo!** To improve your chances of succeeding with this project, we want to make sure you're sufficiently prepared to invest a full summer's worth of time on it. **In addition to the usual application, there are 2 specific actions and 2 pieces of info we would like to see from you:**

- **Use the tools that you will use in your project to make changes to code** (e.g., source code management \[SCM\] software such as CVS, Subversion, or git). Please use the same SCM as you will use for your project to check out one of our [repositories](https://www.gentoo.org/get-involved/get-code/), make a change to it, and post that change as a [patch](http://www.network-theory.co.uk/articles/patchintro.html) on a mailing list or [bug](https://bugs.gentoo.org/). Please fix a real bug reported in [Bugzilla](https://bugs.gentoo.org/) to show that you can use the tools to make a meaningful change. Your contact in Gentoo can help you determine which SCM and repository you should use for this as well as a good bug to fix. If your idea doesn't have a contact, please get in touch with us on the [gentoo-soc mailing list](https://www.gentoo.org/get-involved/mailing-lists/) or in real-time [on IRC](irc://irc.gentoo.org/gentoo-soc). Once you've made your change, link to it from your application.

- **Participate in our development community.** Please make a post to one of our [mailing lists](https://www.gentoo.org/get-involved/mailing-lists/) and link to it from your application ([archives.gentoo.org](https://archives.gentoo.org) holds past postings). The gentoo-soc list would be a good starting point, if you aren't subscribed to any others already. The best posts would be an introduction of the project you're applying for and a little background about you, to introduce yourself to the community and get some broader input about your project.

- **Give us your contact info and working hours.** Please provide your email address, home mailing address, and phone number. This is a requirement and provides for accountability on both your side and ours. Also, please tell us what hours you will be working and responsive to contact via email and IRC; these should sum to at least 35 hours a week.

These actions are things you will do extremely commonly as an open-source developer, and they really aren't that hard, so don't let them hold you back! The remainder of the application is free-form. **Please read our [application guidelines](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2018/Application_Guidelines) and [Google's FAQ](https://developers.google.com/open-source/gsoc/faq) to complete it.** Good luck!

## Ideas



The project aims at making Rust a first class citizen in Gentoo.

- Make easy to generate ebuilds for rust packages
- Integrate rustup-like capabilities as an eselect module.
- Provide a mean to be script-compatible with rustup and cargo while having portage tracking what is installed.



| Contacts | Required Skills | 
|---|---|
|  |  | 

There are numerous MPI (Message Passing Interface) implementations available, but most of them can't be installed together as is. In HPC world it is often mandatory to provide for users different MPI implementations and/or versions of the same implementation, e.g. due to binary package dependencies, codebase or performance issues. The only way to do this now is by using empi from the [science overlay](https://wiki.gentoo.org/wiki/Project:Science/Overlay). Empi has its shortcomings such as lack of multilib support and was not ported to the main tree.

Your goal will be to implement new eclass taking into account empi experience. The key idea is to make compatible MPI implementations selectable from the ebuild similar to they way how multiple python implementations are supported, and allow to build multiple versions of the same package for different MPI implementations.

Different MPI implementations should be selectable by users without any special privileges. A good start will be to use [modules](http://modules.sourceforge.net/) to control environment variables. You will also need to port existing MPI applications to the new framework.

There was [a previous attempt](https://github.com/gilroy/gentoo-mpi) to solve this task. While it was not completed and oversimplified, it may give some hints on what to do.



| Contacts | Required Skills | 
|---|---|
|  |  | 

A lot of scientific software in Gentoo needs more care, e.g. you can:

- package recent versions of [Pythia](http://home.thep.lu.se/~torbjorn/Pythia.html), [HepMC](https://hepmc.web.cern.ch/hepmc/), and other HEP software;
- package machine learning toolkits: [tensorflow](https://www.tensorflow.org/), [keras](https://keras.io/), and [caffe](http://caffe.berkeleyvision.org/) for starting (possibly with an option of picking up [Intel patches](https://github.com/intel/caffe) for high performance);
- package recent versions of Intel tools: [icc](https://software.intel.com/en-us/c-compilers), [ifc](https://software.intel.com/en-us/fortran-compilers), [vtune](https://software.intel.com/en-us/intel-vtune-amplifier-xe), [daal](https://software.intel.com/en-us/intel-daal), [tbb](https://software.intel.com/en-us/intel-tbb), [ipp](https://software.intel.com/en-us/intel-ipp), [mkl](https://software.intel.com/en-us/intel-mkl), [mpi](https://software.intel.com/en-us/intel-mpi-library), etc.



| Contacts | Required Skills | 
|---|---|
|  |  | 

BLAS (Basic Linear Algebra Subroutines) and LAPACK (Linear Algebra Package) are important mathematical libraries widely used in science, engineering, data science and other areas.

Gentoo supports a large number of BLAS and LAPACK implementations, but [switching between them is not implemented properly](https://bugs.gentoo.org/632624). There are [several approaches](https://github.com/gentoo/sci/issues/805) proposed and even [draft implementation for build-time switching](https://github.com/gentoo/sci/pull/837) is available.

Your goal will be to implement blas and lapack eclasses for run-time switching and port at least some of existing ebuilds to the new framework.



| Contacts | Required Skills | 
|---|---|
|  |  | 

Gentoo is a Meta Distribution, and it's binary instantiation usually does emerge on the users machine(s).

So users actually do have their private "Linux Distribution" - either with or without caching (and redistributing) the binary packages.

Of course there is chance that users do have identical profile setups (USE flags, optimization flags, etc.), which is where some build service (OpenBuildService or similar) may be useful.

But rather than sharing binary packages, the idea is to share Gentoo user's profile setups - with the cache for binary packages to be optional (when powered by some build service). Note that some USE flags disallow binary packaging at all.

The idea came up first in [https://archives.gentoo.org/gentoo-dev/message/e7880d4edb2a250e14b7677675f89931](https://archives.gentoo.org/gentoo-dev/message/e7880d4edb2a250e14b7677675f89931), and there is nothing more than that yet.

The profile sharing mechanism may fit the GSoC scope, either with or without sharing user's binary packages.



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

The project aims to improve the CJK tools in Gentoo.

- Update all manpages to last versions.
- Update CJK packages.
- Provide a script to easy the CJK packages configurations.



| Contacts | Required Skills | 
|---|---|
|  |  | 

The Android custom ROM development has been based on cross-compilation and flashing whole partitions. This paradigm has been serving well for the embedded system developments. But as the performance of personal mobile devices boost, it becomes feasible and desirable to introduce package management like personal computers. Package manangement will make software installation and update reliable, reproducible, incremental and convienent.

This project aims to introduce Gentoo's prestiges package manager, portage, to manage Android software stack.  We are going to use the [Gentoo on Android](https://wiki.gentoo.org/wiki/Project:Android) as a starting point.  Starting from the GNU userland provided, we are going to work with the [LineageOS](https://lineageos.org) build system based on the [Android Open Source Project](http://source.android.com), to write ebuilds for the individual components.  Starting from the linux kernel first, the Android will be reproduced from bottom up.



| Contacts | Required Skills | 
|---|---|
|  |  | 

Although being one of the most popular computer languages, Java has not been adopted smoothly into GNU/Linux distributions.  The packaging of Java software are considered difficult by the GNU/Linux community (e.g. [Debian](https://wiki.debian.org/Java/Packaging), [Archlinux](https://wiki.archlinux.org/index.php/Java_package_guidelines), [Fedora](https://fedoraproject.org/wiki/Java)).  At the same time, the Java community has its own set of repositories like [maven](https://maven.apache.org), functionally similar to packages in GNU/Linux distributions.

The [Gentoo Java Project](https://wiki.gentoo.org/wiki/Project:Java) has done a good job laying out the framework of the Java ecosystem in Gentoo. Nevertheless, there are still thousands of useful Java packages to be packaged and maintained.  The project will parse the metadata of maven packages and automatically write ebuilds compatible with the Java build system used in Gentoo.  We are going to set up and maintain an automatically updated maven overlay every Gentoo user can use.  The overlay will at least contain [spark](http://spark.apache.org/) and [hadoop](http://hadoop.apache.org/).  We aim to make Gentoo an attractive choice for Java developers, users and system administrators, as well as data scientists.

A preliminary tool, [java-ebuilder](https://github.com/gentoo/java-ebuilder) is available as [app-portage/java-ebuilder](https://packages.gentoo.org/packages/app-portage/java-ebuilder).  A proof-of-concept [overlay](https://github.com/heroxbd/maven-overlay) is also made.



| Contacts | Required Skills | 
|---|---|
|  |  | 

The [Gentoo Kernel CI](https://wiki.gentoo.org/wiki/GKernelCI) is starting to automatize part of the Gentoo kernel releasing process, this is making more fast Gentoo Kernel release possible. Having a working Gentoo Kernel CI is critical for keeping up with the new upstream Kernel release style, but probably more important would give the possibility of fast stabilizing a kernel, with adequate architecture disposable. The task would be to improve the current Gentoo Kernel CI and designing and creating new feature.


| Contacts | Required Skills | 
|---|---|
|  |  | 

The previous year the Gentoo kernel project worked on elivepatch, a new way of thinking live patch services, now we need help on the project. There are many ideas to work on, some of that ideas are already entered as issue in the GitHub repository.


| Contacts | Required Skills | 
|---|---|
|  |  | 

Currently the OpenPGP bugzilla support is defunct in at least three ways:

1. It encrypts to the first public key it considers viable<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, not respecting [usage flags](http://tools.ietf.org/html/rfc4880#section-5.2.3.21), leading to scenarios where the message is un-decryptable.
2. There is no mechanism for refreshing public keys from known public sources (e.g HKP keyservers) leading to a situation where subkey rotation or changers to primary certificate (e.g due to expiry or revocation) is not picked up automatically and needs to be manually adjusted, failure to do so can lead to encryption to a known non-viable certificate.
3. There is no group definition where multiple public keys can be assigned e.g to an alias account (security@) in bugzilla.

Having support for OpenPGP is necessary to retain confidentiality of restricted bugs in bugzilla, a lack of this results in information leakage. Alternatively, bug emails for group restricted bugs should not include metadata or data that can identify the issue, but merely report e.g "bug XXX has been updated, please log in to see the changes"

More details and proposed approaches are discussed [here](https://bugs.gentoo.org/624262).



| Contacts | Required Skills | 
|---|---|
|  |  | 



##### References

The fact that Google Chromebooks are built by portage is an evidence of flexibility and power of portage. However, portage and other Gentoo tools are invisible to end-users because portage is only used to cross compile and build the filesystem image. The resulting image does not have portage or toolchain, and becomes a feature-limited ChromeOS compared to a standard Gentoo system. ChromiumOS, the community counterpart of ChromeOS, is an open operating system that is straightforward to build and hack. In this project, we will make a full-featured Gentoo-like ChromiumOS combining the innovative Chromebook experience with flexibility of portage and software packages from Gentoo.



| Contacts | Required Skills | 
|---|---|
|  |  | 

**Our best proposals, and a significant proportion of our total acceptances every year, come from student-initiated ideas** rather than those suggested by Gentoo developers. We highly encourage you to suggest your own idea based on what you think would make Gentoo a better distribution. If you do so, we **strongly recommend** you work with a potential mentor to develop your idea **before** proposing it formally. You can find a potential mentor by contacting Denis/Rafael or via discussion on the gentoo-soc mailing list or #gentoo-soc IRC channel.



| Contacts | Required Skills | 
|---|---|
|  |  |
