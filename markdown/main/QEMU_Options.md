<!-- source: https://wiki.gentoo.org/wiki/QEMU/Options | group: Gentoo Wiki (Main) | wiki-title: QEMU/Options -->
---
title: QEMU/Options
url: https://wiki.gentoo.org/wiki/QEMU/Options
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-29"
fingerprint: "76059c55cbaabbc0"
license: CC BY-SA 4.0
---

# QEMU/Options

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



This article describes some of the options useful for configuring [QEMU](https://wiki.gentoo.org/wiki/QEMU) virtual machines (VMs). The most up-to-date reference for options can be found in the [qemu(1)](https://man.archlinux.org/man/qemu.1.en) [man page.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

- `-display curses`
- Displays video output via [curses](<https://en.wikipedia.org/wiki/Curses_(programming_library)>), i.e. [sys-libs/ncurses](https://packages.gentoo.org/packages/sys-libs/ncurses).

- `-display gtk`
- Display video output in a [GTK](https://wiki.gentoo.org/wiki/GTK) window. This is probably the option most users are looking for.

- `-display none`
- Do not display video output. This option differs from the `-nographic` option; refer to the [qemu(1)](https://man.archlinux.org/man/qemu.1.en)

- `-display sdl`
- Display video output via [SDL](https://en.wikipedia.org/wiki/Simple_DirectMedia_Layer) (usually in a separate graphics window).

- `-display vnc=127.0.0.1:<n>`
- On [X](https://wiki.gentoo.org/wiki/X) systems, start a VNC server, listening on the localhost interface, for display `<n>`, where `<n>` should be replaced by the number of the relevant display. That display will then be accessible at port 5900.

- `-machine type=q35,accel=kvm`
- Modern chipset ([PCIe](https://en.wikipedia.org/wiki/PCI_Express), [AHCI](https://en.wikipedia.org/wiki/Advanced_Host_Controller_Interface), ...) and hardware virtualization acceleration using [KVM](https://en.wikipedia.org/wiki/Kernel-based_Virtual_Machine).

- `-object rng-random,id=rng0,filename=/dev/urandom -device virtio-rng-pci,rng=rng0`
- Passthrough for host random number generator, to address slow startup of e.g. Debian VMs due to lack of entropy.

- `-cpu <CPU>`
- Specify a processor architecture to emulate. For a list of supported architectures, use `?` as the argument, e.g. qemu-system-x86\_64 -cpu ?.

- `-cpu host`
- (Recommended) Emulate the host processor.

- `-smp <n>`
- Specify the number of cores the guest is permitted to use. The number can be higher than the available cores on the host system. Use `-smp $(nproc)` to use all currently available cores.

- `-m <mem>`
- Specify the amount of memory to make available, e.g. -m 256M. Default: 128 MB.

**2026-08-29**, the information in this section is probably

**outdated**. You can help the Gentoo community by verifying and

[updating this section](https://wiki.gentoo.org/index.php?title=QEMU/Options&action=edit).

- `-hda <image_file>`
- Use the specified image file as a virtual hard drive.

- `-drive`
- Advanced configuration of a virtual hard drive:

  - `-drive file=<image_file>,if=virtio`
  - Use the specified image file as a virtual virtio-blk hard drive.

  - `-drive file=/dev/sd<X#>,cache=none,if=virtio`
  - Use the specified partition as a virtual virtio-blk hard drive.

  - `-drive id=disk,file=<image_file>,if=none -device ahci,id=ahci -device ide-hd,drive=disk,bus=ahci.0`
  - Use the specified image file with ICH-9 AHCI controller emulation. The AHCI emulation supports [NCQ](https://en.wikipedia.org/wiki/Native_Command_Queuing), so multiple read or write requests can be outstanding at the same time.

  - `-device virtio-scsi-pci,id=scsi0 -drive file=/dev/<block_device>,if=none,format=raw,discard=unmap,aio=native,cache=none,id=<id> -device scsi-hd,drive=<id>,bus=scsi0.0`
  - Very fast virtio [SCSI](https://en.wikipedia.org/wiki/SCSI) emulation for block discards ([TRIM](<https://en.wikipedia.org/wiki/Trim_(computing)>)) and native command queuing ([NCQ](https://en.wikipedia.org/wiki/Native_Command_Queuing)). Requires at least one `virtio-scsi`-controller and, for each block device, a `-drive` and `-device scsi-hd` pair.

- `-cdrom <iso_file>`
- Use the specified ISO file as a virtual CD-ROM drive.

- `-cdrom /dev/cdrom`
- Use the host's CD-ROM drive as a virtual CD-ROM drive.

- `-drive`
- Advanced configuration of a virtual CD-ROM drive:

  - `-drive file=<iso_file>,media=cdrom`
  - Use the specified ISO file as a virtual CD-ROM drive. With this syntax one can use multiple drives.

- `-boot c`
- Boot the first virtual hard drive.

- `-boot d`
- Boot the first virtual CD-ROM drive.

- `-boot n`
- Boot from virtual network.

- `-vga cirrus`
- Simple graphics card. Every guest OS has a built-in driver.

- `-vga std`
- Support resolutions >= 1280x1024x16. Linux, Windows XP and newer guests have a built-in driver.

- `-vga vmware`
- VMware SVGA-II, a more powerful graphics card. Install [x11-drivers/xf86-video-vmware](https://packages.gentoo.org/packages/x11-drivers/xf86-video-vmware) on Linux guests, VMware Tools on Windows XP and newer guests.

- `-vga qxl`
- More powerful graphics card for use with [SPICE](<https://en.wikipedia.org/wiki/SPICE_(protocol)>).

For better performance, use the same color depth on the host as the guest.

The default graphics memory for clients is insufficient to be able to run with higher resolutions (eg 4K). In order to overcome this, add `-device VGA,vgamem_mb=64` to the command line; this should make higher screen resolutions available on the client.

**2026-08-29**, the information in this section is probably

**outdated**. You can help the Gentoo community by verifying and

[updating this section](https://wiki.gentoo.org/index.php?title=QEMU/Options&action=edit).

**For Intel processors**

Device Drivers  --->
  \[\*\] IOMMU Hardware Support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_IOMMU\_SUPPORT\</code> to find this item.  --->
    \[\*\] Support for Intel IOMMU using DMA Remapping Devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_INTEL\_IOMMU\</code> to find this item.
Bus options (PCI etc.)  --->
  \<M> PCI Stub driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_{{{2}}}\</code> to find this item.

**For AMD processors**

Device Drivers --->
  \[\*\] IOMMU Hardware Support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_IOMMU\_SUPPORT\</code> to find this item.  --->
    \[\*\] AMD IOMMU support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CONFIG\_AMD\_IOMMU\</code> to find this item.
      \<M> AMD IOMMU Version 2 driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_{{{2}}}\</code> to find this item.
Bus options (PCI etc.)  --->
  \<M> PCI Stub driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_{{{2}}}\</code> to find this item.

Find the host PCI device:

`root #``lspci -nn`
00:1b.0 Audio device \[0403\]: Intel Corporation 82801H (ICH8 Family) HD Audio Controller \[8086:284b\] (rev 02)

Note the device (00:1b.0) and vendor ID (8086:284b) and unbind it:

`root #````
echo "8086 284b" > /sys/bus/pci/drivers/pci-stub/new_id
```
`root #````
echo "0000:00:1b.0" > /sys/bus/pci/devices/0000:00:1b.0/driver/unbind
```
`root #````
echo "0000:00:1b.0" > /sys/bus/pci/drivers/pci-stub/bind
```
Then bind it to the guest:

`-device pci-assign,host=00:1b.0`

For detailed information about configuring QEMU networking, refer to [QEMU/Networking](https://wiki.gentoo.org/wiki/QEMU/Networking).

In the absence of any `-netdev` option, QEMU defaults to performing network passthrough.

- `-netdev user`
- The QEMU process will create TCP and UDP connections for each connection in the virtual machine. The VM does not have an address reachable from the outside.

- `-device virtio-net,netdev=vmnic -netdev user,id=vmnic`
- (Recommended) Pass-through with virtio support.

- `-netdev user,id=vmnic,hostfwd=tcp:127.0.0.1:9001-:22`
- Configure QEMU to listen on port 9001, and forward connections to port 22 in the VM. This allows using ssh -p 9001 localhost to log in to the VM.

- `-device virtio-net,netdev=vmnic -netdev tap,id=vmnic,ifname=vnet0,script=no,downscript=no`
- Creates a `vnet0` device on the host, with the other end of the "cable" in the virtual machine.

- `-nic user,id=nic0,smb=/usr/local/public`
- Emulate a virtual [SMB](https://wiki.gentoo.org/wiki/Samba) server on the guest system, for use with an SMB server on the host system, and specify the folder to be shared. It will be available to the guest at \\10.0.2.4\qemu. An automatically generated smb.conf file will be located at /tmp/qemu-smb.pid-0/.

- `-usbdevice tablet`
- (Recommended) Use a USB tablet instead of the default PS/2 mouse. Recommended because the tablet sends the mouse cursor's position to match the host mouse cursor.

- `-usbdevice host:<vendor_id>:<product_id>`
- Passthrough of a host USB device to the virtual machine. The values for `<vendor_id>` and `<product_id>` can be determined by using [lsusb(8)](https://man.archlinux.org/man/lsusb.8.en)[sys-apps/usbutils](https://packages.gentoo.org/packages/sys-apps/usbutils):

- `user $``lsusb` Bus 001 Device 006: ID: 08ec:2039 M-Systems Flash Disk Pioneers

- `08ec` is the vendor ID, `2039` is the product ID.

- `-audiodev <driver>,id=snd0 -device intel-hda`
- Create a virtual soundcard with `<driver>` as the audio driver, e.g. `alsa` for [ALSA](https://wiki.gentoo.org/wiki/ALSA), `pipewire` for [PipeWire](https://wiki.gentoo.org/wiki/PipeWire), and `pa` for [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio). For a complete list, run:
- `user $``qemu-system-x86_64 -audiodev ?`

- `-device hda-output,audiodev=snd0`
- Link the virtual soundcard to the host's audio server.

- `-device hda-duplex,audiodev=snd0`
- Link the virtual soundcard to the host's audio server, with support for audio input.

- `-audiodev <driver>,id=snd0 -device virtio-sound-pci,audiodev=snd0`
- (For Linux guests) Use `<driver>` with virtio-sound. The guest's kernel needs `CONFIG_SND_VIRTIO`.

- `-k <layout>`
- Specify the keyboard layout, e.g. `de` for German keyboards. Recommend for VNC connections.

- `-snapshot`
- Temporary snapshot: write all changes to temporary files instead of hard drive image.

- `-hda <overlay_image>`
- Overlay snapshot: write all changes to an overlay image instead of hard drive image. The original image is kept unmodified. To create the overlay image:

- `user $``qemu-img create -f qcow2 -b <original_image> <overlay_image>`
