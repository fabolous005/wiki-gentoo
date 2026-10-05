<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2023/Ideas/portage_machine_readability | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2023/Ideas/portage machine readability -->
---
title: Google Summer of Code/2023/Ideas/portage machine readability
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2023/Ideas/portage_machine_readability
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-08"
fingerprint: "320cb015cb2d8d21"
license: CC BY-SA 4.0
---

# Google Summer of Code/2023/Ideas/portage machine readability

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This project will add structured/machine-readable formats for emerge views.

Various example steps:

- \`emerge -p $PKG\` should show a structured output about the change, package, version, slots, use flags etc.
- Emerge errors should have structured output, making them easier to understand what's wrong, and the boundaries of each error (because many errors seem to flow together if there are multiple errors).
- Structured progress output for other tools to watch build status, esp during parallel package building.



| Contacts | Required Skills | 
|---|---|
| [Robbat2](mailto:robbat2@gentoo.org) |  | 
| Expected Project Size | Expected Outcomes | 
| 350 |  | 
| Project Difficulty |  | 
| hard |  |
