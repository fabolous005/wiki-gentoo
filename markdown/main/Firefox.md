<!-- source: https://wiki.gentoo.org/wiki/Firefox | group: Gentoo Wiki (Main) | wiki-title: Firefox -->
---
title: Firefox
url: https://wiki.gentoo.org/wiki/Firefox
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-01"
fingerprint: ee5cd0553883eb67
license: CC BY-SA 4.0
---

# Firefox

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Firefox** is an [open source](https://en.wikipedia.org/wiki/Open_source), [multiplatform](https://en.wikipedia.org/wiki/Cross-platform_software), [web browser](https://wiki.gentoo.org/wiki/Recommended_applications#Web_browsers) developed by [Mozilla](https://en.wikipedia.org/wiki/Mozilla).

Firefox has decades-old roots in [Netscape](https://en.wikipedia.org/wiki/Firefox#History) and serves as a [foundation for other projects](https://wiki.gentoo.org/wiki/Firefox#Firefox_forked_projects), such as [GNU Icecat](https://wiki.gentoo.org/wiki/GNU_Icecat) or [LibreWolf](https://wiki.gentoo.org/wiki/LibreWolf).

## Installation

### USE flags

#### www-client/firefox


| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [+clang](https://packages.gentoo.org/useflags/+clang) | Use Clang compiler instead of GCC | 
| [+gmp-autoupdate](https://packages.gentoo.org/useflags/+gmp-autoupdate) | Allow Gecko Media Plugins (binary blobs) to be automatically downloaded and kept up-to-date in user profiles | 
| [+jumbo-build](https://packages.gentoo.org/useflags/+jumbo-build) | Enable unified build - combines source files to speed up build process, but requires more memory | 
| [+system-av1](https://packages.gentoo.org/useflags/+system-av1) | Use the system-wide media-libs/dav1d and media-libs/libaom library instead of bundled | 
| [+system-harfbuzz](https://packages.gentoo.org/useflags/+system-harfbuzz) | Use the system-wide media-libs/harfbuzz instead of bundled and media-gfx/graphite2 in most cases | 
| [+system-icu](https://packages.gentoo.org/useflags/+system-icu) | Use the system-wide dev-libs/icu instead of bundled | 
| [+system-jpeg](https://packages.gentoo.org/useflags/+system-jpeg) | Use the system-wide media-libs/libjpeg-turbo instead of bundled | 
| [+system-libevent](https://packages.gentoo.org/useflags/+system-libevent) | Use the system-wide dev-libs/libevent instead of bundled | 
| [+system-libvpx](https://packages.gentoo.org/useflags/+system-libvpx) | Use the system-wide media-libs/libvpx instead of bundled | 
| [+system-webp](https://packages.gentoo.org/useflags/+system-webp) | Use the system-wide media-libs/libwebp instead of bundled | 
| [+telemetry](https://packages.gentoo.org/useflags/+telemetry) | Send anonymized usage information to upstream so they can better understand our users | 
| [dbus](https://packages.gentoo.org/useflags/dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [eme-free](https://packages.gentoo.org/useflags/eme-free) | Disable EME (DRM plugin) capability at build time | 
| [gnome-shell](https://packages.gentoo.org/useflags/gnome-shell) | Integrate with gnome-base/gnome-shell search | 
| [hardened](https://packages.gentoo.org/useflags/hardened) | Activate default security enhancements for toolchain (gcc, glibc, binutils) | 
| [hwaccel](https://packages.gentoo.org/useflags/hwaccel) | Force-enable hardware-accelerated rendering (Mozilla bug 594876) | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [jpegxl](https://packages.gentoo.org/useflags/jpegxl) | Add JPEG XL / jxl image support | 
| [libproxy](https://packages.gentoo.org/useflags/libproxy) | Enable libproxy support | 
| [openh264](https://packages.gentoo.org/useflags/openh264) | Use media-libs/openh264 for H264 support instead of downloading binary blob from Mozilla at runtime | 
| [pgo](https://packages.gentoo.org/useflags/pgo) | Add support for profile-guided optimization for faster binaries - this option will double the compile time | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or Pipewire, or apulse if installed) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [sndio](https://packages.gentoo.org/useflags/sndio) | Enable support for the media-sound/sndio backend | 
| [system-pipewire](https://packages.gentoo.org/useflags/system-pipewire) | Use system media-video/pipewire for WebRTC and screencast instead of bundled one | 
| [system-png](https://packages.gentoo.org/useflags/system-png) | Use the system-wide media-libs/libpng instead of bundled (requires APNG patches) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [valgrind](https://packages.gentoo.org/useflags/valgrind) | Enable annotations for accuracy. May slow down runtime slightly. Safe to use even if not currently using dev-debug/valgrind | 
| [wasm-sandbox](https://packages.gentoo.org/useflags/wasm-sandbox) | Sandbox certain third-party libraries through WebAssembly using RLBox | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 
| [wifi](https://packages.gentoo.org/useflags/wifi) | Enable necko-wifi for NetworkManager integration, and access point MAC address scanning for better precision with opt-in geolocation services | 

The above list of USE flags is not comprehensive. [equery](https://wiki.gentoo.org/wiki/Equery) can be used if a full list is required:

`user $``equery uses www-client/firefox`
Firefox is one package that is often compiled with [LLVM](https://wiki.gentoo.org/wiki/LLVM), notably by Mozilla itself, which ensures this as a well-tested path. Using [Clang](https://wiki.gentoo.org/wiki/LLVM/Clang) to compile Firefox (by keeping the [clang](https://packages.gentoo.org/useflags/clang) [USE flag) could allow cross language optimizaton](https://wiki.gentoo.org/wiki/USE_flag)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> because Firefox mixes [C](https://wiki.gentoo.org/wiki/C) and [C++](https://wiki.gentoo.org/wiki/C%2B%2B) code with [Rust](https://wiki.gentoo.org/wiki/Rust) code<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>. Compiling with [GCC](https://wiki.gentoo.org/wiki/GCC) ([-clang](https://packages.gentoo.org/useflags/clang)[) is also supported, and there is some](https://wiki.gentoo.org/wiki/USE_flag) [analysis](https://wiki.gentoo.org/wiki/Project:Mozilla/Firefox_Benchmarks_2025_Q1) of potential small differences between each choice.

Note that `USE="-pulseaudio"` will select the ALSA audio back-end.

#### www-client/firefox-bin


| [+gmp-autoupdate](https://packages.gentoo.org/useflags/+gmp-autoupdate) | Allow Gecko Media Plugins (binary blobs) to be automatically downloaded and kept up-to-date in user profiles | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 

### Emerge

Gentoo Firefox packages are available both for the *Rapid Release* and *Extended Support Release (ESR)* Firefox update channels. To choose a specific Firefox update channel, see the [specify a slot section](https://wiki.gentoo.org/wiki/Firefox#Specify_a_slot). For information about Firefox releases, see [choosing a Firefox update channel](https://support.mozilla.org/en-US/kb/choosing-firefox-update-channel).

#### www-client/firefox

To install Firefox from source:

`root #``emerge --ask www-client/firefox`
This will install Firefox Extended Support Release (ESR) on a stable branch Gentoo system (or Firefox "Rapid release" if \~amd64 keyword is selected).

[www-client/firefox-bin](https://packages.gentoo.org/packages/www-client/firefox-bin) provides an optimized binary build of Firefox, by Mozilla. This will install much quicker than [www-client/firefox](https://packages.gentoo.org/packages/www-client/firefox) but offers less compile-time configuration options.

To emerge [www-client/firefox-bin](https://packages.gentoo.org/packages/www-client/firefox-bin):

`root #``emerge --ask www-client/firefox-bin`
This will explain how to select a Firefox package from a specific slot.

To select a release irrespective of keywords, subscribe to a slot:

`root #``emerge --ask www-client/firefox:esr`
Or:

`root #``emerge --ask www-client/firefox:rapid`
This will add the package from a specific slot to the [selected set](<https://wiki.gentoo.org/wiki/Selected-packages_set_(Portage)>) (/var/lib/portage/world). [Read more about the motivation of slotting](https://wiki.gentoo.org/wiki/Project:Mozilla#Benefits_and_disadvantages_of_the_slotting_system).

## Configuration

### Fonts

The fonts used by Firefox to display Web pages can be specified via the "Settings" UI, or via about:config.

#### Via the "Settings" UI

From the menu bar, select "Edit" -> "Settings", or visit about:preferences#general, and scroll down for the "Fonts" section.

There, specify the default font and the default size for that font, or select "Advanced" for a more fine-grained approach.

The "Advanced" dialog allows one to select a writing system (e.g. Latin, Simplified Chinese, Devanagari, etc.) and the fonts to be used for that writing system when a Web page needs to use a certain class of typeface: proportional, serif, sans-serif, or monospace. The minimum font size can be specified, as well as the default size for proportional and monospace fonts.

#### Via about:config

Search for `font.name`; this will list all settings available in the `font.name` and `font.name-list` namespaces.

### Enabling multitouch

#### Xinput2 scrolling

This brings touch scrolling and multitouch support for Firefox:

`MOZ_USE_XINPUT2` [environment variable](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/EnvVar) has to be set to a value of `1` in /etc/env.d/80firefox, or just before launching firefox in a shell. for example:

`user $``MOZ_USE_XINPUT2="1" firefox`
This also *eliminates* the predefined *scroll step size* for touchpad scrolling! All scrolling will be *really* smooth.

Wacom tablets/touchscreens may need [extra configuration](https://wiki.gentoo.org/wiki/Wacom#Xinput2_multitouch) so they emit true touch events for [Xorg](https://wiki.gentoo.org/wiki/Xorg).

#### Multitouch zoom

This only works when the multitouch events reach Firefox, therefore the `Xinput2` activation above has to be done first.

| Description | about:config option | Value | 
|---|---|---|
| Multitouch activation | `gestures.enable_single_finger_input` | `False` | 
| Zoom in | `browser.gesture.pinch.in` | `cmd_fullZoomReduce` | 
| Zoom out | `browser.gesture.pinch.out` | `cmd_fullZoomEnlarge` | 

### Middle mouse scroll (autoscroll)

Traditionally in Linux, the middle mouse button is used to paste the currently selected (highlighted) text into a text field. On Windows systems, the middle mouse button in Firefox is used for click-and-drag scrolling up and down the page. This functionality can be enabled in Firefox by opening `about:config` and setting `general.autoScroll = true` value<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup>:

Middle click-and-drag scrolling should now be enabled.

Although not necessary, sometimes it is desirable to disable all other middle-click functionality within Firefox when using click-and-drag scrolling. Open `about:config` and set the following values to disable middle-click functionality:

- `middlemouse.contentLoadURL = false`
- `middlemouse.openNewWindow = false`
- `middlemouse.paste = false`

### Bigger scrolling regions for Up/Down

PageUp/PageDown scroll for a page, Up/Down scroll for a line, many will benefit if Up/Down would scroll for a few lines:

| Description | about:config option | Value | 
|---|---|---|
| Vertical scroll distance, not for mouse (default is 3) | `toolkit.scrollbox.verticalScrollDistance` | `30` | 

To increase scrolling size for a mouse also:

| Description | about:config option | Value | 
|---|---|---|
| Vertical scroll distance, for mouse and keyboard (default is 1) | `mousewheel.min_line_scroll_amount` | `N` | 

### Threads

Firefox >= 54 \< 66 has 4 threads enabled by default<sup>[\[6\]](https://wiki.gentoo.org#cite_note-6)</sup>. Firefox >= 97 also has fission site isolation enabled by default<sup>[\[7\]](https://wiki.gentoo.org#cite_note-7)</sup>. Adjusting threads now does nothing with fission enabled and the UI option for changing the number of threads is also gone when fission is enabled<sup>[\[8\]](https://wiki.gentoo.org#cite_note-8)</sup>, Number of process per-site<sup>[\[9\]](https://wiki.gentoo.org#cite_note-9)</sup> can be adjusted by modifying the corresponding option in the `about:config` interface:

| Description | about:config option | Value | 
|---|---|---|
| Increase the threads | `dom.ipc.processCount` | `N` | 
| Increase the Number of process per-site | `dom.ipc.processCount.webisolated` | `N` | 

Where `N` is an integer number.

### Audio backend

Firefox's cubeb audio library supports a number of different backends.[\[10\]](https://wiki.gentoo.org#cite_note-10)<sup>[\[11\]](https://wiki.gentoo.org#cite_note-11)</sup> A backend can be specified by creating and setting `media.cubeb.backend` in about:config. A selection of available backends is listed below; the 'Value' column indicates the appropriate value for `media.cubeb.backend`, and the 'Status' column indicates the current level of support for that backend:

- Tier-1: Actively maintained. Should have CI coverage. Critical for Firefox.
- Tier-2: Actively maintained by contributors. CI coverage appreciated.
- Tier-3: Maintainers/patches accepted. Status unclear.
- Tier-4: Deprecated, obsolete. Scheduled to be removed.

| Description | Status | Value | 
|---|---|---|
| ALSA | Tier-3 | `alsa` | 
| JACK | Tier-3 | `jack` | 
| OSS | Tier-2 | `oss` | 
| PulseAudio (C) | Tier-4 | `pulse` | 
| PulseAudio (Rust) | Tier-1 | `pulse-rust` | 
| sndio | Tier-2 | `sndio` | 

Obviously, to use Firefox with JACK, [www-client/firefox](https://packages.gentoo.org/packages/www-client/firefox) should be installed with the USE flag [jack](https://packages.gentoo.org/useflags/jack) [enabled.](https://wiki.gentoo.org/wiki/USE_flag)

### Disabling percent-encoding

Normally, URLs that are copied from the address bar get [percent-encoded](https://en.wikipedia.org/wiki/Percent-encoding). This may cause an annoyance when certain non-Latin symbols (such as Cyrillic) get encoded, as they become unreadable to humans.

To disable percent-encoding when copying from the address bar, set the `about:config` option `network.standard-url.escape-utf8` to `false`.

### Special URLs

Firefox includes a few dozen special URLs that can be helpful in determining more information about various Firefox settings. These URLs can be entered into the Super Bar (via copy and paste) to view the special pages. A few of the more significant ones include:

- `about:addons` - Page for managing extensions.
- `about:buildconfig` - Page containing build information about the currently running version of Firefox. Use this page to check what compiler flags were set during Firefox's build.
- `about:cache` - Information about the Network Cache Storage Service.
- `about:config` - Modify internal browser settings and preferences.
- `about:memory` - Measure and show memory reports, free memory, etc.
- `about:networking` - Analyze current network information such as HTTP, socket, and DNS connections.
- `about:plugins` - List installed plugins such as Widevine Content Decryption Module (DRM software).
- `about:support` - A page containing technical information that might be useful when trying to solve a problem. Includes information on WebGL, Window Protocol (X11 or Wayland), and compositing graphics backends.
- `about:telemetry`

Finally, `about:about` will display the whole list of Firefox' “about” pages. A description for each page is available at [Firefox and the "about" protocol](https://developer.mozilla.org/en-US/docs/Mozilla/Firefox/The_about_protocol).

See also [Firefox chrome:// document URLs](https://superuser.com/questions/392471/firefox-chrome-document-urls).

### XDG integration

In order to make Firefox use [XDG file associations](https://wiki.gentoo.org/wiki/Default_applications#Setting_the_default_application_via_mimeapps.list_files) set Content Type's Action to /usr/bin/xdg-open.

To ensure Firefox is being used by other applications for handling HTTP and HTTPS links, run the following command:

`user $``xdg-mime default firefox.desktop x-scheme-handler/http x-scheme-handler/https text/html`
### Running under KDE

Firefox is built with GTK, and by default will use the GTK file picker. To enable the KDE file picker, install [kde-plasma/xdg-desktop-portal-kde](https://packages.gentoo.org/packages/kde-plasma/xdg-desktop-portal-kde), which is installed by default if [kde-plasma/plasma-meta](https://packages.gentoo.org/packages/kde-plasma/plasma-meta) is installed. After that set `widget.use-xdg-desktop-portal` to `true` and `widget.use-xdg-desktop-portal.file-picker` to `1` in `about:config`.

### Enabling color management

See the dedicated [Color management section](https://wiki.gentoo.org/wiki/Color_management#Firefox).

### Disabling Privacy-Preserving Attribution

To disable the Privacy-Preserving Attribution API added in version 128,[\[12\]](https://wiki.gentoo.org#cite_note-12)[\[13\]](https://wiki.gentoo.org#cite_note-13)<sup>[\[14\]](https://wiki.gentoo.org#cite_note-14)</sup> set `dom.private-attribution.submission.enabled` to false.<sup>[\[15\]](https://wiki.gentoo.org#cite_note-15)</sup> Or compile Firefox with `-telemetry` use flag.

### Disabling AI Chatbots

To disable the AI Chatbot sidebar feature added since version 130,[\[16\]](https://wiki.gentoo.org#cite_note-16)<sup>[\[17\]](https://wiki.gentoo.org#cite_note-17)</sup> set `browser.ml.chat.enabled` to false.[\[18\]](https://wiki.gentoo.org#cite_note-18)

Although the Firefox UI doesn't directly allow users to hide certain entries in the context menu / 'right-click' menu, it's possible to do so via the userChrome.css stylesheet. Refer to [this Reddit post](https://www.reddit.com/r/firefox/comments/1eoo6k7/firefox_context_menu/) for details.

## Security

### Running in sandbox

It is highly recommended to run your browser inside a sandbox, to limit its access to e.g. your home directory. There are many alternative sandboxing applications with this functionality to choose from.

Please see [Simple sandbox](https://wiki.gentoo.org/wiki/Simple_sandbox) for suggestions on sandboxing Firefox.

### SSL/TLS security enhancements

Some `about:config` SSL/TLS security options (as of Firefox 149) which increase the security of HTTPS connections are listed below.

| Description | about:config option | Value | 
|---|---|---|
| Minimum TLS version set to [1.2](https://en.wikipedia.org/wiki/Transport_Layer_Security#TLS_1.2). *Changing to 4 may break access to some websites.* | `security.tls.version.min` | `3` | 
| Avoiding old SSL/TLS version. *May break access to some badly configured websites.* | `security.ssl.require_safe_negotiation` | `true` | 
| Inform user about insecure SSL/TLS negotiation (broken padlock). | `security.ssl.treat_unsafe_negotiation_as_broken` | `true` | 
| Require [Online Certificate Status Protocol](https://en.wikipedia.org/wiki/Online_Certificate_Status_Protocol). Introduces some latency. | `security.OCSP.require` | `true` | 
| Strict [Certificate Pinning](https://en.wikipedia.org/wiki/HTTP_Public_Key_Pinning). | `security.cert_pinning.enforcement_level` | `2` | 
| No Google [SSL False Start](https://en.wikipedia.org/wiki/TLS_False_Start#Downgrade_attacks:_FREAK_attack_and_Logjam_attack). | `security.ssl.enable_false_start` | `false` | 

### Safer browsing with add-ons

Many users are concerned about their privacy (tracking, bubbling, targeting, etc) while web browsing. Installing Add-ons can aid in adding an extra level of privacy to their browsing.

The add-on menu can be accessed by navigating the following menus: Hamburger button (top right under the X) → Add-ons

#### uBlock Origin

uBlock Origin is "a wide-spectrum content blocker with CPU and memory efficiency as a primary feature".<sup>[\[19\]](https://wiki.gentoo.org#cite_note-19)</sup> It enables 5 filter lists by default and others are available.

- Mozilla Add-ons page: [https://addons.mozilla.org/en/firefox/addon/ublock-origin/](https://addons.mozilla.org/en/firefox/addon/ublock-origin/)
- GitHub: [https://github.com/gorhill/uBlock](https://github.com/gorhill/uBlock)
- Wikipedia: [https://en.wikipedia.org/wiki/uBlock\_Origin](https://en.wikipedia.org/wiki/uBlock_Origin)

#### AdNauseam

AdNauseam provides the same functionality as uBlock Origin but also "quietly clicks on every blocked ad"<sup>[\[20\]](https://wiki.gentoo.org#cite_note-20)</sup> with the goal of resisting ad network tracking efforts through obfuscation. Some research in 2021 showed it to be effective for individual users.[\[21\]](https://wiki.gentoo.org#cite_note-21)

- Mozilla Add-ons page: [https://addons.mozilla.org/en-GB/firefox/addon/adnauseam/](https://addons.mozilla.org/en-GB/firefox/addon/adnauseam/)
- Homepage: [https://adnauseam.io/](https://adnauseam.io/)
- GitHub: [https://github.com/dhowe/AdNauseam](https://github.com/dhowe/AdNauseam)

#### LibRedirect

A web extension that redirects YouTube, Twitter, TikTok, and other websites to alternative privacy friendly frontends. The extension is very customizable, including the possibility of enabling/disabling redirection per service category.

- Mozilla Add-ons page: [https://addons.mozilla.org/en-US/firefox/addon/libredirect/](https://addons.mozilla.org/en-US/firefox/addon/libredirect/)
- Homepage: [https://libredirect.github.io/](https://libredirect.github.io/)

#### Facebook Container

Facebook is able to track the activity of both loged-in and logged-out users through the presence of widgets such as a "like"/"share" button on a website. This add-on, created by Mozilla themselves, utilizes Firefox container tabs to isolate Facebook (and related sites such as Instagram) from the rest of your web sessions. When installed, accessing any Facebook sites will automatically open the page in a Facebook container tab. Accessing a non-Facebook site from within the container will automatically open the page in a tab outside of the container. Additionally, it disables the Facebook widgets outside of the Facebook container.

- Mozilla Add-ons page: [https://addons.mozilla.org/en-US/firefox/addon/facebook-container/](https://addons.mozilla.org/en-US/firefox/addon/facebook-container/)
- GitHub: [https://github.com/mozilla/contain-facebook](https://github.com/mozilla/contain-facebook)

#### NoScript

NoScript blocks JavaScript that is normally enabled by default. It can keep users safe and speed up web browsing.

- Mozilla Add-ons page: [https://addons.mozilla.org/en-US/firefox/addon/noscript/](https://addons.mozilla.org/en-US/firefox/addon/noscript/)
- Homepage: [https://noscript.net/](https://noscript.net/)

#### Video speed controller

Using an HTML5 video speed controller can be helpful in accelerating the playback rate of HTML 5 video. This is useful when binging video content on sites that do not offer increases in video playback speed (such as Amazon Prime Video or Netflix).

- Mozilla Add-ons page: [https://addons.mozilla.org/en-US/firefox/addon/videospeed/](https://addons.mozilla.org/en-US/firefox/addon/videospeed/)
- GitHub: [https://github.com/codebicycle/videospeed](https://github.com/codebicycle/videospeed)

#### Behind The Overlay

Some websites use modal overlays to display pop-ups, such that manually blocking the overlay with an add-on like uBlock Origin causes the page to become unscrollable. This add-on provides a one-click solution for bypassing such cases without the need to tinker with custom filtering rules.

- Mozilla Add-ons page: [https://addons.mozilla.org/en-US/firefox/addon/behind\_the\_overlay/](https://addons.mozilla.org/en-US/firefox/addon/behind_the_overlay/)
- GitHub: [https://github.com/NicolaeNMV/BehindTheOverlay](https://github.com/NicolaeNMV/BehindTheOverlay)

### Policies

Custom policies can be configured in Firefox, which is particularly useful in cases where Firefox is set up for an organization or end users. However, it can also be beneficial to block specific content for personal use. [\[22\]](https://wiki.gentoo.org#cite_note-22)

The procedure may vary if the binary has been utilized. For this particular procedure, it is assumed that Firefox has been compiled on the system. To configure custom policies in this scenario, it is necessary to create a JSON file in the following directory:

/etc/firefox/policies/policies.json

The JSON file must meet the following structure:

**`/etc/firefox/policies/policies.json`**

```
{
  "policies": {
    "policy_name": policy_value
  }
}
```
As an example, creating a policy that blocks `about:config` and all URLs except for the Gentoo page and its subpages would require the following contents in the corresponding JSON file:

**`/etc/firefox/policies/policies.json`**

```
{
  "policies": {
    "BlockAboutConfig": true,
    "WebsiteFilter": {
      "Block": ["<all_urls>"],
      "Exceptions": ["https://www.gentoo.org/*"]
    }
  }
}
```
The success of the configuration can and should always be verified by checking the special page `about:policies`. This page also contains documentation and examples for other policy options. A complete list of policies can also be found in the [policy templates provided by Mozilla on GitHub](https://github.com/mozilla/policy-templates/blob/master/README.md).

It is recommended to carefully study the list, especially when the configuration is for other users. An approach of first blocking everything and then allowing specific desired pages is always preferable, as it reduces the likelihood of overlooking pages. Special pages and protocols (such as the `file://` protocol) that are not desired to be used, should also be considered to be blocked to prevent abuse of the security policies.

### Local certificates

Although Firefox uses the NSS library for handling the secure communications, it doesn't use the \~/.pki/nssdb/ location, nor the system-wide CA certificate list in /etc/ssl/certs/. Instead, it uses its own list, stored in the file cert9.db in each Firefox profile directory (e.g. \~/.mozilla/firefox/xxxxxxxx.default/. The cert9.db file is an SQLite database; previously, a BerkeleyDB database was used, and the relevant file was cert8.db.

To list Firefox's intermediate CA certificates, use [certutil(1)](https://man.archlinux.org/man/certutil.1.en) [(provided by](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [dev-libs/nss](https://packages.gentoo.org/packages/dev-libs/nss)) and installed as a dependency of Firefox and other packages):

`user $``certutil -L -d ~/.mozilla/firefox/*.default/````
Certificate Nickname                                         Trust Attributes
                                                             SSL,S/MIME,JAR/XPI
 
DigiCert High Assurance CA-3                                 ,,   
DigiCert Secure Server CA                                    ,,   
InCommon Server CA                                           ,,   
...
```
Local certificates should be installed in /usr/local/share/ca-certificates, and have the extension .crt (instead of e.g. .pem), as the extension checked by the update-ca-certificates script.

To add the certificate to the certificate list for a particular profile:

- In Firefox, select Edit > Settings > Privacy & Security. Scroll to the "Certificates" section, then select the "View certificates" button; this will open the "Certificate Manager" dialog. Select the "Import" button.

- On the command line, use the [certutil(1)](https://man.archlinux.org/man/certutil.1.en)

`user $``certutil -d ~/.mozilla/firefox/xxxxxxxx.default/ -A -n cert-nickname -i /usr/local/share/ca-certificates/my-cert.crt -t "CT,,"`
To add a certificate to the certificate list for all profiles, add the following to the /etc/firefox/policies/policies.json file (as described in the [Firefox](https://wiki.gentoo.org/wiki/Firefox#Policies) section), creating it if necessary:

**`/etc/firefox/policies/policies.json`**

**FireFox certificates**

```
{
  "policies": {
    "DisableAppUpdate": true,
    "Certificates": {
      "ImportEnterpriseRoots": true,
      "Install": [
        "/usr/local/share/ca-certificates/my-cert.crt"
      ]
    }
  }
}
```
## Privacy

### Font visibility

Having unique fonts installed results in a more unique browser fingerprint. The setting **layout.css.font-visibility** allows one to control which fonts are visible to websites.[\[23\]](https://wiki.gentoo.org#cite_note-23)[\[24\]](https://wiki.gentoo.org#cite_note-24)<sup>[\[25\]](https://wiki.gentoo.org#cite_note-25)</sup> There are 3 options:

- 1 - only base system fonts (default with resistFingerprinting)
- 2 - also fonts from optional language packs
- 3 - also user-installed fonts (default except with resistFingerprinting)

The add-on Font Fingerprint Defender can also be used to randomize the font fingerprint.[\[26\]](https://wiki.gentoo.org#cite_note-26)

### Locales

If `privacy.spoof_english` is set to 2, en-US will always be the locale reported to and requested from websites.[\[27\]](https://wiki.gentoo.org#cite_note-27)

## Hardware acceleration

Enable the `hwaccel` USE flag for Firefox to get correct `about:config` options. Firefox >116 will also install helper binaries, like vaapitest. This flag may cause Firefox to crash and is not necessary for hardware acceleration. If so, try the options below.

Verify that *web render* hardware acceleration is working by going to `about:support#graphics` and searching for **Compositing**. A value of `WebRender` means hardware acceleration is enabled, while `WebRender (software)` means it's not. If you have installed the correct video card drivers but WebRender is still using software rendering, try setting **gfx.webrender.all** to **true** in `about:config`.

Hardware accelerated *video* decoding status can also be checked on the `about:support#graphics` page. Search for `HARDWARE_VIDEO_DECODING`. `Available` means it's working. If video decoding is disabled, you may need to setup VA-API or VDPAU drivers.

Since 116, the `wayland` USE flag isn't needed to get hardware acceleration on supported cards for Rapid. For ESR, 115 needs the `hwaccel wayland` USE flags enabled to get hardware acceleration. `X` and `wayland` can be enabled simultaneously. > 116 was fixed to have hardware acceleration without `wayland` support.

|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | AMD | Intel | Nvidia | 
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| ESR |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | Should 'just work' if the card is supported. > 116 will have more cards supported than 115. | Should work if the card is supported (> Haswell). | Install [media-libs/nvidia-vaapi-driver](https://packages.gentoo.org/packages/media-libs/nvidia-vaapi-driver) and check [its upstream documentation](https://github.com/elFarto/nvidia-vaapi-driver). Currently, nouveau does not support hardware acceleration in Firefox. | 
| Rapid |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | Should work if the card is supported. | Should work if the card is supported (> Haswell). | Install [media-libs/nvidia-vaapi-driver](https://packages.gentoo.org/packages/media-libs/nvidia-vaapi-driver) and check [its upstream documentation](https://github.com/elFarto/nvidia-vaapi-driver). Currently, nouveau does not support hardware acceleration in Firefox. | 

## Troubleshooting

Refer to [Firefox/troubleshooting](https://wiki.gentoo.org/wiki/Firefox/troubleshooting).

## Firefox forked projects

- [GNU Icecat](https://wiki.gentoo.org/wiki/GNU_Icecat)
- [LibreWolf](https://wiki.gentoo.org/wiki/LibreWolf)
- [Pale Moon](https://www.palemoon.org) — Pale Moon Web Browser.

Wikipedia maintains a list of [projects based on Firefox](https://en.wikipedia.org/wiki/List_of_web_browsers#Gecko-based) (may not be complete).

## See also

- [Thunderbird](https://wiki.gentoo.org/wiki/Thunderbird) — Mozilla's solution to the e-mail client.
