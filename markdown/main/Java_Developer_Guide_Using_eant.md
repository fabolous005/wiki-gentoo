<!-- source: https://wiki.gentoo.org/wiki/Java_Developer_Guide/Using_eant | group: Gentoo Wiki (Main) | wiki-title: Java Developer Guide/Using eant -->
---
title: Java Developer Guide/Using eant
url: https://wiki.gentoo.org/wiki/Java_Developer_Guide/Using_eant
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-18"
fingerprint: "2f9b313316f78738"
license: CC BY-SA 4.0
---

# Java Developer Guide/Using eant

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

eant is a Gentoo wrapper around ant. One should never invoke ant directly in an ebuild but instead call `eant`.

## Preparing sources for building with ant

Additionally to what's described in [Java\_Developer\_Guide#Preparing\_sources](https://wiki.gentoo.org/wiki/Java_Developer_Guide#Preparing_sources):

For projects that use ant as build system, any eventually hardcoded `-target` and `-source` attributes must be patched out from any build.xml files which are used.

If a project uses ant as a build system, there can be unwanted targets that are always called. Or targets that will download dependencies outside of portage. Other times there might be unwanted classes or resources on the classpath, or it is missing a classpath that will be set/added to the ebuild.

To avoid unwanted targets you can either remove them entirely from the build.xml file, or you can comment out the targets via XML comments, `<!-- ••• -->`. This can be done via one or more patches and/or sed if minimal. Pay close attention to target dependencies, and dependent targets. At times bypassing one unwanted target can bypass other wanted targets. There are times you might need to change a target's dependencies rather than bypassing a target entirely.

## Compiling with eant

There are several ebuilds in ::gentoo which can be used as examples, see [the build log](https://gitweb.gentoo.org/repo/gentoo.git/log/?qt=grep&q=without+BSFIX).

## Typical examples using eant

**`foo-1.0.ebuild`**

**Example of a Java ant ebuild**

```
# Copyright 1999-2024 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2
EAPI=8
# Tests require an existing running SQL server and 'junit.jar.file' property
JAVA_PKG_IUSE="doc examples source"
inherit java-pkg-2
DESCRIPTION="JDBC drivers with JNDI-bindable DataSources"
HOMEPAGE="https://www.mchange.com/projects/c3p0/"
SRC_URI="https://downloads.sourceforge.net/project/c3p0/c3p0-src/c3p0-${PV}/${P}.src.tgz"
S="${WORKDIR}/${P}.src"
LICENSE="|| ( EPL-1.0 LGPL-2.1 )"
SLOT="0"
KEYWORDS="~amd64 ~ppc64 ~x86 ~amd64-linux ~x86-linux"
CP_DEPEND="
	dev-java/log4j-12-api:2
	dev-java/mchange-commons:0
"
DEPEND="
	>=virtual/jdk-1.8:*
	${CP_DEPEND}
"
RDEPEND="
	>=virtual/jre-1.8:*
	${CP_DEPEND}
"
PATCHES=( "${FILESDIR}/c3p0-0.9.5.5-source-target.patch" )
src_prepare() {
	java-pkg_clean
	default #780585
	java-pkg-2_src_prepare
	java-pkg_jar-from --into lib/ log4j-12-api-2
	java-pkg_jar-from --into lib/ mchange-commons
	# Test sources interfere with Javadoc generation on JDK 11
	# Remove since the tests will never be run
	rm -r src/java/com/mchange/v2/c3p0/test ||
		die "Failed to remove unused test sources"
}
src_compile() {
	eant jar $(usev doc javadoc) \
		-Dant.build.javac.source="$(java-pkg_get-source)" \
		-Dant.build.javac.target="$(java-pkg_get-target)"
}
src_install() {
	java-pkg_newjar "build/${P}.jar"
	einstalldocs
	use doc && java-pkg_dojavadoc build/apidocs
	use examples && java-pkg_doexamples src/java/com/mchange/v2/c3p0/example
	use source && java-pkg_dosrc src/java/com/mchange/v2
}
```
