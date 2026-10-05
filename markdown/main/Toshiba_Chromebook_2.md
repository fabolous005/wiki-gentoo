<!-- source: https://wiki.gentoo.org/wiki/Toshiba_Chromebook_2 | group: Gentoo Wiki (Main) | wiki-title: Toshiba Chromebook 2 -->
---
title: Toshiba Chromebook 2
url: https://wiki.gentoo.org/wiki/Toshiba_Chromebook_2
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: d72fb07711ea5b6f
license: CC BY-SA 4.0
---

# Toshiba Chromebook 2

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



## Summary

Everything works as of 13th August, after this [SeaBIOS update](https://johnlewis.ie/baytrail-update/) by John Lewis. Latest ChromeOS fw + John Lewis' SeaBIOS has both working suspend and working VPX (kvm)

## Hardware

- Intel Celeron N2840 - Dual Core, fanless
- 4 GB RAM (for model CB30-B-104) or 2 GB RAM (for model CB30-B-103)
- 1080p IPS screen (non-touch)
- Elan Touchpad
- 16 GB eMMC card as storage (embedded in mainboard, cannot be replaced)
- SD Card slot
- Intel 7260 AC-Wifi with Bluetooth
- Chromebook codename from Google: swanky

## Disclaimer/Prerequisites

- You have to open the machine, dismantle and cut off a piece of the heat shield to disable flash write protect.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-coreboot-1)</sup>
- You must install [John Lewis' SeaBIOS](https://johnlewis.ie/custom-chromebook-firmware/rom-download/) to the flash, and to begin this you first have to do the aforementioned modification, and then enable developer mode on your Chromebook. You can enter this mode in the same way as on a Acer C7 Chromebook, and the procedure is described [here](https://www.chromium.org/chromium-os/developer-information-for-chrome-os-devices/acer-c7-chromebook#TOC-Entering).
- You should have a large SD Card (SDXC UHC-1-type cards seems to work) if you want to use it as a normal laptop, as 16 GB storage can be a tad small. The internal storage cannot be upgraded, it's soldered to the mainboard.
- The swanky board code, blobs and config are not upstreamed to coreboot as of 2015/08/21, so it's not an easy task to compile your own firmware for this. So, the best (and maybe only) solution yet is to use John Lewis' SeaBIOS flasher.

## Notes

- The sound driver name is CONFIG\_SND\_SOC\_INTEL\_BYT\_MAX98090\_MACH
- You cannot use genkernel on this if you want sound, the sound driver (BYT\_MAX98090) depends on having all other codecs disabled, so normal distro kernels do not support sound<sup>[\[2\]](https://wiki.gentoo.org#cite_note-BYT_MAX98090-2)</sup>
- This makes the arch wiki<sup>[\[3\]](https://wiki.gentoo.org#cite_note-Arch_Wiki_Toshiba_Chromebook_2-3)</sup> refer to sound not having support after kernel 4.5.0, but this is not true, you just have to enable the right config and disable all others.
- First time you boot you might have to use alsamixer and unmute "Left Speaker Mixer Right DAC" and "Right Speaker Mixer Right DAC" (see [this blogpost](https://johnlewis.ie/procedure-to-get-sound-working-in-fedora-22-on-asus-c300-chromebook/))
- Setting up the kernel config is a bit hard if doing it from scratch. A nice way to make your own kernel config right, is using a Fedora LiveUSB environment and use \`make localmodconfig´ or \`make localyesconfig´ when configuring your kernel.
- The kernel need some msecs of time to enumerate your eMMC device and eventual SD card, so you won't get this system to boot without either an initrd, or if you don't use an initrd, the kernel boot parameters \`rootdelay=2´. One second might be enough, but with two seconds you're sure.
- On my setup I use pulseaudio, and I had to have this .asoundrc<sup>[\[4\]](https://wiki.gentoo.org#cite_note-PulseAudio_Perfect_Setup-4)</sup> file in my home folder to make sound in alsa applications works (for instance adobe-flash)

FILE **`.asoundrc`**
