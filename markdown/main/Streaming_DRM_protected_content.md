<!-- source: https://wiki.gentoo.org/wiki/Streaming_DRM_protected_content | group: Gentoo Wiki (Main) | wiki-title: Streaming DRM protected content -->
---
title: Streaming DRM protected content
url: https://wiki.gentoo.org/wiki/Streaming_DRM_protected_content
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-17"
fingerprint: "865855429d1fb9eb"
license: CC BY-SA 4.0
---

# Streaming DRM protected content

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**D**igital **r**ights **m**anagement (**DRM**) is a system to prevent piracy of copyrighted material. It is not possible to access DRM protected content with only open source software. DRM is highly controversial which does not matter for this article.

## Firefox

The default USE flags for [Firefox](https://wiki.gentoo.org/wiki/Firefox) allow DRM-protected content to be played. When changing the defaults USE flags, both `eme-free` and `gmp-autoupdate` USE flags affect Firefox's ability to play DRM-protected content.

For [www-client/firefox](https://packages.gentoo.org/packages/www-client/firefox), the `eme-free` USE flag must be disabled, otherwise Firefox will be compiled with no DRM capability. And the `gmp-autoupdate` USE flag must be enabled in order to allow Firefox to download the Widevine plugin at runtime, which is needed to play DRM content.

When using the [www-client/firefox-bin](https://packages.gentoo.org/packages/www-client/firefox-bin) package, no `eme-free` USE flag exists and therefore does not need to be disabled. The `gmp-autoupdate` USE flag still needs to be enabled.

Now emerge Firefox:

`root #``emerge --ask www-client/firefox`
For Firefox to download the plugins required to decrypt DRM content, go to: Settings → General → Digital Rights Management (DRM) Content and tick Play DRM-controlled content. When a video player which plays DRM content is opened, Firefox will start downloading the required plugins. This process might take a few minutes. Refresh the page to check if the plugins were successfully downloaded.

## Chromium and Google Chrome

See [bug #547630](https://bugs.gentoo.org/show_bug.cgi?id=547630).

Enable the `widevine` USE flag for [www-client/chromium](https://packages.gentoo.org/packages/www-client/chromium), as well as [www-plugins/chrome-binary-plugins](https://packages.gentoo.org/packages/www-plugins/chrome-binary-plugins) in /etc/portage/package.use. This can be done by adding these lines to the file:

**`/etc/portage/package.use`**

Next emerge the packages:

`root #``emerge --ask www-plugins/chrome-binary-plugins www-client/chromium`
If Google Chrome is used instead of Chromium, replace `www-client/chromium` with `www-client/google-chrome` in /etc/portage/package.use and emerge [www-client/google-chrome](https://packages.gentoo.org/packages/www-client/google-chrome) instead of [www-client/chromium](https://packages.gentoo.org/packages/www-client/chromium):

`root #``emerge --ask www-client/google-chrome`
## Browsers installed with Flatpak

Any browser that is installed through Flatpak and that supports DRM decryption should not require any extra steps related to Gentoo to play DRM content.

## Troubleshooting

### Disney+ not working

There have been multiple incidents where it was not possible to stream anything from Disney+ on Linux because of a new DRM.[\[1\]](https://wiki.gentoo.org#cite_note-1)<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> This issue can be bypassed by switching the browser's user agent so that it imitates Microsoft Windows. There are browser extensions for browsers using Blink (e. g. Chromium) and Gecko (e. g. Firefox) as their browser engine.<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> It is also possible to change the browser's user agent to anything in Firefox and browsers based on Firefox. This is not advised as it can lead to fingerprinting.

### LibreWolf not downloading DRM plugins

For older versions of [LibreWolf](https://wiki.gentoo.org/wiki/LibreWolf) are extra steps required to get the DRM plugins downloading.<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup> A few lines need to be commented out. If LibreWolf was installed using Portage, edit /usr/lib/librewolf/librewolf.cfg. If LibreWolf was installed using Flatpak, edit \~/.local/share/flatpak/app/io.gitlab.librewolf-community/current/active/files/lib/librewolf/librewolf.cfg.

**`/usr/lib/librewolf/librewolf.cfg`**

### Netflix

In Netflix settings navigate to: Your Account → Your Profile → Playback Settings, ensure that "Prefer HTML5 player instead of Silverlight" is checked (it should be by default; just verify).
