<!-- source: https://wiki.gentoo.org/wiki/Log4j | group: Gentoo Wiki (Main) | wiki-title: Log4j -->
---
title: log4j
url: https://wiki.gentoo.org/wiki/Log4j
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-23"
fingerprint: c6400acbf8c3fd4a
license: CC BY-SA 4.0
---

# log4j

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


Apache Log4j is a Java-based [logging](https://wiki.gentoo.org/wiki/Logging) utility. It consists of several modules the most important which of are already available as packages in Gentoo:


These packages do not yet have [tests enabled](https://bugs.gentoo.org/784263) because of still missing dependencies, e.g. [bug #829070](https://bugs.gentoo.org/show_bug.cgi?id=829070), [bug #833456](https://bugs.gentoo.org/show_bug.cgi?id=833456).  Engaged hackers are welcome to make themselves familiar with the [Gentoo Java Packing Policy](https://wiki.gentoo.org/wiki/Gentoo_Java_Packing_Policy) and [pullrequest](https://wiki.gentoo.org/wiki/GitHub_Pull_Requests) their [contributions](https://wiki.gentoo.org/wiki/Contributing_to_Gentoo).

## Installation

The log4j-\* packages like libraries in general are pulled-in by their [reverse dependencies](https://packages.gentoo.org/packages/dev-java/log4j-12-api/reverse-dependencies) so that there is no need to emerge them directly and recording them in [/var/lib/portage/world](https://wiki.gentoo.org/wiki//var/lib/portage) is a bad idea.

### USE flags

USE flags are the same as for [most java packages](https://wiki.gentoo.org/wiki/Gentoo_Java_USE_flags).


| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [source](https://packages.gentoo.org/useflags/source) | Zip the sources and install them | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 



| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [source](https://packages.gentoo.org/useflags/source) | Zip the sources and install them | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles |
