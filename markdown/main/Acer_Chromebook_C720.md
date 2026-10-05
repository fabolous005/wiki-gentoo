<!-- source: https://wiki.gentoo.org/wiki/Acer_Chromebook_C720 | group: Gentoo Wiki (Main) | wiki-title: Acer Chromebook C720 -->
---
title: Acer Chromebook C720
url: https://wiki.gentoo.org/wiki/Acer_Chromebook_C720
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: "944ffd0b009781cc"
license: CC BY-SA 4.0
---

# Acer Chromebook C720

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A short guide containing relevant instructions to get Gentoo running on an Acer C720 chromebook.

## Preinstallation

### Firmware wipe

If the user would like to completely remove Chrome OS from their computer, they can choose to run the Chrome OS Firmware Utility Script written by Mr. Chromebox. However, this script is entirely optional.

- After successfully running this script the remaining [Preinstallation] sections can be skipped. Additionally, the computer is effectively a "regular" computer and the [AMD64 Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64) guide can be followed.

### Developer mode

In order get lower-level access to the hardware to install Gentoo, it is necessary to put the system into [developer mode](https://sites.google.com/site/chromeoswikisite/home/what-s-new-in-dev-and-beta/developer-mode).

Follow these steps to gain access to developer mode:

1. Reboot the system to the login screen, then do not login.
2. At the login screen hold `Esc`+`⟳` (the `F2` key) then press the Power button (top right).
3. The machine should boot to a special recovery screen, when it gets there press `Ctrl`+`d`. The system will reboot.

### Enable the legacy bootloader

1. Presuming the system in a a powered down state, power up the unit.
2. When the login screen appears, hold `Ctrl`+`Alt` and press`→`. This should enter the developer console.
3. Login by entering `chronos` at the prompt, and pressing the `Enter` key.
4. sudo bash to gain a bash shell.
5. crossystem dev\_boot\_usb=1 dev\_boot\_legacy=1
6. Type reboot and then press `Enter`.
7. The system will reboot again.

### Loading bootable media

After downloading a 64-bit minimal installation CD, proceed to make it USB drive bootable with isohybrid. Copy the .iso image to a USB drive with dd:

- `root #``dd if=/path/to/install-amd64-minimal-.iso of=/path/to/raw/dev/device/file`

1. Once the dd process is complete, insert the USB to a USB port on the Chromebook.
2. If the Chromebook was powered down or waiting at the legacy bootload screen, restart the Chromebook by pressing the power button.
3. At the recovery screen, press `Ctrl`+`l` to load legacy boot options.
4. The SeaBIOS bootloader should appear prompting a press of the `Esc` key. Press `Esc`.
5. Select the appropriate bootable device by pressing the associated integer number on the keyboard.
6. The Gentoo ISO Linux prompt should appear. Press `Enter` to boot the minimal CD.

    - If the init process gets stuck, try adding `edd=off` to the kernel command-line parameters.

Once the init process completes the system is ready to receive a Gentoo install!

### Create a backup of the current OS

It may be a good idea to create a backup of the Chromebook's installation on embedded flash disk (typically /dev/sda) before proceeding. If the Chromebook may be transferred to a new owner one day, the backup of the original OS could be restored. After configuring networking and enabling the SSH daemon, something like the following command should suffice:

`root #``ssh user@remote "dd if=/dev/sda | gzip -1 -" | dd of=chromebook_image.gz`
This will send the entire contents of /dev/sda over the network to the current running directory. Be sure there's enough free space in the current partition to receive the file!

To restore the backup, boot the Gentoo minimal install CD again (or alternative live media) and issue:

`root #``dd if=chromebook_image.gz | gzip -d | ssh user@local dd of=/dev/sda`
## Installation

- Continue following the MBR disk partitioning path if the firmware is stock.
- Continue following either the MBR or GPT disk partitioning paths if the firmware was flashed using MrChromebox's firmware utility.

For installation instructions on either MBR or GPT disk partitioning, follow the [AMD64 Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64).

### Kernel

**Enable c720 chromebook support**

Atheros ath9k wireless card

**Enable ath9k support**

Cypress APA I2C touchpad support

**Enable Cypress APA touchpad support**

### Wireless networking

The setup of wireless networking is detailed in the [wpa supplicant](https://wiki.gentoo.org/wiki/Wpa_supplicant) and [Wifi](https://wiki.gentoo.org/wiki/Wifi) articles.

### Trackpad configuration

- Although the trackpad will work, after enabling required kernel options, in a basic X environment, some users may desire further configuration. A fantastic source with friendly documentation is located in a page of the [Ubuntu manuals](https://manpages.ubuntu.com/manpages/xenial/man4/libinput.4.html). Among other features the manual displays options for disabling the trackpad while typing or tap-to-click.

- If users desire disabling the trackpad while typing or enabling tap-to-click, they can download the Acer C720 X*org* configuration file written by github user sedwardsmarsh. `root #``cd /etc/X11/xorg.conf.d/``root #``wget https://github.com/sedwardsmarsh/Acer-C720-X-Conf-File/blob/master/47-touchpad.conf`

## Additional information

### Swap partition

- Since the stock SSD provided in the Acer C720 chromebook is only 16GB, free space is valuable. Space can be saved when creating disk partitions: instead of creating a separate partition for swap space create a swap file instead. This way, the amount required for swap space can be resized when needed.

1. To create a swap file, run the following command, *Since count=1M equals 512MB, count=8M equals 4,096MB or 4GB.*`root #``dd if=/dev/zero of=/swapfile count=8M`
2. Use mkswap to get your file ready for swaping:`root #``mkswap /swapfile`
3. Make an fstab entry:`/swapfile none swap sw,loop 0 0`
4. Run as root to activate your file swap space:`root #``swapon -a`

## See also

- [Power management/Guide](https://wiki.gentoo.org/wiki/Power_management/Guide) — a guide to setup power management features of a laptop.
- [SSD](https://wiki.gentoo.org/wiki/SSD) — provides guidelines for basic maintenance, such as enabling discard/trim support, for **SSD**s ([Solid State Drives](https://en.wikipedia.org/wiki/Solid-state_drive)) on Linux.

## External resources

- [swapfile configuration](https://forums.gentoo.org/viewtopic-p-5099313.html) - link to Gentoo Forums post where swap file tutorial was copied from.
- [https://www.chromium.org/chromium-os/developer-information-for-chrome-os-devices/acer-c720-chromebook](https://www.chromium.org/chromium-os/developer-information-for-chrome-os-devices/acer-c720-chromebook) - A very informative chromium projects page contains information about the Acer C720, C720P, and C740 chromebook models.
- [https://wiki.archlinux.org/index.php/Acer\_C720\_Chromebook](https://wiki.archlinux.org/index.php/Acer_C720_Chromebook) - The Acer C720 archlinux wiki page.
- [https://www.linux.com/learn/how-install-linux-acer-c720-chromebook](https://www.linux.com/learn/how-install-linux-acer-c720-chromebook) - A Linux.com article on installing Ubuntu/Bodhi Linux.
