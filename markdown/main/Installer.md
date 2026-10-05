<!-- source: https://wiki.gentoo.org/wiki/Installer | group: Gentoo Wiki (Main) | wiki-title: Installer -->
---
title: Installer
url: https://wiki.gentoo.org/wiki/Installer
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-11-27"
fingerprint: cc30d5fb5d97ec10
license: CC BY-SA 4.0
---

# Installer

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**



installer is designed to aid users during the installation process of Gentoo Linux. It is capable of walking a beginner user through the regular installation process following key topics from the Gentoo Handbook. It is ChrisADR's personal project, the official installer project is in charge of designing the official Gentoo installer.

## Installation

installer is designed to be part of the Gentoo Live CD image, but it can also be installed via Portage.

### Emerge

`root #``emerge --ask app-admin/installer`
## Usage

Once inside the live environment, it is ready to begin an installation process from scratch via:

`root #``installer beginner`
You can also resume in a specific point of installation with:

`root #``installer beginner -s N` `root #``installer beginner --step N`
Where N is a number between 1 and 11.

or

`root #``installer beginner -t` `root #``installer beginner --tui` installer can also be used by root to generate stageX tarball, which contains some configuration files that may help as backup configuration or in further installations, via:

`root #``installer generate`
## Removal

Since installer will be only available by default in the live CD image, the only reason to unmerge installer is if you already installed it with Portage.

### Unmerge

`root #``emerge --ask --depclean --verbose app-admin/installer`
## See also

- [Project:Installer](https://wiki.gentoo.org/wiki/Project:Installer) — an attempt to increase the usability of Gentoo and related Linux distributions by providing an interface for users to backup and recover existing installations.  Official Installer Project for Gentoo Linux

## External resources

- [ChrisADR's guide of installer](https://blogs.gentoo.org/chrisadr/2018/05/02/installer-a-basic-gentoo-system-anyone-can-install/) - a Blog post which describes installer functionalities.
