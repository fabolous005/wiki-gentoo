<!-- source: https://wiki.gentoo.org/wiki/Web_eID | group: Gentoo Wiki (Main) | wiki-title: Web eID -->
---
title: Web eID
url: https://wiki.gentoo.org/wiki/Web_eID
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-17"
fingerprint: "47b5b6584de2f19a"
license: CC BY-SA 4.0
---

# Web eID

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **Web eID** is a suite of [browser extension](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions), [native application](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Native_messaging), and JavaScript library that provides a way to perform cryptographic operations (authentication, signing) using smart cards on the Web. One of the purposes of the project is to replace the legacy architecture of the [Open eID](https://github.com/open-eid) project <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

## Installation

### Overlay

Gentoo is not officially supported by the Web eID project <sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>, and there are no packages in the official Gentoo repository. However, there is an [official community-driven overlay](https://github.com/open-eid/gentoo) in the Open eID project. To enable the overlay, first it is necessary to install [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git) and [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository):

`root #``emerge --ask dev-vcs/git app-eselect/eselect-repository`
The overlay can then be enabled as follows:

And the Gentoo ebuild repository needs to updated:

`root #``emerge --sync`
As all packages in the overlay are masked with the **amd64** keyword, they need to be unmasked (see [/etc/portage/package.accept\_keywords](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords) for more information):

**`/etc/portage/package.accept_keywords`**

### Package

#### Web eID

To install the Web eID package, run the following command:

`root #``emerge --ask www-plugins/web-eid`
#### DigiDoc4

##### Portage package

As of 2024-05-07, [dev-cpp/libcutl](https://github.com/open-eid/gentoo/blob/master/dev-cpp/libcutl) incorrectly defines dependencies, so the dependency needs to be installed manually:

`root #``emerge --ask dev-libs/boost`
As of 2024-05-07, [dev-libs/libdigidocpp](https://github.com/open-eid/gentoo/tree/master/dev-libs/libdigidocpp) requires the following patch on a musl-based system (the patch will force the library to compile, but it will still crash at runtime):

**`/etc/portage/patches/dev-libs/libdigidocpp-3.16.0/ctime.patch`**

To install DigiDoc4, run the following command:

`root #``emerge --ask app-crypt/qdigidoc4`
##### Flatpak package

Install DigiDoc4 using [Flatpak](https://wiki.gentoo.org/wiki/Flatpak):

`user $``flatpak install --user flathub ee.ria.qdigidoc4`
To run DigiDoc4 use the following command:

`user $``flatpak run --user ee.ria.qdigidoc4`
### Smart card reader driver

Follow the instructions provided [here](https://wiki.gentoo.org/wiki/Electronic_identification#Smart_card_reader_driver).

### Testing

The [official website](https://web-eid.eu/) has a button to test authentication and signing.

## See also

- [Electronic identification](https://wiki.gentoo.org/wiki/Electronic_identification) — the core part of e-government implementation, providing a way to identify citizens and organizations.
