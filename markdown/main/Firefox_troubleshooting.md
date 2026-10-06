<!-- source: https://wiki.gentoo.org/wiki/Firefox/troubleshooting | group: Gentoo Wiki (Main) | wiki-title: Firefox/troubleshooting -->
---
title: Firefox/troubleshooting
url: https://wiki.gentoo.org/wiki/Firefox/troubleshooting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-06"
fingerprint: "8f0cd1d72dc7bbc6"
license: CC BY-SA 4.0
---

# Firefox/troubleshooting

[Firefox](https://wiki.gentoo.org/wiki/Firefox)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## General tips

A good starting point for investigation can be the special Firefox page about:support, which contains various pieces of technical information, together with further about: pages, such as about:memory. For a complete list, visit about:about.

This page assumes that Firefox was compiled on the system. If the binary provided by upstream is being utilized, replace commands on this page that call firefox with a call to firefox-bin.

Finally, some items on this page might require a running graphical environment, and might not work if Firefox is used in headless mode on a system without any graphical environment.

### Determine Firefox's GUI environment

Check the value of "Window Protocol" under about:support#graphics. Possible values include `wayland`, `Xwayland`, and `x11`.

### Safe mode

Starting Firefox in safe mode temporarily disables add-ons, hardware acceleration and [WebGL](https://en.wikipedia.org/wiki/WebGL), window and sidebar size along with other position settings, userChrome and userContent customizations, and the JavaScript JIT compiler.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> This can help isolate where the problem might be, and whether it can be resolved by removing the relevant custom settings.

To start Firefox in safe mode:

- enable Troubleshoot Mode on the about:support page<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>; or
- pass the `--safe-mode` option to the binary at startup, which is particularly useful when the about:support page is not accessible (e.g. when Firefox doesn't even start):

`user $``firefox --safe-mode`
If prompted to choose between a refresh or proceeding with Troubleshoot Mode, first test whether Troubleshoot Mode solves the problem.

If the problem doesn't persist in safe mode, then it's probably related to hardware acceleration, custom themes, or to custom extensions. To deactivate these things step-by-step and further narrow down the problem, [follow this guide by Mozilla](https://support.mozilla.org/en-US/kb/troubleshoot-extensions-themes-to-fix-problems#w_the-problem-does-not-occur-in-troubleshoot-mode).

If the problem was not caused by themes or extensions, but rather by hardware acceleration, review the [Gentoo Hardware Acceleration Guide](https://wiki.gentoo.org/wiki/Xorg/Hardware_3D_acceleration_guide), to check if the issue can be resolved using the steps suggested there.

If the problem was caused by custom themes, extensions, or other custom settings within the following removal scope, a simple refresh may solve the problem. Refreshing Firefox will remove these settings and reset them to their default values:

- Extensions and themes
- Web site permissions
- Modified preferences
- Added search engines
- DOM storage
- Security certificates and device settings
- Download actions
- Toolbar customization and user styles

The final category also removes any userChrome and/or userContent [CSS](https://en.wikipedia.org/wiki/CSS) files present; if necessary, backup these before proceeding, to prevent data loss<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>.

A refresh can be performed either via about:support - search for the "Refresh Firefox" option - or via the command line interface:

`user $``firefox --safe-mode`
If prompted, select "Refresh Firefox". This will not reset bookmarks, history, passwords, cookies, auto fill, and the personal directory<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>.

### Startup cache

Problems related to startup, such as long startup times or crashes immediately on startup, might be a result of a broken startup cache. To check, simply clear the startup cache, typically located at \~/.cache/mozilla/firefox.

To clear the cache from within Firefox, search the about:support page for the phrase "Clear startup cache".

Alternatively, move the existing cache to a new location, which will cause Firefox to create a new cache:

`user $````
cd ~/.cache/mozilla
```
`user $``mv firefox firefox_old`
If using a fresh cache solves the problem, and the old cache is not needed anymore, it can be deleted. If the problem was not caused by the startup cache, revert the changes:

`user $````
cd ~/.cache/mozilla
```
`user $````
rm -r firefox
```
`user $````
mv firefox_old firefox
```
### Reset profile

Firefox stores personal information, e.g. bookmarks, passwords, and user preferences, in a *profile*. Detailed information about what's stored in a profile can be found in [this Mozilla guide](https://support.mozilla.org/en-US/kb/profiles-where-firefox-stores-user-data#w_what-information-is-stored-in-my-profile).

A Firefox profile is stored at a specific location. The location of the currently active profile can be found on the about:support page. For example, the location might be something like /home/larry/.mozilla/firefox/ntgqave6.default-esr-1679659083216.

If the about:support page isn't accessible, the usual storage location for profiles is \~/.mozilla/firefox. The profiles.ini file in that directory specifies the name for each profile.

Some ways of testing whether a profile is the source of a problem include:

- Starting Firefox with the `--ProfileManager` option, which allows creating and using a fresh profile, to check if the problem persists with the new profile; and
- Starting Firefox with the `-P` option to specify a profile by name, as per the profiles.ini file, or with the `--profile` option, to specify a profile by its absolute path.

## Video and graphics

### Green video screen (YouTube)

If disabling (graphics) acceleration in settings does not work, disabling the equivalent options under `about:config` like `layers.acceleration.force-enabled` (85.0) might.

### Screen tearing / stuttering smooth scrolling

Build [www-client/firefox](https://packages.gentoo.org/packages/www-client/firefox) with the `hwaccel` *USE* flag , then check the **Compositing** value of the `about:support#graphics` table for WebRender.

![](https://wiki.gentoo.org/images/thumb/4/43/Firefox_Compositing_is_WebRender.png/300px-Firefox_Compositing_is_WebRender.png)

Systems with [Wayland](https://wiki.gentoo.org/wiki/Wayland) support should not have issues with screen tearing.

If WebRender is enabled, then the problem could be with the video drivers. For example about [Intel](https://wiki.gentoo.org/wiki/Intel#Screen_tearing).

### gtk+:3 pulls in D-Bus

Since version ≥53.0, Firefox has dropped [gtk+](https://wiki.gentoo.org/wiki/GTK):2 support urging [Larry](https://wiki.gentoo.org/wiki/Larry_the_cow) to use gtk+:3. This, however, by default, pulls-in dependencies like [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) unconditionally. This can be avoided by using a [patch from BSD](https://forums.gentoo.org/viewtopic-t-1060964-start-38-highlight-BSD%20maintain%20a%20patch.html) available in [bug #669234](https://bugs.gentoo.org/show_bug.cgi?id=669234) or the [mv overlay](https://github.com/gentoo-mirror/mv/tree/master/x11-libs/gtk%2B).

Please note that, when running Firefox under native Wayland (i.e. not using XWayland), [Firefox will implicitly try to use D-Bus to enable its remote control feature](https://utcc.utoronto.ca/~cks/space/blog/unix/FirefoxDBusRemoteControl) and crash, likely with a segfault, due to the lack of D-Bus. Thus, it is necessary to invoke Firefox with the `--no-remote` command-line argument or `MOZ_NO_REMOTE` environment variable set (to anything).

### KDE Plasma integration: "failed to connect to the native host"

If using [www-client/firefox-bin](https://packages.gentoo.org/packages/www-client/firefox-bin), Plasma integration might not work by default due to org.kde.plasma.browser\_integration.json getting installed in an unexpected directory, /usr/lib64/mozilla instead of /usr/lib/mozilla). Refer to [bug #687736](https://bugs.gentoo.org/show_bug.cgi?id=687736) for details.

As a workaround, create a symlink:

`root #``ln -s /usr/lib64/mozilla /usr/lib/mozilla`
### "Failed to load cursor theme Adwaita" under Wayland

This happens when Firefox attempts to find a cursor theme in /usr/local/share/icons/ which doesn't exist. Refer to [this forums post](https://forums.gentoo.org/viewtopic.php?p=8769434#p8769434) for the fix.

### Touchpad scrolling feels too fast on Wayland

Change the option `apz.gtk.pangesture.delta_mode` from 0 to 2. Further tweaks are discussed in [this Mozilla bugtracker thread](https://bugzilla.mozilla.org/show_bug.cgi?id=1752862).

### Windows decorations missing in Fluxbox since FF-91.3.0

- [https://forums.gentoo.org/viewtopic-t-1141870.html](https://forums.gentoo.org/viewtopic-t-1141870.html) Solved in 91.9.0esr and back with 102.3.0esr (64-bit)

Once in a while this happens. What might then help is to [restart fluxbox with the menu](https://sourceforge.net/p/fluxbox/bugs/1111/#3efa).

### Green artifacts on a video only with hardware acceleration

Try setting **media.navigator.mediadatadecoder\_vpx\_enabled** to false in `about:config`.

### Hardware acceleration not working

Make sure [media-video/libva-utils](https://packages.gentoo.org/packages/media-video/libva-utils) and [sys-apps/pciutils](https://packages.gentoo.org/packages/sys-apps/pciutils) are installed. Make sure vaapi works outside Firefox first with **vainfo** program. Try a different program to confirm vaapi works there, e.g. **mpv --hwdec=vaapi**.

Debug the issue with **MOZ\_LOG="PlatformDecoderModule:5" firefox**. On startup this will show a handshake between Firefox and the vaapi system - the browser inquires which codecs are supported by hardware. You'll see a list of the codecs followed by SW or HW.

You may attempt to force Firefox to use hardware acceleration by setting the about:config flag **media.hardware-video-decoding.force-enabled** to true.

### Hardware acceleration not working within a sandbox

When using an external sandbox application, such as [sys-apps/bubblewrap](https://packages.gentoo.org/packages/sys-apps/bubblewrap) or [sys-apps/firejail](https://packages.gentoo.org/packages/sys-apps/firejail), make sure that hardware acceleration works **outside** the sandboxing. Hardware acceleration will require more `ro` access permissions from /dev and /sys.

### No video with supported format and MIME type found

In **about:config** try to toggle **media.rdd-process.enabled** (default is true).

## Audio

### Lack of sound (www-client/firefox-bin)

[www-client/firefox-bin](https://packages.gentoo.org/packages/www-client/firefox-bin) expects [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio). [ALSA](https://wiki.gentoo.org/wiki/ALSA)-only systems might work around this limitation by using [media-sound/apulse](https://packages.gentoo.org/packages/media-sound/apulse). For this to work, modify Firefox sandbox settings by going to `about:config` and adding /dev/snd/ (note the trailing slash) to the `security.sandbox.content.write_path_whitelist` option.

If storing ALSA settings in $HOME, also, be sure to add $HOME/.asoundrc to the `security.sandbox.content.write_path_whitelist` option. Whitelist path could be separated by comma.

Since around Firefox 58 there is additional modification needed to work around seccomp sandbox: `security.sandbox.content.syscall_whitelist = 16` It is now possible to go ahead and create alias for running Firefox through apulse:

`user $``alias firefox='apulse firefox-bin'`
### Lack of sound when using PipeWire (www-client/firefox)

**Problem:** System sound is working properly, but Firefox itself is unable to provide sound playback.

**Cause:** The [www-client/firefox](https://packages.gentoo.org/packages/www-client/firefox) package has support for three audio backends: ALSA, JACK and PulseAudio. With [PipeWire](https://wiki.gentoo.org/wiki/PipeWire) becoming the standard audio backend on Linux, support for ALSA (via the `alsa` USE flag) or PulseAudio (via the `pulseaudio` USE flag) may not be enabled by the target [desktop profile](<https://wiki.gentoo.org/wiki/Profile_(Portage)>). The default audio backend for Firefox can be checked under `about:support#media` under **Audio Backend**:

![](https://wiki.gentoo.org/images/thumb/9/9d/Firefox_audio_backend_screenshot.png/300px-Firefox_audio_backend_screenshot.png)

**Solution:** Enable the `pulseaudio` USE flag for Firefox and then recompile Firefox. Be sure to follow the steps detailed in the [PipeWire article](https://wiki.gentoo.org/wiki/PipeWire#USE_flags) to setup pipewire-alsa.

### Sound crackling when using Pipewire or JACK (www-client/firefox)

**Problem:** System sound is crackling.

**Cause:** The [www-client/firefox](https://packages.gentoo.org/packages/www-client/firefox) package has support for two audio backends: JACK and PulseAudio. When using Pipewire, see [Arch Linux forum post on the issue](https://bbs.archlinux.org/viewtopic.php?id=280654). When using JACK, in the Configure dialog of [media-sound/qjackctl](https://packages.gentoo.org/packages/media-sound/qjackctl) or [media-sound/cadence](https://packages.gentoo.org/packages/media-sound/cadence), to set **Buffer Size: 1024** and **Periods/Buffer: 8** works fine for me.

### Speech dispatcher library missing

Some webpages may result in Firefox giving a notice that "You can’t use speech synthesis because the Speech Dispatcher library is missing.”<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup> To fix this,

`root #``emerge --ask app-accessibility/speech-dispatcher`
## Crashes

If Firefox crashes for no apparent reason every few minutes with an error message like `ABORT: X_GLXDestroyContext: GLXBadContext; 15 requests ago` it might help to add the Firefox user(s) to the video group:

`root #``gpasswd -a <username> video`
