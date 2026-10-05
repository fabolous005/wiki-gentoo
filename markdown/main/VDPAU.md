<!-- source: https://wiki.gentoo.org/wiki/VDPAU | group: Gentoo Wiki (Main) | wiki-title: VDPAU -->
---
title: VDPAU
url: https://wiki.gentoo.org/wiki/VDPAU
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-02"
fingerprint: "870cdc5b5cef098d"
license: CC BY-SA 4.0
---

# VDPAU

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes how to setup the **V**ideo **D**ecode and **P**resentation **A**PI for **U**nix (VDPAU).

## Hardware support

| Driver | Chipset | 
|---|---|
| [nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) | GeForce 8 (2. Generation) PureVideo HD and newer. See /usr/share/doc/nvidia-drivers-\<version>/html/supportedchips.html for the full list of supported cards. | 
| S3 | Chrome 430 GT and newer | 
| [Radeon](https://wiki.gentoo.org/wiki/Radeon) | Support is available for [r300](https://wiki.gentoo.org/wiki/Radeon#Feature_support) and newer | 

## Installation

### make.conf

Portage knows the global USE flag *vdpau* for enabling support for VDPAU in other packages. Enabling this USE flag will pull in [x11-libs/libvdpau](https://packages.gentoo.org/packages/x11-libs/libvdpau) automatically:

**`/etc/portage/make.conf`**

```
USE="vdpau"
```
After adding the above USE flag update the system so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
### USE flags

The USE flags of libvdpau are:


| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [dri](https://packages.gentoo.org/useflags/dri) | Enable direct rendering: used for accelerated 3D and some 2D, like DMA | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### AMD/Radeon specific steps

By default applications such as mplayer, vlc, vdpauinfo, make VDPAU specific calls via libvdpau library. This library then dynamically loads appropriate back-end driver.

At the time of writing the mechanism to automatically decide which back-end driver needs to be loaded was not established. Currently libvdpau is hardcoded<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> to load NVIDIA backend driver. It means that VDPAU will not work properly on Radeon cards. The only possible way to change that behavior is by specifying the correct back-end driver manually.

First you need to find the name of the driver related to your hardware (here we assume that the *vdpau* USE flag was enabled and the system was updated successfully). One way to find that name is by checking Xorg log file:

`user $``grep -i vdpau /var/log/Xorg.0.log`
(II) RADEON(0): \[DRI2\]   VDPAU driver: r300

The output will show if VDPAU driver has been initialized properly (two leading (II) letters in the sample output above). It also should specify the name of back-end driver (r300 in the sample output).

Now you can manually setup the name of back-end driver with help of VDPAU\_DRIVER environment variable. To do that you need to add the following line to \~/.bashrc file (provided that Bash is the default shell of a user who is going to run graphical environment). For the sample case described above the mentioned line would look like:

**`~/.bashrc`**

**Logged in as a user who run graphical interface**

```
export VDPAU_DRIVER=r300
```
Now [x11-misc/vdpauinfo](https://packages.gentoo.org/packages/x11-misc/vdpauinfo) should show you an information about your VDPAU configuration instead of an error message.

`user $``vdpauinfo`
display: :0   screen: 0
API version: 1
Information string: G3DVL VDPAU Driver Shared Library version 1.0
 
Video surface:
 
name   width height types
-------------------------------------------
420     2048  2048  NV12 YV12
422     2048  2048  NV12 YV12
444     2048  2048  NV12 YV12 Y8U8V8A8 V8U8Y8A8
 
Decoder capabilities:
 
name               level macbs width height
-------------------------------------------
MPEG1                16 16384  2048  2048
MPEG2\_SIMPLE         16 16384  2048  2048
MPEG2\_MAIN           16 16384  2048  2048
...

#### Additional software

- [x11-misc/vdpauinfo](https://packages.gentoo.org/packages/x11-misc/vdpauinfo): shows if VDPAU is supported.
- VDPAU can be used as a [VAAPI](https://wiki.gentoo.org/wiki/VAAPI) backend.

### NVIDIA specific steps

With the proprietary driver vdpau works out of the box, however [nouveau](https://wiki.gentoo.org/wiki/Nouveau) driver requires firmware:

`root #``emerge nvidia-firmware`
## Configuration

You may need to tweak the config files of the programs, you can make use of *VDPAU*.

### mplayer

Detect the available VDPAU video codecs:

`user $``mplayer -vc help | grep --color vdpau`
Add them to the mplayer config file, e.g.:

**`/etc/mplayer/mplayer.conf`**

```
[vo.vdpau]
vc=ffh264vdpau,ffodivxvdpau,ffmpeg12vdpau,
```
## See also

- [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU) — the open source graphics drivers for AMD Radeon and other GPUs.
- [AMDGPU-PRO](https://wiki.gentoo.org/wiki/AMDGPU-PRO) — the next generation *closed source* graphics component that operates on top of the open source [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU) drivers for newer AMD/ATI Radeon graphics cards.
- [VAAPI](https://wiki.gentoo.org/wiki/VAAPI) — provides access to graphics hardware (GPU) acceleration for video processing.
- [Vulkan](https://wiki.gentoo.org/wiki/Vulkan) — a next-generation graphics API created by The Khronos Group.
- [Xorg/Hardware 3D acceleration guide](https://wiki.gentoo.org/wiki/Xorg/Hardware_3D_acceleration_guide) — a guide to getting 3D acceleration working using the DRM with Xorg in Gentoo.
