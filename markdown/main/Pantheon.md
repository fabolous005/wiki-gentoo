<!-- source: https://wiki.gentoo.org/wiki/Pantheon | group: Gentoo Wiki (Main) | wiki-title: Pantheon -->
---
title: Pantheon
url: https://wiki.gentoo.org/wiki/Pantheon
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-21"
fingerprint: cf49d23f86af9386
license: CC BY-SA 4.0
---

# Pantheon

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Pantheon** is a lightweight, modular desktop environment primarily written in Vala and GTK. It was originally written for use with Elementary OS, but is in the process of being ported to Gentoo.

## Prerequisites

Users should be aware that the *elementary* [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) is undergoing rapid development, thus the following instructions are subject to significant changes.

### elementary ebuild repository (overlay)

Pantheon currently resides in the elementary ebuild repository. There are two methods for installing repositories in Gentoo:

1. The eselect repository method.
2. The manual repos.conf method.

#### Method 1. eselect repostiory

Ensure [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository) is installed. See [Eselect/Repository](https://wiki.gentoo.org/wiki/Eselect/Repository) for full details.

`root #``emerge --ask app-eselect/eselect-repostitory`
eselect repository will create an automatic repos.conf entry. When ready, run:

`root #````
eselect repository enable elementary
```
`root #````
emaint sync --repo elementary
```
#### Method 2. repos.conf

Create a file in the [/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf) directory (create the repos.conf directory first if it does not exist) called elementary.conf. Fill the file's contents with the following code:

**`/etc/portage/repos.conf/elementary.conf`**

```
[elementary]
location = /var/db/repos/elementary
sync-type = git
sync-uri = https://github.com/pimvullers/elementary.git
auto-sync = yes
```
Pull in the new repo:

`root #``emaint sync --repo elementary`
### Profile

Profile changes can be non trivial. Please ensure the profile version, like 23.0 below, does not change without following the corresponding [news items](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Reading_news_items). Avoid, for example, changing from an [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) profile to a [systemd](https://wiki.gentoo.org/wiki/Systemd) profile, or vice versa, without consulting the appropriate documentation.

Setting a GNOME desktop profile for the system will help prepare the base system for pantheon to be installed:

#### OpenRC

When using OpenRC, select:

`root #``eselect profile set default/linux/amd64/23.0/desktop/gnome`
#### systemd

systemd users will need the GNOME profile with systemd on the end:

`root #``eselect profile set default/linux/amd64/23.0/desktop/gnome/systemd`
## Installation

### Emerge

The pantheon meta-package pulls in the necessary packages. This can be done by emerging just the pantheon-base/pantheon-shell meta package and then select the desired elementary applications:

`root #``emerge --ask pantheon-base/pantheon-shell`
Then, for example, emerge Audience and Pantheon terminal:

`root #``emerge --ask media-video/audience x11-terms/pantheon-terminal`
### package.use

Ensure that `X` and `-gnome`USE flag is included in the system's global [USE flags](https://wiki.gentoo.org/wiki/USE_flag). It is also possible to remove the requirement of webkit-gtk by setting `gnome-online-accounts`

### Keywording

Those that wish to test the latest version of Pantheon then add the \~arch (unstable) keyword in order to allow the installation of the latest version of Pantheon package atoms. For now, only the **\~amd64** and **\~x86** keywords are supported (although in the future and with more testing other architectures may be supported, arch testers are always welcomed to help with this.)

It is wise to provide Portage with only the explicit instructions Patheon needs for (dependent) packages that require keywording. Copy the following list in the package.accept\_keywords file:

**`/etc/portage/package.accept_keywords`**

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose pantheon-base/pantheon-shell`
## Troubleshooting

### Reporting issues

As mentioned in the beginning, the elementary repository is undergoing rapid development, just like the upstream elementary OS project. This means that things might break, or do not work properly yet. If you discover any issues, or if you want to contribute, just create a [new issue](https://github.com/pimvullers/elementary/issues/new) on the [elementary ebuild repository](https://github.com/pimvullers/elementary) Github project or contact the [maintainer](mailto:elementary@vullersmail.nl).
