<!-- source: https://wiki.gentoo.org/wiki/Maven | group: Gentoo Wiki (Main) | wiki-title: Maven -->
---
title: Maven
url: https://wiki.gentoo.org/wiki/Maven
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-07"
tags: ['https://github.com/FasterXML/jackson-databind/archive/refs/tags/jackson-databind-2.13.0.tar.gz']
fingerprint: baa18e2e87a03b28
license: CC BY-SA 4.0
---

# Maven

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Maven is a very wide spread [Java build system](https://wiki.gentoo.org/wiki/Gentoo_Java_Packing_Policy#Java_build_systems). It is presently not supported in the [eclasses](https://wiki.gentoo.org/wiki/Eclass) but in many cases it is possible to translate a package's pom.xml file into an ebuild using the [java-pkg-simple.eclass](https://wiki.gentoo.org/wiki/Java_Developer_Guide#Using_java-pkg-simple).

As of 2019-06-23, Gentoo only has a binary ebuild for Maven ([dev-java/maven-bin](https://packages.gentoo.org/packages/dev-java/maven-bin)) in the Gentoo ebuild repository.
However [dev-java/maven](https://packages.gentoo.org/packages/dev-java/maven) is still not available [bug #63285](https://bugs.gentoo.org/show_bug.cgi?id=63285)

## Usage

### mvn dependency:tree

Maven can be used to get the dependency tree of a new package version in order to know what dependencies need to get updated or packaged.

To do so, the package should first be downloaded and unpacked.

`user $``wget` [https://github.com/FasterXML/jackson-databind/archive/refs/tags/jackson-databind-2.13.0.tar.gz](https://github.com/FasterXML/jackson-databind/archive/refs/tags/jackson-databind-2.13.0.tar.gz)
`user $``tar xzf jackson-databind-2.13.0.tar.gz`
Then step into the new directory and ask for the dependency tree

`user $``cd jackson-databind-jackson-databind-2.13.0``user $``mvn dependency:tree`
\[INFO\] Scanning for projects...
\[INFO\] Inspecting build with total of 1 modules...
\[INFO\] Installing Nexus Staging features:
\[INFO\]   ... total of 1 executions of maven-deploy-plugin replaced with nexus-staging-maven-plugin
\[INFO\] 
\[INFO\] ------------\< com.fasterxml.jackson.core:jackson-databind >-------------
\[INFO\] Building jackson-databind 2.13.0
\[INFO\] -------------------------------\[ bundle \]-------------------------------
\[INFO\] 
\[INFO\] --- maven-dependency-plugin:3.1.2:tree (default-cli) @ jackson-databind ---
\[INFO\] com.fasterxml.jackson.core:jackson-databind:bundle:2.13.0
\[INFO\] +- com.fasterxml.jackson.core:jackson-annotations:jar:2.13.0:compile
\[INFO\] +- com.fasterxml.jackson.core:jackson-core:jar:2.13.0:compile
\[INFO\] +- org.powermock:powermock-core:jar:2.0.0:test
\[INFO\] |  +- org.powermock:powermock-reflect:jar:2.0.0:test
\[INFO\] |  |  \- org.objenesis:objenesis:jar:3.0.1:test
\[INFO\] |  +- org.javassist:javassist:jar:3.24.0-GA:test
\[INFO\] |  +- net.bytebuddy:byte-buddy:jar:1.9.3:test
\[INFO\] |  \- net.bytebuddy:byte-buddy-agent:jar:1.9.3:test
\[INFO\] +- org.powermock:powermock-module-junit4:jar:2.0.0:test
\[INFO\] |  +- org.powermock:powermock-module-junit4-common:jar:2.0.0:test
\[INFO\] |  \- org.hamcrest:hamcrest-core:jar:1.3:test
\[INFO\] +- org.powermock:powermock-api-mockito2:jar:2.0.0:test
\[INFO\] |  +- org.powermock:powermock-api-support:jar:2.0.0:test
\[INFO\] |  \- org.mockito:mockito-core:jar:2.23.0:test
\[INFO\] +- javax.measure:jsr-275:jar:0.9.1:test
\[INFO\] \- junit:junit:jar:4.13.1:test
\[INFO\] ------------------------------------------------------------------------
\[INFO\] BUILD SUCCESS
\[INFO\] ------------------------------------------------------------------------
\[INFO\] Total time:  1.805 s
\[INFO\] Finished at: 2021-12-15T17:17:37+01:00
\[INFO\] ------------------------------------------------------------------------

### Options

To skip tests and/or shading use the **-D...** syntax:

`user $``mvn -DskipTests -DskipShade clean package`
To execute specific test/s use the **-DTest...** syntax:

`user $``mvn -DTest=TestClass1#testMethod1 -DTest=TestClass2#testMethod2 ... test`
To enable debug output use the **-X** option.

### java-ebuilder

Maven is used internally by [app-portage/java-ebuilder](https://wiki.gentoo.org/wiki/Java_Developer_Guide#Ebuild_generation) for creation of java package ebuilds.

## Google Summer of Code

Improving the support of Maven was goal of several Google Summer of Code projects:

## Packages waiting for Maven support

New packages for the tree which depend on Maven are organized by the tracker ticket [bug #688542](https://bugs.gentoo.org/show_bug.cgi?id=688542).
