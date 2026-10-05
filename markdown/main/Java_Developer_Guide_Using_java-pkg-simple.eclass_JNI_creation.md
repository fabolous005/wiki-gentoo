<!-- source: https://wiki.gentoo.org/wiki/Java_Developer_Guide/Using_java-pkg-simple.eclass/JNI_creation | group: Gentoo Wiki (Main) | wiki-title: Java Developer Guide/Using java-pkg-simple.eclass/JNI creation -->
---
title: Java Developer Guide/Using java-pkg-simple.eclass/JNI creation
url: https://wiki.gentoo.org/wiki/Java_Developer_Guide/Using_java-pkg-simple.eclass/JNI_creation
hostname: gentoo.org
sitename: Java Developer Guide/Using java-pkg-simple.eclass/JNI creation
date: "2023-12-03"
fingerprint: "3fd3e8ecdc93bafd"
license: CC BY-SA 4.0
---

# Java Developer Guide/Using java-pkg-simple.eclass/JNI creation

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

An example of JNI creation without scripts for external tools can be found in [dev-java/lz4-java](https://packages.gentoo.org/packages/dev-java/lz4-java):

For linking to a system library, **lz4** in this case, it is essential to put the arguments in the right order, **-llz4** at the end.
