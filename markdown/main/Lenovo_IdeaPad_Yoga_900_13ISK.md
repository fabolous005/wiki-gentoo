<!-- source: https://wiki.gentoo.org/wiki/Lenovo_IdeaPad_Yoga_900_13ISK | group: Gentoo Wiki (Main) | wiki-title: Lenovo IdeaPad Yoga 900 13ISK -->
---
title: Lenovo IdeaPad Yoga 900 13ISK
url: https://wiki.gentoo.org/wiki/Lenovo_IdeaPad_Yoga_900_13ISK
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "57481051f5b6a3f4"
license: CC BY-SA 4.0
---

# Lenovo IdeaPad Yoga 900 13ISK

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

Still working on installation, wiki will come soon.

## Hardware Lenovo Yoga 900-13ISK

on linux-5.3.9 kernel

### Laptop Specifications

| Device | Model | Works | Notes | 
|---|---|---|---|
| Intel® Core™ i7 | Intel Core i7 (6th Gen) 6500U / 2.5 GHz |  |  | 
| Intel® HD Graphics | 520 |  | by default i915 driver will be used, need USE change to use i965 idriver instead See [Intel](https://wiki.gentoo.org/wiki/Intel). | 
| samsung 13.3"3200×1800 touchscreen | Wide QXGA+ (WQXGA+) 3200×1800, 16:9 aspect ratio |  | intel\_backlight works, PPI of 276.05 | 
| Wireless Intel Corporation Wireless 8260 (rev 3a) |  |  |  | 
| Bluetooth |  |  |  | 
| Sound |  |  |  | 
| Camera |  |  |  | 
| Card Reader |  |  |  | 
| Touchscreen |  |  | multitouch ok | 
| Touchpad |  |  | some multitouch support (two finger scrolling ok) monotouch ok, left and right click ok, touch click ok | 

#### Forum

see [https://forums.gentoo.org/viewtopic-p-8223726.html#8223726](https://forums.gentoo.org/viewtopic-p-8223726.html#8223726) for intallation discussion

## Configuration details

### host bridge

`root #``lspci -nn -k`
00:00.0 Host bridge \[0600\]: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Host Bridge/DRAM Registers \[8086:1904\] (rev 08)
	Subsystem: Lenovo Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Host Bridge/DRAM Registers \[17aa:3800\]
	Kernel driver in use: skl\_uncore

### Graphics

`root #``lspci -nn -k`
00:02.0 VGA compatible controller \[0300\]: Intel Corporation HD Graphics 520 \[8086:1916\] (rev 07)
	Subsystem: Lenovo HD Graphics 520 \[17aa:3800\]
	Kernel driver in use: i915

See [Intel](https://wiki.gentoo.org/wiki/Intel).

See [\[1\]](https://forums.gentoo.org/viewtopic-p-8538508.html#8538508) for video decoding hardware acceleration

### pcie bus

`root #``lspci -nn -k`
00:1c.0 PCI bridge \[0604\]: Intel Corporation Sunrise Point-LP PCI Express Root Port #5 \[8086:9d14\] (rev f1)
	Kernel driver in use: pcieport
00:1c.5 PCI bridge \[0604\]: Intel Corporation Sunrise Point-LP PCI Express Root Port #6 \[8086:9d15\] (rev f1)
	Kernel driver in use: pcieport

### USB Bus

`root #``lspci -nn -k`
00:14.0 USB controller \[0c03\]: Intel Corporation Sunrise Point-LP USB 3.0 xHCI Controller \[8086:9d2f\] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP USB 3.0 xHCI Controller \[17aa:3800\]
	Kernel driver in use: xhci\_hcd

### SMBus

`root #``lspci -nn -k`
00:1f.4 SMBus \[0c05\]: Intel Corporation Sunrise Point-LP SMBus \[8086:9d23\] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP SMBus \[17aa:3800\]
	Kernel driver in use: i801\_smbus
	Kernel modules: i2c\_i801

### ISA bus

`root #``lspci -nn -k`
00:1f.0 ISA bridge \[0601\]: Intel Corporation Sunrise Point-LP LPC Controller \[8086:9d48\] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP LPC Controller \[17aa:3800\]

### Power Management Controler

`root #``lspci -nn -k`
00:04.0 Signal processing controller \[1180\]: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem \[8086:1903\] (rev 08)
	Subsystem: Lenovo Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem \[17aa:3800\]
00:14.2 Signal processing controller \[1180\]: Intel Corporation Sunrise Point-LP Thermal subsystem \[8086:9d31\] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP Thermal subsystem \[17aa:3800\]
00:1f.2 Memory controller \[0580\]: Intel Corporation Sunrise Point-LP PMC \[8086:9d21\] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP PMC \[17aa:3800\]

### SATA SSD

`root #``lspci -nn -k`
00:17.0 SATA controller \[0106\]: Intel Corporation Sunrise Point-LP SATA Controller \[AHCI mode\] \[8086:9d03\] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP SATA Controller \[AHCI mode\] \[17aa:3800\]
	Kernel driver in use: ahci



### Display

Backlight control through brightness buttons works without modification on X screen resolution is a little bit tricky to tune

`root #``cat /etc/X11/xorg.conf.d/90-monitor````
Section "Monitor"
    Identifier             "Monitor-eDP-1"
    DisplaySize            293 165    # In millimeters
EndSection
```
`root #``cat .xinitrc` xrandr  --dpi 192
xrdb -merge \~/.Xresources
xinput set-prop "Synaptics TM3066-002" "Synaptics Palm Detection" 1
xinput set-prop "Synaptics TM3066-002" "Synaptics Palm Dimensions" 5, 5
#exec ck-launch-session dbus-launch --sh-syntax --exit-with-session xfce4-session
exec ck-launch-session dbus-launch --sh-syntax --exit-with-session fluxbox

`root #``cat .Xresources` Xft.dpi: 144
Xft.autohint: 0
Xft.lcdfilter:  lcddefault
Xft.hintstyle:  hintfull
Xft.hinting: 1
Xft.antialias: 1
Xft.rgba: rgb

In xfce->settings->appearance->fonts, set custom DPI to 140
see [https://wiki.archlinux.org/index.php/Xorg#Display\_size\_and\_DPI](https://wiki.archlinux.org/index.php/Xorg#Display_size_and_DPI) for more settings on DPI especially for gtk3 based app

### Wireless

#### Method 1: Using the kernel driver

##### Configure and compile kernel

`root #``lspci -nn -k`
01:00.0 Network controller \[0280\]: Intel Corporation Wireless 8260 \[8086:24f3\] (rev 3a)
	Subsystem: Intel Corporation Wireless 8260 \[8086:1130\]
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi

##### Install firmware package

`root #``emerge --ask sys-kernel/linux-firmware`
### sound

`root #``lspci -nn -k`
00:1f.3 Audio device \[0403\]: Intel Corporation Sunrise Point-LP HD Audio \[8086:9d70\] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP HD Audio \[17aa:3800\]
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel, snd\_soc\_skl

The package [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) is required.

`root #``emerge --ask sys-kernel/linux-firmware`
### bluetooth

`root #``lsusb -t````
/:  Bus 02.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/6p, 5000M
/:  Bus 01.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/12p, 480M
    |__ Port 7: Dev 4, If 0, Class=Wireless, Driver=btusb, 12M
    |__ Port 7: Dev 4, If 1, Class=Wireless, Driver=btusb, 12M 
```
working under command line (net-wireless/bluez-tools) could pair to an android phone

[Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth) — describes the configuration and usage of Bluetooth controllers and devices.

### misc

`root #``lspci -nn -k`
00:16.0 Communication controller \[0780\]: Intel Corporation Sunrise Point-LP CSME HECI #1 \[8086:9d3a\] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP CSME HECI \[17aa:3800\]
	Kernel driver in use: mei\_me

### Touchpad and TouchScreen

`root #``lspci -nn -k`
00:15.0 Signal processing controller \[1180\]: Intel Corporation Sunrise Point-LP Serial IO I2C Controller #0 \[8086:9d60\] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP Serial IO I2C Controller \[17aa:3800\]
	Kernel driver in use: intel-lpss
	Kernel modules: intel\_lpss\_pci
00:15.1 Signal processing controller \[1180\]: Intel Corporation Sunrise Point-LP Serial IO I2C Controller #1 \[8086:9d61\] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP Serial IO I2C Controller \[17aa:3800\]
	Kernel driver in use: intel-lpss
	Kernel modules: intel\_lpss\_pci
00:15.3 Signal processing controller \[1180\]: Intel Corporation Sunrise Point-LP Serial IO I2C Controller #3 \[8086:9d63\] (rev 21)
	Subsystem: Lenovo Sunrise Point-LP Serial IO I2C Controller \[17aa:3800\]
	Kernel driver in use: intel-lpss
	Kernel modules: intel\_lpss\_pci

`root #````
xinput list
```
```
Virtual core pointer                    	id=2	[master pointer  (3)]
   ↳ Virtual core XTEST pointer              	id=4	[slave  pointer  (2)]
   ↳ Synaptics TM3066-002                    	id=10	[slave  pointer  (2)]
   ↳ ELAN21EF:00 04F3:21EF                   	id=11	[slave  pointer  (2)]
Virtual core keyboard                   	id=3	[master keyboard (2)]
    ↳ Virtual core XTEST keyboard             	id=5	[slave  keyboard (3)]
    ↳ Power Button                            	id=6	[slave  keyboard (3)]
    ↳ Video Bus                               	id=7	[slave  keyboard (3)]
    ↳ Power Button                            	id=8	[slave  keyboard (3)]
    ↳ Lenovo EasyCamera: Lenovo EasyC         	id=9	[slave  keyboard (3)]
    ↳ AT Translated Set 2 keyboard            	id=12	[slave  keyboard (3)]
```


`root #````
less /proc/bus/input/devices
```
I: Bus=0018 Vendor=06cb Product=77c6 Version=0100
N: Name="Synaptics TM3066-002"
P: Phys=i2c-SYNA2B29:00
S: Sysfs=/devices/pci0000:00/0000:00:15.1/i2c_designware.1/i2c-6/i2c-SYNA2B29:00/0018:06CB:77C6.0002/input/input23
U: Uniq=
H: Handlers=mouse0 event15 
B: PROP=5
B: EV=b
B: KEY=e520 10000 0 0 0 0
B: ABS=6f3800001000003
I: Bus=0018 Vendor=04f3 Product=21ef Version=0100
N: Name="ELAN21EF:00 04F3:21EF"
P: Phys=i2c-ELAN21EF:00
S: Sysfs=/devices/pci0000:00/0000:00:15.3/i2c_designware.2/i2c-7/i2c-ELAN21EF:00/0018:04F3:21EF.0003/input/input24
U: Uniq=
H: Handlers=mouse1 event16 
B: PROP=2
B: EV=1b
B: KEY=400 0 0 0 0 0
B: ABS=3273800000000003
B: MSC=20

See [Synaptics](https://wiki.gentoo.org/wiki/Synaptics) to configure.

### SD card

`root #``lspci -nn -k`
02:00.0 SD Host controller \[0805\]: O2 Micro, Inc. Device \[1217:8620\] (rev 01)
	Subsystem: Lenovo Device \[17aa:3800\]
	Kernel driver in use: sdhci-pci
	Kernel modules: sdhci\_pci

The reader detects the partition but gives timeouts on linux 4.16.11

`root #``dd if=/dev/mmcblk0 bs=512 count=1 of=test`
\[385702.654671\] mmc0: Tuning timeout, falling back to fixed sampling clock
\[385702.654769\] mmc0: new ultra high speed SDR104 SDXC card at address 0001
\[385702.655364\] mmcblk0: mmc0:0001 EE8QT 239 GiB 
\[385702.657337\]  mmcblk0: p1
\[385713.140629\] mmc0: Timeout waiting for hardware interrupt.
\[385713.140635\] mmc0: sdhci: ============ SDHCI REGISTER DUMP ===========
\[385713.140643\] mmc0: sdhci: Sys addr:  0x00000008 | Version:  0x00000603
\[385713.140649\] mmc0: sdhci: Blk size:  0x00007200 | Blk cnt:  0x00000008
\[385713.140655\] mmc0: sdhci: Argument:  0x1dcffff0 | Trn mode: 0x0000003b
\[385713.140661\] mmc0: sdhci: Present:   0x01ff0000 | Host ctl: 0x00000017
\[385713.140667\] mmc0: sdhci: Power:     0x0000000f | Blk gap:  0x00000000
\[385713.140673\] mmc0: sdhci: Wake-up:   0x00000000 | Clock:    0x00000007
\[385713.140678\] mmc0: sdhci: Timeout:   0x0000000a | Int stat: 0x00000000
\[385713.140684\] mmc0: sdhci: Int enab:  0x02ff008b | Sig enab: 0x02ff008b
\[385713.140690\] mmc0: sdhci: AC12 err:  0x00000004 | Slot int: 0x00000000
\[385713.140696\] mmc0: sdhci: Caps:      0x25fcc8bf | Caps\_1:   0x00002077
\[385713.140702\] mmc0: sdhci: Cmd:       0x0000123a | Max curr: 0x005800c8
\[385713.140708\] mmc0: sdhci: Resp\[0\]:   0x00000900 | Resp\[1\]:  0x00000000
\[385713.140714\] mmc0: sdhci: Resp\[2\]:   0x00000000 | Resp\[3\]:  0x00001b00
\[385713.140718\] mmc0: sdhci: Host ctl2: 0x0000800b
\[385713.140723\] mmc0: sdhci: ADMA Err:  0x00000000 | ADMA Ptr: 0xfffff208
\[385713.140726\] mmc0: sdhci: ============================================
\[385713.191687\] mmc0: Tuning timeout, falling back to fixed sampling clock

patch to fix this issue (no more required on 5.9.1)

```
diff --git a/drivers/mmc/host/sdhci-pci-o2micro.c b/drivers/mmc/host/sdhci-pci-o2micro.c
index 19944b004..1d8938a68 100644
--- a/drivers/mmc/host/sdhci-pci-o2micro.c
+++ b/drivers/mmc/host/sdhci-pci-o2micro.c
@@ -681,7 +681,6 @@ static const struct sdhci_ops sdhci_pci_o2_ops = {
 const struct sdhci_pci_fixes sdhci_o2 = {
        .probe = sdhci_pci_o2_probe,
        .quirks = SDHCI_QUIRK_NO_ENDATTR_IN_NOPDESC,
-       .quirks2 = SDHCI_QUIRK2_CLEAR_TRANSFERMODE_REG_BEFORE_CMD,
        .probe_slot = sdhci_pci_o2_probe_slot,
 #ifdef CONFIG_PM_SLEEP
        .resume = sdhci_pci_o2_resume,
```
### webcam

`root #``lsusb -t````
  
    |__ Port 6: Dev 3, If 0, Class=Video, Driver=uvcvideo, 480M
    |__ Port 6: Dev 3, If 1, Class=Video, Driver=uvcvideo, 480M
```
### HEVC

### driver summary

`root #``lsmod`
Module                  Size  Used by
8021q                  32768  0
garp                   16384  1 8021q
stp                    16384  1 garp
llc                    16384  2 stp,garp
ctr                    16384  2
ccm                    20480  6
tcp\_diag               16384  0
udp\_diag               16384  0
inet\_diag              24576  2 tcp\_diag,udp\_diag
xt\_conntrack           16384  18
nf\_conntrack          151552  1 xt\_conntrack
nf\_defrag\_ipv4         16384  1 nf\_conntrack
ip6table\_filter        16384  0
ip6\_tables             28672  1 ip6table\_filter
ipv6                  540672  39 udp\_diag
crc\_ccitt              16384  1 ipv6
nf\_defrag\_ipv6         24576  2 nf\_conntrack,ipv6
iptable\_filter         16384  1
ip\_tables              28672  1 iptable\_filter
rfcomm                 90112  4
cmac                   16384  15
ecb                    16384  8
algif\_skcipher         16384  7
bnep                   28672  2
snd\_hda\_codec\_hdmi     65536  1
snd\_hda\_codec\_realtek   126976  1
snd\_hda\_codec\_generic    94208  1 snd\_hda\_codec\_realtek
ledtrig\_audio          16384  2 snd\_hda\_codec\_generic,snd\_hda\_codec\_realtek
hid\_multitouch         32768  0
mmc\_block              49152  2
hid\_sensor\_custom      28672  0
hid\_rmi                24576  0
rmi\_core               61440  1 hid\_rmi
hid\_sensor\_hub         24576  1 hid\_sensor\_custom
vfat                   20480  1
i2c\_designware\_platform    16384  0
iTCO\_wdt               16384  0
i2c\_designware\_core    28672  1 i2c\_designware\_platform
iTCO\_vendor\_support    16384  1 iTCO\_wdt
wmi\_bmof               16384  0
uvcvideo              114688  0
i915                 1921024  17
btusb                  57344  0
x86\_pkg\_temp\_thermal    20480  0
btrtl                  24576  1 btusb
videobuf2\_vmalloc      20480  1 uvcvideo
btbcm                  16384  1 btusb
btintel                28672  1 btusb
videobuf2\_memops       20480  1 videobuf2\_vmalloc
coretemp               20480  0
iwlmvm                323584  0
videobuf2\_v4l2         28672  1 uvcvideo
videobuf2\_common       57344  2 videobuf2\_v4l2,uvcvideo
bluetooth             655360  33 btrtl,btintel,btbcm,bnep,btusb,rfcomm
snd\_hda\_intel          49152  3
mac80211              802816  1 iwlmvm
kvm\_intel             229376  0
drm\_kms\_helper        200704  1 i915
libarc4                16384  1 mac80211
ecdh\_generic           16384  2 bluetooth
videodev              233472  3 videobuf2\_v4l2,uvcvideo,videobuf2\_common
snd\_hda\_codec         147456  4 snd\_hda\_codec\_generic,snd\_hda\_codec\_hdmi,snd\_hda\_intel,snd\_hda\_codec\_realtek
ecc                    32768  1 ecdh\_generic
kvm                   753664  1 kvm\_intel
mc                     61440  4 videodev,videobuf2\_v4l2,uvcvideo,videobuf2\_common
irqbypass              16384  1 kvm
snd\_hda\_core          102400  5 snd\_hda\_codec\_generic,snd\_hda\_codec\_hdmi,snd\_hda\_intel,snd\_hda\_codec,snd\_hda\_codec\_realtek
iwlwifi               282624  1 iwlmvm
sdhci\_pci              49152  0
drm                   573440  7 drm\_kms\_helper,i915
snd\_hwdep              16384  1 snd\_hda\_codec
cqhci                  32768  1 sdhci\_pci
ghash\_clmulni\_intel    16384  0
snd\_pcm               118784  4 snd\_hda\_codec\_hdmi,snd\_hda\_intel,snd\_hda\_codec,snd\_hda\_core
sdhci                  65536  1 sdhci\_pci
syscopyarea            16384  1 drm\_kms\_helper
cryptd                 24576  1 ghash\_clmulni\_intel
sysfillrect            16384  1 drm\_kms\_helper
mmc\_core              176128  4 sdhci,cqhci,mmc\_block,sdhci\_pci
pcspkr                 16384  0
sysimgblt              16384  1 drm\_kms\_helper
snd\_timer              40960  1 snd\_pcm
serio\_raw              20480  0
cfg80211              823296  3 iwlmvm,iwlwifi,mac80211
fb\_sys\_fops            16384  1 drm\_kms\_helper
iosf\_mbi               24576  3 i2c\_designware\_platform,i915,sdhci\_pci
snd                    94208  14 snd\_hda\_codec\_generic,snd\_hda\_codec\_hdmi,snd\_hwdep,snd\_hda\_intel,snd\_hda\_codec,snd\_hda\_codec\_realtek,snd\_timer,snd\_pcm
soundcore              16384  1 snd
i2c\_i801               32768  0
i2c\_algo\_bit           16384  1 i915
intel\_lpss\_pci         20480  0
rfkill                 28672  5 bluetooth,cfg80211
intel\_lpss             16384  1 intel\_lpss\_pci
wmi                    32768  1 wmi\_bmof
i2c\_hid                32768  0
video                  49152  1 i915
backlight              20480  2 video,i915
vboxpci                28672  0
vboxnetadp             28672  0
vboxnetflt             32768  0
vboxdrv               466944  3 vboxpci,vboxnetadp,vboxnetflt
elan\_i2c               49152  0
i2c\_core               94208  10 i2c\_designware\_platform,videodev,i2c\_hid,i2c\_designware\_core,drm\_kms\_helper,i2c\_algo\_bit,elan\_i2c,i2c\_i801,i915,drm
xts                    16384  1
aes\_generic            36864  22
crc32c\_intel           24576  6
cbc                    16384  0
sha256\_generic         20480  0
msdos                  20480  0
fat                    86016  2 msdos,vfat
efivarfs               16384  1
configfs               53248  1
cramfs                 53248  0
squashfs               65536  0
fuse                  131072  3
xfs                  1482752  0
nfs                   319488  0
lockd                  98304  1 nfs
grace                  16384  1 lockd
sunrpc                405504  2 lockd,nfs
fscache               385024  1 nfs
jfs                   200704  0
reiserfs              270336  0
btrfs                1454080  0
zstd\_decompress        90112  1 btrfs
zstd\_compress         192512  1 btrfs
bcache                270336  0
crc64                  16384  1 bcache
ext4                  737280  4
jbd2                  126976  1 ext4
ext2                   86016  0
mbcache                16384  2 ext4,ext2
linear                 20480  0
raid10                 61440  0
raid1                  49152  0
raid0                  24576  0
dm\_zero                16384  0
dm\_verity              32768  0
reed\_solomon           20480  1 dm\_verity
dm\_thin\_pool           81920  0
dm\_switch              16384  0
dm\_snapshot            53248  0
dm\_raid                45056  0
raid456               172032  1 dm\_raid
async\_raid6\_recov      24576  1 raid456
async\_memcpy           20480  2 raid456,async\_raid6\_recov
async\_pq               20480  2 raid456,async\_raid6\_recov
raid6\_pq              122880  4 async\_pq,btrfs,raid456,async\_raid6\_recov
dm\_mirror              28672  0
dm\_region\_hash         16384  1 dm\_mirror
dm\_log\_writes          20480  0
dm\_log\_userspace       24576  0
dm\_log                 20480  3 dm\_region\_hash,dm\_log\_userspace,dm\_mirror
dm\_integrity           61440  0
async\_xor              20480  4 dm\_integrity,async\_pq,raid456,async\_raid6\_recov
async\_tx               20480  5 async\_pq,async\_memcpy,async\_xor,raid456,async\_raid6\_recov
xor                    24576  2 async\_xor,btrfs
dm\_flakey              16384  0
dm\_era                 28672  0
dm\_delay               16384  0
dm\_crypt               49152  1
dm\_cache\_smq           28672  0
dm\_cache               69632  1 dm\_cache\_smq
dm\_persistent\_data     81920  3 dm\_era,dm\_thin\_pool,dm\_cache
libcrc32c              16384  5 nf\_conntrack,dm\_persistent\_data,btrfs,xfs,raid456
dm\_bufio               32768  4 dm\_verity,dm\_integrity,dm\_persistent\_data,dm\_snapshot
dm\_bio\_prison          20480  2 dm\_thin\_pool,dm\_cache
dm\_mod                147456  19 dm\_verity,dm\_integrity,dm\_raid,dm\_crypt,dm\_era,dm\_thin\_pool,dm\_zero,dm\_log,dm\_delay,dm\_flakey,dm\_log\_writes,dm\_log\_userspace,dm\_cache,dm\_snapshot,dm\_mirror,dm\_switch,dm\_bufio
firewire\_core          73728  0
crc\_itu\_t              16384  1 firewire\_core
hid\_sunplus            16384  0
hid\_sony               36864  0
hid\_samsung            16384  0
hid\_pl                 20480  0
hid\_petalynx           16384  0
hid\_monterey           16384  0
hid\_microsoft          16384  0
hid\_logitech\_dj        28672  0
hid\_logitech           20480  0
ff\_memless             20480  4 hid\_logitech,hid\_sony,hid\_microsoft,hid\_pl
hid\_gyration           16384  0
hid\_ezkey              16384  0
hid\_cypress            16384  0
hid\_chicony            16384  0
hid\_cherry             16384  0
hid\_belkin             16384  0
hid\_apple              16384  0
hid\_a4tech             16384  0
sl811\_hcd              32768  0
xhci\_pci               20480  0
xhci\_hcd              278528  1 xhci\_pci
usb\_storage            77824  0
mpt3sas               290816  0
raid\_class             16384  1 mpt3sas
aic94xx                94208  0
libsas                 94208  1 aic94xx
lpfc                  942080  0
qla2xxx               860160  0
megaraid\_sas          176128  0
megaraid\_mbox          45056  0
megaraid\_mm            20480  1 megaraid\_mbox
aacraid               131072  0
sx8                    20480  0
hpsa                  110592  0
3w\_9xxx                45056  0
3w\_xxxx                32768  0
3w\_sas                 32768  0
mptsas                 69632  0
scsi\_transport\_sas     45056  5 mptsas,aic94xx,hpsa,libsas,mpt3sas
mptfc                  24576  0
scsi\_transport\_fc      65536  3 lpfc,qla2xxx,mptfc
mptspi                 28672  0
mptscsih               45056  3 mptsas,mptspi,mptfc
mptbase                98304  4 mptsas,mptspi,mptfc,mptscsih
imm                    20480  0
parport                61440  1 imm
sym53c8xx              94208  0
initio                 28672  0
arcmsr                 53248  0
aic7xxx               143360  0
aic79xx               155648  0
scsi\_transport\_spi     40960  4 mptspi,aic79xx,aic7xxx,sym53c8xx
sr\_mod                 28672  0
cdrom                  73728  1 sr\_mod
sg                     40960  0
sd\_mod                 49152  5
pdc\_adma               16384  0
sata\_inic162x          16384  0
sata\_mv                40960  0
ata\_piix               36864  0
ahci                   40960  4
libahci                40960  1 ahci
sata\_qstor             16384  0
sata\_vsc               16384  0
sata\_uli               16384  0
sata\_sis               16384  0
sata\_sx4               20480  0
sata\_nv                32768  0
sata\_via               24576  0
sata\_svw               16384  0
sata\_sil24             24576  0
sata\_sil               16384  0
sata\_promise           20480  0
pata\_via               20480  0
pata\_jmicron           16384  0
pata\_marvell           16384  0
pata\_sis               20480  1 sata\_sis
pata\_netcell           16384  0
pata\_pdc202xx\_old      16384  0
pata\_atiixp            16384  0
pata\_amd               24576  0
pata\_ali               20480  0
pata\_it8213            16384  0
pata\_pcmcia            20480  0
pata\_serverworks       16384  0
pata\_oldpiix           16384  0
pata\_artop             16384  0
pata\_it821x            16384  0
pata\_hpt3x2n           16384  0
pata\_hpt3x3            16384  0
pata\_hpt37x            24576  0
pata\_hpt366            16384  0
pata\_cmd64x            16384  0
pata\_sil680            20480  0
pata\_pdc2027x          16384  0
nvme                   49152  0
nvme\_core             102400  1 nvme
virtio\_net             57344  0
net\_failover           20480  1 virtio\_net
failover               16384  1 net\_failover
virtio\_crypto          28672  0
crypto\_engine          16384  1 virtio\_crypto
virtio\_mmio            16384  0
virtio\_pci             28672  0
virtio\_balloon         20480  0
virtio\_rng             16384  0
virtio\_console         36864  0
virtio\_blk             20480  0
virtio\_scsi            24576  0
virtio\_ring            40960  9 virtio\_rng,virtio\_mmio,virtio\_console,virtio\_balloon,virtio\_scsi,virtio\_crypto,virtio\_pci,virtio\_blk,virtio\_net
virtio                 16384  9 virtio\_rng,virtio\_mmio,virtio\_console,virtio\_balloon,virtio\_scsi,virtio\_crypto,virtio\_pci,virtio\_blk,virtio\_net

`root #``lsusb -t````
/:  Bus 02.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/6p, 5000M
/:  Bus 01.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/12p, 480M
    |__ Port 1: Dev 2, If 0, Class=Human Interface Device, Driver=usbhid, 1.5M
    |__ Port 6: Dev 3, If 0, Class=Video, Driver=uvcvideo, 480M
    |__ Port 6: Dev 3, If 1, Class=Video, Driver=uvcvideo, 480M
    |__ Port 7: Dev 4, If 0, Class=Wireless, Driver=btusb, 12M
    |__ Port 7: Dev 4, If 1, Class=Wireless, Driver=btusb, 12M 
```
