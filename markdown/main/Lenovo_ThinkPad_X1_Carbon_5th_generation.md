<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X1_Carbon_5th_generation | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad X1 Carbon 5th generation -->
---
title: Lenovo ThinkPad X1 Carbon 5th generation
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X1_Carbon_5th_generation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "6b925a3f57829bb0"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad X1 Carbon 5th generation

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

These are my notes on getting my Thinkpad X1 Carbon 5th gen working under Gentoo.

Overall, it's been pretty straight forward and worked well. Be sure to check out [The ArchLinux wiki](<https://wiki.archlinux.org/index.php/Lenovo_ThinkPad_X1_Carbon_(Gen_5)>) for some information as well.

If this is useful, please contribute - I didn't write great notes as I went.

### SSD

Worked well out of the box, but took me ten minutes to find it under [/dev/nvme0](https://wiki.gentoo.org/wiki/NVMe).
I assume you need the "CONFIG\_BLK\_DEV\_NVME", but I didn't test without.

### Graphics

Using the i915 kernel module (which X11 calls "intel").

make.conf has [VIDEO\_CARDS](https://wiki.gentoo.org/wiki//etc/portage/make.conf#VIDEO_CARDS)="intel i965"

Currently DRI disabled with: Option "DRI" "false", not sure if I need that.

### Sound - Speakers, Microphone

Worked out of the box.

Alsamixer reports: Card: HDA Intel PCH Chip: Conexant CX8200

### Wifi

Worked out of the box, [iwlwifi](https://wiki.gentoo.org/wiki/Iwlwifi) module

### 4G / LTE / "Qualcomm® Snapdragon™ X7 LTE-A" / EM7455

I haven't got this working yet, but I've emailed the mbim developers for some information [here](https://lists.freedesktop.org/archives/libmbim-devel/2017-October/000960.html).

Uses the cdc\_mbim module, device shows up under /dev/cdc-wdm0, mine's stuck in "sim-missing". It may be due to me trying Project Fi as a carrier.

### Webcam

Works well, can't compare quality or anything but no setup issues. Used the "uvcvideo" module.

### Thunderbolt Dock - 40AC0135US

I've tested running multiple monitors (via displayport), and usb devices, over this dock.

It works great out of the box, and for hotplugging I needed to enable CONFIG\_HOTPLUG\_PCI\_ACPI in the kernel.
