<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi/ARM64_in_place_installation | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi/ARM64 in place installation -->
---
title: Raspberry Pi/ARM64 in place installation
url: https://wiki.gentoo.org/wiki/Raspberry_Pi/ARM64_in_place_installation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-19"
fingerprint: "9b16721e358ed1af"
license: CC BY-SA 4.0
---

# Raspberry Pi/ARM64 in place installation

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page is based on [https://cmpct.info/\~sam/gentoo/arm64/PiGentoo64.html](https://cmpct.info/~sam/gentoo/arm64/PiGentoo64.html) by [User:Sam](https://wiki.gentoo.org/wiki/User:Sam) (CC-BY-SA 3.0)

We will bootstrap the Gentoo install from a [stage3](https://wiki.gentoo.org/wiki/Stage3) provided by gentoo.org via RaspberryPi OS 64bit.

You do **not** need a Gentoo system already or to [cross-compile](https://wiki.gentoo.org/wiki/Crossdev). The instructions here are very similar for 32-bit Raspberry Pis too.

## IRC

If you need help join #gentoo-arm on irc.libera.chat, ask your question and wait for at least 24h (see also: [IRC](https://wiki.gentoo.org/wiki/IRC)). This guide was originally written by sam\_ on libera.chat.

## Notes about the installation

[f2fs](https://wiki.gentoo.org/wiki/F2fs) is used for the root [filesystem](https://wiki.gentoo.org/wiki/Filesystem) in this document, you don’t have to. [Ext4](https://wiki.gentoo.org/wiki/Ext4), [btrfs](https://wiki.gentoo.org/wiki/Btrfs) and other filesystems should work, too. Please install the appropriate [packages](https://packages.gentoo.org/) and customize the configuration.

The installation in this document was tested with [RaspberryPi OS](https://www.raspberrypi.com/software/) (former Raspbian) and bootstrapped from there. In theory, you could jump in from any arm64 system (read: kernel).

The [terminal multiplexer](https://en.wikipedia.org/wiki/Terminal_multiplexer) [app-misc/screen](https://packages.gentoo.org/packages/app-misc/screen) is handy to make it easy to do other things, like check [sys-process/htop](https://packages.gentoo.org/packages/sys-process/htop) during setup & compilation.

## Prerequisites

- Raspberry Pi 4B or 400 (should work with 3B/B+ too)
- An SD card with at least 32GB minimum is recommended. (Using a 16GB card may be possible when using e.g. rust-bin.)
- The latest RaspberryPi OS 64bit (former Raspbian) flashed onto the card

## Nice-to-haves

- The biggest the SD card, the better; it’ll wear out slower;
- A fan and heatsinks (to reduce heat when compiling, prevent throttling);
- A high-quality power supply (prevent corruption, prevent throttling).
- A Linux machine sitting around to make tweaks if needed.

## Useful links

- [Raspberry\_Pi\_3\_64\_bit\_Install](https://wiki.gentoo.org/wiki/Raspberry_Pi_3_64_bit_Install)
- [https://github.com/sakaki-/gentoo-on-rpi-64bit](https://github.com/sakaki-/gentoo-on-rpi-64bit)
- For RPis with 4GB RAM or less: [Knowledge\_Base:Emerge\_out\_of\_memory](https://wiki.gentoo.org/wiki/Knowledge_Base:Emerge_out_of_memory)

## Guide

### Prepare

Flash with latest Raspbian 64bit (September 2019 at time of creation).

The easiest way to avoid filesystem mess is to modify cmdline.txt to change `init=/usr/lib/raspi-config/init_resize.sh` to

**`/boot/cmdline.txt`**

Edit config.txt and tell the bootloader to use the 64 bit kernel (kernel8.img).

**`/boot/config.txt`**

Place a file in /boot called ssh. This enables sshd on the next boot:

`root #``touch /boot/ssh`
Setup WiFi if appropriate in Raspbian.

### Boot

Verify that you are running 64bit by looking for *aarch64* in uname -a:

`user $``uname -a` Linux raspberrypi 4.19.75-v8+ #1270 SMP PREEMPT Tue Sep 24 18:59:17 BST 2019 aarch64 GNU/Linux

### Update and reboot your system

To get the latest [bootloader](https://wiki.gentoo.org/wiki/Bootloader), [kernel](https://wiki.gentoo.org/wiki/Kernel), etc.

`user $``sudo apt update && sudo apt upgrade``user $``sudo reboot`
### Filesystem setup

Recommended filesystems for the root partition are [ext4](https://wiki.gentoo.org/wiki/Ext4), [btrfs](https://wiki.gentoo.org/wiki/Btrfs) or [f2fs](https://wiki.gentoo.org/wiki/F2fs). f2fs is used as an example. Install [sys-fs/f2fs-tools](https://packages.gentoo.org/packages/sys-fs/f2fs-tools) for mkfs.f2fs and resize.f2fs:

`root #``emerge --ask sys-fs/f2fs-tools`
Create an f2fs partition:

`root #``mkfs.f2fs /dev/mmcblk0pX`
(Raspberry Pi OS configured SSD cards have a gap at the start before the boot partition and FDisk will automatically create a partition there with its defaults. Prevent this by manually entering the start cylinder as at least one more than the end cylinder of the main 2Gb partition)

Mount the f2fs partition at /mnt/gentoo and cd there:

`root #``mkdir /mnt/gentoo``root #``mount /dev/mmcblk0pX /mnt/gentoo``root #``cd /mnt/gentoo`
Unmount /boot from Raspbian (so we can mount it in the chroot):

`root #``umount /boot`
### Fetch the stage file

Fetch the [stage3](https://wiki.gentoo.org/wiki/Stage3) (please check for a newer one on [gentoo.org/downloads](https://www.gentoo.org/downloads/) -> ARM64).

### Preparations and chroot

[Untar](https://wiki.gentoo.org/wiki/Tar) the [stage file](https://wiki.gentoo.org/wiki/Stage_file) as usual:

`root #``tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner`
Start customising [make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf). Set [MAKEOPTS](https://wiki.gentoo.org/wiki/MAKEOPTS)="-j*x*" suitableAs a rule of thumb, MAKEOPTS jobs should be less than or equal to the minimum of the size of RAM/2GB or CPU thread count. for the system:

**`/mnt/gentoo/etc/portage/make.conf`**

```
# Different to default
COMMON_FLAGS="-O2 -pipe -march=native"
MAKEOPTS="-j4"
# Default
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
```
Note that you can [set -march etc appropriately](https://wiki.gentoo.org/wiki/Safe_CFLAGS#ARMv8-A.2FBCM2837) to use [distcc](https://wiki.gentoo.org/wiki/Distcc), but still get the benefits.


Copy over some firmware for [WiFi](https://wiki.gentoo.org/wiki/Wifi) and [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth) from RaspberryPi OS:

`root #``mkdir -p /mnt/gentoo/lib/firmware/``root #``cp -rv /lib/firmware/brcm/ /mnt/gentoo/lib/firmware/`
Enter chroot as usual: Copy [DNS settings](https://wiki.gentoo.org/wiki/Resolv.conf) first, then [mount](https://wiki.gentoo.org/wiki/Mount), then [chroot](https://wiki.gentoo.org/wiki/Chroot):

`root #``cp --dereference /etc/resolv.conf /mnt/gentoo/etc/resolv.conf``root #``mount --types proc /proc /mnt/gentoo/proc``root #``mount --rbind /sys /mnt/gentoo/sys``root #``mount --make-rslave /mnt/gentoo/sys``root #``mount --rbind /dev /mnt/gentoo/dev``root #``mount --make-rslave /mnt/gentoo/dev``root #``chroot /mnt/gentoo /bin/bash``root #``source /etc/profile`
### Build the (RPi, not upstream) kernel:

#### Prerequisites

Updating the system is likely needed to avoid e.g. bindist conflicts! You may be able to for now --exclude sys-devel/gcc though:

`root #``emerge --sync``root #``emerge -a -uvDU @world`
Fetch build requirements for the kernel:

`root #``emerge -av dev-vcs/git``root #``emerge -av sys-devel/bc`
Get the sources so e.g. emerge knows what kernel we're running:

`root #``cd /opt``root #``ln -s /opt/linux /usr/src/linux`
#### Configure and build the kernel

Generate base configs. For all 64bit configurations (including Pi 3, 3+, 4, and 400, as per [upsream](https://www.raspberrypi.com/documentation/computers/linux_kernel.html)) this should be:

`root #``make bcm2711_defconfig`
Customise the kernel:

`root #``make menuconfig`
Modify arch/arm64/Makefile (optional, probably not very useful):

**`/usr/src/linux/arch/arm64/Makefile`**

Start compiling, the time will differ:

`root #``time make -j4 Image modules dtbs````
        real    66m55.242s
        user    234m46.421s
        sys     27m24.339s
```
Mount /boot inside the chroot this time:

`root #``mount /dev/mmcblk0p1 /boot`
Take a backup of the old kernel. kernel8 is for ARM64:

`root #``mv /boot/kernel8.img /boot/kernel8.img.bak`
Install modules and kernel:

`root #``make modules_install dtbs_install``root #``cp arch/arm64/boot/Image /boot/kernel8.img`
#### Optional: Compressing the kernel

On 64bit installations the kernel8.img may be a gzip compressed image with the same filename. The bootloader recognizes the gzip header  <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

`root #``cd /boot` `root #``gzip -9cf kernel8.img > kernel8.img` ### Tasks before reboot

Update world:

`root #``emerge -a -uvDN @world`
Create /etc/fstab:

**`/etc/fstab`**

Modify two parameters (`root`, `rootfstype`) in /boot/cmdline.txt:

**`/boot/cmdline.txt`**

### Optional extras before reboot

#### Set up network and SSH

Needed for WiFi / [network](https://wiki.gentoo.org/wiki/Network_management_using_DHCPCD):

`root #``emerge -av dhcpcd wpa_supplicant`
Create [wpa\_supplicant.conf](https://wiki.gentoo.org/wiki/Wpa_supplicant#Configuration) as appropriate (see [Raspberry Pi docs](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)).

Tell wpa\_supplicant and sshd to start on boot:

`root #``rc-update add wpa_supplicant default``root #``rc-update add sshd default`
Setup the network scripts: [Wpa\_supplicant#Setup\_for\_Gentoo\_net..2A\_scripts](https://wiki.gentoo.org/wiki/Wpa_supplicant#Setup_for_Gentoo_net..2A_scripts)

#### Create a user with sudo rights

Create a user, e.g. gentoo:

`root #``useradd -m gentoo -G wheel,video,users,audio,usb`
Set a password (optional):

`root #``passwd gentoo`
Add in an [SSH](https://wiki.gentoo.org/wiki/SSH) key (optional):

`root #``su gentoo``root #``mkdir ~/.ssh`
Put your public ssh key in \~/.ssh/authorized\_keys.

Install [app-admin/sudo](https://packages.gentoo.org/packages/app-admin/sudo)

`root #``emerge --ask app-admin/sudo`
#### Utilize the built-in random number generator and additional tools

Install [sys-apps/rng-tools](https://packages.gentoo.org/packages/sys-apps/rng-tools) and configure the system to start the RNG at boot:

`root #``emerge -av rng-tools``root #``mkdir /etc/modules-load.d/``root #``echo 'bcm2835-rng' > /etc/modules-load.d/random.conf``root #``rc-update add rngd`
Emerge useful tools by running:

`root #``emerge --ask app-portage/gentoolkit app-portage/eix sys-process/htop`
#### Syncing time

Install [net-misc/chrony](https://packages.gentoo.org/packages/net-misc/chrony):

`root #``emerge --ask net-misc/chrony`
Start chronyd on boot:

`root #``rc-update add chronyd`
Set up swclock to save last clock on shutdown and restore on boot:

`root #``rc-update add swclock boot`
Reboot and hope for the best!

### Post-boot

#### Set CPU\_FLAGS\_ARM

Install, use and remove a [tool](https://wiki.gentoo.org/wiki/CPU_FLAGS_X86#Using_cpuid2cpuflags) for setting the [CPU flags](https://packages.gentoo.org/useflags/search?q=cpu_flags_arm):

`root #``emerge --oneshot -av cpuid2cpuflags``root #``mkdir /etc/portage/package.use``root #``echo "*/* $(cpuid2cpuflags)" > /etc/portage/package.use/00cpuflags``root #``emerge -c cpuid2cpuflags`
#### Raspberry Pi specific utilities

##### Installing Raspberry Pi userland

`root #``emerge -av dev-util/cmake  dev-vcs/git``root #``mkdir -p ~/git/build/ && cd ~/git/build``root #``cd userland``root #``./buildme`
Now it's installed, we need to make it accessible

`root #``echo 'export PATH="/opt/vc/bin:${PATH}"' >> ~/.bashrc``root #``echo '/opt/vc/lib' > /etc/ld.so.conf.d/06-rpi.conf``root #``. ~/.bashrc`
Regenerate the dynamic loader (ld) cache

`root #``env-update`
Happily use [vcgencmd](https://www.raspberrypi.com/documentation/computers/os.html#vcgencmd) and friends!

##### Update the firmware (Pi 4/Pi 400 only)

For convenience, add a dir to `PATH`. Only run this part the first time you update.

`root #``mkdir ~/bin``root #``echo 'export PATH="$HOME/bin:${PATH}"' >> ~/.bashrc|. ~/.bashrc`
Run git clone ... the first time, then run git pull in future to check for updates

Copy the firmware files to where tool wants:

`root #``sudo mkdir -p /lib/firmware/raspberrypi/bootloader/``root #``sudo cp -rv firmware/* /lib/firmware/raspberrypi/bootloader`
Copy vl805 to your bin:

`root #``cp firmware/vl805 ~/bin`
Run the actual update:

`user $``sudo --preserve-env=PATH env ./rpi-eeprom-update`
#### Install dosfsutils

fsck.vfat is needed for the boot partition. Install [sys-fs/dosfsutils](https://packages.gentoo.org/packages/sys-fs/dosfsutils):

`root #``emerge --ask sys-fs/dosfstools`
