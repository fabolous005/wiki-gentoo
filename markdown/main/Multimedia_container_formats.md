<!-- source: https://wiki.gentoo.org/wiki/Multimedia_container_formats | group: Gentoo Wiki (Main) | wiki-title: Multimedia container formats -->
---
title: Multimedia container formats
url: https://wiki.gentoo.org/wiki/Multimedia_container_formats
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-12"
fingerprint: "4103c37e0b972b09"
license: CC BY-SA 4.0
---

# Multimedia container formats

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

For a computer novice or any person unfamiliar with the technical details of multimedia production, transmission, and *even* consumption, understanding how to view certain media can be quite a challenge. This article adds clarity and insight for end-users (generally on desktop profiles) on how enable Gentoo to display multimedia.

## Introduction

Under the surface, multimedia (video, audio, or both) is generally shipped in a [container format](<https://en.wikipedia.org/wiki/Container_format_(computing)>). These container formats have various names and can hold various multimedia content (codecs).

The three most relevant multimedia formats on Gentoo are (links to Wikipedia for each entry):

For example, in 2017 Apple started using the [High Efficiency Image File Format (HEIF)](https://en.wikipedia.org/wiki/High_Efficiency_Image_File_Format) to store  photos (digital images) that were saved using a .heic file extension. This same format can also be enabled for photo storage on Android. Without the proper USE flags enabled and supporting libraries (packages) installed, viewing HEIF coded files (.heic files) will not be possible on Gentoo.

## Configuration

Depending on the container format desired, a search can be performed in order to determine what packages and USE flags correspond to the container format and the codecs.

### Discovery

For example, supposing a system administrator desired to enable HEIF support so that photos taken on an iPhone or Android device could be viewed while using the [GNOME](https://wiki.gentoo.org/wiki/GNOME) desktop environment. The system administrator could [use packages.gentoo.org to perform a heif USE flag search](https://packages.gentoo.org/useflags/search?q=heif) to see if any `heif` or `heic` USE flags are available for any packages.

Alternatively a system administrator could also search the local system using the quse command (included with [app-portage/portage-utils](https://packages.gentoo.org/packages/app-portage/portage-utils)):

`user $``euse heif`
Ah! A search on both packages.g.o and the local system revealed that an `heif` USE flag is supported by some packages, and the `heic` USE flag does not exist.

To ease support, the `heif` USE flag can be added for all installed packages (globally) via a make.conf addition:

**`/etc/portage/make.conf`**

```
USE="heif"
```
In order for the package manager to recognize this change for already installed packages, a [world set](<https://wiki.gentoo.org/wiki/World_set_(Portage)>) rebuild is necessary:

`root #``emerge --update --deep --changed-use @world`
## See also

- [HEIF](https://wiki.gentoo.org/wiki/HEIF) — provides configuration information to add HEIF container format support to [desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment) supporting its integration.
