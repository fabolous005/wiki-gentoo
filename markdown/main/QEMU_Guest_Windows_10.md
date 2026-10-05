<!-- source: https://wiki.gentoo.org/wiki/QEMU/Guest/Windows_10 | group: Gentoo Wiki (Main) | wiki-title: QEMU/Guest/Windows 10 -->
---
title: QEMU/Guest/Windows 10
url: https://wiki.gentoo.org/wiki/QEMU/Guest/Windows_10
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-01"
fingerprint: "2c8d8a7f9d22f0d4"
license: CC BY-SA 4.0
---

# QEMU/Guest/Windows 10

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the setup of a Windows guest using [QEMU](https://wiki.gentoo.org/wiki/QEMU).

## Configuration

### Host

To create a disk image for the virtual machine, run:

`user $``qemu-img create -f qcow2 WindowsVM.img 25G`
Download a Windows driver image from [this location](https://github.com/virtio-win/virtio-win-pkg-scripts/blob/master/README.md).

Since QEMU requires a lot of [options](https://wiki.gentoo.org/wiki/QEMU/Options), it would be a good idea to put them into a shell script, e.g.:

**`WindowsVM.sh`**

```
#!/bin/sh
exec qemu-system-x86_64 -enable-kvm \
        -cpu host \
        -drive file=WindowsVM.img,if=virtio \
        -net nic -net user,hostname=windowsvm \
        -m 1G \
        -monitor stdio \
        -name "Windows" \
        "$@"
```
Change the path to the disk image WindowsVM.img in the script. Additional options can be used when calling the script. To boot the disk image, run:

`user $``./WindowsVM.sh -boot d -drive file=WINDOWS.iso,media=cdrom -drive file=DRIVER.iso,media=cdrom`
Install the Windows guest. See the [guest section](https://wiki.gentoo.org#Guest) for optimum support. After the installation start the script without the additional options.

### Guest

- During installation at the partition step Windows doesn't detect the VirtIO hard drive. Windows will require the *viostor* driver. Emerge them via [app-emulation/virtio-win](https://packages.gentoo.org/packages/app-emulation/virtio-win) or download rpm packages or ISOs [here](https://github.com/virtio-win/virtio-win-pkg-scripts/blob/master/README.md). For more information see Fedoras [documentation](https://docs.fedoraproject.org/en-US/quick-docs/creating-windows-virtual-machines-using-virtio-drivers/index.html).
- After installation the VirtIO Ethernet adapter will appear without a driver. Windows will require the *netkvm* driver from the driver image listed above.

- For 32-bit Windows 7 a sound driver for the [Intel 82801AA AC97](http://www.linux-kvm.org/page/Sound) exists.

- For 64-bit Windows 7 Intel HDA is available as an option (QEMU option: `-soundhw hda`)
- Windows 10 guest audio emulation is tricky for ALSA users. Pass these options to QEMU:`-audiodev alsa,id=snd0,out.try-poll=off -device ich9-intel-hda -device hda-output,audiodev=snd0`
  - This assumes use of the default ALSA device for playback.
  - we cannot use AC97 for Windows 10 because there is no driver for it.
  - `out.try-poll=off` is an odd option, but without it I got nothing but clicks and pops during playback.
  - there may be more options required for line-in and line-out (`-device hda-duplex`) or microphone and line-out (`-device hda-micro`).
  - For PulseAudio, see [ArchWiki](https://wiki.archlinux.org/index.php/QEMU#Guest)

- USB 2.0 pass through can be configured from host to guest with variations of: `-usb -device usb-ehci,id=ehci -device usb-host,bus=ehci.0,vendorid=1452`

- For Windows 8.1 USB tablet is available only with USB 2.0 pass through (QEMU option: `-device usb-ehci,id=ehci -device usb-tablet,bus=ehci.0`
  - The USB tablet device helps the Windows guest to accurately track mouse movements. Without it mouse movements will be jerky.
- Another device that can be presented to the Windows guest is the random number generator.  Add QEMU option: `-device virtio-rng-pci` .  Now install the *viorng* driver from the driver image.
- For Windows 10, to boot using UEFI the [sys-firmware/edk2-bin](https://packages.gentoo.org/packages/sys-firmware/edk2-bin) is required on the host, then add QEMU option: `-bios /usr/share/edk2-ovmf/OVMF_CODE.fd.` to the qemu call. This option is essential for running Hyper-V guest images.

### SPICE

[QEMU](https://wiki.gentoo.org/wiki/QEMU) with [SPICE](<https://en.wikipedia.org/wiki/SPICE_(protocol)>) support enables (among other things) the more powerful QXL display device and makes clipboard sharing possible (copy/paste between clients and the virtual machine).

To use SPICE with [QEMU](https://wiki.gentoo.org/wiki/QEMU) enable the following USE flag in package.use:

**`/etc/portage/package.use`**

**Setting USE variable**

Build QEMU:

`root #``emerge app-emulation/qemu`
To connect spice server, a client like [net-misc/spice-gtk](https://packages.gentoo.org/packages/net-misc/spice-gtk) is required.

#### Guest

On Windows guests, install [Windows guest tools](http://www.spice-space.org/download.html).

On Windows 8.1 guest, to set screen resolution more than 1024x768, install drivers from [https://github.com/virtio-win/virtio-win-pkg-scripts/blob/master/README.md](https://github.com/virtio-win/virtio-win-pkg-scripts/blob/master/README.md) (or emerge app-emulation/virtio-win) and [QXL WDDM DOD driver](https://github.com/vrozenfe/qxl-dod). If trying to install Windows guest tool, QXL WDDM DOD driver won't work well because of Windows SPICE agent .

To try the new qlx-dod driver linked above, in order to use the driver, update' the basic windows display adapter driver and point it to the unzipped folder for the new driver. This can be found by: Right click the Start button -> Device Manager. Expand 'Display adapters' and right click on the sub entry and hit update driver.

#### Initialization script

To run [QEMU](https://wiki.gentoo.org/wiki/QEMU) from a script (the spicy viewer requires [net-misc/spice-gtk](https://packages.gentoo.org/packages/net-misc/spice-gtk)):

**`WindowsVM.sh`**

```
#!/bin/sh
SPICE_PORT=5924
qemu-system-x86_64 -enable-kvm -daemonize \
    -cpu host \
    -drive file=WindowsVM.img,if=virtio \
    -net nic -net user,hostname=windowsvm \
    -m 1G \
    -vga qxl \
    -spice port=${SPICE_PORT},disable-ticketing=on \
    -usbdevice tablet \
    -device virtio-serial \
    -chardev spicevmc,id=vdagent,name=vdagent \
    -device virtserialport,chardev=vdagent,name=com.redhat.spice.0 \
    "$@"
exec spicy --title Windows 127.0.0.1 -p ${SPICE_PORT}
```
Or remote-viewer ([app-emulation/virt-viewer](https://packages.gentoo.org/packages/app-emulation/virt-viewer)):

**`WindowsVM.sh`**

```
#!/bin/sh
SPICE_PORT=5924
qemu-system-x86_64 -enable-kvm -daemonize \
    -cpu host \
    -drive file=WindowsVM.img,if=virtio \
    -net nic -net user,hostname=windowsvm \
    -m 1G \
    -vga qxl \
    -spice port=${SPICE_PORT},disable-ticketing=on \
    -usbdevice tablet \
    -device virtio-serial \
    -chardev spicevmc,id=vdagent,name=vdagent \
    -device virtserialport,chardev=vdagent,name=com.redhat.spice.0 \
    "$@"
exec remote-viewer --title Windows spice://127.0.0.1:${SPICE_PORT}
```
