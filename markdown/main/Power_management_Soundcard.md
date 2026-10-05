<!-- source: https://wiki.gentoo.org/wiki/Power_management/Soundcard | group: Gentoo Wiki (Main) | wiki-title: Power management/Soundcard -->
---
title: Power management/Soundcard
url: https://wiki.gentoo.org/wiki/Power_management/Soundcard
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-25"
fingerprint: de077956c3aa7b20
license: CC BY-SA 4.0
---

# Power management/Soundcard

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the setup of [power management](https://wiki.gentoo.org/wiki/Power_management) of [sound devices](https://wiki.gentoo.org/wiki/Category:Sound_devices).

## Power-saving mode

The Intel HDA driver has a power-saving mode to suspend the soundcard after some time of inactivity.

### Kernel

Systems with an AC97 soundcard need to manage the following kernel options:

Device Drivers  --->
  \<\*> Sound card support ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SOUND\</code> to find this item.
    \<\*> Advanced Linux Sound Architecture ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\</code> to find this item.
      \[\*\] Generic sound devices ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_DRIVERS\</code> to find this item.
        \[\*\] AC97 Power-Saving Mode [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_AC97\_POWER\_SAVE\</code> to find this item.
        (10) Default time-out for AC97 power-save mode [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_AC97\_POWER\_SAVE\_DEFAULT\</code> to find this item.
      \[\*\] PCI sound devices ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_PCI\</code> to find this item.
        \<\*> Intel/SiS/nVidia/AMD/ALi AC97 Controller [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_INTEL8X0\</code> to find this item.

Systems with an HDA soundcard need to manage the following kernel options:

Device Drivers  --->
  \<\*> Sound card support ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SOUND\</code> to find this item.
    \<\*> Advanced Linux Sound Architecture ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\</code> to find this item.
      \[\*\] PCI sound devices ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_PCI\</code> to find this item.
      HD-Audio --->
        (1) Default time-out for HD-audio power-save mode [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_HDA\_POWER\_SAVE\_DEFAULT\</code> to find this item.
        (2048) Pre-allocated buffer size for HD-audio driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_HDA\_PREALLOC\_SIZE\</code> to find this item.
        \<M> HD-audio HDMI codec support ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_HDA\_CODEC\_HDMI\</code> to find this item.
          \<M> Intel HDMI/DisplayPort HD-audio codec support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_HDA\_CODEC\_HDMI\_INTEL\</code> to find this item.
          \[ \] Enable Silent Stream always for HDMI [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_HDA\_INTEL\_HDMI\_SILENT\_STREAM\</code> to find this item.

When the driver is loaded as a module then it may be needed to enable power saving by means of a module parameter:

### Runtime tuning

The driver can be tuned in the [sysfs](https://wiki.gentoo.org/wiki/Sysfs) filesystem under /sys/module/snd\_hda\_intel/parameters:

- The power\_save\_controller knob controls, if power-saving mode is enabled. It is preset by the kernel option *... power-saving ...*.
- The power\_save knob sets the time-out in seconds. It is preset by the kernel option *Default time-out ...*

### pm-utils

The [sys-power/pm-utils](https://packages.gentoo.org/packages/sys-power/pm-utils) package contains a script to enable the power-saving mode when on battery and disable when on AC. It overrides the default values of the kernel.

If you use pm-utils, but don't want this kind of regulation, disable the script:

`root #``touch /etc/pm/power.d/intel-audio-powersave`
## Troubleshooting

If you hear an unwanted click sound when the power-saving mode enables, disable the power-saving mode in the kernel.
