<!-- source: https://wiki.gentoo.org/wiki/Java_Developer_Guide/Using_java-pkg-simple.eclass | group: Gentoo Wiki (Main) | wiki-title: Java Developer Guide/Using java-pkg-simple.eclass -->
---
title: Java Developer Guide/Using java-pkg-simple.eclass
url: https://wiki.gentoo.org/wiki/Java_Developer_Guide/Using_java-pkg-simple.eclass
hostname: gentoo.org
sitename: Java Developer Guide/Using java-pkg-simple.eclass
date: "2026-01-25"
fingerprint: "9fe37a082bfb6bbb"
license: CC BY-SA 4.0
---

# Java Developer Guide/Using java-pkg-simple.eclass

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[java-pkg-simple.eclass](https://devmanual.gentoo.org/eclass-reference/java-pkg-simple.eclass)  is an eclass that provides a way to build Java packages on Gentoo in a simplistic manner. This is package system independent. Most packages can be built using **java-pkg-simple.eclass** global variables in the ebuild. Using such one can omit the entire `src_prepare()`, `src_compile()` and `src_install()` sections in most cases.

## Basic Java ebuild skeleton

Users of [vim](https://wiki.gentoo.org/wiki/Vim) get a basic skeleton automatically as shown below (provided by [app-vim/gentoo-syntax](https://packages.gentoo.org/packages/app-vim/gentoo-syntax)):

`user $``mkdir -p repository/dev-java/foobar && cd $_``user $``vim foobar-1.0.ebuild````
# Copyright 2025 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2
EAPI=8
JAVA_PKG_IUSE="doc source test"
JAVA_TESTING_FRAMEWORKS="junit-4"
inherit java-pkg-2 java-pkg-simple
DESCRIPTION=""
HOMEPAGE=""
SRC_URI=""
S="${WORKDIR}/${P}"
LICENSE=""
SLOT="0"
KEYWORDS="~amd64"
CP_DEPEND=""
DEPEND="${CP_DEPEND}
	>=virtual/jdk-1.8:*"
RDEPEND="${CP_DEPEND}
	>=virtual/jre-1.8:*"
```
## Preparing sources

[java-pkg-simple.eclass](https://devmanual.gentoo.org/eclass-reference/java-pkg-simple.eclass)  uses the normal [`src_unpack()`](https://devmanual.gentoo.org/ebuild-writing/functions/src_unpack/) and [`src_prepare()`](https://devmanual.gentoo.org/ebuild-writing/functions/src_prepare/) ebuild phase functions. Within `src_prepare()`, it is essential to call java-pkg-2\_src\_prepare and, until EAPI 8 in case there is a PATCHES array, also **default**.

Due to java-pkg-simple\_src\_install defining exactly what is getting installed in the [`src_install()`](https://devmanual.gentoo.org/ebuild-writing/functions/src_install/) phase, it is except some very rare cases not needed to remove bundled .jar or .class files. However, running java-pkg\_clean can make packaging easier, not getting confused by stuff not created from the **ebuild**.

## Missing dependencies

If compilation doesn't succeed out of the box then usually one or more dependencies are missing. Grepping the build.log should then give a condensed summary:

`user $``grep 'does not exist' build.log | cut -d':' -f4-7 | sort | uniq`
package com.thoughtworks.xstream does not exist
 package com.thoughtworks.xstream.converters does not exist
 package com.thoughtworks.xstream.io does not exist
 package freemarker.template does not exist
 package org.nibblesec.tools does not exist



## Typical examples using java-pkg-simple.eclass

One of the most common directory structures has src and resources directories like

This would accordingly need

resulting in an ebuild like

**`commons-codec-1.15-r1.ebuild`**

**Example of a Gentoo Java ebuild**

```
# Copyright 1999-2022 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2
EAPI=8
JAVA_PKG_IUSE="doc source test"
JAVA_TESTING_FRAMEWORKS="junit-4"
inherit java-pkg-2 java-pkg-simple
DESCRIPTION="Implementations of common encoders and decoders in Java"
HOMEPAGE="https://commons.apache.org/proper/commons-codec/"
SRC_URI="mirror://apache/commons/codec/source/${P}-src.tar.gz -> ${P}.tar.gz"
S="${WORKDIR}/${P}-src"
LICENSE="Apache-2.0"
SLOT="0"
KEYWORDS="~amd64 ~arm ~arm64 ~ppc64 ~x86 ~amd64-linux ~x86-linux"
DEPEND="
	>=virtual/jdk-1.8:*
	test? (
		>=dev-java/commons-lang-3.11:3.6
	)
"
RDEPEND=">=virtual/jre-1.8:*"
JAVA_AUTOMATIC_MODULE_NAME="org.apache.commons.codec"
JAVA_RESOURCE_DIRS="src/main/resources"
JAVA_SRC_DIR="src/main/java"
JAVA_TEST_GENTOO_CLASSPATH="junit-4,commons-lang-3.6"
JAVA_TEST_RESOURCE_DIRS="src/test/resources"
JAVA_TEST_SRC_DIR="src/test/java"
```
## Tests

Tests can run only when **JAVA\_PKG\_IUSE** has the [test](https://wiki.gentoo.org/wiki/Java_Developer_Guide#Java_specific_USE_flags) flag and **JAVA\_TESTING\_FRAMEWORKS** is filled.

### Conditional test exclusions - JAVA\_TEST\_EXCLUDES

Tests can be excluded using the **JAVA\_TEST\_EXCLUDES** eclass variable. In cases where exclusions should apply only to certain Java versions it can be done using the ver\_test function as shown in the following example.  Disadvantage: Loosing tests if the excluded test class has more only one test.

### Conditional test exclusions - junit assume

A more sophisticated way to conditionally skip single tests is using [junit assume](https://github.com/gentoo/gentoo/pull/28334#issuecomment-1835816459) as is demonstrated in [commit #eabdf0892f](https://gitweb.gentoo.org/repo/gentoo.git/commit/?id=eabdf0892f)

## External tools

While in many if not most cases build.xml, [pom.xml](https://wiki.gentoo.org/wiki/Maven), [build.gradle](https://wiki.gentoo.org/wiki/Gradle) can be easily translated into [ebuild](https://devmanual.gentoo.org/ebuild-writing/) it could get more chellenging when it comes to code generation be it java code, parsers or other code. There are already examples in the [main ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) of how it can be done solely in the ebuild without using [external tools](https://wiki.gentoo.org/wiki/Build_automation) or even writing scripts for them.
