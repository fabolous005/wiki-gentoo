<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2016/Ideas/Gentoo-GPG | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2016/Ideas/Gentoo-GPG -->
---
title: Google Summer of Code/2016/Ideas/Gentoo-GPG
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2016/Ideas/Gentoo-GPG
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: "3784f06a6222f778"
license: CC BY-SA 4.0
---

# Google Summer of Code/2016/Ideas/Gentoo-GPG

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gentoo-gpg is to continue the work done in a previous years GSOC project "gentoo-keys".

The gentoo-keys project has a system for managing and distributing the Gentoo developers and release OpenPGP keys. This project will encompass several sub tasks.

1. Work with the infrastructure team for the completion and generation of needed scripts to utilize the gentoo-keys system. This will include automated scripts to update the gpg keys seeds and keyrings. Generate email reminders for keys nearing expiry. Generate automatic update scripts for user systems. Extend the various gentoo-keys library code and command line interfaces as needed.
2. Code a new Meta-Manifest system for the Gentoo ebuild tree.  See [GLEP 58](https://www.gentoo.org/glep/glep-0058.html) for details
3. Integrate gkeys library code into Gentoo's package management system, portage (could also include pkgcore). This includes meta-manifest handling changes as well their gpg verification. This can also include verification of git based ebuild tree commits.
4. Extending gkeys and gkeys-gen features.



| Contacts | Required Skills | 
|---|---|
|  |  |
