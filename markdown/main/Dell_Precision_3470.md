<!-- source: https://wiki.gentoo.org/wiki/Dell_Precision_3470 | group: Gentoo Wiki (Main) | wiki-title: Dell Precision 3470 -->
---
title: Dell Precision 3470
url: https://wiki.gentoo.org/wiki/Dell_Precision_3470
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: "5505627fd0e3b1c7"
license: CC BY-SA 4.0
---

# Dell Precision 3470

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is a work in progress as I manage thru the installation and initial setup. Please add from your experience.



## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | 12th Gen Intel Core i7-1270p |  |  | N/A | 6.1.2 |  | 
| Video card (integrated) | Intel Iris |  |  |  | 6.1.2 | Note example. | 
| Video card | Nvidia T550 |  |  | nvidia (propriety) | 6.1.2 | Note example. | 
| Wifi |  |  |  |  |  |  | 
| Bluetooth |  |  |  |  |  |  | 
| Sound |  |  |  |  |  |  | 
| Camera (RGB) | Microdia Integrated Webcam FHD |  | 0c45:6733 |  |  |  | 
| Camera (IR) |  |  |  |  |  | Using v4l2. /dev/video0 | 
| Fingerprint Reader | Broadcom ControlVault 3 |  | 0a5c:5843 |  |  | Not recognized by fprintd <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> | 



## Installation

Look at the troubleshooting section for info about installation media not loading.

For installation use kernel 6.1 and above (the 5.15) doesn't work. In order to do so, it is necessary at this time (January 4th, 2 3) to use the testing package. In order to do so add the \~amd64 keyword to the relevant packages. If using distribution kernel one can do so by creating the following file:

**`/etc/portage/package.accept_keywords/gentoo-kernel`**

### Kernel

In order to get the framebuffer working at the earliest time possible (necessary for example if using encrypted LUKS root partition), you should enable the EFI framebuffer support:

**Enable EFI-based Framebuffer Support (CONFIG\_FB\_EFI)**

Alternatively, if you use the distribution kernel, you can create the following file to add the option:

**`/etc/kernel/config.d/100_framebuffer.config`**

## Configuration

In order to boot the Gentoo installation media, first you need to disable secure boot.

### Bluetooth

Make sure to run appropriate service according to [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth):

`root #``rc-service bluetooth start``root #``rc-update add bluetooth default`
### Fingerprint Reader

Fprintd does not recognize the fingerprint reader out of the box.

It seems that Ubuntu has a propriety driver for this fingerprint reader<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>.

## Troubleshooting

### Intel video driver with \<=5.15 kernel

Using distribution kernel version 5.15 result in video freeze due to incompatible Intel display driver. One should use kernel 6.1 from testing. As long as kernel 6.1 is in testing, this can be done by adding the \~amd64 keyword prior to emerging the kernel. If the Gentoo liveUSB is used for the installation, it also sometimes freezes during loading. If that the case, either try again (for some reason this is not consistent with the liveUSB unlike with the install) or use a different distro liveUSB for the installation.

### Xorg fails on boot but works later

This is due to a race condition while loading the kernel modules and Xorg. Please add the i915 driver to the initramsfs.

**`/etc/dracut.conf.d/nvidiaoptimus.conf`**

Regenerate the initramfs image:

`root #``dracut --force`
Please see [NVIDIA/Optimus](https://wiki.gentoo.org/wiki/NVIDIA/Optimus#Xorg).
