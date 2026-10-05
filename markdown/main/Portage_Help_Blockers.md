<!-- source: https://wiki.gentoo.org/wiki/Portage/Help/Blockers | group: Gentoo Wiki (Main) | wiki-title: Portage/Help/Blockers -->
---
title: Portage/Help/Blockers
url: https://wiki.gentoo.org/wiki/Portage/Help/Blockers
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-21"
fingerprint: da8fedbe0773b742
license: CC BY-SA 4.0
---

# Portage/Help/Blockers

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

If your blocker isn't listed here, there's a very good chance it's to do with world file pollution: [Maintaining a Gentoo system#World file hygiene](https://wiki.gentoo.org/wiki/User:Sam/Portage_help/Maintaining_a_Gentoo_system#World_file_hygiene).

## xwayland, xorg-server, and libxcvt

Manifests as:

`root #``emerge -p -uvDU @world````
[...]
 * Error: The above package list contains packages which cannot be
 * installed at the same time on the same system.
  (x11-base/xorg-server-1.20.11:0/1.20.11::gentoo, installed) pulled in by
    x11-base/xorg-server:0/1.20.11= required by (x11-drivers/xf86-video-vesa-2.5.0:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    >=x11-base/xorg-server-1.6 required by (x11-drivers/xf86-video-vesa-2.5.0:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    x11-base/xorg-server:= required by (x11-drivers/xf86-video-vesa-2.5.0:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    >=x11-base/xorg-server-1.20[xorg] required by (x11-drivers/xf86-video-vesa-2.5.0:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    >=x11-base/xorg-server-1.20[udev] required by (x11-base/xorg-drivers-1.20-r2:0/0::gentoo, installed) USE="" ABI_X86="(64)" INPUT_DEVICES="evdev synaptics wacom -elographics -joystick -libinput -vmmouse -void" VIDEO_CARDS="amdgpu dummy fbdev i965 intel nouveau radeon radeonsi vesa -ast (-freedreno) (-geode) -glint -i915 -mga -nv -nvidia (-omap) -qxl -r128 -siliconmotion (-tegra) (-vc4) -via -virtualbox -vmware"
    >=x11-base/xorg-server-1.20[-minimal] required by (x11-base/xorg-drivers-1.20-r2:0/0::gentoo, installed) USE="" ABI_X86="(64)" INPUT_DEVICES="evdev synaptics wacom -elographics -joystick -libinput -vmmouse -void" VIDEO_CARDS="amdgpu dummy fbdev i965 intel nouveau radeon radeonsi vesa -ast (-freedreno) (-geode) -glint -i915 -mga -nv -nvidia (-omap) -qxl -r128 -siliconmotion (-tegra) (-vc4) -via -virtualbox -vmware"
    x11-base/xorg-server[-minimal] required by (x11-drivers/xf86-video-ati-19.1.0:0/0::gentoo, installed) USE="udev" ABI_X86="(64)"
    x11-base/xorg-server:= required by (x11-drivers/xf86-video-ati-19.1.0:0/0::gentoo, installed) USE="udev" ABI_X86="(64)"
    >=x11-base/xorg-server-1.20[xorg] required by (x11-drivers/xf86-video-ati-19.1.0:0/0::gentoo, installed) USE="udev" ABI_X86="(64)"
    x11-base/xorg-server:0/1.20.11= required by (x11-drivers/xf86-video-ati-19.1.0:0/0::gentoo, installed) USE="udev" ABI_X86="(64)"
    x11-base/xorg-server:= required by (x11-drivers/xf86-video-nouveau-1.0.17:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    x11-base/xorg-server[-minimal] required by (x11-drivers/xf86-video-nouveau-1.0.17:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    >=x11-base/xorg-server-1.20[xorg] required by (x11-drivers/xf86-video-nouveau-1.0.17:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    x11-base/xorg-server:0/1.20.11= required by (x11-drivers/xf86-video-nouveau-1.0.17:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    x11-base/xorg-server required by (x11-drivers/xf86-video-fbdev-0.5.0-r1:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    x11-base/xorg-server:= required by (x11-drivers/xf86-video-fbdev-0.5.0-r1:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    >=x11-base/xorg-server-1.20[xorg] required by (x11-drivers/xf86-video-fbdev-0.5.0-r1:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    x11-base/xorg-server:0/1.20.11= required by (x11-drivers/xf86-video-fbdev-0.5.0-r1:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    x11-base/xorg-server required by (x11-drivers/xf86-video-dummy-0.3.8:0/0::gentoo, installed) USE="-dga" ABI_X86="(64)"
    x11-base/xorg-server:= required by (x11-drivers/xf86-video-dummy-0.3.8:0/0::gentoo, installed) USE="-dga" ABI_X86="(64)"
    >=x11-base/xorg-server-1.20[xorg] required by (x11-drivers/xf86-video-dummy-0.3.8:0/0::gentoo, installed) USE="-dga" ABI_X86="(64)"
    x11-base/xorg-server:0/1.20.11= required by (x11-drivers/xf86-video-dummy-0.3.8:0/0::gentoo, installed) USE="-dga" ABI_X86="(64)"
    x11-base/xorg-server[-minimal] required by (x11-drivers/xf86-video-amdgpu-19.1.0:0/0::gentoo, installed) USE="udev" ABI_X86="(64)"
    x11-base/xorg-server:= required by (x11-drivers/xf86-video-amdgpu-19.1.0:0/0::gentoo, installed) USE="udev" ABI_X86="(64)"
    >=x11-base/xorg-server-1.20[xorg] required by (x11-drivers/xf86-video-amdgpu-19.1.0:0/0::gentoo, installed) USE="udev" ABI_X86="(64)"
    x11-base/xorg-server:0/1.20.11= required by (x11-drivers/xf86-video-amdgpu-19.1.0:0/0::gentoo, installed) USE="udev" ABI_X86="(64)"
    >=x11-base/xorg-server-1.7 required by (x11-drivers/xf86-input-wacom-0.40.0:0/0::gentoo, installed) USE="-debug" ABI_X86="(64)"
    x11-base/xorg-server:= required by (x11-drivers/xf86-input-wacom-0.40.0:0/0::gentoo, installed) USE="-debug" ABI_X86="(64)"
    >=x11-base/xorg-server-1.20[xorg] required by (x11-drivers/xf86-input-wacom-0.40.0:0/0::gentoo, installed) USE="-debug" ABI_X86="(64)"
    x11-base/xorg-server:0/1.20.11= required by (x11-drivers/xf86-input-wacom-0.40.0:0/0::gentoo, installed) USE="-debug" ABI_X86="(64)"
    >=x11-base/xorg-server-1.14 required by (x11-drivers/xf86-input-synaptics-1.9.1:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    x11-base/xorg-server:= required by (x11-drivers/xf86-input-synaptics-1.9.1:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    >=x11-base/xorg-server-1.20[xorg] required by (x11-drivers/xf86-input-synaptics-1.9.1:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    x11-base/xorg-server:0/1.20.11= required by (x11-drivers/xf86-input-synaptics-1.9.1:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    >=x11-base/xorg-server-1.18[udev] required by (x11-drivers/xf86-input-evdev-2.10.6:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    x11-base/xorg-server:= required by (x11-drivers/xf86-input-evdev-2.10.6:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    >=x11-base/xorg-server-1.20[xorg] required by (x11-drivers/xf86-input-evdev-2.10.6:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    x11-base/xorg-server:0/1.20.11= required by (x11-drivers/xf86-input-evdev-2.10.6:0/0::gentoo, installed) USE="" ABI_X86="(64)"
    >=x11-base/xorg-server-1.15.1 required by (x11-misc/sddm-0.18.1-r5:0/0::gentoo, installed) USE="pam systemd (-elogind) -test" ABI_X86="(64)"
    x11-base/xorg-server[-minimal] required by (gnome-base/gdm-40.1:0/0::gentoo, ebuild scheduled for merge) USE="branding ipv6 plymouth systemd tcpd wayland -accessibility -audit -bluetooth-sound (-elogind) -fprint (-selinux) -test" ABI_X86="(64)"
    <x11-base/xorg-server-1.20.11-r3[xorg,-minimal] required by (x11-wm/mutter-40.3-r1:0/8::gentoo, ebuild scheduled for merge) USE="introspection screencast systemd udev wayland (-elogind) -sysprof -test" ABI_X86="(64)" INPUT_DEVICES="wacom" VIDEO_CARDS="-nvidia"
  (x11-base/xwayland-21.1.2-r2:0/0::gentoo, ebuild scheduled for merge) pulled in by
    x11-base/xwayland required by (x11-wm/mutter-40.3-r1:0/8::gentoo, ebuild scheduled for merge) USE="introspection screencast systemd udev wayland (-elogind) -sysprof -test" ABI_X86="(64)" INPUT_DEVICES="wacom" VIDEO_CARDS="-nvidia"
    x11-base/xwayland required by (kde-plasma/kwin-5.22.4-r1:5/5::gentoo, installed) USE="handbook multimedia plasma screencast -accessibility -caps -debug -gles2-only -test" ABI_X86="(64)"
```
### Solution

Portage needs a nudge to consider [x11-libs/libxcvt](https://packages.gentoo.org/packages/x11-libs/libxcvt) instead given it's supported by e.g. [x11-wm/mutter](https://packages.gentoo.org/packages/x11-wm/mutter):
```
	wayland? (
		>=sys-kernel/linux-headers-4.4
		|| (
			x11-libs/libxcvt
			<x11-base/xorg-server-1.20.11-r3[xorg,-minimal]
		)
	)
```

That is, run (need `-1` to avoid adding libxcvt to world file)

`root #``emerge -a -1 -uvDU @world x11-libs/libxcvt` ## jack-connection-kit, virtual/jack

Manifests as:

`root #``emerge -p -uvDU @world````
Calculating dependencies ... done!
[ebuild  N     ] media-sound/jack2-1.9.19:2::gentoo  USE="alsa classic dbus pam readline -doc -ieee1394 -libsamplerate -metadata -opus -sndfile" ABI_X86="(64) -32 (-x32)" PYTHON_SINGLE_TARGET="python3_9 (-python3_10) -python3_8" 966 KiB
[ebuild  N     ] media-plugins/gst-plugins-jack-1.18.4-r1:1.0::gentoo  USE="-test" ABI_X86="(64) -32 (-x32)" 0 KiB
[ebuild     U  ] www-client/chromium-94.0.4606.61::gentoo [93.0.4577.63::gentoo] USE="cups hangouts js-type-check (pic) proprietary-codecs pulseaudio suid system-harfbuzz%* system-icu (-component-build) -custom-cflags -debug% (-headless) -kerberos -official -screencast (-selinux) (-system-ffmpeg) -vaapi -wayland -widevine" L10N="fr -am -ar -bg -bn -ca -cs -da -de -el -en-GB -es -es-419 -et -fa -fi -fil -gu -he -hi -hr -hu -id -it -ja -kn -ko -lt -lv -ml -mr -ms -nb -nl -pl -pt-BR -pt-PT -ro -ru -sk -sl -sr -sv -sw -ta -te -th -tr -uk -vi -zh-CN -zh-TW" 0 KiB
[ebuild     U  ] media-plugins/gst-plugins-meta-1.18.4:1.0::gentoo [1.16.3:1.0::gentoo] USE="X a52 aac alsa cdda dts dv dvb dvd ffmpeg flac http jack lame libass libvisual mms modplug mp3 mpeg ogg opus pulseaudio taglib theora v4l vaapi vcd vorbis vpx wavpack x264 -oss" ABI_X86="(64) -32 (-x32)" 0 KiB
[blocks B      ] media-sound/jack-audio-connection-kit:0 ("media-sound/jack-audio-connection-kit:0" is blocking media-sound/jack2-1.9.19)
Total: 4 packages (2 upgrades, 2 new), Size of downloads: 966 KiB
Conflict: 1 block (1 unsatisfied)
 * Error: The above package list contains packages which cannot be
 * installed at the same time on the same system.
  (media-sound/jack-audio-connection-kit-0.125.0-r1:0/0::gentoo, installed) pulled in by
    media-sound/jack-audio-connection-kit[abi_x86_32(-)?,abi_x86_64(-)?,abi_x86_x32(-)?,abi_mips_n32(-)?,abi_mips_n64(-)?,abi_mips_o32(-)?,abi_s390_32(-)?,abi_s390_64(-)?] (media-sound/jack-audio-connection-kit[abi_x86_32(-),abi_x86_64(-)]) required by (virtual/jack-2:0/0::gentoo, installed) USE="" ABI_X86="32 (64) (-x32)"
  (media-sound/jack2-1.9.19:2/2::gentoo, ebuild scheduled for merge) pulled in by
    media-sound/jack2[abi_x86_32(-)?,abi_x86_64(-)?,abi_x86_x32(-)?,abi_mips_n32(-)?,abi_mips_n64(-)?,abi_mips_o32(-)?,abi_s390_32(-)?,abi_s390_64(-)?] (media-sound/jack2[abi_x86_64(-)]) required by (media-plugins/gst-plugins-jack-1.18.4-r1:1.0/1.0::gentoo, ebuild scheduled for merge) USE="-test" ABI_X86="(64) -32 (-x32)"
```
Note that in this case, given [virtual/jack](https://packages.gentoo.org/packages/virtual/jack) is currently installed with ABI\_X86="32 64", if switching to jack2, we need it enabled for jack2 as well.
But even when making this change, Portage is still sometimes confused:

`root #``emerge -p -uvDU @world````
[ebuild  N     ] media-sound/jack2-1.9.19:2::gentoo  USE="alsa classic dbus pam readline -doc -ieee1394 -libsamplerate -metadata -opus -sndfile" ABI_X86="32 (64) (-x32)" PYTHON_SINGLE_TARGET="python3_9 (-python3_10) -python3_8" 966 KiB
[ebuild  N     ] media-plugins/gst-plugins-jack-1.18.4-r1:1.0::gentoo  USE="-test" ABI_X86="(64) -32 (-x32)" 0 KiB
[ebuild     U  ] www-client/chromium-94.0.4606.61::gentoo [93.0.4577.63::gentoo] USE="cups hangouts js-type-check (pic) proprietary-codecs pulseaudio suid system-harfbuzz%* system-icu (-component-build) -custom-cflags -debug% (-headless) -kerberos -official -screencast (-selinux) (-system-ffmpeg) -vaapi -wayland -widevine" L10N="fr -am -ar -bg -bn -ca -cs -da -de -el -en-GB -es -es-419 -et -fa -fi -fil -gu -he -hi -hr -hu -id -it -ja -kn -ko -lt -lv -ml -mr -ms -nb -nl -pl -pt-BR -pt-PT -ro -ru -sk -sl -sr -sv -sw -ta -te -th -tr -uk -vi -zh-CN -zh-TW" 0 KiB
[ebuild     U  ] media-plugins/gst-plugins-meta-1.18.4:1.0::gentoo [1.16.3:1.0::gentoo] USE="X a52 aac alsa cdda dts dv dvb dvd ffmpeg flac http jack lame libass libvisual mms modplug mp3 mpeg ogg opus pulseaudio taglib theora v4l vaapi vcd vorbis vpx wavpack x264 -oss" ABI_X86="(64) -32 (-x32)" 0 KiB
[blocks B      ] media-sound/jack-audio-connection-kit:0 ("media-sound/jack-audio-connection-kit:0" is blocking media-sound/jack2-1.9.19)
Total: 4 packages (2 upgrades, 2 new), Size of downloads: 966 KiB
Conflict: 1 block (1 unsatisfied)
 * Error: The above package list contains packages which cannot be
 * installed at the same time on the same system.
  (media-sound/jack-audio-connection-kit-0.125.0-r1:0/0::gentoo, installed) pulled in by
    media-sound/jack-audio-connection-kit[abi_x86_32(-)?,abi_x86_64(-)?,abi_x86_x32(-)?,abi_mips_n32(-)?,abi_mips_n64(-)?,abi_mips_o32(-)?,abi_s390_32(-)?,abi_s390_64(-)?] (media-sound/jack-audio-connection-kit[abi_x86_32(-),abi_x86_64(-)]) required by (virtual/jack-2:0/0::gentoo, installed) USE="" ABI_X86="32 (64) (-x32)"
  (media-sound/jack2-1.9.19:2/2::gentoo, ebuild scheduled for merge) pulled in by
    media-sound/jack2[abi_x86_32(-)?,abi_x86_64(-)?,abi_x86_x32(-)?,abi_mips_n32(-)?,abi_mips_n64(-)?,abi_mips_o32(-)?,abi_s390_32(-)?,abi_s390_64(-)?] (media-sound/jack2[abi_x86_32(-),abi_x86_64(-)]) required by (virtual/jack-2:0/0::gentoo, installed) USE="" ABI_X86="32 (64) (-x32)"
    media-sound/jack2[abi_x86_32(-)?,abi_x86_64(-)?,abi_x86_x32(-)?,abi_mips_n32(-)?,abi_mips_n64(-)?,abi_mips_o32(-)?,abi_s390_32(-)?,abi_s390_64(-)?] (media-sound/jack2[abi_x86_64(-)]) required by (media-plugins/gst-plugins-jack-1.18.4-r1:1.0/1.0::gentoo, ebuild scheduled for merge) USE="-test" ABI_X86="(64) -32 (-x32)"
```
In such cases, try adding a package.mask entry for [media-sound/jack-audio-connection-kit](https://packages.gentoo.org/packages/media-sound/jack-audio-connection-kit) to help/nudge Portage's dependency resolution:

**`/etc/portage/package.mask`**

```
# Temporary mask to help Portage resolve a conflict
# https://wiki.gentoo.org/index.php?title=User:Sam/Portage_help/Blockers
media-sound/jack-audio-connection-kit
```
## libcrypt and glibc

## Qt

See [Qt/FAQ](https://wiki.gentoo.org/wiki/Qt/FAQ)
