<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2021/Ideas/Big_Data_Infrastructure_by_Gentoo | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2021/Ideas/Big Data Infrastructure by Gentoo -->
---
title: Google Summer of Code/2021/Ideas/Big Data Infrastructure by Gentoo
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2021/Ideas/Big_Data_Infrastructure_by_Gentoo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-05-20"
fingerprint: "79aaf62851dd61ef"
license: CC BY-SA 4.0
---

# Google Summer of Code/2021/Ideas/Big Data Infrastructure by Gentoo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The big data infrastructures are mostly built on the Java virtual machine ecosystem, most notably in Java and Scala.

Nevertheless, Java has not been adopted smoothly into GNU/Linux distributions.  The packaging of Java software are considered difficult by the GNU/Linux community (e.g. [Debian](https://wiki.debian.org/Java/Packaging), [Archlinux](https://wiki.archlinux.org/index.php/Java_package_guidelines), [Fedora](https://fedoraproject.org/wiki/Java)).  At the same time, the Java community has its own set of repositories like [maven](https://maven.apache.org), functionally similar to packages in GNU/Linux distributions.

The [Gentoo Java Project](https://wiki.gentoo.org/wiki/Project:Java) has done a good job laying out the framework of the Java ecosystem in Gentoo. At the same time, there are still thousands of useful Java packages to be packaged and maintained.  Last year, Zongyu Zhang has developed the Maven ebuild generator and published the [spark overlay](https://github.com/6-6-6/spark-overlay), make spark available for Gentoo users.  This project will build upon that, to design and set up a test framework for the generated ebuilds, handle kotlin and scala packages, and add [h2o](https://mvnrepository.com/artifact/ai.h2o/h2o-core) big data analysis platform into the overlay.



| Contacts | Required Skills | 
|---|---|
|  |  |
