<!-- source: https://wiki.gentoo.org/wiki/Surf | group: Gentoo Wiki (Main) | wiki-title: Surf -->
---
title: surf
url: https://wiki.gentoo.org/wiki/Surf
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-03-22"
fingerprint: c6d6864b80b3da78
license: CC BY-SA 4.0
---

# surf

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


surf is a simple web browser based on WebKit/GTK. It is able to display websites and follow links. It supports the XEmbed protocol which makes it possible to embed it in another application. Furthermore, one can point surf to another URI by setting its XProperties.

## Installation

### USE flags


Preferably, you'll want to enable [savedconfig](https://wiki.gentoo.org/wiki/Savedconfig)

`root #``euse --enable savedconfig`
### Emerge

Install [www-client/surf](https://packages.gentoo.org/packages/www-client/surf):

`root #``emerge --ask www-client/surf`
## Configuration

As stated previously, the main surf configuration file is the /etc/portage/savedconfig/www-client/surf-2.0 file and after each change, surf needs to be recompiled for any changes to take effect.

### Patches and additional features

There are many user-created [patches](http://surf.suckless.org/patches/) available from the official site that greatly extend the functionality of surf.

### Tabbed browsing

The [x11-misc/tabbed](https://packages.gentoo.org/packages/x11-misc/tabbed) program can be used with surf for a simple tabbed browsing experience.

A basic set-up:

`user $``tabbed surf -e`
To achieve a similar behavior to Firefox or Chromium where upon closing the last tab, the browser exits, use:

`user $``tabbed -c surf -e`
