<!-- source: https://wiki.gentoo.org/wiki/Gradle | group: Gentoo Wiki (Main) | wiki-title: Gradle -->
---
title: Gradle
url: https://wiki.gentoo.org/wiki/Gradle
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-23"
fingerprint: f94f0504fedd7fe
license: CC BY-SA 4.0
---

# Gradle

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Gradle** is a [Java](https://wiki.gentoo.org/wiki/Java)-based build, automation and delivery tool.

## Usage

### Dependency tree

Packagers wanting to see the dependency tree of a package, e.g. [v1.72](https://github.com/bcgit/bc-java/tree/r1rv72/mail) of [dev-java/bcmail](https://packages.gentoo.org/packages/dev-java/bcmail), use:

`user $``gradle dependencies | sed -n '/Classpath/,/^$/p'````
compileClasspath - Compile classpath for source set 'main'.
+--- project :core
+--- project :util
+--- project :pkix
+--- project :prov
\--- javax.mail:mail:1.4
     \--- javax.activation:activation:1.1
runtimeClasspath - Runtime classpath of source set 'main'.
+--- project :core
|    \--- org.openjdk.jmh:jmh-core:1.33
|         +--- net.sf.jopt-simple:jopt-simple:4.6
|         \--- org.apache.commons:commons-math3:3.2
+--- project :util
|    \--- project :core (*)
+--- project :pkix
|    +--- project :core (*)
|    +--- project :util (*)
|    \--- project :prov
|         \--- project :core (*)
+--- project :prov (*)
\--- javax.mail:mail:1.4
     \--- javax.activation:activation:1.1
testCompileClasspath - Compile classpath for source set 'test'.
+--- project :core
+--- project :util
+--- project :pkix
+--- project :prov
+--- javax.mail:mail:1.4
|    \--- javax.activation:activation:1.1
\--- junit:junit:4.11
     \--- org.hamcrest:hamcrest-core:1.3
testRuntimeClasspath - Runtime classpath of source set 'test'.
+--- project :core
|    \--- org.openjdk.jmh:jmh-core:1.33
|         +--- net.sf.jopt-simple:jopt-simple:4.6
|         \--- org.apache.commons:commons-math3:3.2
+--- project :util
|    \--- project :core (*)
+--- project :pkix
|    +--- project :core (*)
|    +--- project :util (*)
|    \--- project :prov
|         \--- project :core (*)
+--- project :prov (*)
+--- javax.mail:mail:1.4
|    \--- javax.activation:activation:1.1
\--- junit:junit:4.11
     \--- org.hamcrest:hamcrest-core:1.3
```
For a specific Java classpath, e.g. `runtimeClasspath`, the command is:

`user $``gradle dependencies --configuration runtimeClasspath`
In case the above commands give errors it's sometimes still possible to run gradlew:

`user $``./gradlew dependencies`
### Further command line tasks

[Command line](https://wiki.gentoo.org/wiki/Shell) tasks are explained on [https://docs.gradle.org/current/userguide/command\_line\_interface.html#common\_tasks](https://docs.gradle.org/current/userguide/command_line_interface.html#common_tasks)

## Availability

An ebuild is available in the [mva-overlay](https://github.com/msva/mva-overlay/tree/master/dev-java/gradle) ebuild repository.

## Packages waiting for Gradle support

New packages for the tree which depend on Gradle are organized by the tracker ticket [bug #777609](https://bugs.gentoo.org/show_bug.cgi?id=777609)
