<!-- source: https://wiki.gentoo.org/wiki/PCSX2 | group: Gentoo Wiki (Main) | wiki-title: PCSX2 -->
---
title: PCSX2
url: https://wiki.gentoo.org/wiki/PCSX2
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-25"
fingerprint: b5033c15586799f4
license: CC BY-SA 4.0
---

# PCSX2

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**PCSX2** is a free and open-source PlayStation 2 emulator for x86 computers. It supports most PlayStation 2 games with high compatibility rates and can apply enhancements to gameplay, including features like widescreen patches, improved resolutions or use of custom textures.

## Installation


| [+clang](https://packages.gentoo.org/useflags/+clang) | Use Clang compiler to build (the only compiler that is currently supported by upstream on Linux, do not report bugs if force-disabled) | 
| [+filecaps](https://packages.gentoo.org/useflags/+filecaps) | Use Linux file capabilities to control privilege rather than set\*id (this is orthogonal to USE=caps which uses capabilities at runtime e.g. libcap) | 
| [alsa](https://packages.gentoo.org/useflags/alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [sndio](https://packages.gentoo.org/useflags/sndio) | Enable support for the media-sound/sndio backend | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 

### Emerge

PCSX2 is available in the main Gentoo repository. You can install it by issuing:

`root #``emerge --ask games-emulation/pcsx2`
#### Unstable version

PCSX2 is a fast moving piece of software, and lots of bugs and compatibility issues are fixed daily. Upstream usually recommends using the unstable version of PCSX2, which may be enabled via adding PCSX2 to package.accept\_keywords. An example file may look like this:

**`/etc/portage/package.accept_keywords/pcsx2`**

Then PCSX2 should be built:

`root #``emerge --ask games-emulation/pcsx2`
### Flatpak

PCSX2 is also available on Flathub, the main [Flatpak](https://wiki.gentoo.org/wiki/Flatpak) repository.
To add the flathub repository:

And then install the PCSX2 package:

`user $``flatpak install --user flathub net.pcsx2.PCSX2`
## Configuration

PCSX2's configuration folder is automatically set to \~/.config/PCSX2 (or \~/.var/app/net.pcsx2.PCSX2/config/PCSX2 for Flatpak installations). This folder contains subfolders where BIOS files, virtual Memory Cards, save states, screenshots and cheats are stored by default, amongst other resources. These paths can be changed inside PCSX2's GUI configuration menu.

## Troubleshooting

### PCSX2 requires a PS2 BIOS in order to run

Because of copyright law, PCSX2 can't nor will ever provide any BIOS files with the emulator. Check the official documentation at [https://pcsx2.net/docs/setup/bios](https://pcsx2.net/docs/setup/bios) to learn how to dump the BIOS files from an original PlayStation 2 to a computer, necessary to run games on PCSX2.

### Games run slow or have graphical glitches

The PlayStation 2 may not be as easy to emulate as previous, older video game consoles. A game running slow may be because of underpowered hardware, a rendering or compatibility issue with the specific game, or a combination of the two. PCSX2's minimum (and recommended) hardware requirements are listed at [https://pcsx2.net/docs/setup/requirements](https://pcsx2.net/docs/setup/requirements), and there's a full game compatibility list at [https://pcsx2.net/compat/](https://pcsx2.net/compat/) for users to check if a video game is considered playable in the first place.

## See also

- [Games/emulation](https://wiki.gentoo.org/wiki/Games/emulation) — provides an overview of game emulators available in the ::gentoo ebuild repository.
