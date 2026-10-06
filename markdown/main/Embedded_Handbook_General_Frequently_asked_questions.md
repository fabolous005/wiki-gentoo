<!-- source: https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Frequently_asked_questions | group: Gentoo Wiki (Main) | wiki-title: Embedded Handbook/General/Frequently asked questions -->
---
title: Embedded Handbook/General/Frequently asked questions
url: https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Frequently_asked_questions
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-24"
fingerprint: "5eca990387279dd4"
license: CC BY-SA 4.0
---

# Embedded Handbook/General/Frequently asked questions

[Embedded Handbook](https://wiki.gentoo.org/wiki/Embedded_Handbook) |

[General](https://wiki.gentoo.org/wiki/Embedded_Handbook/General)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Frequently asked questions for Gentoo Embedded.

### I get "configure: error: C compiler cannot create executables"

This is a generic error and can be caused by just about anything. The test is pretty simple: can the requested compiler create an executable? However, this relies on many things being correct: the toolchain itself being completely sane, the compiler and compiler flags being appropriate, your environment set up properly, etc... The only way to find out the real source of the problem is to open up the generated config.log file and scroll down to where this test is run and see what exactly the error message is that the toolchain is spitting out.

### "epatch" always fails in newly compiled system

The bash package does not properly cross-compile and mixes the host signal definitions with those of the target. This manifests itself differently depending on the combination of host architecture and target architecture. To resolve the issue, simply re-compile bash natively. "But bash uses epatch!" you exclaim. In that case, you will need to modify the ebuild and comment out all the calls to epatch. Once you've installed the fixed bash this way, uncomment all of the bash lines and rebuild it again.

### uClibc build segfaults/crashes while building locale

The uClibc locale support is pretty experimental at this point. Unless you really need support for it (and you're willing to help bang on the problem), simply disable support by adding `-nls -iconv -pregen -userlocales` values to the `USE` flags when building uClibc.
