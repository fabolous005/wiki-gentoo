<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2017/Ideas | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2017/Ideas -->
---
title: Google Summer of Code/2017/Ideas
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2017/Ideas
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2017-03-24"
fingerprint: b6a99a484e77a9f8
license: CC BY-SA 4.0
---

# Google Summer of Code/2017/Ideas

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![GSoC 2017 logo GSoC 2017 logo](https://wiki.gentoo.org/images/c/ce/Gsoc2016-sun-160x160.png)

Want to spend your summer contributing full-time to Gentoo, and get paid for it? Gentoo is in its 10th year in the Google Summer of Code. In the past, **most of our successful students have become Gentoo developers**, so your chances of becoming one are very good if you're accepted into this program.

**Most ideas listed here have a contact person associated with them. Please get in touch with them earlier rather than later to develop your idea into a complete application.** You can find many of them on Freenode's IRC network under the same username. If there is no contact information, please join the [gentoo-soc mailing list](https://www.gentoo.org/get-involved/mailing-lists/) or [#gentoo-soc on the Freenode IRC network](irc://irc.gentoo.org/gentoo-soc), and we will work with you to find a mentor and discuss your idea.

You don't have to apply for one of these ideas! You can come up with your own, and as long as it fits into Gentoo, we'll be happy to work with you to develop it. Remember, your project needs to have deliverables in less than 3 months of work in most cases. Be ambitious but not too ambitious ;)

## Students, please read this first

We have a **custom application template** that we will ask you to fill out. Here it is:

**Congratulations on applying for a project with Gentoo!** To improve your chances of succeeding with this project, we want to make sure you're sufficiently prepared to invest a full summer's worth of time on it. **In addition to the usual application, there are 2 specific actions and 2 pieces of info we would like to see from you:**

- **Use the tools that you will use in your project to make changes to code** (e.g., source code management \[SCM\] software such as CVS, Subversion, or git). Please use the same SCM as you will use for your project to check out one of our [repositories](https://www.gentoo.org/get-involved/get-code/), make a change to it, and post that change as a [patch](http://www.network-theory.co.uk/articles/patchintro.html) on a mailing list or [bug](https://bugs.gentoo.org/). Please fix a real bug reported in [Bugzilla](https://bugs.gentoo.org/) to show that you can use the tools to make a meaningful change. Your contact in Gentoo can help you determine which SCM and repository you should use for this as well as a good bug to fix. If your idea doesn't have a contact, please get in touch with us on the [gentoo-soc mailing list](https://www.gentoo.org/get-involved/mailing-lists/) or in real-time [on IRC](irc://irc.gentoo.org/gentoo-soc). Once you've made your change, link to it from your application.

- **Participate in our development community.** Please make a post to one of our [mailing lists](https://www.gentoo.org/get-involved/mailing-lists/) and link to it from your application ([archives.gentoo.org](https://archives.gentoo.org) holds past postings). The gentoo-soc list would be a good starting point, if you aren't subscribed to any others already. The best posts would be an introduction of the project you're applying for and a little background about you, to introduce yourself to the community and get some broader input about your project.

- **Give us your contact info and working hours.** Please provide your email address, home mailing address, and phone number. This is a requirement and provides for accountability on both your side and ours. Also, please tell us what hours you will be working and responsive to contact via email and IRC; these should sum to at least 35 hours a week.

These actions are things you will do extremely commonly as an open-source developer, and they really aren't that hard, so don't let them hold you back! The remainder of the application is free-form. **Please read our [application guidelines](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2017/Application_Guidelines) and [Google's FAQ](https://developers.google.com/open-source/gsoc/faq) to complete it.** Good luck!

## Ideas



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

Currently the OpenPGP bugzilla support is defunct in at least three ways:

1. It encrypts to the first public key it considers viable<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, not respecting usage flags<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>, leading to scenarios where the message is un-decryptable
2. There is no mechanism for refreshing public keys from known public sources (e.g HKP keyservers) leading to a situation where subkey rotation or changers to primary certificate (e.g due to expiry or revocation) is not picked up automatically and needs to be manually adjusted, failure to do so can lead to encryption to a known non-viable certificate
3. There is no group definition where multiple public keys can be assigned e.g to an alias account (security@) in bugzilla.

Having support for OpenPGP is necessary to retain confidentiality of restricted bugs in bugzilla, a lack of this results in information leakage. Alternatively, bug emails for group restricted bugs should not include metadata or data that can identify the issue, but merely report e.g "bug XXX has been updated, please log in to see the changes"


| Contacts | Required Skills | 
|---|---|
|  |  | 

##### References

g-octave is a generator of ebuilds for octave-forge package, that does everything on its own. We have the g-sorcery framework now, that tries to encapsulate everything needed to create such ebuild generators easily. This project aims to port g-octave to g-sorcery framework, including implement the features that lacks on the framework but are used by g-octave.


| Contacts | Required Skills | 
|---|---|
|  |  | 

Write a tool to manage global and local USE flags as well as expand variables, review them, audit them, filter them based on various criteria, etc... Get inspiration from:

Integrate it into emaint. The code will have to be written in Python and use the Portage API.

As it is unlikely this is enough for 3 months of full-time work, you'll have to be creative enough to expand the features sets of USEful and/or emaint in a meaningful way.



| Contacts | Required Skills | 
|---|---|
|  |  | 

There are numerous MPI (Message Passing Interface) implementations available, but most of them can't be installed together as is. In HPC world it is often mandatory to provide for users different MPI implementations and/or versions of the same implementation, e.g. due to binary package dependencies, codebase or performance issues. The only way to do this now is by using empi from the [science overlay](https://wiki.gentoo.org/wiki/Project:Science/Overlay). Empi has its shortcomings such as lack of multilib support and was not ported to the main tree.

Your goal will be to implement new eclass taking into account empi experience. The key idea is to make compatible MPI implementations selectable from the ebuild similar to they way how multiple python implementations are supported, and allow to build multiple versions of the same package for different MPI implementations.

Different MPI implementations should be selectable by users without any special privileges. A good start will be to use [modules](http://modules.sourceforge.net/) to control environment variables. You will also need to port existing MPI applications to the new framework.



| Contacts | Required Skills | 
|---|---|
|  |  | 

cloud-init currently uses a deprecated network back end for Gentoo. This proposal seeks to convert cloud-init's Gentoo back end to the new back end. A possible further goal would be to extend systemd support for cloud-init on Gentoo, mainly on the network side.



| Contacts | Required Skills | 
|---|---|
|  |  | 

The idea is to build a stateless operating system based on Gentoo. It would consist of a very minimal Gentoo image with automatic atomic updates. The work could consist of putting together a build service, and/or focusing on the update system, or anything around it.

Some inspirations:

- Stateless [CoreOS Container Linux](https://coreos.com/os/docs/latest/sdk-modifying-coreos.html) build scripts
- [ChromeOS](https://www.chromium.org/chromium-os/chromiumos-design-docs/filesystem-autoupdate) file system updates
- [OSTree](https://ostree.readthedocs.io/) to boot and update filesystem trees
- The minimal [CERN](https://github.com/cernvm/cernvm-micro) image and its HTTP [read-only POSIX file system](https://cernvm.cern.ch/portal/filesystem).



| Contacts | Required Skills | 
|---|---|
|  |  | 

Gentoo could be an ideal framework for the Big Data ecosystem. A project could consist of bringing popular Big Data tools such as Spark, Hadoop, Flink, noSQL databases, etc... to Gentoo users in a very smooth and integrated way, while still satisfying best Gentoo practices.



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

binhost is a provider of binpkg. Currently binhost is unable to accept any request(e.g. Build binpkg, Upgrade old package, Specify useflag or CC or CXX or arch) from clients of binpkg.

Proposal:

binhost provide API of request to create and upgrade binpkg.

And request support for specify useflag or CC or CXX or arch.

Provide official binhost server of building and caching.




| Contacts | Required Skills | 
|---|---|
|  |  | 

[ROOT](https://root.cern.ch/) is a modular scientific software framework, a standard de-facto analysis tool in HEP, but used in other scientific and industrial areas too. We support ROOT in Gentoo and have a long list of improvements awaiting implementation:

- move ROOT from configure to cmake;
- add new USE flags and dependencies;
- move documentation generation from THtml to doxygen;
- improve PyROOT:
  - dependency cleanup (split ROOT.py and put Pythonize.cxx in its own .so);
  - make the core of PyROOT standalone, so that it as "PyCling" when it's packaged with Cling and libCore/libCling can be used standalone;
  - get clingcwrapper.cxx from PyPy into ROOT as a standalone .so, so that PyROOT can run on PyPy w/o further ado;
- make Cling installable as a separate package.



| Contacts | Required Skills | 
|---|---|
|  |  | 

It often takes a while for Gentoo to move to the latest versions of Python, Ruby, etc. This is often due to the fact, that our eclasses are build in a way, that they require PYTHON\_COMPAT or RUBY\_TARGETS to be set to the latest version. The goal of this idea is to build an interface for developers, that provides them information (probably a graph) on which packages require the language target to be enabled first.

The basic idea does probably not have enough content for 3 months full-time work, it needs to be extended, e.g. by automating the test process, building a frontend, thinking about solutions for cyclic graphs (test dependencies depend on the to be tested package themselves) or verifying with upstream information (e.g. if this python version supported is mentioned in packages' setup.py).



| Contacts | Required Skills | 
|---|---|
|  |  | 

Porthole is a Gentoo tree browser with basic emerge capabilities. Its primary focus is for package browsing and viewing various information about the packages.

It is is need of updates to bring it back in line with changes to the Gentoo ebuild eco-system and the package managers code used to gather the data it displays.

Main areas needing work are:

- python 3 porting
- portage backend updates for api changes
- pkgcore backend updates for api changes
- gtk3 porting
- changelog view needs a git module, rsync changelogs is now a sub-repo, so need a way to sync those.

Other code needing completion or just new:

- finish modifying the primary view windows to allow plug-ins to make use of it's windows to display data
- Update exisiting plug-in modules
- create a new gentoolkit plugin for many of the common queries.
- create a layman plugin
- other new ideas



| Contacts | Required Skills | 
|---|---|
|  |  | 

The project aims at making Rust a fist class citizen in Gentoo.

- Make easy to generate ebuilds for rust packages
- Integrate rustup-like capabilities as an eselect module.
- Provide a mean to be script-compatible with rustup while having portage tracking what is installed.



| Contacts | Required Skills | 
|---|---|
|  |  | 

**Our best proposals, and a significant proportion of our total acceptances every year, come from student-initiated ideas** rather than those suggested by Gentoo developers. We highly encourage you to suggest your own idea based on what you think would make Gentoo a better distribution. If you do so, we **strongly recommend** you work with a potential mentor to develop your idea **before** proposing it formally. You can find a potential mentor by contacting Denis/Rafael or via discussion on the gentoo-soc mailing list or #gentoo-soc IRC channel.


| Contacts | Required Skills | 
|---|---|
|  |  |
