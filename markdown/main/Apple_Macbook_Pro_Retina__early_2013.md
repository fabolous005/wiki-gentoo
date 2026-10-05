<!-- source: https://wiki.gentoo.org/wiki/Apple_Macbook_Pro_Retina_(early_2013) | group: Gentoo Wiki (Main) | wiki-title: Apple Macbook Pro Retina (early 2013) -->
---
title: Apple Macbook Pro Retina (early 2013)
url: https://wiki.gentoo.org/wiki/Apple_Macbook_Pro_Retina_(early_2013)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "9685791d99a69b87"
license: CC BY-SA 4.0
---

# Apple Macbook Pro Retina (early 2013)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


Of the MacBook Retina models the 13 and 15-inch models are slightly different. The 15-inch model is more complicated, as it has both an Intel and an nVidia graphics card. The setup steps common to both are here, and specifics are separated off below:

In theory, the steps here should be very similar to the 11,x model.

## Installation

These instructions will perform a Dual Boot setup with MacOS. A single boot installation could be performed, but you will need to use the rEFIt instructions for installing rEFIt to the [EFI system partition](https://wiki.gentoo.org/wiki/EFI_System_Partition) using bless instead of to the MAC partition (which is the default). I have not tried this yet but you can read about it on the rEFIt website.

### Preparation

To bootstrap using Ubuntu, grab a [mac image](http://releases.ubuntu.com/13.04/) and [flash it to a USB drive](http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-mac-osx). SD cards don't seem to work.

Use MacOS Disk Utility to resize the MacOS partition on the fly, which by default takes up most of the disk, and behind it create a Linux root partition of the size you desire. Note: if full disk encryption exists, it will necessary to temporarily disable it (takes a while) to make any changes to the partition table.

To connect to Ethernet a Thunderbolt (e.g. [Apple Thunderbolt to Gigabit Ethernet Adapter](https://wiki.gentoo.org/wiki/Apple_Thunderbolt_to_Gigabit_Ethernet_Adapter)) or USB adapter is needed. From kernel version 4.3 onward Thunderbolt hot-plugging should work on recent Apple hardware so it is no longer necessary to have the Thunderbolt Ethernet adapter plugged in during boot. [\[1\]](https://wiki.gentoo.org#cite_note-thunderbolt-hotplug-1)

Now, choose a bootloader to boot the liveUSB and the kernel to be made later.

## Bootloader

Now you must choose your a EFI boot manager. Currently Apple's Bootcamp does not support Linux. However there are two option for you: rEFIt and rEFInd.

### Using Refit

**NB:** Refit is no longer supported by OS X Yosemite, this due to changes in the OS X boot loader now treats local installation as quasi LVM. See ReFind for further details [http://www.rodsbooks.com/refind/yosemite.html](http://www.rodsbooks.com/refind/yosemite.html)

Refit it is very easy to install however be aware that it is dead.  There is longer any active development for it.  I can confirm that is works well with OS X Mountain Lion however it is untested on OS X Mavericks which it hasn't been tested on. If you want to install it go [here](http://refit.sourceforge.net/doc/c1s1_install.html).

A major drawback of this method is that you cannot use an initramfs, so if you have lvm or encryption then you must use rEFInd.

You cannot pass kernel arguments using rEFIt, so it is useless on 15 in MacBooks where kernel arguments are required (unless you hardcode all arguments, which is very cumbersome).

In this method, the Mac app rEFIt will act as the boot manager/boot loader and it will boot the kernel directly.

Use at least a 3.7 kernel. The wireless drivers you need later do not compile with a kernel greater than 3.8 at the present time.

Complete the rest of the Gentoo installation, and reboot. rEFIt should show Gentoo\_37\_x64.efi as an option in the boot menu.

### Using Refind

Refind which is a  fork of Refit and is being actively development and maintained. For installation see follow [this link](http://www.rodsbooks.com/refind/installing.html#installsh).

This option is recommended for 15 inch versions, as it allows for passing kernel parameters.

Setting up rEFInd must be done from OS X. Essentially it boils down to:

Getting rEFInd

- Go to [http://sourceforge.net/projects/refind/files/](http://sourceforge.net/projects/refind/files/)
- Get the latest binary zip
- Extract it

Installing rEFInd

`user $````
mkdir /Volumes/esp
```
`user $````
sudo mount -t msdos /dev/disk0s1 /Volumes/esp
```
`user $````
sudo cp -r refind /Volumes/esp/efi/refind
```
`user $````
sudo bless --mount /Volumes/esp --setBoot --file /Volumes/esp/efi/refind/refind_x64.efi # actually sets it active
```
Configuring rEFInd

Later, once you've built a kernel, you'll need to edit /Volumes/efi/refind/refind.conf (from within Linux) to add your kernel, as would be done with GRUB.

**`/Volumes/efi/refind/refind.conf`**

```
menuentry Linux-Bleeding-Modeset-Switch {
    icon EFI/refind/icons/os_gentoo.icns
    loader EFI/gentoo/kernel-3.14-m
    initrd EFI/gentoo/initrd-3.14-m
    options "ro root=UUID=a3b54691-7d5b-3b20-8b80-08cbfa521fbf noapic" # Basic kernel options, specifying root partition by uuid.
 #    options "ro crypt_root=/dev/vg1/root dolvm root=/dev/mapper/root rootfstype=ext4 noapic" # kernel options for lvm + luks encryption
 #    options "ro root=/dev/sda4" # if you plan to use this, please change 'loader' to 'loader EFI\gentoo\kernel-3.14-m'
}
```
You can (and probably should) have several entries, and can edit this file from within Linux.

Just compile the kernels normally and install them to EFI/gentoo on the EFI partition.

## LiveUSB bootstrap

Now that the bootloader has been installed and the disk is prepared, we are ready to start bootstrapping.

Shut down the Mac, and boot it up twice (Requirement for rEFIt). You should start seeing the rEFIt or rEFInd boot menu after the second reboot.

Now, plug the Ubuntu USB stick into the Mac and boot the Mac. Select the new option in the menu which should be the USB stick. If there are multiple options, keep trying until you get one that boots. At the Ubuntu boot screen, press `F6` to disable APIC, or you may experience random crashes.

When it boots, most everything should work except the sound on the LiveCD.

Proceed with a standard Gentoo install using the Ubuntu live environment. Install your snapshot into the root partition you created (Probably /dev/sdb4) and configure it per the installation instructions but do not install the boot loader. Note that by default the EFI system partition will be /dev/sdb1, Apple root will be /dev/sdb2 and Apple recovery will be /dev/sdb3.

## Configuration

### Kernel

I've included [my 3.14 kernel config](https://gist.github.com/8995282) as a reference, which supports:

- Open source intel and nvidia drivers
- Apple magic mouse
- LVM
- LUKS encryption
- Bluetooth
- Everything below

These options are in addition to the standard options you would select for Intel based PC's

Configure your kernel as normal, but add the following options for EFI Stub (needed for rEFIt or rEFInd). More details about Kernel configuration for EFI stub are here: [EFI stub](https://wiki.gentoo.org/wiki/EFI_stub):

Ensure the following options are NOT set (required for proper Broadcom wireless):

If your machine is equipped with a SD card reader, you can enable the driver for it using:

If your machine is equipped with Thunderbolt ports be sure to enable these options in the kernel to enable hot-plugging:

For Bluetooth, follow the [wiki](https://wiki.gentoo.org/wiki/Bluetooth).

If and only if you are using rEFIt, specify the root hard-coded in the kernel:

Make sure the `root=` line on your kernel command line configuration is correct.

Once configured, compile the kernel as normal but do not copy it to /boot. Once it is compiled, do the following:

~~Required for Audio: Add the following to the bottom of /etc/modprobe.d/alsa:~~ It is not necessary.

**`/etc/modprobe.d/alsa`**

```
 snd_hda_intel model=mbp101
```
#### Installing the kernel

Your built in MacBook SSD should be /dev/sdb if you booted from the Ubuntu USB which will probably be /dev/sda. Assuming it is /dev/sdb, your "EFI System Partition" will be /dev/sdb1, in which case, copy it to the EFI partition as such:

`user $````
mkdir /mnt/efi
```
`user $````
mount /dev/sdb1 /mnt/efi
```
`user $````
cp /usr/src/linux/arch/x86_64/boot/bzImage /mnt/efi/EFI/Boot/Gentoo_37_x64.efi
```
If you used rEFInd and need an initramfs, copy it over too, and update refind.conf to have a matching menu entry.

It is a good idea with rEFInd to keep a few working kernels around, in case you bork one - this way you can get back in to rescue the system.

### Wireless

There are two driver options for Broadcom wireless cards.

#### Open source B43 driver

The BCM 4331 wireless network card is supported by the B43 open source kernel driver. Note: Only B/G modes are supported by this in-kernel driver. 802.11**n** is not supported. To install, disregard the above kernel configuration for wireless and follow the [Wifi](https://wiki.gentoo.org/wiki/Wifi) guide.

#### Closed source Broadcom driver

The Broadcom closed source driver is the alternate option for the BCM 4331. In which case please disable the kernel options as detailed above. This driver supports 802.11**n**. It is the only package that supports wireless N for this chipset at this time. NB broadcom driver must be version 6.30.223.30 or higher. Enable the following kernel options to expose `LIB80211` in the kernel:

**Kernel configuration option for the Broadcom-sta driver**

If Portage complains, it may be necessary to accept the unstable version of the package then repeat the steps above:

`root #``echo "net-wireless/broadcom-sta" >> /etc/portage/package.accept_keywords`
### Media keys

We will control the media keys using xbindkeys. Create an .xbindkeysrc in your home directory with the options mentioned. You can also use xbindkey -k to generate new mappings. To install, add "xbindkeys" to your .xinitrc. You can access function keys by holding the `Fn` modifier in the bottom left of the keyboard.

We'll also need to create a local.d script:

**`/etc/local.d/01-keyboard.start`**

```
#!/bin/bash
# http://askubuntu.com/questions/370857/cant-adjust-screen-brightness-on-macbook-pro-10-1-ubuntu-13-10 
/usr/bin/setpci -v -H1 -s 00:01.00 BRIDGE_CONTROL=0
/bin/chmod a+w /sys/devices/platform/applesmc.768/leds/smc\:\:kbd_backlight/brightness
/bin/chmod a+w /sys/class/backlight/gmux_backlight/brightness
```
#### Keyboard backlight

Install xbindkeys and add the following to your .xbindkeysrc:

**`~/.xbindkeysrc`**

```
"/usr/bin/kb-brightness down"
  m:0x0 + c:237
"/usr/bin/kb-brightness up"
  m:0x0 + c:238
```
**`/usr/bin/kb-brightness`**

```
#!/bin/bash
kbdb='/sys/devices/platform/applesmc.768/leds/smc::kbd_backlight/brightness'
step=25
current=`cat $kbdb`
new=$current
if [ "$1" == "up" ];then
   new=$(($current + $step))
elif [ "$1" == "down" ];then 
   new=$(($current - $step))
fi
if [ $new -le 0 ];then
   new=0
fi
echo $new > $kbdb
```
#### Display backlight

Add the following tho .xbindkeysrc:

**`~/.xbindkeysrc`**

```
"/usr/bin/bl-brightness down"
  m:0x0 + c:232
"/usr/bin/bl-brightness up"
  m:0x0 + c:233
```
**`/usr/bin/bl-brightness`**

```
#!/bin/bash
bldb='/sys/class/backlight/gmux_backlight/brightness'
step=25
current=`cat $bldb`
new=$current
if [ "$1" == "up" ];then
   new=$(($current + $step))
elif [ "$1" == "down" ];then 
   new=$(($current - $step))
fi
if [ $new -le 0 ];then
   new=0
fi
echo $new > $bldb
current=`cat $bldb`
```
#### Audio control keys

This assumes you are using ALSA, and have your card properly configured:

Add the following to your .xbindkeysrc:

**`~/.xbindkeysrc`**

```
# Increase volume
"amixer set Master playback 1+ unmute"
   XF86AudioRaiseVolume
# Decrease volume
"amixer set Master playback 1-"
   XF86AudioLowerVolume
"amixer set Master toggle"
   XF86AudioMute
```
### Applications

#### Keeping retina resolution

If you plan to use the retina display at native resolution on the 13" or 15" MBP, you probably want to make it a little more readable. To do this, most toolkits have DPI configuration. In KDE, you can find it in System settings > Application appearance > Font > DPI, set it at 160.0 for 1.5x resolution and 220.0 for 1.0x resolution (letting it at 96.0 will be equal to 2.0x resolution). For terminals such as xterm and urxvt, edit \~/.Xdefaults to add:

**`~/.Xdefaults`**

```
Xft.dpi:       160.0
Xft.antialias: 1
Xft.hinting:   1
Xft.hintstyle: hintfull
Xft.rgba:      rgb
urxvt*font: xft:DejaVu Sans:pixelsize=16
```
For GTK, font size can be set in \~/.gtkrc-2.0. It can also share KDE settings when using oxygen-gtk.

#### Downscaling resolution

This will just globally cut the resolution in half, and provides a much more "normal" experience, while sacrificing the additional resolution. IMHO, this is totally worth it and actually works way better.

Add the following to your .xinitrc:

**`~/.xinitrc`**

```
monitor=`xrandr | grep -i edp | cut -d " " -f 1`
xrandr --newmode  "1440x900_60.00"  106.50  1440 1528 1672 1904  900 903 909 934 -hsync +vsync
xrandr --addmode $monitor 1440x900_60.00
xrandr --output $monitor --mode 1440x900_60.00
```
### Turn off backlight when screen closed

The hardware does not automatically shut off the backlight like other laptops. Make the following change. Edit /etc/acpi/default.sh and find the commented #lid) section, the lid) case should look like this:

**`/etc/acpi/default.sh`**

```
)
  xset -display :0 dpms force off
  ;;
```
In addition, edit /etc/X11/xinit/xinitrc.d/00-xhost and add the following line to the bottom of the file. This gives root permission to run xset.

**`/etc/X11/xinit/xinitrc.d/00-xhost`**

```
 +local:0 > /dev/null 2>&1
```
Restart ACPID and the X server for this to take effect.

### CPU fan control

To prevent the CPU from overheating while building/compiling software, install [sys-apps/lm-sensors](https://packages.gentoo.org/packages/sys-apps/lm-sensors) and [app-laptop/mbpfan](https://packages.gentoo.org/packages/app-laptop/mbpfan).

`root #``emerge --ask sys-apps/lm-sensors`
After installing lm-sensors hardware needs to be recognized and configured. To create a initial configuration use following command:

`root #``/usr/sbin/sensors-detect`
After the hardware has been recognized and a initial configuration has been written add lm\_sensors to the default startup:

`root #``rc-update add lm_sensors default`
After the [sys-apps/lm-sensors](https://packages.gentoo.org/packages/sys-apps/lm-sensors) has been installed and configured install [app-laptop/mbpfan](https://packages.gentoo.org/packages/app-laptop/mbpfan).

`root #``emerge --ask app-laptop/mbpfan`
The installation routine will generate following configuration file:

**`/etc/mbpfan.conf`**

Start the mbfan daemon:

`root #``/etc/init.d/mbpfan start`
Add mbfan daemon to the default startup:

`root #``rc-update add mbpfan default`
## Not working

Everything I can think to test works, except the following:

- ~~Thunderbolt hot-plugging does not work. Boot with the adapter connected.~~ With Linux 4.3, Thunderbolt hot-plugging should work on recent Apple hardware.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-thunderbolt-hotplug-1)</sup> See [Apple Thunderbolt to Gigabit Ethernet Adapter](https://wiki.gentoo.org/wiki/Apple_Thunderbolt_to_Gigabit_Ethernet_Adapter)



- The SD card reader is giving me some troubles. It works fine with some cards but I am having errors with other cards. But it does seem to work sometimes. I get the following:

\[16032.337737\] mmc0: Switching to 1.8V signalling voltage failed, retrying with S18R set to 0
 \[16049.257421\] mmc0: error -110 whilst initialising SD card

## Troubleshooting

### Resetting the System Managment Controller (SMC)

The internal USB controller sometimes fails to init from right after booting. This behaviour has only been seen with the xHC USB 3.0 kernel driver. It needs some time to be usable, sometimes up to 180 seconds. Following error messages appear in dmesg, while USB hardware is not usable at that time:

\[   25.250102\] usb 1-3.1: USB disconnect, device number 7
\[   30.127039\] xhci\_hcd 0000:00:14.0: Timeout while waiting for setup device command
...
\[   78.769783\] usb 2-3: device not accepting address 8, error -62
...
\[   89.589428\] usb usb2-port3: unable to enumerate USB device
\[  104.778661\] usb 1-2.3: new high-speed USB device number 21 using xhci\_hci
...
\[  104.871874\] hub 1-2.3:1.0: 3 ports detected
\[  104.971604\] hub 1-2.3:1.0: hub\_port\_status failed (err = -71)
...
\[  104.992711\] hub 1-2.3:1.0: hub\_port\_status failed (err = -71)
\[  104.992789\] xhci\_hcd 0000:00:14.0: URB transfer length is wrong, xHC issue? req. len = 0, act. len = 429496728
...

This problem can occur after some long time of sucessfully using the gentoo/linux system. Sometimes it occurs randomly. In my own case it was every time the MacBook has booted.

- The internal keyboard did not work.
- the external USB hub was not usable.
- the USB ports were not usable.
- All USB hardware was not usable at all.

Plugging unplugging the hardware did not work very well. Sometimes i had to wait 180 seconds after the USB ports recognized the hardware. Sometimes it did not work at all.

This behaviour might be a hint for problems with external hardware like:

- External hub power supply
- Cable to external hub

If the problem still persists after checking the external hub power supply and cable. The manufacturer describes a procedure on howto reset the SMC (System Management Controller) on a MacBookPro, this helped on my hardware with the xHCI driver failing to load:

Before applying this workaround please read the full list of indicators on the manufacturer website:

The procedure on howto reset the SMC:

1. Shut down your computer.
2. Unplug the power adapter from your computer.
3. Using the built-in keyboard, press Shift-Control-Option `Shift`+`cmd`+`alt` on the left side of the keyboard, then press the power button `power` at the same time. Hold these keys and the power button for 10 seconds.
4. Release all keys.
5. Reconnect the power adapter.
6. Press the power button again to turn on your Mac.


After resetting the SMC all problems with loading the xHCI driver have gone right on the first boot. Still this behaviour also might be a indicator of failing MacBook hardware, in this case it would be the internal USB hub.

## See also

## External resources

- The [Arch Linux wiki](https://wiki.archlinux.org/index.php/MacBookPro_Retina) is a very good reference.
- If you're interested in setting up an encrypted rootfs on lvm [Funtoo](http://www.funtoo.org/Rootfs_over_encrypted_lvm) has a great article.
