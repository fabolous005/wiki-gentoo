<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X1_Carbon_8th_generation | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad X1 Carbon 8th generation -->
---
title: Lenovo ThinkPad X1 Carbon 8th generation
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X1_Carbon_8th_generation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-02-26"
fingerprint: "955ab8075b96817d"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad X1 Carbon 8th generation

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

Lenovo has announced Linux support for numerous of its systems.[\[1\]](https://wiki.gentoo.org#cite_note-1)

Amongst them the Lenovo "ThinkPad X1 Carbon Gen 8".

Quoting Igor Bergman, Vice President of PCSD Software & Cloud at Lenovo, "\[...\] Our goal is to remove the complexity and provide the Linux community with the premium experience that our customers know us for. This is why we have taken this next step to offer Linux-ready devices right out of the box \[...\]".

These are steps on getting Gentoo fully functional on the Lenovo X1 Carbon, 8th generation.

The Arch Linux wiki pages for the 7th<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> and 8th<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> generation of this laptop had been a great source of information.

As well as ThinkWiki<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>.

## Hardware

### System Model Verfication

`root #``dmidecode -s system-version`
ThinkPad X1 Carbon Gen 8



### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | Intel(R) Core(TM) i7-10510U CPU |  | N/A | N/A | 5.8.13 |  | 
| Video card | Intel Corporation UHD Graphics |  | 8086:9b41 | i915 | 5.8.13 |  | 
| Wireless | Intel Corporation Wireless-AC 9462 |  | 8086:02f0 | iwlmvm | 5.8.13 |  | 
| Speakers | Skylake+ |  | 8086:02c8 | snd\_soc\_skl\_hda\_dsp, snd\_hda\_intel | 5.8.13 |  | 
| Microphone | Skylake+ |  | 8086:02c8 | snd\_soc\_skl\_hda\_dsp, snd\_hda\_intel | 5.8.13 | DSP Digital mic, Skylake+ platform | 
| Touchpad | Synaptics |  | 06cb:00bd | hid-generic,hid-multitouch | 5.8.13 | USB | 
| Trackpoint | Elan |  | N/A | psmouse | 5.8.13 |  | 
| Cameras (internal) | Chicony Electronics |  | 04f2:b6cb | uvcvideo | 5.8.13 | USB, Two cameras, C & I | 
| Fingerprint reader | Synaptics |  | 06cb:00bd | N/A | 5.8.13 | This device is reported to work by Arch Linux | 
| Power Management | Lenovo |  | N/A | thinkpad\_acpi | 5.8.13 | Including keyboard backlight and Fn keys | 
| Bluetooth | Intel |  | 8087:0026 | btusb | 5.8.13 | USB | 
| Disk | Samsung |  | 144d:a808 | nvme | 5.8.13 | This might vary on your system | 



### Accessories

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Docking Station | ThinkPad Pro Dock |  | N/A | N/A | N/A | To be tested | 



## Installation



### BIOS

The following settings are recommended<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup>.



### USEflags

| USEflag | Purpose | 
|---|---|
| `alsa` | The ALSA Linux sound system | 
| `alsa-plugin` | Plugins for ALSA | 
| `bluetooth` | Bluetooth for Linux | 
| `nvme` | nvme disk support and firmware update | 
| `synaptics` | Synaptics touchpad and fingerprint reader support und firmware update | 
| `thunderbolt` | Thunderbolt support and firwmare update | 
| `tpm` | Trusted Platform support and firmware update | 
| `uefi` | UEFI support and firmware update | 



### Firmware

Use fwupdmgr to update your systems firmware. Note to USEflags section. System must be started in efi mode and `efivarfs` mounted.

`root #``emerge --ask sys-apps/fwupd`
Fetch information from upstream.

`root #``fwupdmgr refresh`
Update the system firmware. This will involve reboots. Repeat until no more updates applicable.

`root #``fwupdmgr update`
### Audio



#### Alsa

Add `hdsp` and `hdspm` alsa cards to the standard alsa cards.

**`/etc/portage/package.use/00audio`**

```
 ALSA_CARDS: hdsp hdspm
```
The following packaes needed to be installed.

`root #``emerge --ask sys-firmware/alsa-firmware media-sound/alsa-utils media-libs/alsa-topology-conf media-libs/alsa-ucm-conf sys-firmware/sof-firmware`
Run `alsamixer`, tune up the volume master and all microphones to maximum and store the settings.

`root #``alsamixer``root #``alsactl store`
#### Pulseaudio

The DMIC (Digital Microphone) consists of 4 channels and that topology must be specifically set for pulseaudio.

**`/etc/pulse/default.pa`**

Finally restart the system.



## See also



## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [Lenovo Launches Linux-Ready ThinkPad and ThinkStation PCs Preinstalled with Ubuntu](https://news.lenovo.com/pressroom/press-releases/lenovo-launches-linux-ready-thinkpad-and-thinkstation-pcs-preinstalled-with-ubuntu/), accessed 9th Oct 2020
2. [↑](https://wiki.gentoo.org#cite_ref-2) [https://wiki.archlinux.org/index.php/Lenovo\_ThinkPad\_X1\_Carbon\_(Gen\_7)](<https://wiki.archlinux.org/index.php/Lenovo_ThinkPad_X1_Carbon_(Gen_7)>), accessed 9th Oct 2020
3. [↑](https://wiki.gentoo.org#cite_ref-3) [https://wiki.archlinux.org/index.php/Lenovo\_ThinkPad\_X1\_Carbon\_(Gen\_8)](<https://wiki.archlinux.org/index.php/Lenovo_ThinkPad_X1_Carbon_(Gen_8)>), accessed 9th Oct 2020
4. [↑](https://wiki.gentoo.org#cite_ref-4) [https://www.thinkwiki.org/wiki/ThinkWiki](https://www.thinkwiki.org/wiki/ThinkWiki), accessed 9th Oct 2020
5. [↑](https://wiki.gentoo.org#cite_ref-5) [Arch Linux Recommendation](<https://wiki.archlinux.org/index.php/Lenovo_ThinkPad_X1_Carbon_(Gen_7)#BIOS_configurations>), accessed 9th Oct 2020
