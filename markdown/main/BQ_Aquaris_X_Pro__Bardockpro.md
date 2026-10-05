<!-- source: https://wiki.gentoo.org/wiki/BQ_Aquaris_X_Pro_(Bardockpro) | group: Gentoo Wiki (Main) | wiki-title: BQ Aquaris X Pro (Bardockpro) -->
---
title: BQ Aquaris X Pro (Bardockpro)
url: https://wiki.gentoo.org/wiki/BQ_Aquaris_X_Pro_(Bardockpro)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-30"
fingerprint: "9705901f5967a3ea"
license: CC BY-SA 4.0
---

# BQ Aquaris X Pro (Bardockpro)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![](https://wiki.gentoo.org/images/thumb/7/7f/Gentoo_running_in_a_BQ_Aquaris_X_Pro.jpg/300px-Gentoo_running_in_a_BQ_Aquaris_X_Pro.jpg)

BQ Aquaris X Pro (bardockpro) is a 2017 mid-range device by Spanish manufacturer BQ. it has a glass back. It differs from the non-pro version in the camera, so instructions are pretty much the same for both devices.

## Previous setup

The following tools should be installed, and configured in the computer which is going to be used to install Gentoo in the device:

- [Edl from bkerler](https://github.com/bkerler/edl) This tool is needed to extract firmware partitions.
- [Android SDK](https://developer.android.com/studio) Download from the link a file that looks like commandlinetools-linux-13114758\_latest.zip, uncompress in a directory like $HOME/Android/Sdk.
- Platform tools installed with Android SDK Manager.
- Crossdev arm-none-eabi environment.
- Crossdev aarch64-pc-linux-gnu environment.
- Qemu with aarch64, and static-user support.

Additionally the following physical hardware is needed:

- A BQ Aquaris X Pro
- A usb-c cable fresh enough to carry data. (Worn out cables may not be able to carry data in a consistent way)
- A Linux computer with root access, and a modern enough Kernel.

Add the following to the user's .bashrc.

**`$HOME/.bashrc`**

**Android Sdk Config**

```
export ANDROID_SDK_ROOT="$HOME/Android/Sdk"
```
## Installing Bootloader

### Build LK2ND

`user $` `git clone https://github.com/bq-msm8953-mainline/lk2nd $HOME/bq-lk2nd` `user $` `cd $HOME/bq-lk2nd` `user $````
 make TOOLCHAIN_PREFIX=arm-none-eabi- lk2nd-msm8953
```
### Flash

Unplug from power cable the device, and shut it completely down pressing `Power` until the screen completely turns off (It is not possible to shut it down while it is receiving power, it would reboot)

Plug the usb-c cable data to the computer, and press `Volume Up`+`Volume Down` (Do not press `Power`) in the BQ Aquaris X Pro while doing this plug the cable to the phone.

#### Backup the entire phone (Optional, but very recommended)

`user $``edl rl bq-backup`
#### Flash the bootloader

`user $` `edl w boot $HOME/bq-lk2nd/build-lk2nd-msm8953/lk2nd.img`
## Partitioning

The partition touched are the following:

- boot (LK2ND Bootloader)
- system (/boot)
- userdata (/)

The bootloader is already handled so now it is time to focus on /boot, and /.

Retrieve a copy of both partitions.

`user $``edl r userdata bq-userdata.img && edl r system bq-system.img`
These .img files are going to be used as the base images for partitioning so do not rely on them as backups since this process will remove their contents.

`root #``mkfs.ext2 bq-system.img``root #``mkfs.ext4 bq-userdata.img`
Now mount these images like this, and follow the normal procedure to install gentoo.

`root #``mount bq-userdata.img /mnt/gentoo``root #``mkdir /mnt/gentoo/boot``root #``mount bq-system.img /mnt/gentoo/boot`
## Notes on installing Gentoo

Once uncompressed the correct tarball (An appropriate aarch64 one), and before chrooting, do the following:

### Modify FEATURES to disable some sandboxing capabilities which break qemu

**`/mnt/gentoo/etc/portage/make.conf`**

```
FEATURES="-pid-sandbox -network-sandbox"
```
Now the system is actually ready to follow the steps needed to enter in the chroot environment (Follow [Handbook:AMD64](https://wiki.gentoo.org/wiki/Handbook:AMD64) — A handbook dedicated to installing and configuring Gentoo on the **amd64** architecture., an effort to centralize documentation into a coherent handbook.)

### Additional advice while following the handbook

Ignore everything related to partitioning, bootloader, firmware or kernel in the handbook since it is going to be device specific.

To look up the correct uuids for /etc/fstab use this commands.

`root #``blkid bq-system.img # /boot``root #``blkid bq-userdata.img # /`
Use noauto as option for /boot in fstab since it takes a lot to mount, and it can slow down the system's boot, also ext2 partitions can easily become corrupt if not correctly unmounted so the less time it is mounted the better.

Flashing the system each time a minor configuration error is made, and the system is not usable it is very slow so during the installation ensure to create a user, enable sshd, add a password to the user, append the ssh id to the users authorized\_keys, and give it access to root to that user. (Check everything possible to work)

Ensure to transfer the wifi password to a file in the system for easy first time wifi setup since typing it in a touchscreen is not pleasant.

## Configuring bootloader

Inside the chroot:

`root #``mkdir /boot/extlinux`
**`/boot/extlinux/extlinux.conf`**

## Compiling the kernel

This should be done outside of the chroot:

`user $``git clone https://github.com/bq-msm8953-mainline/linux msm8953-linux``user $``cd msm8953-linux`
Download the Postmarketos Kernel sources for this soc.

`user $``curl -o .config -L https://gitlab.postmarketos.org/postmarketOS/pmaports/-/raw/master/device/community/linux-postmarketos-qcom-msm8953/config-postmarketos-qcom-msm8953.aarch64?ref_type=heads&inline=false`
Run olddefconfig:

`user $``ARCH=arm64 CROSS_COMPILE=aarch64-pc-linux-gnu- make -j$(nproc) olddefconfig`
Join it with the bq config:

`user $``ARCH=arm64 CROSS_COMPILE=aarch64-pc-linux-gnu- make -j$(nproc) olddefconfig bq-common.config`
Create a script for fast compiling:

**`install_kernel.sh`**

Start compiling:

`user $``bash install_kernel.sh`
This will install the kernel in the mounted partitions.

## Install firmware

Anything related to venus currently causes the phone to kernel panic, so avoid it.

These instructions are being written from experience installing in the device but not while installing it so it will point in general lines what firmwares will the device need, and where to get it but it may not be enough precise yet.

These are the links related to firmware by priority.

It is possible to retrieve the original firmware that msm-firmware-loader would seek mounting the backups done already with EDL, it is also possible to do the backup now of those partitions, for example:

`root #``edl r modem modem.img``root #``mkdir /mnt/modem``root #``mount modem.img /mnt/modem`
- [This repository contains some firmware that may be handy, and cannot be found in the partitions like a\d{3}\_zap.\*](https://github.com/bq-msm8953-mainline/firmware-bq-bardockpro)
- [In the official linux-firmware repository missing firmware files can be found](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/qcom)

Some firmware may be signed, and not be transferable from other phones with the same soc that do not share model to bardockpro.

While copying a .mbn file always create a symlink from the same file name but with the extension .mdt.

This is what a working but not cleaned up device looks like: (It can be seen that it has venus firmwares they are not in a location where the kernel finds it, they are not needed like many things in this setup)

While installing please contribute to this wiki page firmware findings.

**`/lib/firmware`**

## Compiling various services needed for the modem to work

### qrtr

It comes with a service qrtr-ns, it is not needed any more since it is done in the kernel side.

#### Install the repository

In the chroot clone the repository.

{(RootCmd|git clone [https://github.com/linux-msm/qrtr}}](https://github.com/linux-msm/qrtr}})

Change directory into the repository.

`root #``cd qrtr`
Execute the following command to see the supported options:

`root #``cat meson_options.txt`
An example configuration of the project to be compiled could look like this for a systemd phone:

`root #``meson setup build -Dsystemd-service=disabled -Dqrtr-ns=disabled --prefix=/usr`
Compile the project:

`root #``meson compile -C build`
Install it:

`root #``meson install -C build`
### Alsa

`root #``git clone https://github.com/bq-msm8953-mainline/alsa-ucm-conf``root #``cd alsa-ucm-conf``root #``cp ucm{,2} /usr/share/alsa`
### Q6voiced

Compile it, and install it, the pmaports package has a init service for openrc which it can used

#### Installing

Clone the repository.

Change to the directory:

({RootCmd|cd q6voiced}}

Configure the project:

`root #``meson setup build`
Compile:

`root #``meson compile -C build`
Install:

`root #``meson install -C build`
#### For Systemd

**`/etc/sytemd/system/q6voiced.service`**

Enable the service

`root #``systemctl enable q6voiced`
#### For Openrc

**`/etc/init.d/q6voiced`**

```
#!/sbin/openrc-run
supervisor=supervise-daemon
name="q6voiced"
description="Enable q6voice audio when call is performed with oFono"
# Note: q6voice_card/q6voice_device need to be set in /etc/conf.d/q6voiced
command="/usr/bin/q6voiced"
command_args="hw:0,4"
supervise_daemon_args="--user nobody --group audio"
depend() {
	need dbus
}
```
Give the correct permissions to the service:

`root #``chmod +x /etc/init.d/q6voiced`
Enable the service:

`root #``rc-update add q6voiced default`
### tqftpserv

#### Install the project

Clone the repository:

Change into the directory:

`root #``cd tqftpserv`
Configure the project:

`root #``meson setup build`
Compile:

`root #``meson compile -C build`
Install:

`root #``meson install -C build`
#### Enable the service in Systemd

`root #``systemctl enable tqftpserv`
#### Create, and enable the service in Openrc

**`/etc/init.d/tqftpserv`**

```
#!/sbin/openrc-run
supervisor=supervise-daemon
name="tqftpserv"
description="Qualcomm Trivial File Transfer Protocol Server"
command="/usr/bin/tqftpserv"
depend() {
	before rmtfs
}
```
Add execution permission:

`root #``chmod +x /etc/init.d/tqftpserv`
Enable the service:

`root #``rc-update add tqftpserv default`
### rmtfs

#### Install the project

Clone the repository:

Compile the repository:

`root #``make rmtfs`
Install the resulting binary:

`root #``install -Dm755 rmtfs /usr/sbin/rmtfs`
#### Create the Systemd service

Create the file:

**`/etc/systemd/system/rmtfs.service`**

Enable the service:

`root #``systemctl enable rmtfs`
#### Create the Openrc service

**`/etc/init.d/rmtfs`**

Add execution permission to the service:

`root #``chmod +x /etc/init.d/rmtfs`
Enable the service:

`root #``rc-update add rmtfs default`
### Udev rules

Search [pmaports](https://gitlab.postmarketos.org/postmarketOS/pmaports) for udev rules to install into /etc/udev/rules.d, this is how it looks in an example installation:

**`/etc/udev/rules.d`**

**Example content of custom udev rules**

### Quirks

Search [pmaports](https://gitlab.postmarketos.org/postmarketOS/pmaports) for needed bash quirks, only adreno-a506-quirks.sh were found by know.

## Install a mobile environment

Check the useflags to support everything the phone needs:

`root #``emerge -a eselect-repository``root #``eselect repository enable guru``root #``emerge -a gdm phosh NetworkManager modemmanager gnome-terminal --autounmask`
With some luck the images are ready to be flashed now.

## Running dracut

`root #``emerge -a dracut``root #``dracut --kver <Everything after vmlinuz- from the file in /boot>`
## Copying the kernel, and initramfs to the correct location

`root #``cp /boot/vmlinuz-<version> /boot/vmlinuz``root #``cp /boot/initramfs-<version> /boot/initrd`
## Flashing the image

Ensure the chroot is completely closed, every bash instance is outside of the /mnt/gentoo, and killed any process from:

`root #``lsof /mnt/gentoo`
And run the following:

`root #``umount -R /mnt/gentoo`
Now run:

`user $``edl w userdata bq-userdata.img``user $``edl w system bq-system.img`
## End of installation

With some luck the phone should boot up with gentoo, and show the gdm init screen, it will be possible to login using the gnome keyboard, and the phone will login into phosh, where it will be possible to join a wifi network, and continue tuning further the phone.

## Rsync updating the phone

It is difficult to keep updated the installation using normal means since trying to do heavy compilations in the phone will cause it to auto shutdown, and also the phone will be wanted to be suspended most of the time, the following method can become handy to keep the phone updated while avoding to use portage at all on the phone:

First fetch the rootfs to the computer using this script:

**`copy-bardockpro.sh`**

**Copy the rootfs excluding heavy files**

Then enter in it as chroot, make the necesary updates, and installations.

When finished the following script can be used to send the updates to the phone.

**`upload-bardockpro.sh`**

**Upload the rootfs to the phone excluding heavy files**

## Missing pieces

Almost everything other venus video encoding, and camera should be working, even calls work pretty well.

## Troubleshooting

### The phone's touchscreen becomes unresponsive after suspend

A workaround for this bug is to create a script that detects new fails in dmesg and removes the touchscreen kernel module and inserts it again: (The script uses sudo to be able to be used as normal user)

**`/usr/local/bin/fix-touch-bq.pl`**

```
#!/usr/bin/env perl
use v5.38.2;
use strict;
use warnings;
my $last_time = 0;
my $last_line;
while (1) {
	eval {
	my $dmesg = `sudo dmesg`;
	my @dmesg = split /\n/, $dmesg;
	my $found = 0;
	for my $line (@dmesg) {
		my ($time, $line) = $line =~ /^\[\s*(\d+\.\d+)\](.*ili210x_i2c.*Unable to get touch data.*)$/m;
		if ($line) {
			if ($time <= $last_time) {
				next;
			}
			$last_line = $line;
			$last_time = $time;
			$found = 1;
		}
	}
	if ($found) {
		say "Solving touch: $last_line.";
		system 'sudo', 'rmmod', 'ili210x';
		system 'sudo', 'modprobe', 'ili210x';
	}
	sleep 1;
	};
	if ($@) {
		warn $@;
	}
}
```
Then it can be added it to the crontab:

`root #``crontab -e`
With this content:

**`Crontab Contents`**

### Modem disconnects randomly

It is possible to overcome this failure running this commands:

`root #``systemctl restart rmtfs``root #``sleep 1``root #``systemctl restart ModemManager``root #``sleep 1``root #``systemctl restart NetworkManager`
If the administrator wanted to automate this process they could do it creating a file like this one:

**`/usr/local/bin/fix-modem-bq.pl`**

```
#!/usr/bin/env perl
use v5.38.2;
use strict;
use warnings;
my $start_time = time;
my $last_time_locked = 0;
while (1) {
    eval {
        if ( ( scalar time ) < $start_time + 60 || ( scalar time ) < $last_time_locked + 60 ) {
            return;
        }
        if ( `mmcli -m any` =~ /state:[^\n]*locked/ ) {
            $last_time_locked = time;
            say 'SIM Card is locked';
            return;
        }
        my $ip_a = `ip a`;
        if ( $ip_a !~ /^[^\n]*\@rmnet_ipa[^\n]*UP[^\n]*$/m ) {
            reset_rmtfs( 'No net' );
            sleep 20;
        }
    };
    if ($@) {
        warn $@;
    }
    sleep 1;
}
sub reset_rmtfs( $message ) {
        say "Reseting rmtfs: $message.";
        system 'sudo', 'systemctl', 'restart', 'rmtfs';
        sleep 1;
        system 'sudo', 'systemctl', 'restart', 'ModemManager';
        sleep 1;
        system 'sudo', 'systemctl', 'restart', 'NetworkManager';
        sleep 1;
}
```
Later executing:

`root #``crontab -e`
And adding this contents to the crontab:

**`Crontab Contents`**

This script will reset all your network again if the phone does not connect to mobile data after unlocking the pin code of the phone in 60 seconds, so it may not be useful for every user depending on the needs.

### Suspend messes up modem, touch and other things

Disable suspend and substitute it with this script:

**`/usr/local/bin/nuke-cpus-on-screen-off.pl`**

```
#!/usr/bin/env perl
use v5.38.2;
use strict;
use warnings;
my $last_state = 0;
while (1) {
    my $current_state;
    eval {
        open my $fh, '<', '/sys/class/backlight/1a94000.dsi.0/actual_brightness'
          or die 'No backlight info';
        $current_state = ( scalar(<$fh>) // 0 ) > 0;
    };
    if ($@) {
        if ( $@ !~ /No backlight info/ ) {
            die $@;
        }
        $current_state = 0;
    }
    if ( ( !!$last_state ) == ( !!$current_state ) ) {
        sleep 1;
        next;
    }
    $last_state    = $current_state;
    $current_state = 0 + !!$current_state;
    for my $cpu_number ( 1 .. 7 ) {
        my $message = $current_state ? 'Powering on' : 'Shutting down';
        say $message . ' core ' . $cpu_number . '.';
	say "/sys/devices/system/cpu/cpu${cpu_number}/online";
        open my $fh, '>', "/sys/devices/system/cpu/cpu${cpu_number}/online" or die 'Whatever';
	say $current_state;
        $fh->print($current_state);
	$fh->flush;
        close $fh;
    }
    sleep 5;
}
```
With this crontab line:

**`Crontab Contents`**

This script will reduce the phone's battery waste without suspend to the half, so the phone will have more or less 6 hours of phone battery if it is unused, better battery saving scripts exist on the internet based in stopping processes, if interested in more battery time looking what the internet has to offer would be a good idea.

### There are too few key shortcut options in the terminal in Phosh for the user's needs

While using the terminal the user may find itself unable to do certain actions because the terminal shortcut options are too limited for example to use vim, this is solvable using dconf to add new shortcuts like this: (In this case adding `Esc`, `Page Up`, and `Page Down`)

`user $``dconf write /sm/puri/phosh/osk/terminal/shortcuts "['Escape', 'Page_Up', 'Page_Down', '<ctrl>', '<alt>', '<ctrl>r', 'Home', 'End', '<ctrl>w', '<alt>b', '<alt>f', '<ctrl>v', '<ctrl>c', '<ctrl><shift>v', '<ctrl><shift>c', 'Menu']"`
The names of the keys are from [gdkkeysyms.h](https://gitlab.gnome.org/GNOME/gtk/-/blob/main/gdk/gdkkeysyms.h) ignoring the leading GDK\_KEY\_.

The full documentation about how those accelerators are parsed can be found in [func.accelerator\_parse.html from GTK4](https://docs.gtk.org/gtk4/func.accelerator_parse.html). The administrator should be able with this hints to craft it's own shortcuts.

The order chosen is respected by Phosh, so the user can tailor it completely to their needs.
