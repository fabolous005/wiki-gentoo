<!-- source: https://wiki.gentoo.org/wiki/Sony_Vaio_VPCEB39FJ | group: Gentoo Wiki (Main) | wiki-title: Sony Vaio VPCEB39FJ -->
---
title: Sony Vaio VPCEB39FJ
url: https://wiki.gentoo.org/wiki/Sony_Vaio_VPCEB39FJ
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-07-26"
fingerprint: "7f5a8524db2a366a"
license: CC BY-SA 4.0
---

# Sony Vaio VPCEB39FJ

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



## Hardware configuration

- Intel(R) Core(TM) i5 CPU M 460 @ 2.53GHz
- 4GB（2GB×2）RAM
- 120GB SSD SATA (changed from HDD 500GB 5400rpm)
- Intel HD Graphics
- 15.5" display with 1366×768 resolution
- LAN 10/100/1000BASE-T
- Atheros Wireless Network Adapter IEEE802.11b/g/n
- Bluetooth
- USB2.0×3,eSATA/USB×1,eSATA×1,D-sub15pin,HDMI×1,mini-jack×1
- Express Card slot x 1
- SD card slotx1,Sony card slotx1
- Size（W×D×H）329×228.5×27.6～31.5mm
- Weight 2.7kg



### PCI, USB buses and devices

#### Resolving of device ID's to names

Printout of [lspci](http://linuxcommand.org/man_pages/lspci8.html) -nn:

`root #``lspci -nn`
00:00.0 Host bridge \[0600\]: Intel Corporation Core Processor DRAM Controller \[8086:0044\] (rev 02)
00:02.0 VGA compatible controller \[0300\]: Intel Corporation Core Processor Integrated Graphics Controller \[8086:0046\] (rev 02)
00:16.0 Communication controller \[0780\]: Intel Corporation 5 Series/3400 Series Chipset HECI Controller \[8086:3b64\] (rev 06)
00:1a.0 USB controller \[0c03\]: Intel Corporation 5 Series/3400 Series Chipset USB2 Enhanced Host Controller \[8086:3b3c\] (rev 05)
00:1b.0 Audio device \[0403\]: Intel Corporation 5 Series/3400 Series Chipset High Definition Audio \[8086:3b56\] (rev 05)
00:1c.0 PCI bridge \[0604\]: Intel Corporation 5 Series/3400 Series Chipset PCI Express Root Port 1 \[8086:3b42\] (rev 05)
00:1c.1 PCI bridge \[0604\]: Intel Corporation 5 Series/3400 Series Chipset PCI Express Root Port 2 \[8086:3b44\] (rev 05)
00:1c.2 PCI bridge \[0604\]: Intel Corporation 5 Series/3400 Series Chipset PCI Express Root Port 3 \[8086:3b46\] (rev 05)
00:1c.5 PCI bridge \[0604\]: Intel Corporation 5 Series/3400 Series Chipset PCI Express Root Port 6 \[8086:3b4c\] (rev 05)
00:1d.0 USB controller \[0c03\]: Intel Corporation 5 Series/3400 Series Chipset USB2 Enhanced Host Controller \[8086:3b34\] (rev 05)
00:1e.0 PCI bridge \[0604\]: Intel Corporation 82801 Mobile PCI Bridge \[8086:2448\] (rev a5)
00:1f.0 ISA bridge \[0601\]: Intel Corporation Mobile 5 Series Chipset LPC Interface Controller \[8086:3b09\] (rev 05)
00:1f.2 SATA controller \[0106\]: Intel Corporation 5 Series/3400 Series Chipset 4 port SATA AHCI Controller \[8086:3b29\] (rev 05)
00:1f.3 SMBus \[0c05\]: Intel Corporation 5 Series/3400 Series Chipset SMBus Controller \[8086:3b30\] (rev 05)
00:1f.6 Signal processing controller \[1180\]: Intel Corporation 5 Series/3400 Series Chipset Thermal Subsystem \[8086:3b32\] (rev 05)
02:00.0 Network controller \[0280\]: Qualcomm Atheros AR9285 Wireless Network Adapter (PCI-Express) \[168c:002b\] (rev 01)
03:00.0 SD Host controller \[0805\]: Ricoh Co Ltd MMC/SD Host Controller \[1180:e822\]
03:00.1 System peripheral \[0880\]: Ricoh Co Ltd R5U2xx (R5U230 / R5U231 / R5U241) \[Memory Stick Host Controller\] \[1180:e230\]
03:00.4 SD Host controller \[0805\]: Ricoh Co Ltd MMC/SD Host Controller \[1180:e822\]
04:00.0 Ethernet controller \[0200\]: Marvell Technology Group Ltd. Yukon Optima 88E8059 \[PCIe Gigabit Ethernet Controller with AVB\] \[11ab:4381\] (rev 11)
3f:00.0 Host bridge \[0600\]: Intel Corporation Core Processor QuickPath Architecture Generic Non-core Registers \[8086:2c62\] (rev 02)
3f:00.1 Host bridge \[0600\]: Intel Corporation Core Processor QuickPath Architecture System Address Decoder \[8086:2d01\] (rev 02)
3f:02.0 Host bridge \[0600\]: Intel Corporation Core Processor QPI Link 0 \[8086:2d10\] (rev 02)
3f:02.1 Host bridge \[0600\]: Intel Corporation 1st Generation Core Processor QPI Physical 0 \[8086:2d11\] (rev 02)
3f:02.2 Host bridge \[0600\]: Intel Corporation 1st Generation Core Processor Reserved \[8086:2d12\] (rev 02)
3f:02.3 Host bridge \[0600\]: Intel Corporation 1st Generation Core Processor Reserved \[8086:2d13\] (rev 02)

#### Kernel drivers handling

Printout of [lspci](http://linuxcommand.org/man_pages/lspci8.html) -kk:

`root #``lspci -kk`
00:00.0 Host bridge: Intel Corporation Core Processor DRAM Controller (rev 02)
	Subsystem: Sony Corporation Device 9071
	Kernel driver in use: agpgart-intel
00:02.0 VGA compatible controller: Intel Corporation Core Processor Integrated Graphics Controller (rev 02)
	Subsystem: Sony Corporation Device 9071
	Kernel driver in use: i915
00:16.0 Communication controller: Intel Corporation 5 Series/3400 Series Chipset HECI Controller (rev 06)
	Subsystem: Sony Corporation Device 9071
	Kernel driver in use: mei\_me
00:1a.0 USB controller: Intel Corporation 5 Series/3400 Series Chipset USB2 Enhanced Host Controller (rev 05)
	Subsystem: Sony Corporation Device 9071
	Kernel driver in use: ehci-pci
00:1b.0 Audio device: Intel Corporation 5 Series/3400 Series Chipset High Definition Audio (rev 05)
	Subsystem: Sony Corporation Device 9071
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel
00:1c.0 PCI bridge: Intel Corporation 5 Series/3400 Series Chipset PCI Express Root Port 1 (rev 05)
	Kernel driver in use: pcieport
	Kernel modules: shpchp
00:1c.1 PCI bridge: Intel Corporation 5 Series/3400 Series Chipset PCI Express Root Port 2 (rev 05)
	Kernel driver in use: pcieport
	Kernel modules: shpchp
00:1c.2 PCI bridge: Intel Corporation 5 Series/3400 Series Chipset PCI Express Root Port 3 (rev 05)
	Kernel driver in use: pcieport
	Kernel modules: shpchp
00:1c.5 PCI bridge: Intel Corporation 5 Series/3400 Series Chipset PCI Express Root Port 6 (rev 05)
	Kernel driver in use: pcieport
	Kernel modules: shpchp
00:1d.0 USB controller: Intel Corporation 5 Series/3400 Series Chipset USB2 Enhanced Host Controller (rev 05)
	Subsystem: Sony Corporation Device 9071
	Kernel driver in use: ehci-pci
00:1e.0 PCI bridge: Intel Corporation 82801 Mobile PCI Bridge (rev a5)
00:1f.0 ISA bridge: Intel Corporation Mobile 5 Series Chipset LPC Interface Controller (rev 05)
	Subsystem: Sony Corporation Device 9071
	Kernel driver in use: lpc\_ich
00:1f.2 SATA controller: Intel Corporation 5 Series/3400 Series Chipset 4 port SATA AHCI Controller (rev 05)
	Subsystem: Sony Corporation Device 9071
	Kernel driver in use: ahci
00:1f.3 SMBus: Intel Corporation 5 Series/3400 Series Chipset SMBus Controller (rev 05)
	Subsystem: Sony Corporation Device 9071
00:1f.6 Signal processing controller: Intel Corporation 5 Series/3400 Series Chipset Thermal Subsystem (rev 05)
	Subsystem: Sony Corporation Device 9071
	Kernel driver in use: intel ips
02:00.0 Network controller: Qualcomm Atheros AR9285 Wireless Network Adapter (PCI-Express) (rev 01)
	Subsystem: Foxconn International, Inc. T77H126.00 802.11bgn Wireless Half-size Mini PCIe Card
	Kernel driver in use: ath9k
	Kernel modules: ath9k
03:00.0 SD Host controller: Ricoh Co Ltd MMC/SD Host Controller
	Subsystem: Sony Corporation Device 9071
	Kernel driver in use: sdhci-pci
03:00.1 System peripheral: Ricoh Co Ltd R5U2xx (R5U230 / R5U231 / R5U241) \[Memory Stick Host Controller\]
	Subsystem: Sony Corporation Device 9071
03:00.4 SD Host controller: Ricoh Co Ltd MMC/SD Host Controller
	Subsystem: Sony Corporation Device 9071
	Kernel driver in use: sdhci-pci
04:00.0 Ethernet controller: Marvell Technology Group Ltd. Yukon Optima 88E8059 \[PCIe Gigabit Ethernet Controller with AVB\] (rev 11)
	Subsystem: Sony Corporation Device 9071
	Kernel driver in use: sky2
3f:00.0 Host bridge: Intel Corporation Core Processor QuickPath Architecture Generic Non-core Registers (rev 02)
	Subsystem: Sony Corporation Device 9071
3f:00.1 Host bridge: Intel Corporation Core Processor QuickPath Architecture System Address Decoder (rev 02)
	Subsystem: Sony Corporation Device 9071
3f:02.0 Host bridge: Intel Corporation Core Processor QPI Link 0 (rev 02)
	Subsystem: Sony Corporation Device 9071
3f:02.1 Host bridge: Intel Corporation 1st Generation Core Processor QPI Physical 0 (rev 02)
	Subsystem: Sony Corporation Device 9071
3f:02.2 Host bridge: Intel Corporation 1st Generation Core Processor Reserved (rev 02)
	Subsystem: Sony Corporation Device 9071
3f:02.3 Host bridge: Intel Corporation 1st Generation Core Processor Reserved (rev 02)
	Subsystem: Sony Corporation Device 9071



#### List USB devices

Printout of [lsusb](http://www.linuxcommand.org/man_pages/lsusb8.html) (no external devices connected):

`root #``lsusb` Bus 002 Device 003: ID 192f:0916 Avago Technologies, Pte. 
Bus 002 Device 002: ID 8087:0020 Intel Corp. Integrated Rate Matching Hub
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 004: ID 054c:02e1 Sony Corp. FeliCa S330 \[PaSoRi\]
Bus 001 Device 003: ID 0c45:6464 Microdia 
Bus 001 Device 002: ID 8087:0020 Intel Corp. Integrated Rate Matching Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

### The status of modules in the Linux Kernel

Printout of [lsmod](http://linuxcommand.org/man_pages/lsmod8.html) (builtin devices, no external devices connected):

`root #``lsmod`
Module                  Size  Used by
nf\_conntrack\_irc        3483  0 
nf\_conntrack\_tftp       3777  0 
nf\_conntrack\_h323      37132  0 
ts\_kmp                  1639  5 
nf\_conntrack\_amanda     2261  0 
nf\_conntrack\_netlink    19743  0 
nfnetlink               4800  1 nf\_conntrack\_netlink
nf\_conntrack\_proto\_udplite     3319  0 
nf\_conntrack\_ftp        6063  0 
nf\_conntrack\_sane       3980  0 
nf\_conntrack\_proto\_sctp     7373  0 
nf\_conntrack\_pptp       3675  0 
nf\_conntrack\_proto\_gre     3596  1 nf\_conntrack\_pptp
nf\_conntrack\_sip       18149  0 
nf\_conntrack\_netbios\_ns     1005  0 
nf\_conntrack\_broadcast     1085  1 nf\_conntrack\_netbios\_ns
iptable\_mangle          1400  0 
xt\_tcpudp               2255  24 
nf\_conntrack\_ipv4      10989  33 
nf\_defrag\_ipv4          1283  1 nf\_conntrack\_ipv4
xt\_multiport            1542  16 
xt\_conntrack            2793  33 
nf\_conntrack           59236  16 nf\_conntrack\_netbios\_ns,nf\_conntrack\_proto\_gre,nf\_conntrack\_proto\_udplite,xt\_conntrack,nf\_conntrack\_amanda,nf\_conntrack\_proto\_sctp,nf\_conntrack\_netlink,nf\_conntrack\_broadcast,nf\_conntrack\_ftp,nf\_conntrack\_irc,nf\_conntrack\_sip,nf\_conntrack\_h323,nf\_conntrack\_ipv4,nf\_conntrack\_pptp,nf\_conntrack\_sane,nf\_conntrack\_tftp
iptable\_filter          1304  1 
ip\_tables              13715  2 iptable\_filter,iptable\_mangle
x\_tables               13936  6 ip\_tables,xt\_tcpudp,xt\_conntrack,xt\_multiport,iptable\_filter,iptable\_mangle
binfmt\_misc             5926  1 
snd\_hda\_codec\_hdmi     32343  1 
microcode               7660  0 
snd\_hda\_codec\_realtek    48738  1 
snd\_hda\_codec\_generic    40105  1 snd\_hda\_codec\_realtek
ath9k                  62660  0 
ath9k\_common           17829  1 ath9k
snd\_hda\_intel          17056  4 
ath9k\_hw              362833  2 ath9k\_common,ath9k
snd\_hda\_controller     13792  1 snd\_hda\_intel
ath                    16848  3 ath9k\_common,ath9k,ath9k\_hw
snd\_hda\_codec          68182  5 snd\_hda\_codec\_realtek,snd\_hda\_codec\_hdmi,snd\_hda\_codec\_generic,snd\_hda\_intel,snd\_hda\_controller
snd\_hwdep               5171  1 snd\_hda\_codec
snd\_pcm                63611  4 snd\_hda\_codec\_hdmi,snd\_hda\_codec,snd\_hda\_intel,snd\_hda\_controller
snd\_timer              16089  1 snd\_pcm
snd                    48999  15 snd\_hwdep,snd\_timer,snd\_hda\_codec\_hdmi,snd\_pcm,snd\_hda\_codec\_generic,snd\_hda\_codec,snd\_hda\_intel
shpchp                 21769  0 
xts                     2759  0 
aes\_x86\_64              7263  4 
cbc                     2472  0 
nfs                   102981  0 
lockd                  52774  1 nfs
grace                   1570  1 lockd
sunrpc                152697  2 nfs,lockd
sx8                    10860  0 
mptsas                 32954  0 
scsi\_transport\_sas     21430  1 mptsas
mptfc                  10205  0 
scsi\_transport\_fc      39461  1 mptfc
mptspi                 11267  0 
scsi\_transport\_spi     17449  1 mptspi
mptscsih               16097  3 mptfc,mptsas,mptspi
mptbase                54487  4 mptfc,mptsas,mptspi,mptscsih

### Bluetooth

Configure Bluetooth by following instructions found in the [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth) article.

### Laptop sound on TV with HDMI cable

Emerge the [media-sound/pavucontrol](https://packages.gentoo.org/packages/media-sound/pavucontrol) utility if laptop has a sound output problem with HDMI cable, it helps to configure sound through HDMI cable.

## Kernel configuration

To build a custom kernel configuration, it is helpful to get output with console: lspci -nn (lsusb) and lspci -k. Use any bootable Linux LiveCD for that. There is **\[vendor id:device id\]** (for example, see lspci -nn above, **00:16.0** Communication controller **\[0780\]**: Intel Corporation 5 Series/3400 Series Chipset HECI Controller **\[8086:3b64\]**) and **kernel driver** (for example, see lspci -kk above, Kernel driver in use: **mei\_me** ) information. It is easy to find which kernel modules need to be set with  **\[vendor id:device id\]** and kernel driver. Use search in Internet or kernel documentation.

## Troubleshooting

### Graphics card. Problem with 100% brightness after reboot

How to install graphics driver described (see, Gen5) in the [Intel](https://wiki.gentoo.org/wiki/Intel) article.

Laptop does not remember brightness, after rebooting it sets brightness to 100%

#### Workaround

Install [xbacklight](http://linux.die.net/man/1/xbacklight) (for xf86-video-intel only):

`root #``emerge --ask x11-apps/xbacklight`
or [acpilight](https://github.com/wavexx/acpilight/) (Modern replacement which uses kernel interfaces)

`root #``emerge --ask sys-power/acpilight`
Create udev rule to save brightness

**`/etc/udev/rules.d/91-backlight.rules`**

**udev event to save brightness**

```
# Storing brightness
ACTION=="change", SUBSYSTEM=="backlight", RUN+="/bin/sh /etc/backlight.sh"
```
Create scripts to save and restore actual brightness:

**`/etc/backlight.sh`**

**Script to save brightness**

```
#!/bin/sh
#It saves actual backlight percentage to /etc/backlight
brightness=`cat /sys/class/backlight/acpi_video0/actual_brightness`
brightness_max=`cat /sys/class/backlight/acpi_video0/max_brightness`
brightness_out=$(((100000/brightness_max*brightness+500)/1000))
echo $brightness_out > /etc/backlight
```
This script need to be added to startup, for example /bin/sh /home/user/Scripts/brightness.sh, run it as user:

**`/home/user/Scripts/brightness.sh`**

**Script to restore brightness on startup**

```
#!/bin/sh
brightness=`cat /etc/backlight`
xbacklight -set $brightness
```
Apply udev 91-backlight.rules with:

`root #``udevadm control -R`
### Read-only SD-Card

It found that sometimes SD-card with non-locked switch recognized as read-only after it inserted [\[1\]](http://superuser.com/questions/795034/format-a-protected-corrupted-sd-card)

#### Workaround

Install \[Hdparm\]\]. Use console commands to remove read-only attribute before mounting sd-card partition:

`root #``emerge --ask hdparm``root #````
hdparm -r 0 /dev/mmcblk0
```
`root #````
hdparm -r 0 /dev/mmcblk0p1
```
For example, the following script remounts a SD-card as read-write (rw). You need to correct mount\_options and xuser (user name) to adapt the script for your needs. It needs to run script after sd-card mounted. [udisks-glue](https://wiki.gentoo.org/wiki/Udisks#Additional_software) can be used to start this script automatically.

**`sdcard-mount-fix`**

**Changing sdcard mount from RO to RW**

```
#!/bin/sh
export PATH=/bin:/sbin:/usr/bin:/usr/sbin
sdcard=`mount | grep mmcblk0p1`
if ! grep -q mmcblk0p1 <<< "$sdcard" ;then
	exit 0
fi
if grep -q rw <<< "$sdcard" ;then
	exit 0
fi
logger -i "$sdcard"
logger -i "Fixing that sdcard mounted as read-only"	
cd /dev/disk/by-uuid/
for entry in *
do
  linktext=`readlink $entry` 
  if  /bin/grep -q mmcblk0p1 <<< "$linktext" ;then
		hdparm -r 0 /dev/mmcblk0 2&>1 >/dev/null
		hdparm -r 0 /dev/mmcblk0p1 2&>1 >/dev/null
		xuser="tester"
		mount_options="nodev,nosuid,uid=1000,gid=100,shortname=mixed,dmask=0077,utf8=1,showexec,flush,uhelper=udisks2"
		mount -o remount,rw,$mount_options /dev/mmcblk0p1 /run/media/$xuser/$entry
		mountresult=`mount | grep mmcblk0p1`
		logger -i "sdcard mounted as $mountresult"
  fi 
done
```
### VAIO key (Vendor key) does not work

VAIO key has keycode 360. X does not support keycodes are greater than 255.

#### Workaround 1

It needs to remap VAIO key to make it works with XFCE (probably Gnome, KDE) to bind it in order to start script or application with VAIO key.

Create /etc/udev/hwdb.d/61-keyboard-local.hwdb:

**`/etc/udev/hwdb.d/61-keyboard-local.hwdb`**

**Event creation for VAIO key**

```
# sony-laptop driver
keyboard:name:Sony Vaio Keys:dmi:bvn*:bvr*:bd*:svnSony*:pn*
 KEYBOARD_KEY_3b=prog4                                  # VAIO
```
Update the database as described [here](https://wiki.gentoo.org/wiki/Udev#Remapping_keys_and_buttons) and reboot the PC.

#### Workaround 2

Other method is to edit include/uapi/linux/input-event-codes.h kernel file. It needs to change KEY\_VENDOR keycode from 360 (0x168) to keycode with value less than 255.

Example, swiping KEY\_CHAT and KEY\_VENDOR:

Create /usr/src/linux/input-event-codes.h.patch:

**`/usr/src/linux/input-event-codes.h.patch`**

**KEY\_VENDOR patch**

```
 a/include/uapi/linux/input-event-codes.h	2016-01-11 08:01:32.000000000 +0900
+++ b/include/uapi/linux/input-event-codes.h	2016-07-03 18:19:38.370012319 +0900
@@ -289,7 +289,7 @@
 #define KEY_SOUND		213
 #define KEY_QUESTION		214
 #define KEY_EMAIL		215
-#define KEY_CHAT		216
+#define KEY_VENDOR		216
 #define KEY_SEARCH		217
 #define KEY_CONNECT		218
 #define KEY_FINANCE		219	/* AL Checkbook/Finance */
@@ -425,7 +425,7 @@
 #define KEY_OPTION		0x165
 #define KEY_INFO		0x166	/* AL OEM Features/Tips/Tutorial */
 #define KEY_TIME		0x167
-#define KEY_VENDOR		0x168
+#define KEY_CHAT		0x168
 #define KEY_ARCHIVE		0x169
 #define KEY_PROGRAM		0x16a	/* Media Select Program Guide */
 #define KEY_CHANNEL		0x16b
```
Apply it with:

`root #````
cd /usr/src/linux
```
`root #````
patch -p1 < input-event-codes.h.patch
```
Then recompile kernel, reboot PC.
