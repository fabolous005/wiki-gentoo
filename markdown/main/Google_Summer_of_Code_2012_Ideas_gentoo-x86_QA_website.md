<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/gentoo-x86_QA_website | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2012/Ideas/gentoo-x86 QA website -->
---
title: Google Summer of Code/2012/Ideas/gentoo-x86 QA website
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/gentoo-x86_QA_website
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: d142b2b549f517ec
license: CC BY-SA 4.0
---

# Google Summer of Code/2012/Ideas/gentoo-x86 QA website

[Google Summer of Code](https://wiki.gentoo.org/wiki/Google_Summer_of_Code) |

[2012](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012) |

[Ideas](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## [gentoo-x86 QA website]

The idea is simple enough, take the QA results from various tools and present it via a searchable website. Think [packages.gentoo.org](http://packages.gentoo.org), just for QA results. The implementation work required would primarily be building the website itself- the user could rely upon pkgcore-checks for the initial data stream (it can output it's results as a pickle stream) leaving the candidate to focus on generating a site providing insight into the status of current architectures, current stabling, etc.

One additional constraint would be that the underlying DB schema should be written in a fashion that allows multiple data imports to be used- while pkgcore-checks right now can provide data for a candidate to work with, the candidate should be designing a system also able to pull in other data sources (at some point repoman for example).

Finally, an additional feature could be designing the underlying schema and website to allow for the possibility of being able to handle multiple repositories- think about if the GNOME herd wanted their overlay to be scanned/accessible. This complicates the design a bit (specifically keeping it fast), but is likely to be desired functionality down the line.

The relevant gentoo-soc discussion (with a bit more details) is accessible on [in the gentoo-soc archives](http://archives.gentoo.org/gentoo-soc/msg_474d9ffb2fde05a9fb8ff032cac58622.xml).



| Contacts | Required Skills | 
|---|---|
|  |  |
