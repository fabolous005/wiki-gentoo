<!-- source: https://wiki.gentoo.org/wiki/Gentoo_Alt/Contributor%27s_Guide/Porting | group: Gentoo Wiki (Main) | wiki-title: Gentoo Alt/Contributor's Guide/Porting -->
---
title: Gentoo Alt/Contributor's Guide/Porting
url: https://wiki.gentoo.org/wiki/Gentoo_Alt/Contributor%27s_Guide/Porting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-18"
fingerprint: a7b4787ecfa49f9c
license: CC BY-SA 4.0
---

# Gentoo Alt/Contributor's Guide/Porting

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

This guide will show you that starting a new Gentoo/Alt port isn't difficult once you know what to take care of.

## Introduction

### Preamble

Starting a new Gentoo/Alt port is not too difficult, when you know how to do it and what needs to be considered. This guide will explain the basic generic steps needed to start the porting work.

Because of the Linux nature or the base of Gentoo/Alt structure, some of the internals are based on an Unix-like structure, while the QA checks on the compiled binaries are done considering ELF binaries as result. Tweaking of these checks is a complex process and needs to be done with the help of the devs who wrote those checks. For this reason, they won't be treated in this guide.

There are quite a few decisions that one should start looking at when starting a new Gentoo/Alt project, as there are many different parameters that might change the way the changes should be made. For every decision, this guide will try to provide a selection of problems and solutions to them, trying to give help for every case.

You probably can try to port Gentoo/Alt on different conditions than the ones explained here. They are just general guidelines to explain how the things were done on current working ports.

### Support

It's usually useful, when creating a new port, to get in touch with the other developers working on Gentoo/Alt. This can be done through the gentoo-alt mailing list or through the [#gentoo-alt](ircs://irc.libera.chat/#gentoo-alt) ([webchat](https://web.libera.chat/#gentoo-alt)) IRC channel on Libera Chat.

Mailing lists and IRC channels are a good way to find solutions to problems that are not treated in this guide (because of difference in the environment or simply because they were not present a the time of writing), and to follow the development of common decision inside and outside the project.

Also, if you're going to use Gentoo Portage for your port, you probably also want to watch the alt@gentoo.org mail alias on Bugzilla to see what is going on with portability of most ebuilds or scripts or other things that you might encounter in your process.

### Choices

There are many choices one has to do when starting a Gentoo/Alt project. The first and probably most important one is to choose if making a primary package manager or a secondary. The first case is what is being done by Gentoo/FreeBSD, while the latter is what is being done by Gentoo/OSX.

After deciding this there are other things that should be considered, starting, for example, from the kind of init system you want to use, if you want to just use the one used by your operating system, or replace it with the one used by Gentoo Linux.

Every decision has its own rights and wrongs, and they should always be chosen carefully. Sometimes it's also possible to provide different implementations that allow the user to select what they want to do.

### Defining the userland

Just one decision should be done as soon as possible and should be notified to Gentoo/Alt mailing list and developers, and regards the userland. Every system has its own native userland, sometimes using options different than other userlands.

The most clear distinction is between GNU userland, used by almost every Linux-based distribution, and the BSD userland, used by FreeBSD and other BSDs. The ${USERLAND} variable used by Gentoo represents the set of commands used by the base system and their options handling. For example, in a BSD userland, the GNU packages install with a g- prefix, and the non prefixed commands are BSD, accepting a different range of options and arguments.

When defining a new value for ${USERLAND}, you should send an email to gentoo-alt mailing list telling what it represents, which differences there are between that and the default "GNU" userland, and who provides the basic commands like make, sed, awk, patch, tar. This means that a given userland might use GNU make as make, but still use GNU sed as gsed (this is actually done by Darwin userland).

The userland selection will change the way things will be installed afterwards and also the way some parts of Portage will behave, so please choose carefully what you want to set and declare.

## Porting as primary manager

### What does it mean

A primary package manager is a package manager that takes care of every file, library and dependency of the package it manages. When creating a port that is a primary package manager, every file present in the system must be managed by portage itself.

There are many reasons why not every port can be a primary package manager. To do that, you need to have access to the sources of the whole operating system and be able to patch them, usually. This means that you cannot create a primary package manager for a proprietary operating system where you don't have the sources for the base system. In those cases, you must rely on secondary package management.

The primary package management is what Portage is born to do, and what Gentoo Linux does. Adapting Portage to be the primary package manager in another operating system, when you have sources, it's usually simple enough to be feasible without too much hacking, and should be considered the starting point for projects that wants to create a secondary package manager.

### How to bootstrap

With the term bootstrap this guide will usually refer to the process of creating the first base from where to start doing the port work for portage code, ebuilds and eclasses. While a bootstrapped system is not a complete Gentoo system, it should be able to install many packages with a simple emerge command. From such a system you can usually create a stage, that will be the actual starting point for new Gentoo/Alt systems.

The bootstrap process is sometimes tricky, as it requires to patch, compile and tweak many packages by hand. It also might require to be done once per version of the operating system, and also once per hardware architecture you want to add, if the sources are not cross-compilable (and also in that case, it can be tricky as Portage has very little cross-compile support).

The first thing to do is installing the dependencies of Portage itself, this means Python, bash, GNU make, GNU sed, GNU awk and GNU patch. Depending on what your "classic" userland uses for them, you might need to install them with a g- prefix, making them gmake, gsed, gawk and gpatch.

Additionally, you might want to also install rsync to be able to get the portage tree with emerge sync command. This is not mandatory, though, as you can work on the first steps using a NFS-mounted tree or simply downloading a snapshot of the tree to work on. This way is also suggested to be able to get easily the difference between the original ebuilds and the ones edited to let them work on your system, when needed.

Python is the critical part, as it needs to be patched with the same patches that are applied by the ebuild itself. Please refer to the latest ebuild in Portage to get the updated patches and make sure they are applied before building and installing it. Also, it should be installed on /usr, not in /usr/local as it defaults.

Bash is instead a quite simple package. The only note to this is that, while many systems can install bash as an extra package, and then install its binary in /usr/bin, that won't work easily on Gentoo/Alt projects, so it should be installed as /bin/bash. This path is important as many scripts and other things in portage refers to that directly.

The other packages (GNU make, GNU sed, GNU awk and GNU patch) are quite simple to handle. They can be installed in /usr/local, while it's suggested to install them in /usr as an ebuild would do. The only thing is that, if you decide to prefix them with g-, you should run the ./configure in a slighly different way.

The `--program-prefix` option is only necessary when they must be g-prefixed:

Once you built and installed all those packages, you can start installing Portage itself. Right now, Portage package does not have an installation script nor is it autotooled, so you need to install it by hand. Depending on the version currently in Portage, you might need to patch the source tarball. For both the patches and the installation procedure, it's highly suggested to refer to the ebuild for that version of Portage itself.

To run Portage, you need to clean a few things up before. The first thing is adding a portage user and group. This group should have uid/gid set at 250. Also, for many things you need a wheel group. This group is used on BSD derived system instead of the root group used by Linux and Solaris. If the operating system under which you're porting Portage does not have a wheel group, add one with either gid 0 or 10 (the first is an alias to root group, as done by BSDs, the latter is instead the wheel replacement used on Linux).

### Dealing with library naming changes (ELF only)

There's one thing that might be tricky when starting a port for a totally open source operating system, if they use so-called contrib packages, like libraries and similar, in other words libraries developed by other open source projects and released separately, used inside the system sources with the sources copied verbatim: when using the same library by the standalone package, it might change soname from the version in the operating system.

A little explanation: the soname is the internal name of the library, used by the dynamic loader to know that the library is the right one (that is, it uses the right ABI) for the executable it's loading. Usually the soname corresponds either to the whole name of the library (libfoo.so.3.4) or to the name with the first version stated (libfoo.so.3.4). Most of the dynamic linkers don't really take into consideration that the soname is the same, but uses the soname to look up the file on disk.

Especially on \*BSD based systems, the libraries are named with a single version after the .so extension, while on GNU-based systems or with a GNU setting in libtool, the name is with two or three parts, to avoid breaking the linkage of executables for every minor update of the library that does not really break the ABI.

When doing the bootstrap of a system that uses this setup, you'll end up with packages pointing to libraries named in a different way than the same libraries in portage. For instance a default FreeBSD 5.4 install will have libiconv.so.4 while portage will install libiconv.so.2.3. To solve these problems, after making sure the version of the library is the same or that the ABI is compatible (no missing symbols), you can do some symlinks to let the dynamic loader to load the new library (2.3). These hacks has to be removed after the whole system is re-emerged, so that they are not needed in the final stage.

### Things to change in Portage and eclasses

There are a few things that usually need to be checked when porting Portage on a new operating system. The most important one is the ldconfig and its way to update the libraries path. Portage uses ldconfig inside env-update script to tell the dynamic loader where to look for libraries.

As the syntax and the way ldconfig work varies vastly on different operating systems, this must be changed in portage itself. This must be looked at with the help of Portage developers, and can be discussed on gentoo-alt mailing list.

Another important thing is to update enewuser, enewgroup, egetent and egethome functions in eutils.eclass and portability.eclass. Those functions are used to create new users and new group, to get data about existing users and groups, and to get the home directory for a given user. They use the ${CHOST} variable to find out how to do that, so you need to add a value for your own value.

If your operating system uses a linker (ld) different from GNU's one, you should also add it to the bindnow\_flags function inside flag-o-matic.eclass, and you're suggested to improve the nowbinding.m4 macro file to select the right flags. The flags used for "bindnow" binding are used to avoid lazy binding of libraries on setuid binaries. Please refer to your own linker manual to find which flag should be used.
