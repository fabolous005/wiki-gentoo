<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2020/Ideas/Big_Data_Infrastructure_by_Gentoo | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2020/Ideas/Big Data Infrastructure by Gentoo -->
---
title: Google Summer of Code/2020/Ideas/Big Data Infrastructure by Gentoo
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2020/Ideas/Big_Data_Infrastructure_by_Gentoo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-01-25"
fingerprint: "79a976285dc763ec"
license: CC BY-SA 4.0
---

# Google Summer of Code/2020/Ideas/Big Data Infrastructure by Gentoo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The big data infrastructures are mostly built on the Java virtual machine ecosystem, most notably in Java and Scala.

Nevertheless, Java has not been adopted smoothly into GNU/Linux distributions.  The packaging of Java software are considered difficult by the GNU/Linux community (e.g. [Debian](https://wiki.debian.org/Java/Packaging), [Archlinux](https://wiki.archlinux.org/index.php/Java_package_guidelines), [Fedora](https://fedoraproject.org/wiki/Java)).  At the same time, the Java community has its own set of repositories like [maven](https://maven.apache.org), functionally similar to packages in GNU/Linux distributions.

The [Gentoo Java Project](https://wiki.gentoo.org/wiki/Project:Java) has done a good job laying out the framework of the Java ecosystem in Gentoo. At the same time, there are still thousands of useful Java packages to be packaged and maintained.  The project will parse the metadata of maven packages and automatically write ebuilds compatible with the Java build system used in Gentoo.  We are going to set up and maintain an automatically updated maven overlay every Gentoo user can use.  The overlay will at least contain [spark](http://spark.apache.org/) and [hadoop](http://hadoop.apache.org/).  We aim to make Gentoo an attractive choice for data scientists, Java developers, users and big data system administrators.

A preliminary tool, [java-ebuilder](https://github.com/gentoo/java-ebuilder) is available as [app-portage/java-ebuilder](https://packages.gentoo.org/packages/app-portage/java-ebuilder).  A proof-of-concept [overlay](https://github.com/heroxbd/maven-overlay) is also made.



| Contacts | Required Skills | 
|---|---|
|  |  |
