<!-- source: https://wiki.gentoo.org/wiki/Java-ebuilder | group: Gentoo Wiki (Main) | wiki-title: Java-ebuilder -->
---
title: java-ebuilder
url: https://wiki.gentoo.org/wiki/Java-ebuilder
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-27"
fingerprint: "97719c4ba6b418a9"
license: CC BY-SA 4.0
---

# java-ebuilder

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


java-ebuilder is an experimental package being developed by Gentoo Java developers to generate initial ebuilds from [Maven](https://wiki.gentoo.org/wiki/Maven) `pom.xml` files. In its simplest form, it takes `pom.xml` file and several other parameters and creates an ebuild that should be checked and tested before committed. There are also add-on scripts that will generate ebuilds for any dependencies that are not packaged yet. Currently this application generates some features that are not supported by [java-pkg-simple.eclass](https://devmanual.gentoo.org/eclass-reference/java-pkg-simple.eclass)  yet.

## Usage

After a package source is downloaded and unpacked

`user $``tar xzf apache-log4j-2.15.0-src.tar.gz`
Step into its directory and find the pom files

`user $````
cd apache-log4j-2.15.0-src
```
`user $``find . -name pom.xml`
Refresh (create) java-ebuilder's java ebuild cache

`user $``java-ebuilder -c -t /var/db/repos/gentoo/`
Then let it generate the ebuild, say from the log4j-api-java9 subdirectory

`user $````
java-ebuilder --generate-ebuild --workdir . \
```
`user $````
--pom log4j-api-java9/pom.xml \
```
`user $````
--download-uri mirror://apache/logging/log4j/2.15.0/apache-log4j-2.15.0-src.tar.gz \
```
`user $````
--slot 2 \
```
`user $````
--keywords "~amd64 ~arm ~arm64 ~ppc64 ~x86" \
```
`user $``--ebuild log4j-api-java9-2.15.0.ebuild`
