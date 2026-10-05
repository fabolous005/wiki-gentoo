<!-- source: https://wiki.gentoo.org/wiki/Apple_MacBook_Pro_15-inch_(2016,_Intel,_Four_Thunderbolt_3_Ports) | group: Gentoo Wiki (Main) | wiki-title: Apple MacBook Pro 15-inch (2016, Intel, Four Thunderbolt 3 Ports) -->
---
title: Apple MacBook Pro 15-inch (2016, Intel, Four Thunderbolt 3 Ports)
url: https://wiki.gentoo.org/wiki/Apple_MacBook_Pro_15-inch_(2016,_Intel,_Four_Thunderbolt_3_Ports)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: b4416953ccb7138b
license: CC BY-SA 4.0
---

# Apple MacBook Pro 15-inch (2016, Intel, Four Thunderbolt 3 Ports)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide will cover the process on how to get Gentoo Linux working on Apple MacBook Pro 2016 (known as MacBookPro13,3) with T1 chip, 4 Thunderbolt ports and Touchbar.

Procedure in this guide may also apply to other variants of MacBooks using T1 chips, including 2015 and 2017 models as well.

## Introduction

To install Gentoo Linux on newer MacBook we will first need to choose boot manager and prepare disk partitions.

After installing Gentoo Linux for the first time, some of the functionality will be provided already by the kernel. Default approach would be to use distribution kernel and stable desktop profile.

Notice that using profiles such as musl and llvm may result in this process to fail, so using desktop profile either with openrc or systemd is recommended.

### What works out of the box:

- Basic keyboard (partially) - all keys should work, excluding touchbar
- Trackpad
- USB-C ports / ThunderBolt (buggy) - when no devices are plugged it is causing unnecessary power drain
- Bluetooth
- Camera
- Battery (buggy) - performance is under optimal, likely due to other hardware causing unnecessary power drain
- Graphics (Intel + AMD) - For the MacBookPro 13,3 with dedicated AMD GPU, only the AMD GPU is enabled when booting an OS which isn't macOS
- Screen
- NVMe (buggy) - currently unknown how power saving works for those SSDs it's likely they consume way more power than they need to, therefore reducing the battery life.
- System Management Controller (SMC)
- Wi-Fi (partially) - this is not fully functional, it will only find and connect to 2.4 GHz networks/access points

### What doesn't work out of the box:

- Sound
- Touchbar
- Wi-Fi 5 GHz networks

### What remains broken:

- Suspend (doesn't resume properly)
- Hibernate (works as shut down)
- TouchID (works only as power button, no fingerprint functionality)

### Hardware Table

#### Components

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes |  | 
|---|---|---|---|---|---|---|---|
| CPU | Intel Core i7-6820HQ@ 2.70GHz |  | GenuineIntel | N/A | 6.6.41-dist |  |  | 
| iGPU | Intel HD 530 Graphics |  | N/a | [i915](https://wiki.gentoo.org/wiki/Intel) | 6.6.41-dist | Does not work out of the box, possible to get working with patching, see [Intel GPU](<https://wiki.gentoo.org/wiki/Apple_MacBook_Pro_15-inch_(2016,_Intel,_Four_Thunderbolt_3_Ports)#Intel_GPU>) |  | 
| dGPU | AMD R455 Pro Graphics |  | PCI 0000:00 | [amdgpu](https://wiki.gentoo.org/wiki/AMD) | 6.6.41-dist | Works out of the box |  | 
| I2C/SMBus Controller | Intel SMBus Controller |  | PCI 0000:00:1f.4 | i2c\_i801 | 6.6.41-dist | Works out of the box |  | 
| SPI Controller | Apple SPI Controller |  | PCI APP000D:00 | applespi | 6.6.41-dist |  |  | 
| LPSS Controller | Intel Low Power Subsystem Controller |  | PCI 8086:A160 | intel-lpss | 6.6.41-dist | required for Bluetooth |  | 
| Serial Bus Controller | JHL6540 Thunderbolt 3 USB Controller (C step) \[Alpine Ridge 4C 2016\] |  | PCI 8086:15D4 | xhci\_hcd | 6.6.41-dist | Works out of the box |  | 
| NVMe | Samsung Electronics SM963 2.5" NVMe PCIe SSD |  | PCI 0000:02:00.0 | nvme | 6.6.41-dist | Works out of the box |  | 
| Audio | Intel Corporation HD Audio device |  | PCI 0000:00:1f.3 | snd\_soc\_avs, snd\_soc\_hda\_codec, snd\_hda\_codec\_hdmi, snd\_hda\_intel | 6.6.41-dist | Does not work out of the box, needs patching, see [Sound](<https://wiki.gentoo.org/wiki/Apple_MacBook_Pro_15-inch_(2016,_Intel,_Four_Thunderbolt_3_Ports)#Sound>) |  | 
| Audio | ATI Baffin HDMI/DP Audio \[Radeon RX 550 640SP / RX 560/560X\] |  | PCI 0000:01:00.1 | snd\_hda\_intel | 6.6.41-dist | Does not work out of the box, needs patching, see [Sound](<https://wiki.gentoo.org/wiki/Apple_MacBook_Pro_15-inch_(2016,_Intel,_Four_Thunderbolt_3_Ports)#Sound>) |  | 
| Wireless LAN | Broadcom BCM43602 802.11ac Wireless Network Adapter |  | PCI 14e4:43ba | brcmfmac | 6.6.41-dist | Finds only 2.4 GHz networks, needs additional firmware tweaking, see [Wi-Fi](<https://wiki.gentoo.org/wiki/Apple_MacBook_Pro_15-inch_(2016,_Intel,_Four_Thunderbolt_3_Ports)#Wi-Fi>) |  | 
| Bluetooth | Broadcom BCM43602 Bluetooth |  | PCI 14e4:4464 ? | hci\_uart, hci\_uart\_bcm | 6.6.41-dist |  |  | 
| Thunderbolt 3 | Intel JHL6540 Thunderbolt 3 Bridge (C step) \[Alpine Ridge 4C 2016\] |  | PCI 0000:7b:04.0, PCI 0000:7b:01.0, PCI 0000:04:00.0, PCI 0000:7c:00.0 | thunderbolt | 6.6.41-dist |  |  | 
| USB 3 | Intel Skylake USB Controller |  | PCI 0000:00:14.0 | xhci\_hcd | 6.6.41-dist |  |  | 
| Keyboard | Apple SPI Keyboard |  | pci-0000:00:1e.3-platform-pxa2xx-spi.5-cs-00-event-kbd | usbhid | 6.6.41-dist |  |  | 
| Touchpad | Apple SPI Touchpad |  | pci-0000:00:1e.3-platform-pxa2xx-spi.5-cs-00-event-mouse | bcm5974 | 6.6.41-dist |  |  | 
| Touchbar | Apple Touchbar |  | APP7777:00 | apple\_ib\_tb |  | Does not work out of box, needs patching, see [Touchbar](<https://wiki.gentoo.org/wiki/Apple_MacBook_Pro_15-inch_(2016,_Intel,_Four_Thunderbolt_3_Ports)#Touchbar>) |  | 
| Fingerprint sensor | Apple Touch ID sensor |  | ? |  |  |  |  | 
| Webcam | Apple FaceTime HD Camera |  | pci-0000:00:14.0-usb-0:3:1.0-video-index1 | uvcvideo | 6.6.41-dist | Should work out of the box, if not, see [Camera](<https://wiki.gentoo.org/wiki/Apple_MacBook_Pro_15-inch_(2016,_Intel,_Four_Thunderbolt_3_Ports)#Camera>) |  | 
| Hardware Monitoring | various |  |  | acpi\_battery, applesmc, coretemp, int340x, nvme, sbs | 6.6.41-dist | applesmc needs out of tree patches |  | 
| Ambient light sensor | Apple Backlight Keyboard Sensor |  | ACPI:APP0002:00:BACKLIGHT | kbd\_backlight |  |  |  | 

#### ACPI / Power management

| Function | Status | Kernel driver(s) | Kernel version | BIOS version | Notes | 
|---|---|---|---|---|---|
| CPU frequency scaling |  | intel\_rapl\_msr, intel\_rapl\_common, intel\_tcc\_cooling | 6.6.41-dist | 529.120.1.0.0 |  | 
| GPU Powersaving |  | i915,amdgpu |  |  | Possibly requires dGPU to be blacklisted, see [Intel GPU](<https://wiki.gentoo.org/wiki/Apple_MacBook_Pro_15-inch_(2016,_Intel,_Four_Thunderbolt_3_Ports)#Intel_GPU>) | 
| PCIe Power Management (ASPM) |  |  |  |  | Power performance is not optimized | 
| USB Type C Power Delivery |  |  | 6.6.41-dist | 529.120.1.0.0 | When no devices connected, draws unnecessary power | 
| Battery |  | sbs | 6.6.41-dist | 529.120.1.0.0 | Performance is suboptimal | 
| Suspend to RAM |  |  |  |  | Will instantly wake, needs blacklisted dGPU, see [Intel GPU](<https://wiki.gentoo.org/wiki/Apple_MacBook_Pro_15-inch_(2016,_Intel,_Four_Thunderbolt_3_Ports)#Intel_GPU>) and [Suspend](<https://wiki.gentoo.org/wiki/Apple_MacBook_Pro_15-inch_(2016,_Intel,_Four_Thunderbolt_3_Ports)#Suspend>) | 
| Suspend to disk (hibernate) |  |  |  |  | Will shut down instead of hibernate | 
| Display backlight control |  | gmux\_backlight | 6.6.41-dist | 529.120.1.0.0 |  | 
| Keyboard backlight control |  | kbd\_backlight | out of tree driver | 529.120.1.0.0 | Touchbar needs to be fixed for keyboard control to work, see [Touchbar](<https://wiki.gentoo.org/wiki/Apple_MacBook_Pro_15-inch_(2016,_Intel,_Four_Thunderbolt_3_Ports)#Touchbar>) | 

## Installation

### Choosing boot manager

There are two choices: rEFInd and OpenCore Legacy Patcher. Both of these need to be installed from macOS.

#### rEFInd

Refind which is a  fork of Refit and is being actively development and maintained. For more information see [this link](http://www.rodsbooks.com/refind/installing.html#installsh).

This option is recommended for 15 inch versions, as it allows for passing kernel parameters.

##### Getting rEFInd

rEFInd can be installed just by following official [REFInd](https://wiki.gentoo.org/wiki/REFInd) Gentoo Wiki guide:

`root #``emerge --ask sys-boot/refind`
##### Installing rEFInd

`root #````
refind-install
```
#### OpenCore Legacy Patcher

OpenCore Legacy Patcher is a graphical tool that should be run from macOS.

It is a boot manager that can detect other operating systems and is installed on EFI partition.

Also, it can be used to install newer macOS versions for unsupported Mac machines.

For the installation guide, please refer to [OpenCore Legacy Patcher documentation](https://dortania.github.io/OpenCore-Legacy-Patcher/INSTALLER.html)

### Preparing partitions

The easiest way to prepare partition is using the Disk Utility on macOS. After opening Disk Utility, just select disk (it should be always the first one) and press '+', then select 'Partition' instead of 'Volume'. Assign as much space as wanted.

### Preparing boot USB

The most convenient way is to grab Linux distribution of a choice. For the sake of guide, please use LiveGUI installation media to conform to the Gentoo Handbook. Format USB as GPT and FAT32, then just extract ISO contents to USB partition.

Aside from Gentoo's own LiveGUI USB media, any modern Linux distribution should be able to boot from USB on MacBook.

To get the latest LiveGUI USB media, please obtain it from [Gentoo Downloads](https://www.gentoo.org/downloads/) page.

To connect to Ethernet a Thunderbolt (e.g. [Apple Thunderbolt to Gigabit Ethernet Adapter](https://wiki.gentoo.org/wiki/Apple_Thunderbolt_to_Gigabit_Ethernet_Adapter)) or USB adapter is needed.

From kernel version 4.3 onward Thunderbolt hot-plugging should work on recent Apple hardware so it is no longer necessary to have the Thunderbolt Ethernet adapter plugged in during boot. [\[1\]](https://wiki.gentoo.org#cite_note-thunderbolt-hotplug-1)

Note that Wi-Fi will only show 2.4 GHz access points and might have limited functionality at this point.

Now proceed to [Handbook:AMD64/Installation/Stage](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage)

Please follow handbook carefully to obtain the best results.

### Hardware detection tools

In order to obtain information about system, the following tools can be used.

To view what hardware is currently detected first install pciutils package:

`root #````
emerge --ask sys-apps/pciutils
```
Then execute the following command:

`root #````
lspci -nnk
```
To view USB devices first install usbutils package:

`root #````
emerge --ask sys-apps/usbutils
```
Then execute the following command:

`root #````
lsusb
```
For CPU information also this command can be issued:

`root #````
lscpu
```
### Configuring flags

#### CPU Flags

This MacBook uses Skylake architecture.

To verify what architecture does CPU use, a tool resolve-march-native can be used:

`root #````
emerge resolve-march-native
```
Which will produce the following output:

`root #````
resolve-march-native
```
-march=skylake -mabm --param=l1-cache-line-size=64 --param=l1-cache-size=32 --param=l2-cache-size=8192

What needs to be added is *-march=skylake*

Please make sure to add appropriate flags to /etc/portage/make.conf

Edit /etc/portage/make.conf and add following section:

**`/etc/dracut.conf.d/applespi.conf`**

```
COMMON_FLAGS="-O2 -pipe -march=skylake"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
```
Also, do the following for cpuid2cpuflags:

`root #````
emerge --ask app-portage/cpuid2cpuflags
```
Create the file in /etc/portage/package.use:

`root #````
echo "*/* $(cpuid2cpuflags)" > /etc/portage/package.use/00cpu-flags
```
File /etc/portage/package.use/00cpu-flags should have this content:

**`/etc/portage/package.use/00cpu-flags`**

```
 CPU_FLAGS_X86: aes avx avx2 f16c fma3 mmx mmxext pclmul popcnt rdrand sse sse2 sse3 sse4_1 sse4_2 ssse3
```
#### GPU Flags

This MacBook uses both Intel® HD Graphics 530 and AMD Radeon Pro 455 (AMDGPU Polaris11 chip).

**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* intel amdgpu radeonsi radeon
```
### Kernel configuration

No special kernel configuration is needed. It is advised that distribution kernel package is used for the optimal results. Note that missing drivers will still need to be configured manually, as they are not present in kernel.

For distribution kernel, please refer to Handbook section [Distribution kernels](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel#Distribution_kernels).

If still manual kernel configuration is preferred ensure that following is set correctly:

**Kernel configuration options for spi\_pxa2xx\_platform and intel\_lpss\_pci**

## Configuration

### Installing tools

Before we install required modules, we will need two tools: dkms and git. To install dkms:

`root #````
emerge --ask sys-kernel/dkms
```
To install git:

`root #````
emerge --ask dev-vcs/git
```
Now we can proceed to building modules with dkms.

### Dracut configuration

To make modules load at the startup, we should create the file named applespi.conf in /etc/dracut.conf.d/ and define all modules:

**`/etc/dracut.conf.d/applespi.conf`**

```
add_drivers+=" applespi apple-ib-tb intel_lpss_pci spi_px2axx_platform "
```
### Building modules

To build modules we will need to clone the following repository that contains required files. Execute following commands as root:

`root #````
dkms install applespi/0.1
```
Save file and exit.

After reboot listed modules will be loaded.

### Touchbar

To make changes permanent, we should write an init script which will execute these two commands at startup.

If using OpenRC, create file /etc/init.d/macbook-quirks.conf:

**`/etc/init.d/macbook-quirks.conf`**

```
#!/sbin/openrc-run
depend() {
    after display-manager
}
start_pre() {
    sleep 2
}
start() {
    echo '1-3' > /sys/bus/usb/drivers/usb/unbind
    echo '1-3' > /sys/bus/usb/drivers/usb/bind
}
```
Then run as root the following:

`root #````
chmod +x /etc/init.d/macbook-quirks.conf
```
`root #````
rc-update add /etc/init.d/macbook-quirks.conf
```

If using SystemD, create file /etc/systemd/macbook-quirks.service:

**`/etc/systemd/system/macbook-quirks.service`**

```
[Unit]
Description=Re-enable MacBook 13,3 TouchBar
Before=display-manager.service
[Service]
Type=oneshot
ExecStartPre=/bin/sleep 2
ExecStart=/bin/sh -c "echo '1-3' > /sys/bus/usb/drivers/usb/unbind"
ExecStart=/bin/sh -c "echo '1-3' > /sys/bus/usb/drivers/usb/bind"
RemainAfterExit=yes
TimeoutSec=0
[Install]
WantedBy=multi-user.target
```
Then run as root the following

`root #````
systemctl enable macbook-quirks.service
```
After enabling either macbook-quirks.conf or macbook-quirks.service perform reboot.

### Wi-Fi

To make Wi-Fi operates as expected, following configuration needs to be applied: [brcmfmac43602-pcie.txt](https://bugzilla.kernel.org/show_bug.cgi?id=193121#c74)

We need to create new file called brcmfmac43602-pcie.txt and put it in /usr/lib/firmware/brcm/brcmfmac43602-pcie.txt

**`/usr/lib/firmware/brcm/brcmfmac43602-pcie.txt`**

```
sromrev=11
subvid=0x14e4
boardtype=0x61b
boardrev=0x1421
vendid=0x14e4
devid=0x43ba
macaddr=00:90:4c:0d:f4:3e
ccode=00
regrev=245
boardflags=0x10401001
boardflags2=0x00000002
boardflags3=0xC0000303
boardnum=62526
swctrlmap_2g=0x08080808,0x04010401,0x08080808,0x00000000,0x000000ff
swctrlmapext_2g=0x00000000,0x00000000,0x00000000,0x000000,0x003
swctrlmap_5g=0x08080808,0x04010401,0x08080808,0x00000000,0x000000ff
swctrlmapext_5g=0x00000000,0x00000000,0x00000000,0x000000,0x003
aa2g=7
aa5g=7
agbg0=133
agbg1=133
agbg2=133
aga0=71
aga1=71
aga2=71
txchain=7
rxchain=7
antswitch=0
tssiposslope2g=1
epagain2g=0
pdgain2g=28
tworangetssi2g=0
papdcap2g=0
femctrl=2
tssiposslope5g=1
epagain5g=0
pdgain5g=28
tworangetssi5g=0
papdcap5g=0
gainctrlsph=0
tempthresh=120
tempoffset=255
rawtempsense=0x1ff
measpower=0x7f
tempsense_slope=0xff
tempcorrx=0x3f
tempsense_option=0x3
xtalfreq=40000
phycal_tempdelta=40
temps_period=1
temps_hysteresis=5
measpower1=0x7f
measpower2=0x7f
pdoffsetcck=1057
pdoffset20in40m5gb0=0
pdoffset20in40m5gb1=0
pdoffset20in40m5gb2=0
pdoffset20in40m5gb3=0
pdoffset20in40m5gb4=0
pdoffset40in80m5gb0=0
pdoffset40in80m5gb1=0
pdoffset40in80m5gb2=0
pdoffset40in80m5gb3=0
pdoffset40in80m5gb4=0
pdoffset20in80m5gb0=0
pdoffset20in80m5gb1=0
pdoffset20in80m5gb2=0
pdoffset20in80m5gb3=0
pdoffset20in80m5gb4=0
subband5gver=0x5
cckbw202gpo=0
cckbw20ul2gpo=0
mcsbw202gpo=2536714240
mcsbw402gpo=2536714240
dot11agofdmhrbw202gpo=13056
ofdmlrbw202gpo=0
mcsbw205glpo=1966288896
mcsbw405glpo=2252619776
mcsbw805glpo=2252619776
mcsbw205gmpo=1966288896
mcsbw405gmpo=2252619776
mcsbw805gmpo=2252619776
mcsbw205ghpo=1966288896
mcsbw405ghpo=2252619776
mcsbw805ghpo=2252619776
mcsbw205gx1po=1966288896
mcsbw405gx1po=2252619776
mcsbw805gx1po=2252619776
mcsbw205gx2po=1966288896
mcsbw405gx2po=2252619776
mcsbw805gx2po=2252619776
mcslr5glpo=0
mcslr5gmpo=0
mcslr5ghpo=0
mcslr5gx1po=0
mcslr5gx2po=0
sb20in40hrpo=0
sb20in80and160hr5glpo=0
sb40and80hr5glpo=0
sb20in80and160hr5gmpo=0
sb40and80hr5gmpo=0
sb20in80and160hr5ghpo=0
sb40and80hr5ghpo=0
sb20in40lrpo=0
sb20in80and160lr5glpo=0
sb40and80lr5glpo=0
sb20in80and160lr5gmpo=0
sb40and80lr5gmpo=0
sb20in80and160lr5ghpo=0
sb40and80lr5ghpo=0
sb20in80and160hr5gx1po=0
sb20in80and160lr5gx1po=0
sb40and80hr5gx1po=0
sb20in80and160hr5gx2po=0
sb20in80and160lr5gx2po=0
sb40and80hr5gx2po=0
dot11agduphrpo=0
dot11agduplrpo=52416
pcieingress_war=15
sar2g=18
sar5g=15
noiselvl2ga0=31
noiselvl2ga1=31
noiselvl2ga2=31
noiselvl5ga0=18,18,18,20
noiselvl5ga1=1,1,1,17
noiselvl5ga2=11,11,11,11
rxgainerr2ga0=63
rxgainerr2ga1=31
rxgainerr2ga2=31
rxgainerr5ga0=50,50,50,52
rxgainerr5ga1=16,16,16,24
rxgainerr5ga2=5,5,5,5
rpcal2g=65535
rpcal5gb0=65535
rpcal5gb1=65535
rpcal5gb2=65535
rpcal5gb3=65535
rxgains5gmelnagaina0=3
rxgains5gmelnagaina1=3
rxgains5gmelnagaina2=3
rxgains5gmtrisoa0=9
rxgains5gmtrisoa1=9
rxgains5gmtrisoa2=9
rxgains5gmtrelnabypa0=0
rxgains5gmtrelnabypa1=0
rxgains5gmtrelnabypa2=0
rxgains5ghelnagaina0=2
rxgains5ghelnagaina1=2
rxgains5ghelnagaina2=2
rxgains5ghtrisoa0=8
rxgains5ghtrisoa1=8
rxgains5ghtrisoa2=8
rxgains5ghtrelnabypa0=0
rxgains5ghtrelnabypa1=0
rxgains5ghtrelnabypa2=0
gain_cal_temp=39
rssi_delta_2gb0=8,16,8,16,253,18,253,18,254,17,254,17
rssi_delta_2gb1=255,255,255,255,255,255,255,255,255,255,255,255
rssi_delta_2gb2=255,255,255,255,255,255,255,255,255,255,255,255
rssi_delta_2gb3=255,255,255,255,255,255,255,255,255,255,255,255
rssi_delta_2gb4=255,255,255,255,255,255,255,255,255,255,255,255
rssi_cal_freq_grp_2g=0,0,128,0,0,0,0
rud_agc_enable=0
temp_comp_tr_loss=1
rssi_qdB_en=0
rssi_delta_5gl=8,6,8,6,8,6,5,6,5,6,5,6,9,8,9,8,9,8
rssi_delta_5gml=250,237,250,237,250,237,248,236,248,236,248,236,249,237,249,237,249,237
rssi_delta_5gmu=250,237,250,237,250,237,248,234,248,234,248,234,249,237,249,237,249,237
rssi_delta_5gh=250,235,250,235,250,235,247,233,247,233,247,233,249,237,249,237,249,237
rssicorrnorm_c0=251,255
rssicorrnorm_c1=251,255
rssicorrnorm_c2=251,255
trloss_adj_temp_thresh=10
trloss_adj_time_dur=30
rstr_rxgaintempcoeff5gl=50,50,50
rstr_rxgaintempcoeff5gl_elnaoff=44,44,44
rstr_rxgaintempcoeff5gml=50,50,50
rstr_rxgaintempcoeff5gml_elnaoff=44,44,44
rstr_rxgaintempcoeff5gmu=50,50,50
rstr_rxgaintempcoeff5gmu_elnaoff=44,44,44
rstr_rxgaintempcoeff5gh=52,52,52
rstr_rxgaintempcoeff5gh_elnaoff=46,46,46
rstr_rxgaintempcoeff2g_sub=75,75,75,75,75,75,75,75,75,75,75,75,75,75,75
rstr_rxgaintempcoeff2g_sub_elnaoff=66,66,66,66,66,66,66,66,66,66,66,66,66,66,66
length_txcal=65535
version_txcal=65535
devicetype_txcal=65535
caltype_txcal=65535
tempsense_txcal=35
ncores_txcal=255
reserved1_txcal=255
reserved2_txcal=255
20mhz_tbl_len_txcal=65535
nsteps_txcal=22
nchannels_txcal=10
chan_id_txcal_ch0=7
chan_id_txcal_ch1=36
chan_id_txcal_ch2=52
chan_id_txcal_ch3=64
chan_id_txcal_ch4=100
chan_id_txcal_ch5=120
chan_id_txcal_ch6=140
chan_id_txcal_ch7=149
chan_id_txcal_ch8=157
chan_id_txcal_ch9=165
ptssi_txcal_ch0=0
ptssi_txcal_ch1=0
ptssi_txcal_ch2=0
ptssi_txcal_ch3=0
ptssi_txcal_ch4=0
ptssi_txcal_ch5=0
ptssi_txcal_ch6=0
ptssi_txcal_ch7=0
ptssi_txcal_ch8=0
ptssi_txcal_ch9=0
gain_index_c0_ch0=54
gain_index_c0_ch1=57
gain_index_c0_ch2=58
gain_index_c0_ch3=59
gain_index_c0_ch4=60
gain_index_c0_ch5=59
gain_index_c0_ch6=61
gain_index_c0_ch7=60
gain_index_c0_ch8=61
gain_index_c0_ch9=62
gain_index_c1_ch0=54
gain_index_c1_ch1=58
gain_index_c1_ch2=59
gain_index_c1_ch3=60
gain_index_c1_ch4=61
gain_index_c1_ch5=61
gain_index_c1_ch6=62
gain_index_c1_ch7=62
gain_index_c1_ch8=62
gain_index_c1_ch9=62
gain_index_c2_ch0=57
gain_index_c2_ch1=57
gain_index_c2_ch2=59
gain_index_c2_ch3=59
gain_index_c2_ch4=61
gain_index_c2_ch5=60
gain_index_c2_ch6=61
gain_index_c2_ch7=61
gain_index_c2_ch8=61
gain_index_c2_ch9=61
tssi_chan0_c0=248,244,241,237,232,226,220,211,203,193,183,172,162,150,138,128,116,105,94,83,73,61
tssi_chan0_c1=247,245,241,237,232,226,218,210,201,191,181,168,158,146,134,122,111,98,87,75,64,52
tssi_chan0_c2=248,245,242,238,233,227,221,213,204,194,185,173,162,151,140,129,118,107,97,87,77,67
tssi_chan1_c0=234,228,223,218,212,205,198,191,183,176,168,159,150,141,131,121,110,98,87,75,64,53
```
Note the following part:

macaddr=00:90:4c:0d:f5:30

ccode=ALL

We need to obtain MAC address of Broadcom Wireless card using command:

`user $````
ip addr
```
And then in the above file we need to replace *macaddr* to match MAC address from *ip addr* output, also we need to change *ccode* to *ALL* (which should enable both 2.4 GHz and 5 GHz access points; ALL (Channel 1-14), US, EU etc.)

Save file and reboot.

### Sound

After first boot into desktop, sound will show only *Dummy output*. 
To fix this we need to install the following sound driver.

First, we might need kernel sources as root:

`root #````
emerge -av sys-kernel/gentoo-sources
```
Then after installing kernel sources, run following commands as root:

`root #````
cd snd_hda_macbookpro/
```
`root #````
./install.cirrus.driver.sh
```
After this is done, reboot and sound should be working.

### Camera

Camera should be working out of the box, at least if camera application like Cheese is opened.

However, if it does not work for other applications that are using camera, try to create the file /etc/modprobe.d/uvcvideo.conf with following contents:

**`/etc/modprobe.d/uvcvideo.conf`**

```
options uvcvideo quirks=0x100
```
Save file and reboot. Now camera should work fine.

### Suspend

Putting the MacBook Pro into suspend mode works on all models, but successful resume requires additional prerequisites as explained below.

Models with Apple's NVMe controller (**MacBookPro13,1, MacBookPro13,2, MacBookPro14,1 and MacBookPro14,2**) require disabling the *d3cold* PCIe power state for the NVMe controller to successfully wake up again.
To work around suspend issues please try the following steps.

If using OpenRC, create file /etc/init.d/macbook-suspend with the following content:

**`/etc/init.d/macbook-suspend`**

```
#!/sbin/openrc-run
depend() {
     before display-manager
}
start() {
     echo 0 > /sys/bus/pci/devices/0000\:01\:00.0/d3cold_allowed
     echo 0 > /sys/bus/pci/devices/0000\:04\:00.0/d3cold_allowed
}
```
Then as root do the following:

`root #````
chmod +x /etc/init.d/macbook-suspend
```
`root #````
rc-update add macbook-suspend default
```
Reboot to apply changes.



If using SystemD, create file /etc/systemd/system/macbook-suspend.service with the following content:

**`/etc/systemd/system/macbook-suspend.service`**

```
[Unit]
Description=Fix suspend
Before=display-manager.service
[Service]
Type=oneshot
ExecStartPre=/bin/sleep 2
ExecStart=/bin/sh -c "echo 0 > /sys/bus/pci/devices/0000\:01\:00.0/d3cold_allowed"
ExecStart=/bin/sh -c "echo 0 > /sys/bus/pci/devices/0000\:04\:00.0/d3cold_allowed"
RemainAfterExit=yes
TimeoutSec=0
[Install]
WantedBy=multi-user.target
```
Then run as root do the following:

`root #````
systemctl enable macbook-suspend.service
```
Reboot to apply changes.

### Swapping fn and control keys

In order to swap fn and control keys, do the following.

Create file /etc/modprobe.d/swap-fn-control-keys with the following content:

**`/etc/modprobe.d/swap-fn-control-keys`**

```
 applespi fnremap=1
```
Save file and exit.

Reboot to apply changes.

### Intel GPU

By default, Intel GPU is disabled on 2016 15" model that does have discrete graphics.

This requires a few additional steps to get working.

Instructions : Make sure the EFI powers on the iGPU :

By default the Intel GPU gets switched off by the MacBook Pro EFI if anything but OS X is booted. So to use the Intel GPU, "apple\_set\_os" EFI hack needs to be applied either with:

rEFInd version 0.10.0 or above (recommended): [http://www.rodsbooks.com/refind](http://www.rodsbooks.com/refind)

Recent versions of rEFInd have the "apple\_set\_os" hack built-in. 
This can enabled it by setting the **spoof\_osx\_version** option in refind.conf.

There are several options available on how to do this:

- An apple\_set\_os.efi through unmodified EFI bootloader: [apple\_set\_os.efi](https://github.com/0xbb/apple_set_os.efi)

- A patched GRUB:[PATCH-Add apple\_set\_os command](https://lists.gnu.org/archive/html/grub-devel/2013-12/msg00442.html) or [Getting\_the\_integrated\_intel\_card\_to\_work\_on\_11.2C3](https://wiki.archlinux.org/index.php/MacBookPro11,x#Getting_the_integrated_intel_card_to_work_on_11.2C3)

- A patched Kernel: [Patched kernel](https://www.marc.info/?l=grub-deavel&m=141586614924917&w=2)



Otherwise not doing this step would result in powered-down integrated graphics card and a black screen.

#### Spoofing apple\_set\_os.efi with GRUB

Since Intel GPU won't load if we don't spoof MacOS into EFI (which makes our MacBook think that we are actually booting into MacOS) we would need to add an extra apple\_set\_os.efi file to EFI folder.

Since there are several ways mentioned above to achieve this, probably the easiest would be through unmodified EFI bootloader entry (first option).

Note that the following steps below do not involve GRUB or kernel patching.

Execute the following commands as root:

`root #````
cd /boot/EFI/EFI/custom
```
Now we need to edit GRUB:

**`/etc/grub.d/40_custom`**

```
# Add the following lines:
search --no-floppy --set=root --label EFI
chainloader (${root})/EFI/custom/apple_set_os.efi
boot
```
#### Creating Intel configuration file

Create the file /etc/X11/xorg.conf.d/20-intel.conf with the following content:

**`/etc/X11/xorg.conf.d/20-intel.conf`**

```
 "Device"
	Identifier "Intel Graphics"
	Driver "intel"
	BusID "PCI:0:2:0"
	Option "TearFree" "true"
	Option "AccelMethod" "glamor"
EndSection
```


#### Installing GPU switch and switching to Intel GPU

Execute the following commands as root:

`root #````
cd gpu-switch
```
`root #````
./gpu-switch -i # needs root. switches system to iGPU. needs reboot.
```


Now Intel card should be working with proper HiDPI detection! AMD driver can also be blacklisted in **/etc/modprobe.d/amdgpu.conf** by adding **blacklist amdgpu** to the end of the file.

**`/etc/modprobe.d/amdgpu.conf`**

```
blacklist amdgpu
```
Note that we can add this as GRUB\_CMDLINE\_LINUX parameters:

**`/etc/default/grub`**

```
GRUB_DEFAULT=0 // default menu entry
GRUB_TIMEOUT=10 // timeout in second after menu is displayed
GRUB_CMDLINE_LINUX="modprobe.blacklist=amdgpu acpi_backlight=intel_backlight"
```
After this regenerate grub.conf

`root #````
grub-mkconfig -o /boot/grub/grub.cfg
```
Carefully review these steps before rebooting.

If something goes wrong, revert these steps and fallback to AMDGPU with these commands:

`root #````
cd gpu-switch
```
`root #````
./gpu-switch -d # switches system back to dGPU. needs reboot.
```
## Troubleshooting

### Touchbar stopped working

Sometimes, touchbar may fail to load.

This can happen e.g. when performing reboot.

In this case, do a full power off.

Alternatively, try to boot into macOS, then power off the machine and power on again.

Last thing, try to rebuild dkms and reload the 'macbook-quirks' service.

It is unknown why this happens, but doing the steps above may fix this issue.

### Suspend fix does not work

Please read the 'note' part of this section.

For 2016 15" model [Intel GPU](<https://wiki.gentoo.org/wiki/Apple_MacBook_Pro_15-inch_(2016,_Intel,_Four_Thunderbolt_3_Ports)#Intel_GPU>) may need to be forced and discrete AMD GPU Graphics needs to be disabled.

Alternatively, suspend can be disabled completely by setting it either to 'Do nothing' or 'Power off'.

Additional note: it is possible that keyboard backlight will stay on. Keyboard backlight may be turned off completely before performing suspend operation.

### Hibernate does not work

Even after applying fix for [Suspend](<https://wiki.gentoo.org/wiki/Apple_MacBook_Pro_15-inch_(2016,_Intel,_Four_Thunderbolt_3_Ports)#Suspend>), hibernate will remain broken.

Executing **loginctl hibernate** will power off the system completely.

### Battery drains fast

Sadly, power management will remain the issue as some hardware parts draw power unnecessarily.

The best recommendation is to run Gentoo Linux while plugged in wall charger.

### Trackpad gestures not working as expected

Trackpad should work out of the box, but sensitivity may not be as smooth as on MacOS.

To increase smoothness, consider switching from **libinput** to **mtrack** driver by following this guide: [Making the best of Macbook Air touchpad on Ubuntu](https://int3ractive.com/blog/2018/make-the-best-of-macbook-touchpad-on-ubuntu/)

### Resume is slow

Even when suspend fix is applied and works as intended, it may take about minute to fully resume Macbook from sleep.

This is a known issue and nothing can be done to fix it currently.

## See also

## External resources

- The [Arch Linux wiki](https://wiki.archlinux.org/index.php/MacBookPro_Retina) is a very good reference.
- To read more about setting up an encrypted rootfs on lvm [Funtoo](http://www.funtoo.org/Rootfs_over_encrypted_lvm) has a great article.
- For more about macOS partitioning check this [MacOS Partitioning Guide](https://support.apple.com/guide/disk-utility/partition-a-physical-disk-dskutl14027/mac)
- Helpful resources that are from this guide can be found here: [State of Linux on the MacBook Pro 2016 & 2017](https://github.com/Dunedan/mbp-2016-linux)
- Repository containing fixes from this guide can be found here: [Macbook 12 SPI driver](https://github.com/almas/macbook12-spi-driver)
- Same guide, but with more details about partitioning part: [https://dev.to/cmiranda/linux-on-macbook-pro-2016-1onb](https://dev.to/cmiranda/linux-on-macbook-pro-2016-1onb)
