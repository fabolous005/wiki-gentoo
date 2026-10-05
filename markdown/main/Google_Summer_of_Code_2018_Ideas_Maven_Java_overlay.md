<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2018/Ideas/Maven_Java_overlay | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2018/Ideas/Maven Java overlay -->
---
title: Google Summer of Code/2018/Ideas/Maven Java overlay
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2018/Ideas/Maven_Java_overlay
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-11"
fingerprint: f9a976ec59cf63ff
license: CC BY-SA 4.0
---

# Google Summer of Code/2018/Ideas/Maven Java overlay

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Although being one of the most popular computer languages, Java has not been adopted smoothly into GNU/Linux distributions.  The packaging of Java software are considered difficult by the GNU/Linux community (e.g. [Debian](https://wiki.debian.org/Java/Packaging), [Archlinux](https://wiki.archlinux.org/index.php/Java_package_guidelines), [Fedora](https://fedoraproject.org/wiki/Java)).  At the same time, the Java community has its own set of repositories like [maven](https://maven.apache.org), functionally similar to packages in GNU/Linux distributions.

The [Gentoo Java Project](https://wiki.gentoo.org/wiki/Project:Java) has done a good job laying out the framework of the Java ecosystem in Gentoo. Nevertheless, there are still thousands of useful Java packages to be packaged and maintained.  The project will parse the metadata of maven packages and automatically write ebuilds compatible with the Java build system used in Gentoo.  We are going to set up and maintain an automatically updated maven overlay every Gentoo user can use.  The overlay will at least contain [spark](http://spark.apache.org/) and [hadoop](http://hadoop.apache.org/).  We aim to make Gentoo an attractive choice for Java developers, users and system administrators, as well as data scientists.

A preliminary tool, [java-ebuilder](https://github.com/gentoo/java-ebuilder) is available as [app-portage/java-ebuilder](https://packages.gentoo.org/packages/app-portage/java-ebuilder).  A proof-of-concept [overlay](https://github.com/heroxbd/maven-overlay) is also made.



| Contacts | Required Skills | 
|---|---|
|  |  |
