<!-- source: https://wiki.gentoo.org/wiki/Qutebrowser | group: Gentoo Wiki (Main) | wiki-title: Qutebrowser -->
---
title: qutebrowser
url: https://wiki.gentoo.org/wiki/Qutebrowser
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-20"
fingerprint: fa01437a8d8f39dc
license: CC BY-SA 4.0
---

# qutebrowser

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**qutebrowser** is a web browser with [vim-style key bindings](https://qutebrowser.org/img/cheatsheet-big.png) based off QtWebKit (or QtWebEngine in its latest release). It is lightweight using a minimal GUI and is inspired by software such as [Vimperator](http://www.vimperator.org/) and [dwb](https://portix.bitbucket.io/dwb/). It uses [DuckDuckGo](https://duckduckgo.com/) as the default search engine.

qutebrowser was developed by Freya Bruhin, for which she received a CH Open Source award in 2016.

## Installation

### USE flags


| [+adblock](https://packages.gentoo.org/useflags/+adblock) | Enable Brave's ABP-style adblocker library for improved adblocking | 
| [pdf](https://packages.gentoo.org/useflags/pdf) | Add general support for PDF (Portable Document Format), this replaces the pdflib and cpdflib flags | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [widevine](https://packages.gentoo.org/useflags/widevine) | Unsupported closed-source DRM capability (required by Netflix VOD) | 

Turn off the `bindist` flag to enable support for proprietary codecs on WebEngine.

### Emerge

`root #``emerge --ask www-client/qutebrowser`
## Troubleshooting

### Proprietary Codecs

To get proprietary codecs to work, turn off the [bindist](https://packages.gentoo.org/useflags/bindist) [USE flag](https://wiki.gentoo.org/wiki/USE_flag) for [dev-qt/qtwebengine](https://packages.gentoo.org/packages/dev-qt/qtwebengine) in [/etc/portage/package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use) - see [Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/USE#Declaring_USE_flags_for_individual_packages).

Once the use flag is properly set in the file, re-emerge qtwebengine. Remember to use the `--oneshot` parameter so as to not add the dependency to the [world file](<https://wiki.gentoo.org/wiki/Selected_set_(Portage)>):

`root #``emerge --ask --oneshot dev-qt/qtwebengine``root #``emerge --ask www-client/qutebrowser`
### Video or sound not working well

`root #``emerge --ask --verbose media-libs/gst-plugins-{base,good,bad,ugly} media-plugins/gst-plugins-libav`
### No sound with Pipewire

To hear sound when Pipewire is used, turn on `pipewire-alsa` for [media-video/pipewire](https://packages.gentoo.org/packages/media-video/pipewire) in [/etc/portage/package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use).

After the USE flag is set properly, re-emerge Pipewire:

`root #``emerge --ask media-video/pipewire`
### Kerberos authentication does not work

To let the browser know which domains are allowed to authenticate with Kerberos, make sure you have the following config in place. The example shows allowing it to work when visiting subdomains of `fedoraproject.org` and `work.corp.com`:

**`~/.config/qutebrowser/config.py`**

Sometimes, this is not enough, however. The underlying feature is implemented in a transitive dependency of [www-client/qutebrowser](https://packages.gentoo.org/packages/www-client/qutebrowser) — [dev-qt/qtwebengine](https://packages.gentoo.org/packages/dev-qt/qtwebengine). If it's not compiled with [kerberos](https://packages.gentoo.org/useflags/kerberos) [support, the above configuration will not have any effect.](https://wiki.gentoo.org/wiki/USE_flag)

Usually, it's not desired to enable the [kerberos](https://packages.gentoo.org/useflags/kerberos) [USE flag just for a single package and most of the time one would want to have it enabled globally. It can be done with a command like:](https://wiki.gentoo.org/wiki/USE_flag)

`user $``euse -E kerberos`
/etc/portage/make.conf was modified, a backup copy has been placed at /etc/portage/make.conf.euse\_backup

Alternatively, create the following file by hand:

Finally, once the use flag is properly set in the file, re-emerge [dev-qt/qtwebengine](https://packages.gentoo.org/packages/dev-qt/qtwebengine). Remember to use the `--oneshot` parameter so as to not add the dependency to the [world file](<https://wiki.gentoo.org/wiki/Selected_set_(Portage)>):

`root #``emerge --ask --oneshot dev-qt/qtwebengine`
