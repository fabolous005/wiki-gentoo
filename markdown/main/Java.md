<!-- source: https://wiki.gentoo.org/wiki/Java | group: Gentoo Wiki (Main) | wiki-title: Java -->
---
title: Java
url: https://wiki.gentoo.org/wiki/Java
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-09"
fingerprint: fe909b5b80ababc0
license: CC BY-SA 4.0
---

# Java

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Java** is a programming language, originally developed by [Sun Microsystems](https://en.wikipedia.org/wiki/Sun_Microsystems), which uses a platform-independent virtual machine to execute Java bytecode in real-time. It is a popular choice for developers who want to create cross-platform business applications.

## What is Java?

### Overview

Java is a programming language developed by Sun Microsystems. The language is object-oriented and designed to run on multiple platforms without the need of recompiling code for each platform. Although Java can be compiled as a native program, much of Java's popularity can be attributed to its portability, along with other features such as automatic memory management. To make platform independence possible the Java compiler compiles the Java code to an intermediate representation called [Java bytecode](https://en.wikipedia.org/wiki/Java_bytecode) that runs on a *JVM* ([Java Virtual Machine](https://en.wikipedia.org/wiki/Java_virtual_machine)) and not directly on the operating system.

In order to run Java bytecode, one needs to have a *JRE* (Java Runtime Environment) installed. A *JRE* provides core libraries, a platform dependent *JVM*, plugins for browsers, among other things. A *JDK* (Java Development Kit) adds programming tools, such as a bytecode compiler and a debugger.

### JVM languages

The Java virtual machine is not used exclusively by Java programming language. Multiple programming languages use the Java platform and run on the JVM. Examples of such include: [Clojure](https://en.wikipedia.org/wiki/Clojure), [Apache Groovy](https://en.wikipedia.org/wiki/Apache_Groovy), [Kotlin](https://wiki.gentoo.org/wiki/Kotlin) or [Scala](<https://en.wikipedia.org/wiki/Scala_(programming_language)>).

## Installing a virtual machine

### The choices

Gentoo provides Java Runtime Environments (JREs) and Java Development Kits (JDKs). The available choices include:

| Vendor | JDK | 
|---|---|
| [OpenJDK](https://en.wikipedia.org/wiki/OpenJDK) | [dev-java/openjdk](https://packages.gentoo.org/packages/dev-java/openjdk) | 
| [Eclipse Temurin](https://adoptopenjdk.net/) | [dev-java/openjdk-bin](https://packages.gentoo.org/packages/dev-java/openjdk-bin) and [dev-java/openjdk-jre-bin](https://packages.gentoo.org/packages/dev-java/openjdk-jre-bin) | 

### Installing a JRE/JDK

To install the profile's default **JDK** run:

`root #``emerge --ask --oneshot virtual/jdk`
To install the profile's default **JRE** run:

`root #``emerge --ask --oneshot virtual/jre`
### Setting up a headless JRE

Sometimes there is no need for a full JRE with all the capabilities of Java. Using Java on a server often does not require any GUI, graphical, sound or even printer related features. To install a simplified (sometimes also referred to as headless) JRE, a few USE flags need to be changed for the selected JRE flavor.

**`/etc/portage/package.use`**

**Required USE flag changes**

Depending on the current Gentoo profile, this might already be the case. As usual, the USE flag settings that are applicable to a particular package can be checked by running emerge in pretend mode:

`user $``emerge --pretend --verbose virtual/jre`
## Configuring the Java Virtual Machine

### Overview

Gentoo has the ability to have multiple JDKs and JREs installed without causing conflicts.

The eselect command can be used to present a list of installed Java instances (be it JRE or JDK). Here is an example of the output:

`user $``eselect java-vm list`
Available Java Virtual Machines:
  \[1\]   openjdk-8 
  \[2\]   openjdk-11 
  \[3\]   openjdk-17
  \[4\]   openjdk-bin-8  system-vm user-vm

The *user-vm* flag indicates the default JVM for the user. The *system-vm* flag indicates the default JVM for the system and the fallback if a user JVM is not set. The number in the brackets (i.e. \[1\]) is the reference for the particular JVM. To set the default system JVM:

`root #``eselect java-vm set system 1`
To set a preferred user JVM:

`user $``eselect java-vm set user 1`
## Java browser plugins

For those who need a Java-enabled browser for a specific use case, there are [www-client/palemoon::palemoon](https://repos.gentoo.org/#palemoon) or [www-clint/palemoon-bin::palemoon](https://repos.gentoo.org/#palemoon) packages available in the [palemoon overlay](https://github.com/deu/palemoon-overlay), which has long-term support for NPAPI and thus Java plugins up to JDK 8<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>.

## USE flags for use with Java

### Setting USE flags

For more information regarding [USE flags](https://wiki.gentoo.org/wiki/USE_flag), refer to the USE flags chapter from the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:X86/Working/USE).

### USE flags

- The [`java`](https://packages.gentoo.org/useflags/java) flag adds support for Java in a variety of programs;
- The [`nsplugin`](https://packages.gentoo.org/useflags/nsplugin) flag is still used by [www-plugins/lightspark](https://packages.gentoo.org/packages/www-plugins/lightspark);

Following USE flags go in JAVA\_PKG\_IUSE, see [Gentoo Java USE flags](https://wiki.gentoo.org/wiki/Gentoo_Java_USE_flags) for details and other specific USE flags of Java:

- The [`source`](https://packages.gentoo.org/useflags/source) flag installs a zip of the source code of a package. This is traditionally used for IDEs to 'attach' source to the libraries that are being use;
- For Java packages, the [`doc`](https://packages.gentoo.org/useflags/doc) flag will build API documentation using javadoc.

## Multiple Java Versions

For those that may need multiple versions of Java, one may use slotted packages.

`root #``emerge --ask dev-java/openjdk:8``root #``emerge --ask dev-java/openjdk:11``root #``emerge --ask dev-java/openjdk:17``root #``emerge --ask dev-java/openjdk:21`
The default can then be changed by [setting a default](https://wiki.gentoo.org/wiki/Java#Setting_a_default).

## Troubleshooting

### Java 21 Circular Dependencies

If you get an error when installing openjdk:21, emerge openjdk-bin:21.

`root #``emerge --ask dev-java/openjdk-bin:21`
### Minecraft launcher errors

- A specific error in which minecraft-launcher crashed after a few seconds, throwing "Alarm" and "SaveToBuffer failed" error was solved by setting the [threads](https://packages.gentoo.org/useflags/threads)[net-misc/curl](https://packages.gentoo.org/packages/net-misc/curl).

- When executing minecraft-launcher the following error was produced:

`user $``./minecraft-launcher`
\[0229/184549.183275:ERROR:sandbox\_linux.cc(346)\] InitializeSandbox() called with multiple threads in process gpu-process.

This was solved by executing minecraft-launcher with the following option:

`user $``MESA_GLSL_CACHE_DISABLE=true ./minecraft-launcher`
## See also

- [Java Developer Guide](https://wiki.gentoo.org/wiki/Java_Developer_Guide) — covers specific details on Gentoo Java ebuilds.
- [Project:Java/Why build from source](https://wiki.gentoo.org/wiki/Project:Java/Why_build_from_source)
- [Project:Java/Getting Involved](https://wiki.gentoo.org/wiki/Project:Java/Getting_Involved)

## External resources

- Configuring Java per directory with [jEnv](https://www.jenv.be)
- [#gentoo](ircs://irc.libera.chat/#gentoo) ([webchat](https://web.libera.chat/#gentoo)) and [#gentoo-java](ircs://irc.libera.chat/#gentoo-java) ([webchat](https://web.libera.chat/#gentoo-java)) on IRC

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [JDK 9 and the Java Plugin](https://www.java.com/en/download/faq/jdk9_plugin.xml), java.com. Retrieved on November 30, 2018
2. [↑](https://wiki.gentoo.org#cite_ref-2) [How do I enable Java in my web browser?](https://java.com/en/download/help/enable_browser.xml), java.com. Retrieved on November 30, 2018
3. [↑](https://wiki.gentoo.org#cite_ref-3) [Pale Moon future roadmap](https://www.palemoon.org/roadmap.shtml), palemoon.org. Retrieved on June 28, 2019
