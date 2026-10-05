<!-- source: https://wiki.gentoo.org/wiki/LDP_DocBook | group: Gentoo Wiki (Main) | wiki-title: LDP DocBook -->
---
title: LDP DocBook
url: https://wiki.gentoo.org/wiki/LDP_DocBook
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: "5245bb18d85705a7"
license: CC BY-SA 4.0
---

# LDP DocBook

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Linux Documentation Project utilizes DocBook (ie. SGML) for their HowTo's.

There's already documentation on how to use the DocBook tools for creating the SGML files. There's one problem, a default install of Gentoo DocBook tools do not install a few required packages. Without installing these packages, users will be presented with many errors during conversion. Additionally, most on the LDP mailing list might not understand these errors as they're using other Linux Distributions, Distributions likely having meta packages for automatically setting up their environment.

An example initialerror, followed by 200+ stylesheet errors.

## Install Required Additional Packages

`root #``emerge -pv app-text/docbook-sgml-utils`
This should also pull in these additional packages. If for some reason after installing docbook-sgml-utils you still do not have a functioning LDP DocBook utilities or incanatations, ensure the following packages currently pulled in are not being omitted:

## Verify Utilities for LDP Documentation

You should now be able to perform something of the following, with very few to no errors displayed within the console.

Make sure you have the ldp.dsl and any example programs included within the documentation extracted within the current folder!

This will convert SGML (ie. DocBook) to HTML

`user $``openjade -t html -d ./ldp.dsl\#html NCURSES-Programming-HOWTO.sgml`
This will convert SGML (ie. DocBook) to XML

`user $``osx NCURSES-Programming-HOWTO.sgml > NCURSES-Programming-HOWTO.xml`
## Importing Tex files into Lyx

The following might be necessary to enable importing Tex into Lyx.

`root #``USE="jadetex" emerge -q app-text/sgmltools-lite`
