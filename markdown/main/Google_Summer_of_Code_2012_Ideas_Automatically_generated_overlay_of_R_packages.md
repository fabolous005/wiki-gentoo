<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Automatically_generated_overlay_of_R_packages | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2012/Ideas/Automatically generated overlay of R packages -->
---
title: Google Summer of Code/2012/Ideas/Automatically generated overlay of R packages
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Automatically_generated_overlay_of_R_packages
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-04-02"
fingerprint: "9131704f42edf7ee"
license: CC BY-SA 4.0
---

# Google Summer of Code/2012/Ideas/Automatically generated overlay of R packages

[Google Summer of Code](https://wiki.gentoo.org/wiki/Google_Summer_of_Code) |

[2012](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012) |

[Ideas](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## [Automatically generated overlay of R packages]

The R scientific language has a vast ecosystem of about 5000 packages and 40000 individual versions. It was attempted in the past to make Portage manage these packages directly. The last project was somewhat successful but suffered from some drawbacks. Some of them, like the lack of manifests, are a result of the particular kind of implementation which was chosen.

This project is a completely different implementation which consists in a set of scripts which pull all the necessary information from CRAN, BIOC, etc... mirrors and create a central overlay. This overlay will be hosted on Gentoo infrastructure and can be added using Layman by Gentoo users. As far as the regular user is concerned this overlay is no different from other overlays where ebuilds are written manually. The generated packages and eclasses must be compatible with all current package managers used in Gentoo.

In order for this project to be sucessful it will need to not only provide the system which generates the overlay, but also solve issues like fixing errors in R package metadata (quite common), R and system dependency verification, distfiles mirroring, manifest generation, automatic incremental overlay updates, proper logging, and a few others. The end result of this project must be something that the Gentoo infrastructure team will agree to install on Gentoo infrastructure. Discussion with them will be necessary way before the completion of the project to confirm it is going into the right direction. The demonstration of the system actually working will need to be done on GSoC dedicated hardware. Proper documentation of what the system does and how will be considered crucial.

It is not necessary to have strong R skills for this project. The little R knowledge which is actually needed can be picked up in a few hours of reading.

It is recommended you talk to one of the contacts below before you make an official application.



| Contacts | Required Skills | 
|---|---|
|  |  | 

#### Mailing List Archives

 [Automatically generated overlay of R packages - Mailing List Archives](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Automatically_generated_overlay_of_R_packages/MailingListArchives)
