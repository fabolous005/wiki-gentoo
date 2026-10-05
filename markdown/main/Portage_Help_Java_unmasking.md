<!-- source: https://wiki.gentoo.org/wiki/Portage/Help/Java_unmasking | group: Gentoo Wiki (Main) | wiki-title: Portage/Help/Java unmasking -->
---
title: Portage/Help/Java unmasking
url: https://wiki.gentoo.org/wiki/Portage/Help/Java_unmasking
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-15"
fingerprint: fc4c070f7fff656b
license: CC BY-SA 4.0
---

# Portage/Help/Java unmasking

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**2026-01-15**, the information in this article is probably

**outdated**. You can help the Gentoo community by verifying and

[updating this article](https://wiki.gentoo.org/index.php?title=Portage/Help/Java_unmasking&action=edit).

## Background

Java in Gentoo supports two modes once installed:

1. `system-vm` in eselect java-vm (you need the `gentoo-vm` USE flag enabled for this). This is what ebuilds use during installs.
2. `user` in eselect java-vm which allows you to, as your user, run java -version and use the version you desire.

This article is for unmasking **system-vm** for use in ebuilds.  This flag is currently masked for newer Javas like 11 and 17 because packages may break with it. No action is required for the user mode described above.

## Enabling Java 11

Links:

- [bug #810613](https://bugs.gentoo.org/show_bug.cgi?id=810613) covers progress towards unmasking Java 11, focusing on issues likely to hit users / dependencies of packages.
- [bug #697014](https://bugs.gentoo.org/show_bug.cgi?id=697014) covers all known bugs related to Java 11 but may be less important.

### Unmasking

**`/etc/portage/profile/package.use.mask`**

```
# Allow Java 11 (might cause some build failures for now, see bug #810613)
# Undo the mask from profiles/base/package.use.mask
dev-java/openjdk-bin:11 -gentoo-vm
dev-java/openjdk:11 -gentoo-vm
dev-java/openjdk-jre-bin:11 -gentoo-vm
```
**`/etc/portage/package.use`**

```
# Allow Java 11 (might cause some build failures for now, see bug #810613)
# Just to be sure, turn on the flag too.
dev-java/openjdk-bin:11 gentoo-vm
dev-java/openjdk:11 gentoo-vm
dev-java/openjdk-jre-bin:11 gentoo-vm
```
**`/etc/portage/package.unmask`**

```
# Allow Java 11 (might cause some build failures for now, see bug #810613)
# Undo the mask from profiles/package.mask
virtual/jre:11
virtual/jdk:11
```
### Installation

Install any application which needs OpenJDK 11. If it doesn't pull it in, you'll need to emerge it manually as below:

`root #``emerge --ask dev-java/openjdk-bin:11`
### Activating

You may need to activate OpenJDK 11 after installing it.

Check if it's enabled (we want `system-vm` next to openjdk\*11)

`root #``eselect java-vm list`
Available Java Virtual Machines:
  \[1\]   openjdk-bin-8  system-vm
  \[2\]   openjdk-bin-11

Enable it if it's not already (in this case, it's number '2', it might be different for you):

`root #``eselect java-vm set system 2``root #``eselect java-vm list`
Available Java Virtual Machines:
  \[1\]   openjdk-bin-8
  \[2\]   openjdk-bin-11  system-vm

## Enabling Java 17

Links:

- [bug #825574](https://bugs.gentoo.org/show_bug.cgi?id=825574) - covers all known bugs related to Java 17.
- [bug #825710](https://bugs.gentoo.org/show_bug.cgi?id=825710) - covers all test failures specific to Java 17.

### Unmasking

**`/etc/portage/profile/package.use.mask`**

```
# Allow Java 17 (might cause some build failures for now, bug #825574)
# Undo the mask from profiles/base/package.use.mask
dev-java/openjdk-bin:17 -gentoo-vm
dev-java/openjdk:17 -gentoo-vm
dev-java/openjdk-jre-bin:17 -gentoo-vm
```
**`/etc/portage/package.use`**

```
# Allow Java 17 (might cause some build failures for now, bug #825574)
# Just to be sure, turn on the flag too.
dev-java/openjdk-bin:17 gentoo-vm
dev-java/openjdk:17 gentoo-vm
dev-java/openjdk-jre-bin:17 gentoo-vm
```
**`/etc/portage/package.unmask`**

```
# Allow Java 17 (might cause some build failures for now, bug #825574)
# Undo the mask from profiles/package.mask
virtual/jre:17
virtual/jdk:17
```
### Installation

Install any application which needs OpenJDK 17. If it doesn't pull it in, you'll need to emerge it manually as below:

`root #``emerge --ask dev-java/openjdk-bin:17`
### Activating

You may need to activate OpenJDK 17 after installing it.

Check if it's enabled (we want `system-vm` next to openjdk\*17)

`root #``eselect java-vm list`
Available Java Virtual Machines:
  \[1\]   openjdk-bin-8  system-vm
  \[2\]   openjdk-bin-17

Enable it if it's not already (in this case, it's number '2', it might be different for you):

`root #``eselect java-vm set system 2``root #``eselect java-vm list`
Available Java Virtual Machines:
  \[1\]   openjdk-bin-8
  \[2\]   openjdk-bin-17  system-vm
