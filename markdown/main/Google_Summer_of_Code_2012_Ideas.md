<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2012/Ideas -->
---
title: Google Summer of Code/2012/Ideas
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-09-06"
fingerprint: e729ba4f0a21a3b8
license: CC BY-SA 4.0
---

# Google Summer of Code/2012/Ideas

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![GSoC 2012 logo GSoC 2012 logo](https://wiki.gentoo.org/images/a/a7/Gsoc-2012-logo-color.png)

Want to spend your summer contributing full-time to Gentoo, and get paid for it? Gentoo is in its 7th year in the Google Summer of Code. In the past, most of our successful students have become Gentoo developers, so your chances of becoming one are very good if you're accepted into this program.

**Most ideas listed here have a contact person associated with them. Please get in touch with them earlier rather than later to develop your idea into a complete application.** You can find many of them on Freenode's IRC network under the same username. If there is no contact information, please join the [gentoo-soc mailing list](https://www.gentoo.org/main/en/lists.xml) or [#gentoo-soc on the Freenode IRC network](irc://irc.gentoo.org/gentoo-soc), and we will work with you to find a mentor and discuss your idea.

You don't have to apply for one of these ideas! You can come up with your own, and as long as it fits into Gentoo, we'll be happy to work with you to develop it. Remember, your project needs to have deliverables in less than 3 months of work in most cases. Be ambitious but not too ambitious ;)

# Read this first

We have a **custom application template** that we will ask you to fill out. Here it is:

```
'''Congratulations on applying for a project with Gentoo!''' To improve your chances of succeeding with this project, we want to make sure you're sufficiently prepared to invest a full summer's worth of time on it. '''In addition to the usual application, there are 2 specific actions and 2 pieces of info we would like to see from you:'''
* '''Use the tools that you will use in your project to make changes to code''' (e.g., source code management [SCM] software such as CVS, Subversion, or git). Please use the same SCM as you will use for your project to check out one of our [https://anoncvs.gentoo.org/ repositories] ([http://git.overlays.gentoo.org/gitweb/ more repositories]), make a change to it, and post that change as a [http://www.network-theory.co.uk/articles/patchintro.html patch] on a mailing list or [https://bugs.gentoo.org/ bug]. Please fix a real bug reported in [https://bugs.gentoo.org/ Bugzilla] to show that you can use the tools to make a meaningful change. Your contact in Gentoo can help you determine which SCM and repository you should use for this as well as a good bug to fix. If your idea doesn't have a contact, please get in touch with us on the [https://www.gentoo.org/main/en/lists.xml gentoo-soc mailing list] or in real-time [irc://irc.gentoo.org/gentoo-soc on IRC]. Once you've made your change, link to it from your application.
	
* '''Participate in our development community.''' Please make a post to one of our [https://www.gentoo.org/main/en/lists.xml mailing lists] and link to it from your application ([https://archives.gentoo.org archives.gentoo.org] holds past postings). The gentoo-soc list would be a good starting point, if you aren't subscribed to any others already. The best posts would be an introduction of the project you're applying for and a little background about you, to introduce yourself to the community and get some broader input about your project.
	
* '''Give us your contact info and working hours.''' Please provide your email address, home mailing address, and phone number. This is a requirement and provides for accountability on both your side and ours. Also, please tell us what hours you will be working and responsive to contact via email and IRC; these should sum to at least 35 hours a week.
These actions are things you will do extremely commonly as an open-source developer, and they really aren't that hard, so don't let them hold you back! The remainder of the application is free-form. '''Please read our [https://www.gentoo.org/proj/en/userrel/soc/applying.xml application guidelines] and [http://socghop.appspot.com/document/show/gsoc_program/google/gsoc2011/faqs Google's FAQ] to complete it.''' Good luck!
```
# Ideas



## [Cross Container Support](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Cross_Container_Support)

It is already possible to create fully working chroot using [qemu-user](http://www.gentoo.org/proj/en/base/embedded/handbook/?part=1&chap=5) and build quickly packages through it, the natural step further is to make it work as a normal container, providing a similar interface to manage it. This way build for arm targets can be done on faster systems and sidetracks also issues about python and perl not supporting proper cross compilation or widespread build systems such waf and cmake failing completely at the task.

The project aims in providing initscripts, management tools and canned recipes to generate container-like chroot, let developers easily import and export system images and further integrate it with crossdev.

An additional task is to support layered systems so native userspace can be used to further speed up the process (hybrid chroot).




| Contacts | Required Skills | 
|---|---|
| [Luca Barbato](mailto:lu_zero@gentoo.org) jing.huang.pku@×××××.com Jing Huang |  | 

#### Mailing List Archives

 [Cross Container Support - Mailing List Archives](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Cross_Container_Support/MailingListArchives)

## [Dynamic documentation type generation](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Dynamic_documentation_type_generation)

The official gentoo documentation is currently offered through a self-maintained XML format, called GuideXML. Although XML is considered a powerful language for generating other types, we are currently limiting ourselves to HTML output only. We have frequent requests for PDF and, more recently, ePub.

The purpose would be to update our infrastructure to generate multiple output formats, including ePub, for our users.



| Contacts | Required Skills | 
|---|---|
|  |  | 

## [Ebuild Upstream Scanner](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Ebuild_Upstream_Scanner)

euscan (Ebuild Upstream Scanner) is an utility to check if an ebuild
have new upstream version. It was designed to provide the same
features as debian's uscan and DEHS ([http://dehs.alioth.debian.org/](http://dehs.alioth.debian.org/)).
euscan is also a web interface that aggregates the result of euscan
ran on all gentoo ebuilds (currently hosted at
[http://euscan.iksaif.net](http://euscan.iksaif.net) but will move to gentoo.org someday). There
are a lot of things to do on euscan, here are some examples:

#### Celery port

Currently euscan use shell scripts and GNU parallel to scan the portage tree. Using celery (a python task queue that integrates well with django) would allow a more flexible scan process. Scanning the tree or a package could be done with a simple django command starting a celery task, the web interface could rescan old packages on-demand too. While adding celery commands, it would be great to add simple commands to: create the local filesytem needed by euscan, add/sync/remove an overlay, etc... Celery is not really packaged for gentoo, but I got some ebuilds in my overlay, so another side of the project could be to push them to the portage tree.

#### Version Detection Enhancements

euscan currently use multiple heuristics to determine upstream version, but there is a lot to do to enhance that to remove false positive and be able to scan more package:

- create a script to gather statistics on euscan success and failure and use methods.
- use metadata.xml's \<upstream>\<remote-id> tag with the appropriate euscan site-handler (pypi, pearl, cpan, etc...) and make sure that all packages have this tag correctly set in the portage tree
- create a new metadata tag \<upstream>\<watch> that works like debian/watch and make euscan use it. Create a script to import informations from the associated debian/watch file.
- steal ideas from other tools (uscan, portscout) and use other source of data (youri, distrowatch, distromatch, whoas; Equivalent-Packages)

#### Euscan Web enhancements

Add an account system, and allow maintainer to register to subscribe
to automatic weekly notifications. Integrate euscan with other webapps
like [http://portage.gentoo.org](http://portage.gentoo.org), [http://znurt.org/](http://znurt.org/),
[http://gpo.zugaina.org/](http://gpo.zugaina.org/), [Gentoostats](https://wiki.gentoo.org/wiki/Gentoostats) and try
to create a single killer webapp . Add better overlay support and
tweak django administration.



| Contacts | Required Skills | 
|---|---|
|  |  | 

#### Mailing List Archives

 [Ebuild Upstream Scanner - Mailing List Archives](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Ebuild_Upstream_Scanner/MailingListArchives)

## [gentoo-x86 QA website](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/gentoo-x86_QA_website)

The idea is simple enough, take the QA results from various tools and present it via a searchable website. Think [packages.gentoo.org](http://packages.gentoo.org), just for QA results. The implementation work required would primarily be building the website itself- the user could rely upon pkgcore-checks for the initial data stream (it can output it's results as a pickle stream) leaving the candidate to focus on generating a site providing insight into the status of current architectures, current stabling, etc.

One additional constraint would be that the underlying DB schema should be written in a fashion that allows multiple data imports to be used- while pkgcore-checks right now can provide data for a candidate to work with, the candidate should be designing a system also able to pull in other data sources (at some point repoman for example).

Finally, an additional feature could be designing the underlying schema and website to allow for the possibility of being able to handle multiple repositories- think about if the GNOME herd wanted their overlay to be scanned/accessible. This complicates the design a bit (specifically keeping it fast), but is likely to be desired functionality down the line.

The relevant gentoo-soc discussion (with a bit more details) is accessible on [in the gentoo-soc archives](http://archives.gentoo.org/gentoo-soc/msg_474d9ffb2fde05a9fb8ff032cac58622.xml).



| Contacts | Required Skills | 
|---|---|
|  |  | 

## [Gentoo@home](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Gentoo@home)

Gentoo packages are stabilized and architecture tested (keyworded) following a request coming from a package maintainer or a Gentoo developer. The Gentoo architecture teams then test by hand every requested package and its dependencies. For a very large fraction of Gentoo packages, this tedious manual process could be batch automated given some specifications. The idea is to build a framework for such a project, that would help both users by getting much better stabilization process and maintainers by lightening the workflow and find critical bugs. One idea would be to use an automated built of a tinderbox, and distribute the tinderbox with stabilization scripts to the Gentoo user community to install and test packages automatically. It could be done via volunteer computing from the Gentoo user community. Since it could turn into an involved project, it could be split into three projects.



| Contacts | Required Skills | 
|---|---|
|  |  | 

## [Improved binary package support](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Improved_binary_package_support)

Gentoo better for derived binary distros. One of them is more intelligent handling of library versions with binpkgs (and installed packages, which are a form of binpkg). For example, it's possible to build a binpkg against an old version of a library, then install it against a new version and have it be broken by default because of a shared-library version bump. It's also possible to break reverse ABI dependencies when upgrading a package, and there is currently no convenient way for package managers to detect such breakage in advance. Ideally, a package would have a way to specify its ABI dependencies in the built state instead of just which versions it can build against from source. It is possible to create an ABI dependency abstraction that is flexible enough to cover all possible kinds of ABI dependencies. Using an ABI abstraction, it will not matter whether or not there exists a specific soname to be referenced by dependencies. See [bug #192319](https://bugs.gentoo.org/show_bug.cgi?id=192319).

Another problem is saving binpkgs with different USE flag and other build settings on the same host. See [bug #150031](https://bugs.gentoo.org/show_bug.cgi?id=150031). The way forward is one or more hashes of the metadata. A third problem is the lack of binpkg support for the kernel. This could be changed through modifying the kernel eclass to support a binary USE flag that also did configuration & build, or perhaps some kind of genkernel modification, or both. See [bug #154495](https://bugs.gentoo.org/show_bug.cgi?id=154495).

Two other minor problems:

- Compilation related messages are thrown to user when installing a binary package. This should be avoided somehow. Bad developers habit.
- It would be nice to have elog output stored in /var/db for later consumption and perhaps, have it embedded in xpak metadata. This would improve PackageKit support, which doesn't allow any output from package phases during install.



| Contacts | Required Skills | 
|---|---|
|  |  | 

## [libbash](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/libbash)

For the last two summers we have been developing a shared library for bash. We got pretty close last summer by parsing many ebuilds but not 100% due to unexpected problems with the grammar. Hopefully this will be the summer we finally nail the goal.



| Contacts | Required Skills | 
|---|---|
|  |  | 

#### Mailing List Archives

 [libbash - Mailing List Archives](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/libbash/MailingListArchives)

## [lmonade relocation and binary packages](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/lmonade_relocation_and_binary_packages)

The [lmonade](http://www.lmona.de) project is a distribution of mathematical software based on [Gentoo prefix](http://www.gentoo.org/proj/en/gentoo-alt/prefix/). It can be installed without admistrative rights on different linux distributions or OSX. The main aim is to make scientific software available across platforms.

For a user not familiar with Unix and the shell, the quickest solution to get a working copy of some software is to download a binary. However, binary distribution for the all different flavors of GNU/Linux and OSX is too much of a burden for most software developers.  A solution is provided by Gentoo Prefix and Portage's binary package feature.  Gentoo Prefix can handle basic rewriting of absolute paths encoded in binaries present in these packages. Though there are still [quite a few rough edges](https://wiki.gentoo.org#Improved_binary_package_support) that require attention before this can be used in production.

The goal of this project is to bring all these pieces together to form an easy to use solution that will be useful to many software projects. The focus is on making things "just work" for both users and software projects. This task would involve

- modifying eclasses to generate relocatable files,
- writing scripts to fix absolute paths in binaries (e.g., [1](http://hg.sagemath.org/scripts-main/file/78a0b42fab1f/sage-make_relative) [2](http://hg.sagemath.org/scripts-main/file/78a0b42fab1f/sage-location)),
- packaging in a user friendly format and
- finding creative solutions to platform dependent quirks.

Please e-mail us for more information.



| Contacts | Required Skills | 
|---|---|
|  |  | 

## [OpenRC Extensions](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/OpenRC_Extensions)

OpenRC is the default init system in Gentoo, it provides a large deal of features while staying mostly agnostic to the underlying implementation on /sbin/init.

The project aims to be a constructive criticism to the systemd approach by providing the few interesting features not already implemented by OpenRC as stand alone modules allowing integrator not to need to bend their system layouts to accomodate the init system.

Desired extensions include:

- A mechanism by which init scripts can configure OpenRC to detect runtime failures, log them and respond to them. The key response we want to enable is to give regular init scripts respawn functionality like we have in /etc/inittab
- Oom-killer protection via /proc/\*/oom\_adj
- The ability to perform some sort of maintenance action on a timer (e.g. restart)



| Contacts | Required Skills | 
|---|---|
|  |  | 

## Daemons in Gentoo Prefix with OpenRC

Objective:

- Port OpenRC to Gentoo Prefix to organize daemons.


Abstract:

- I am going to take the development of prefix support in OpenRC
- Deploy OpenRC to work with baselayout in Prefix and
- Extend Prefix with the long-waited feature of services daemons.

#### Mailing List Archives

[gentoo-soc - report 7.16-7.23: improving OpenRC heroxbd@×××××.com Tue, 24 Jul 2012 09:06:29](https://archives.gentoo.org/gentoo-soc/message/f768d78d3bf0d1e1069af2a8c9b82d50) 

**Contacts:**

## [Port Fedora UEFI Support](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Port_Fedora_UEFI_Support)

Computer manufacturers are adopting UEFI as a BIOS replacement on amd64 systems, but Gentoo is currently unable to boot on such systems using GRUB 0.97. Intel wrote patches for UEFI support that were adopted by Fedora's GRUB fork. Porting those patches from [Fedora's GRUB fork](https://pkgs.fedoraproject.org/gitweb/?p=grub.git;a=summary) to sys-boot/grub is necessary if sys-boot/grub is to remain a viable bootloader in Gentoo.

There are two existing issues in sys-boot/grub that must be addressed in conjunction with this port. The first is that sys-boot/grub does not compile correctly with GCC 4.6, which is [bug #360513](https://bugs.gentoo.org/show_bug.cgi?id=360513). The second is that sys-boot/grub's grub-probe utility relies on /dev/root, which is newer versions of udev remove. A proper port must compile properly with GCC 4.6 without any dependence on /dev/root.

In addition, sys-boot/grub is GPLv2 licensed, so these improvements may not involve the use of any GPLv3 code.



| Contacts | Required Skills | 
|---|---|
|  |  | 

## [Porthole plug-ins and extensions](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Porthole_plug-ins_and_extensions)

Porthole is a GTK+-based frontend to Portage. This project would enable Porthole to improve its ability to manage remote computers, gather and report package statistics, and more. The work would encompass creating:

- A basic python based control interface API for linking to remote computers and groups of computers to gather information about installed packages, and install/update those using portage and/or pkgcore as remote backends.  One possible means of making those connections is by using [dev-python/pyro](https://packages.gentoo.org/packages/dev-python/pyro).
- Extending the work started in the public\_api branch of portage to include running emerge via the public api.
- Create a simple cli for gathering/reporting update info for #1
- Create a porthole plug-in for connecting to the control interface, displaying the info in portholes views and and dispatching desired actions to the remotes via the control interface.



| Contacts | Required Skills | 
|---|---|
|  |  | 

## [Recruiting Webapp Usability](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Recruiting_Webapp_Usability)

The webapp for Gentoo recruitment at [https://recruiting.gentoo.org](https://recruiting.gentoo.org) was written two summers ago. It has developed a backlog for improved usability and it also needs updating for Rails 3.0.



| Contacts | Required Skills | 
|---|---|
|  |  | 

## [Repoman cleanup](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Repoman_cleanup)

Repoman (the Portage QA tool) could really use some attention. Before elaborating more on how to best solve this I need to check with zmedico.



| Contacts | Required Skills | 
|---|---|
|  |  | 

## [Repository of self-contained ebuild source packages](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Repository_of_self-contained_ebuild_source_packages)

This proposal is similar in scope to the Cache sync proposal, except that it will focus on implementing support for repositories that host self-contained ebuild source packages that are analogous to source RPMs (SRPMs). The repository layout will be similar to existing PORTAGE\_BINHOST repositories (like those hosted at [tinderbox.dev.gentoo.org](http://tinderbox.dev.gentoo.org/default-linux/x86/)), and will include a metadata index file which is similar to [$PKGDIR/Packages](http://tinderbox.dev.gentoo.org/default-linux/x86/Packages). Each source package hosted in the repository will contain a single ebuild, its metadata, and all files it requires from the portage tree (including all inherited eclasses and any additional files such as patches from the files directory). A zip file will be a suitable container for one of these source packages.

A synchronization script will generate source packages from a source portage tree (or overlay), and it will avoid unnecessary regeneration of a given source package in cases when no relevant files have changed timestamps or been added/removed. This script will be run as often as the repository maintainer wants to synchronize with the source tree.

The syncronization script will have an option to preserve source packages that no longer exist in the source tree, perhaps updating such packages if they happen to contain outdated versions of eclasses. The ability to retain packages that no longer exist in the source tree may be useful for some cases of the stable tree idea which was proposed in [GLEP 19](http://www.gentoo.org/proj/en/glep/glep-0019.html).

In order to ensure that repository updates do not interfere with clients, it may be desirable to publish the repository as a series of snapshots that are contained in directories which are named corresponding to snapshot date/time. This will ensure that a previous snapshot is still accessible when a newer snapshot becomes available. So, clients will never have any trouble downloading a specific source package that corresponds to a metadata index which was downloaded earlier and used for a dependency calculation. Without some kind of snapshot mechanism like this (similar to [RCU](https://en.wikipedia.org/wiki/Read-copy-update)), a race condition would exist such that dependency calculations would be somewhat unreliable, and downloaded source packages might have different checksums and dependencies from those listed in a metadata index that was downloaded only a minute earlier.



| Contacts | Required Skills | 
|---|---|
|  |  | 

## [SELinux policy originator](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/SELinux_policy_originator)

Gentoo Hardened is maturing its SELinux support rapidly. In SELinux, policies are written in a higher abstract format (dictated by the reference policy) and converted to the SELinux-specific rules (like allow, dontaudit, type transitions, etc.). For troubleshooting rights however, it is a very daunting task to find out why a particular rule is set (in other words, to find which higher level rule is causing the SELinux rule to exist).

The rules are converted in M4 language from constructs like "corenet\_tcp\_bind\_http\_port" to rules like:

- allow $1 http\_port\_t:tcp\_socket name\_bind
- allow $1 self:capability net\_bind\_service

It is the latter that end users can easily see (with tools such as *sesearch*) but it is not easy to find that "allow openvpn\_t http\_port\_t:tcp\_socket name\_bind" comes from "corenet\_tcp\_bind\_http\_port(openvpn\_t)" defined in the openvpn.te definition.

In this idea, we would like to find a way to register where these lines come from to improve debugging and troubleshooting



| Contacts | Required Skills | 
|---|---|
|  |  | 

#### Mailing List Archives

 [SELinux policy originator - Mailing List Archives](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/SELinux_policy_originator/MailingListArchives)

## [Support for Fortran modules and libraries with multiple compilers](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Support_for_Fortran_modules_and_libraries_with_multiple_compilers)

Fortran modules and libraries are highly compiler dependent. Even minor version change in gfortran renders them incompatible. However, we are often forced to work with multiple compilers at the same time (e.g. scientific software development). The project aim is to create a Fortran framework that would allow to install concurrent versions of Fortran binaries in a PMS (Package Manager Specification)-aware fashion, along with a configuration module for eselect to rule them all.

See also: [Linux problems you never considered handling - Fortran90 modules for multiple compilers](http://dberkholz.com/2010/12/02/linux-problems-you-never-considered-handling-fortran90-modules-for-multiple-compilers/) (Donnie's Berkholz blog post)



| Contacts | Required Skills | 
|---|---|
|  |  | 

## [Cache sync](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Cache_sync)

The portage tree and all its overlays keep growing. Right now only the official portage tree occupies more than 600Mb on a regular filesystem. However the package manager does not need the whole tree of full ebuilds, patches and manifests to perform most of its work. The idea would be to sync a smaller database or a cache of only needed information for global package manager operations, then fetch the required package only when needed. It would speed considerably tree synchronization and reduce the space occupied by portage tree (see the "Repository of Self-Contained Ebuild Source Packages" idea for an alternative approach). Currently the cache system in portage is also really slow and so is the search feature. The project could be inspired by the Debian or RPM system but with the usability and choices offered by Gentoo, and would probably include:

- design and implement automatic cache builder to be produced by a given repository/overlay
- make portage/paludis/pkgcore to do delta-sync with a local cache and fetch only the required files to be installed when requested



| Contacts | Required Skills | 
|---|---|
|  |  | 

## [Package statistics reporting tool](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Package_statistics_reporting_tool)

A user end program to upload anonymous information about installed packages on a users machine to a database that package maintainers and developers have access to. Last year's effort is called [Gentoostats](https://wiki.gentoo.org/wiki/Gentoostats).

This post from planet Gentoo titled, 'Gentoo: A critical look at the QA process' suggests that it is difficult for maintainers to decide whether or not to mark a package as stable. The main issue being: if it's not marked as stable then users wont use it, and it's very difficult for maintainers to test the package properly on their own. This creates a blurring between \~arch and arch. If stable packages are left in \~arch for long periods of time, users will begin to use \~arch as if it were stable more often, which defeats the purpose of \~arch in the first place. If maintainers push packages into arch because it works on their machines but breaks on users due to the high number of different system setups this makes gentoo look unreliable, further blurring \~arch and arch.

Here are some reasons why this project would help Gentoo:

- Now developers can't see when users are happy with a package, only when they are not.
- Finding incompatibilities between specific package versions
- General user interest in specific packages to help trim down unused packages from portage.



| Contacts | Required Skills | 
|---|---|
|  |  | 

#### Mailing List Archives

 [Package statistics reporting tool - Mailing List Archives](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Package_statistics_reporting_tool/MailingListArchives)

## [Tags support for Portage](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Tags_support_for_Portage)

Gentoo uses categories now. A package can only be in a single category, which is very limiting because generally things don't fit perfectly into one place without other possibilities. Tags could make it a lot easier to find packages they're looking for by doing Boolean searches like: kde AND mail. This project would add support for tags to Portage and would allow for backwards compatibility of categories as tags.



| Contacts | Required Skills | 
|---|---|
|  |  | 

## [Automatically generated overlay of R packages](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Automatically_generated_overlay_of_R_packages)

The R scientific language has a vast ecosystem of about 5000 packages and 40000 individual versions. It was attempted in the past to make Portage manage these packages directly. The last project was somewhat successful but suffered from some drawbacks. Some of them, like the lack of manifests, are a result of the particular kind of implementation which was chosen.

This project is a completely different implementation which consists in a set of scripts which pull all the necessary information from CRAN, BIOC, etc... mirrors and create a central overlay. This overlay will be hosted on Gentoo infrastructure and can be added using Layman by Gentoo users. As far as the regular user is concerned this overlay is no different from other overlays where ebuilds are written manually. The generated packages and eclasses must be compatible with all current package managers used in Gentoo.

In order for this project to be sucessful it will need to not only provide the system which generates the overlay, but also solve issues like fixing errors in R package metadata (quite common), R and system dependency verification, distfiles mirroring, manifest generation, automatic incremental overlay updates, proper logging, and a few others. The end result of this project must be something that the Gentoo infrastructure team will agree to install on Gentoo infrastructure. Discussion with them will be necessary way before the completion of the project to confirm it is going into the right direction. The demonstration of the system actually working will need to be done on GSoC dedicated hardware. Proper documentation of what the system does and how will be considered crucial.

It is not necessary to have strong R skills for this project. The little R knowledge which is actually needed can be picked up in a few hours of reading.

It is recommended you talk to one of the contacts below before you make an official application.



| Contacts | Required Skills | 
|---|---|
|  |  | 

#### Mailing List Archives

 [Automatically generated overlay of R packages - Mailing List Archives](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Automatically_generated_overlay_of_R_packages/MailingListArchives)

## [Extend packages.gentoo.org New gentoo packages](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Extend_packages.gentoo.org_New_gentoo_packages)

Creating new packages.gentoo.org with rich web interface and advanced features. Port it to django.

gentoo-packages is new version of packages.gentoo.org site written on Python using Django framework.

You could find sources on GitHub or here.  [https://github.com/bacher09/gentoo-packages](https://github.com/bacher09/gentoo-packages)

Summary:

- implemented package scanning utility, that collect info about herds, maintainers, packages, use flags, licenses. It could collect info about overlays. Scanning tool support collection packages data through portage or pkgcore. For that was developed backends mechanism. It also could validate some data on database.
- implemented web interface with many server pages: Ebuilds view for fresh ebuilds, packages views for searching packages, repositories view for show all available repositories, repository view for displaying repository info and stats, licenses and license view for showing info about license, maintainers view for displaying info about maintainers, categories view for showing categories list, package view for displaying info about package (changelog, use flags, keywords, depends, herds, maintainers, metadata, etc), ebuild view for showing info just about that ebuild. In packages view they could be sorted by update or created time, that allows show newest packages. User could choice info about what arches would be showed to him. Also user could search packages by many params like herd, use flag, license, category, overlay. For ebuild also available rss and atom feeds. For performance boost some data are cached.

Plans for the future:

- Extending package search, allow search by arch.
- Use solr for searching.
- Use celery for scanning tasks.
- Add the ability to show packages screenshots (some thing similar to screenshots.debian.net).
- Extend rss and atom feeds.
- Deploy server to packages.gentoo.org site.
- Some other minor plans, I will add TODO list on next week.

I want thank my mentor Matthew Summers, Brian Dolbec and all other who helped me working on my project.




| Contacts | Required Skills | 
|---|---|
|  |  | 



#### Mailing List Archives

 [Extend packages.gentoo.org New gentoo packages - Mailing List Archives](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Extend_packages.gentoo.org_New_gentoo_packages/MailingListArchives)
