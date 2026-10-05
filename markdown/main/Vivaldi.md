<!-- source: https://wiki.gentoo.org/wiki/Vivaldi | group: Gentoo Wiki (Main) | wiki-title: Vivaldi -->
---
title: Vivaldi
url: https://wiki.gentoo.org/wiki/Vivaldi
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-10"
fingerprint: cf5096569727b9ec
license: CC BY-SA 4.0
---

# Vivaldi

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Vivaldi** is a browser for our friends.

## Installation

### USE flags


| [ffmpeg-chromium](https://packages.gentoo.org/useflags/ffmpeg-chromium) | Use Chromium FFmpeg fork (media-video/ffmpeg-chromium) rather than mainline FFmpeg (media-video/ffmpeg) | 
| [gtk](https://packages.gentoo.org/useflags/gtk) | Add support for x11-libs/gtk+ (The GIMP Toolkit) | 
| [proprietary-codecs](https://packages.gentoo.org/useflags/proprietary-codecs) | Use system FFmpeg library to support patent-encumbered media codecs | 
| [qt6](https://packages.gentoo.org/useflags/qt6) | Add support for the Qt 6 application and UI framework | 
| [widevine](https://packages.gentoo.org/useflags/widevine) | Unsupported closed-source DRM capability (required by Netflix VOD) | 

### Emerge

Vivaldi is distributed as only pre-built binaries. To install:

`root #``emerge --ask www-client/vivaldi`
Alternatively, there is Vivaldi-Snapshot, the work-in-progress build of the browser. Vivaldi-Snapshot is distributed as only pre-built binaries. To install:

`root #``emerge --ask www-client/vivaldi-snapshot`
## Types

Currently there are 2 packages for Vivaldi: [www-client/vivaldi](https://packages.gentoo.org/packages/www-client/vivaldi) and [www-client/vivaldi-snapshot](https://packages.gentoo.org/packages/www-client/vivaldi-snapshot)

vivaldi-snapshot is a preview version of Vivaldi

## Troubleshooting

### File selection dialog does not appear

Some [DEs](https://wiki.gentoo.org/wiki/Desktop_environment)/[WMs](https://wiki.gentoo.org/wiki/Window_manager) don't provide an [xdg-desktop-portal](https://wiki.gentoo.org/wiki/Xdg-desktop-portal) by default ([bug #934819](https://bugs.gentoo.org/show_bug.cgi?id=934819)). This can be solved by running:

`root #``emerge --ask sys-apps/xdg-desktop-portal-gtk`
### DRM-protected content not playing

Ensure the `proprietary-codecs` and `widevine` USE flags are enabled for Vivaldi, and that the `vivaldi:components` page lists "Widevine Content Decryption Module" and is up-to-date. To enable Widevine, in Settings, search for `widevine`, which should bring up the "Plugins" section and the "Enable Widevine Plugin" checkbox.

## See also

- [Chrome](https://wiki.gentoo.org/wiki/Chrome) — Google's proprietary (closed source) web browser.
- [Chromium](https://wiki.gentoo.org/wiki/Chromium) — the open source browser that [Google Chrome](https://wiki.gentoo.org/wiki/Google_Chrome) and many other browsers are based on.
- [Firefox](https://wiki.gentoo.org/wiki/Firefox) — [open source](https://en.wikipedia.org/wiki/Open_source), [multiplatform](https://en.wikipedia.org/wiki/Cross-platform_software), [web browser](https://wiki.gentoo.org/wiki/Recommended_applications#Web_browsers) developed by [Mozilla](https://en.wikipedia.org/wiki/Mozilla).
- [Recommended applications](https://wiki.gentoo.org/wiki/Recommended_applications) — applications recommended for use in a graphical environment ([X11](https://wiki.gentoo.org/wiki/Xorg), [Wayland](https://wiki.gentoo.org/wiki/Wayland)) - [web browser section](https://wiki.gentoo.org/wiki/Recommended_applications#Web_browsers).
