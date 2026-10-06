<!-- source: https://wiki.gentoo.org/wiki/Build_automation | group: Gentoo Wiki (Main) | wiki-title: Build automation -->
---
title: Build automation
url: https://wiki.gentoo.org/wiki/Build_automation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-03-05"
categories: ['dev-build']
fingerprint: a10cb21b5e45693d
license: CC BY-SA 4.0
---

# Build automation

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

Build automation (generally referred to as 'Build systems' in the Gentoo world) is software that automates the compilation, clean up, and installation stages of the software creation process. Recently it has become more common for build automation to perform elements of software testing. Gentoo developers must have at least a general understanding of one or more build systems in order to start writing [ebuilds](https://wiki.gentoo.org/wiki/Ebuild). This article services as a type of meta article in defining a list of build systems in Gentoo Linux.

## Available software

Build systems available in the main Gentoo repository include:

| Name | Package | Homepage | Description | 
|---|---|---|---|
| [ant](https://wiki.gentoo.org/wiki/Gentoo_Java_Packing_Policy#Ant) | [dev-java/ant](https://packages.gentoo.org/packages/dev-java/ant) | [https://ant.apache.org/](https://ant.apache.org/) | [Java-based build tool](https://wiki.gentoo.org/wiki/Gentoo_Java_Packing_Policy#Java_build_systems) similar to 'make' that uses XML configuration files. | 
| [autotools](https://wiki.gentoo.org/wiki/Autotools) | [dev-build/autoconf](https://packages.gentoo.org/packages/dev-build/autoconf) [dev-build/automake](https://packages.gentoo.org/packages/dev-build/automake) | [https://www.gnu.org/software/autoconf/autoconf.html](https://www.gnu.org/software/autoconf/autoconf.html) [https://www.gnu.org/software/automake/](https://www.gnu.org/software/automake/) | A build system used commonly in open source projects. A little austere, but has the advantage of being almost ubiquitous. | 
| cmake | [dev-build/cmake](https://packages.gentoo.org/packages/dev-build/cmake) | [https://cmake.org/](https://cmake.org/) | Open-source, cross-platform tools to build, test and package software. | 
| [Gradle](https://wiki.gentoo.org/wiki/Gradle) | [dev-java/gradle-bin](https://packages.gentoo.org/packages/dev-java/gradle-bin) | [https://gradle.org/](https://gradle.org/) | Java-based build, automation and delivery tool. | 
| [make](https://wiki.gentoo.org/wiki/Make) | [dev-build/make](https://packages.gentoo.org/packages/dev-build/make) | [https://www.gnu.org/software/make/make.html](https://www.gnu.org/software/make/make.html) | Standard tool to compile source trees | 
| meson | [dev-build/meson](https://packages.gentoo.org/packages/dev-build/meson) | [https://mesonbuild.com/](https://mesonbuild.com/) | Open source build system designed to be fast, and user friendly. | 
| ninja | [dev-build/ninja](https://packages.gentoo.org/packages/dev-build/ninja) | [https://ninja-build.org/](https://ninja-build.org/) | A small build system similar to make. | 
| premake | [dev-util/premake](https://packages.gentoo.org/packages/dev-util/premake) | [https://premake.github.io](https://premake.github.io) | Makefile generation tool. | 
| [SCons](https://wiki.gentoo.org/wiki/SCons) | [dev-build/scons](https://packages.gentoo.org/packages/dev-build/scons) | [http://www.scons.org/](http://www.scons.org/) | Scons is a build system written in Python. | 

This is a partial selection of packages available in the Gentoo repository, see for example [dev-build](https://packages.gentoo.org/categories/dev-build) , or use [eix](https://wiki.gentoo.org/wiki/Eix) (eix --category dev-build), to see packages from the *dev-build* category.

## See also

- [User:Maffblaster/Drafts/Comparison of build systems](https://wiki.gentoo.org/wiki/User:Maffblaster/Drafts/Comparison_of_build_systems) — provides a brief comparison of various build systems.
