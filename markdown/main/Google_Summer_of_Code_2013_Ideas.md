<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2013/Ideas -->
---
title: Google Summer of Code/2013/Ideas
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2015-02-15"
fingerprint: b72db04e483183fc
license: CC BY-SA 4.0
---

# Google Summer of Code/2013/Ideas

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![GSoC 2013 logo GSoC 2013 logo](https://wiki.gentoo.org/images/7/73/Gsoc-2013-logo-color.png)

Want to spend your summer contributing full-time to Gentoo, and get paid for it? Gentoo is in its 8th year in the Google Summer of Code. In the past, **most of our successful students have become Gentoo developers**, so your chances of becoming one are very good if you're accepted into this program.

**Most ideas listed here have a contact person associated with them. Please get in touch with them earlier rather than later to develop your idea into a complete application.** You can find many of them on Freenode's IRC network under the same username. If there is no contact information, please join the [gentoo-soc mailing list](http://www.gentoo.org/main/en/lists.xml) or [#gentoo-soc on the Freenode IRC network](irc://irc.gentoo.org/gentoo-soc), and we will work with you to find a mentor and discuss your idea.

You don't have to apply for one of these ideas! You can come up with your own, and as long as it fits into Gentoo, we'll be happy to work with you to develop it. Remember, your project needs to have deliverables in less than 3 months of work in most cases. Be ambitious but not too ambitious ;)

# Read this first

We have a **custom application template** that we will ask you to fill out. Here it is:

**Congratulations on applying for a project with Gentoo!** To improve your chances of succeeding with this project, we want to make sure you're sufficiently prepared to invest a full summer's worth of time on it. **In addition to the usual application, there are 2 specific actions and 2 pieces of info we would like to see from you:**

- **Use the tools that you will use in your project to make changes to code** (e.g., source code management \[SCM\] software such as CVS, Subversion, or git). Please use the same SCM as you will use for your project to check out one of our [repositories](http://anoncvs.gentoo.org/) ([more repositories](http://git.overlays.gentoo.org/gitweb/)), make a change to it, and post that change as a [patch](http://www.network-theory.co.uk/articles/patchintro.html) on a mailing list or [bug](http://bugs.gentoo.org/). Please fix a real bug reported in [Bugzilla](http://bugs.gentoo.org/) to show that you can use the tools to make a meaningful change. Your contact in Gentoo can help you determine which SCM and repository you should use for this as well as a good bug to fix. If your idea doesn't have a contact, please get in touch with us on the [gentoo-soc mailing list](http://www.gentoo.org/main/en/lists.xml) or in real-time [on IRC](irc://irc.gentoo.org/gentoo-soc). Once you've made your change, link to it from your application.

- **Participate in our development community.** Please make a post to one of our [mailing lists](http://www.gentoo.org/main/en/lists.xml) and link to it from your application ([archives.gentoo.org](http://archives.gentoo.org) holds past postings). The gentoo-soc list would be a good starting point, if you aren't subscribed to any others already. The best posts would be an introduction of the project you're applying for and a little background about you, to introduce yourself to the community and get some broader input about your project.

- **Give us your contact info and working hours.** Please provide your email address, home mailing address, and phone number. This is a requirement and provides for accountability on both your side and ours. Also, please tell us what hours you will be working and responsive to contact via email and IRC; these should sum to at least 35 hours a week.

These actions are things you will do extremely commonly as an open-source developer, and they really aren't that hard, so don't let them hold you back! The remainder of the application is free-form. **Please read our [application guidelines](http://www.gentoo.org/proj/en/userrel/soc/applying.xml) and [Google's FAQ](http://www.google-melange.com/gsoc/document/show/gsoc_program/google/gsoc2013/help_page) to complete it.** Good luck!

# Ideas



Catalyst is Gentoo's stage building software which automates the building of Gentoo's release media.  Unfortunately it's  [software base](http://ln-s.net/+vQU) is in need of some major updating.  A rewrite has been started a couple of times.  The most recent [catalyst rewrite branch](http://dev.gentoo.org/~dolsen/catalyst/) will be the starting point for future work.  Catalyst is written in a combination of python and bash, much like it's package manager portage.  Problems with the original code include, not being installed to python's site-packages, poor code organization (growing pains), hard coded paths making changes difficult, not using what are now built-in python features and modules, etc..  An initial restructure has been done with some code split into smaller logical files/modules.  Also some of the hard coded paths have been made configurable.

#### Completion of code re-write

1. Migrate more code to use python built-in's rather than it's own code. ie: ConfigParser, argsparse,...
2. Make more variables configurable, remove all remaining hard coded paths.
3. Modularize the code further where it makes sense and reduce code duplication.
4. Update or create docstrings for all functions and classes.
5. Add more inline code comments to explain why something is done the way it is.
6. Update portions of the code to match new features in portage, compression utilities, etc.
7. Make catalyst package manager agnostic, allowing it to use any PMS compliant package manager.
8. Add more options for image types produced.
9. Possibly add more target types
10. Separate out the command line code from the main operational api. This allows for the creation of alternate front-ends or embedding catalyst's operation into other tools.
11. debug!, debug!, debug!

#### Documentation

1. Produce user instruction documentation about how to create/use the different target types
2. Instructions for image generation options.
3. Instructions for user configurable variables and how they might affect what is produced.
4. Document anything else that may come up.



| Contacts | Required Skills | 
|---|---|
|  |  | 

**Completed in 2013**

Lots of ebuild generators were already created by Gentoo users, developers, GSoC Students, etc., to generate ebuilds for a large number of 3rd party software providers, like octave-forge (Octave), pypi (Python), cran (R), cpan (Perl), and others, but each one tries to solve the very same problems on its own unique and "innovative" way.

This project wants to implement a solid base framework to be used by these tools, implementing all the basic algorithms needed to resolve dependencies, create ebuilds, etc.

Each software provider should be a backend, that implements a common interface, defined by the framework, with all the required provider-only stuff needed by the framework to create the ebuilds, either in runtime, calling a package manager just after the ebuild generation, or creating a big overlay with all the packages and dependencies.



| Contacts | Required Skills | 
|---|---|
|  |  | 

#### Improvements to gentoolkit

Gentoolkit's euse application is in need of a rewrite from bash to python. This conversion would not be enough for a complete GSoC project on it's own, but could be extended with additional features, functions that could be shared with other gentoolkit utilities. There are other changes desired in equery, eclean, enalyze that can be done as well. Familiarity with gentoo and gentoolkit would be a plus in aiding the applicant to prepare a complete and thorough application, but not a necessity. For further information contact us to discuss your ideas and/or proposal.

For some additional gentoolkit info, see some irc chat I've posted that describes more things that can be done.
[gentoolkit-gsoc chat](http://dev.gentoo.org/~dolsen/gentoolkit-gsoc)



| Contacts | Required Skills | 
|---|---|
|  |  | 

**Completed in 2013**

This project is about running Gentoo natively on Android devices.

We will develop libc support and root permission handling in Gentoo Prefix, and deploy the enhanced Gentoo Prefix in parallel to Android userland in mobile devices, such as smart phone and tablet. This will provide a full-featured native GNU userland to Android system.

reference: [http://article.gmane.org/gmane.linux.gentoo.alt/6585](http://article.gmane.org/gmane.linux.gentoo.alt/6585)



| Contacts | Required Skills | 
|---|---|
|  |  | 

**Completed in 2013**

In GSoC 2011 an LDAP authentication backend has been written (in Python/Django) for Gentoo infrastructure so that we could do useful things on our websites like have a single sign-on across all Gentoo sites (forums, bugs, etc) and potentially customize our homepage for specific users. This has to get improved. It needs to parse (or read in some other way) the ACL rules from the LDAP server, instead of committing the ACL in the webapp itself.



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

The death of the cia.vc tool, formerly on code.google.com, for live monitoring and notification of commits to open-source projects has created a significant problem and need, not just for Gentoo but more broadly. Gentoo now uses a [workaround](http://gentooexperimental.org/~patrick/weblog/archives/2012-10.html) implemented by Patrick Lauer (bonsaikitten), but we would greatly prefer highly functional software to monitor the commits in our primary repositories (gentoo/gentoo-x86) and in the various overlay repositories as well.



| Contacts | Required Skills | 
|---|---|
|  |  | 

This is your chance to be creative! Gentoo does not currently have anything to help users manage kernel configurations. This project can go into any direction you want, as long as it stays on topic. Examples of desired features are: managing custom kernel configs and future updates for one or multiple systems, multiple choosable trunks with stackable optional groups of settings, help ebuilds "depend" (optionally or not) on specific kernel settings or combinations of settings, plan for a sustainable way of providing optionable default configs to users, etc...

There may be more than one project on this topic, going into different directions or not. Proposals which cover most types of users (home user with one or a few boxes, sysadmin of a moderately sized network, etc...) will be favored. The finished result should be in the form of one or more command-line tools only, unless you can convince your mentor that some form of GUI can bring a significant value to your project.



| Contacts | Required Skills | 
|---|---|
|  |  | 

**Completed in 2013**

[Puppet](https://puppetlabs.com/) is a tool for [configuration management](https://en.wikipedia.org/wiki/Configuration_management) — in other words, maintaining Gentoo by defining infrastructure as code rather than manually editing config files. This is extremely powerful for scaling servers up, and it greatly increases the efficiency of sysadmins. It further makes it easy to track changes to configurations over time, maintain them in git, do rollbacks, and more. The [Puppet Portage module](https://github.com/adrienthebo/puppet-portage) supports a bunch of Portage specific operations (more info can be found in its README file). There are lots of issues and feature requests needed, that can be found in its [issue tracker](https://github.com/adrienthebo/puppet-portage/issues) in Github that should be implemented.



| Contacts | Required Skills | 
|---|---|
|  |  | 

#### Django and RoR support for webapp-config

webapp-config is a tool that is used for installation of web applications. Amongst its features, it offers multiple installations of the same web application in separate directories/vhosts, upgrade of the web application and cleanup. It works well for PHP web applications, but has no support for Django and Ruby on Rails web applications

The goal of this project is to make webapp-config fully support Django and Ruby on Rails web applications the same way it does for PHP ones. The webapp eclass may also need some work.



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

It has been proposed in the past that oldnet splits from OpenRC, and becomes an independent package, for ease of maintenance. At the same time, oldnet is a viable contender against other advanced network configuration systems, such as netctl. For this project, you would implement a compatibility layer for oldnet's usage of OpenRC functionality, and provide a systemd service to run each net.$IFACE service, via symlinks, similar to the existing net.lo to net.iface symlinks. This project would also encourage usage of oldnet OUTSIDE of Gentoo, so while any applicant should be prepared to start with oldnet on Gentoo, testing on a non-Gentoo distribution towards the end of the project should be included in any project proposal.



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
