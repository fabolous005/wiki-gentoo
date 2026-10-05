<!-- source: https://wiki.gentoo.org/wiki/Hetzner_Cloud_(ARM64) | group: Gentoo Wiki (Main) | wiki-title: Hetzner Cloud (ARM64) -->
---
title: Hetzner Cloud (ARM64)
url: https://wiki.gentoo.org/wiki/Hetzner_Cloud_(ARM64)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-12"
fingerprint: cf11a8db7fa203c1
license: CC BY-SA 4.0
---

# Hetzner Cloud (ARM64)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page describes the installation process of Gentoo Linux on Hetzner Cloud with a shared virtual ARM processor.

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | ARM Neoverse-N1 (QEMU) |  | N/A | N/A | 6.6.13 |  | 
| GPU | Red Hat, Inc. Virtio 1.0 GPU |  | 1af4:1050 | virtio-pci | 6.6.13 | The kernel parameter `console=tty1` is required. | 
| SSD | Red Hat, Inc. Virtio 1.0 SCSI |  | 1af4:1048 | virtio-pci | 6.6.13 |  | 
| Ethernet | Red Hat, Inc. Virtio 1.0 network device |  | 1af4:1041 | virtio-pci | 6.6.13 | The kernel parameter `net.ifnames=0` is required. | 
| Keyboard | QEMU USB Keyboard |  | 0627:0001 | hid-generic usbhid | 6.6.13 |  | 

### Detailed information

`root #``lscpu````
Architecture:           aarch64
  CPU op-mode(s):       32-bit, 64-bit
  Byte Order:           Little Endian
CPU(s):                 2
  On-line CPU(s) list:  0,1
Vendor ID:              ARM
  BIOS Vendor ID:       QEMU
  Model name:           Neoverse-N1
    BIOS Model name:    NotSpecified  CPU @ 2.0GHz
    BIOS CPU family:    1
    Model:              1
    Thread(s) per core: 1
    Core(s) per socket: 2
    Socket(s):          1
    Stepping:           r3p1
    BogoMIPS:           50.00
    Flags:              fp asimd evtstrm aes pmull sha1 sha2 crc32 atomics fphp asimdhp cpuid asimdrdm lrcpc dcpop asimddp ssbs
NUMA:
  NUMA node(s):         1
  NUMA node0 CPU(s):    0,1
Vulnerabilities:
  Gather data sampling: Not affected
  Itlb multihit:        Not affected
  L1tf:                 Not affected
  Mds:                  Not affected
  Meltdown:             Not affected
  Mmio stale data:      Not affected
  Retbleed:             Not affected
  Spec rstack overflow: Not affected
  Spec store bypass:    Mitigation; Speculative Store Bypass disabled via prctl
  Spectre v1:           Mitigation; __user pointer sanitization
  Spectre v2:           Mitigation; CSV2, BHB
  Srbds:                Not affected
  Tsx async abort:      Not affected
```
`root #``lspci -nnk`
00:00.0 Host bridge \[0600\]: Red Hat, Inc. QEMU PCIe Host bridge \[1b36:0008\]
	Subsystem: Red Hat, Inc. QEMU PCIe Host bridge \[1af4:1100\]
00:01.0 Display controller \[0380\]: Red Hat, Inc. Virtio 1.0 GPU \[1af4:1050\] (rev 01)
	Subsystem: Red Hat, Inc. Virtio 1.0 GPU \[1af4:1100\]
	Kernel driver in use: virtio-pci
	Kernel modules: virtio\_pci
00:02.0 PCI bridge \[0604\]: Red Hat, Inc. QEMU PCIe Root port \[1b36:000c\]
	Subsystem: Red Hat, Inc. QEMU PCIe Root port \[1b36:0000\]
	Kernel driver in use: pcieport
00:02.1 PCI bridge \[0604\]: Red Hat, Inc. QEMU PCIe Root port \[1b36:000c\]
	Subsystem: Red Hat, Inc. QEMU PCIe Root port \[1b36:0000\]
	Kernel driver in use: pcieport
00:02.2 PCI bridge \[0604\]: Red Hat, Inc. QEMU PCIe Root port \[1b36:000c\]
	Subsystem: Red Hat, Inc. QEMU PCIe Root port \[1b36:0000\]
	Kernel driver in use: pcieport
00:02.3 PCI bridge \[0604\]: Red Hat, Inc. QEMU PCIe Root port \[1b36:000c\]
	Subsystem: Red Hat, Inc. QEMU PCIe Root port \[1b36:0000\]
	Kernel driver in use: pcieport
00:02.4 PCI bridge \[0604\]: Red Hat, Inc. QEMU PCIe Root port \[1b36:000c\]
	Subsystem: Red Hat, Inc. QEMU PCIe Root port \[1b36:0000\]
	Kernel driver in use: pcieport
00:02.5 PCI bridge \[0604\]: Red Hat, Inc. QEMU PCIe Root port \[1b36:000c\]
	Subsystem: Red Hat, Inc. QEMU PCIe Root port \[1b36:0000\]
	Kernel driver in use: pcieport
00:02.6 PCI bridge \[0604\]: Red Hat, Inc. QEMU PCIe Root port \[1b36:000c\]
	Subsystem: Red Hat, Inc. QEMU PCIe Root port \[1b36:0000\]
	Kernel driver in use: pcieport
00:02.7 PCI bridge \[0604\]: Red Hat, Inc. QEMU PCIe Root port \[1b36:000c\]
	Subsystem: Red Hat, Inc. QEMU PCIe Root port \[1b36:0000\]
	Kernel driver in use: pcieport
00:03.0 PCI bridge \[0604\]: Red Hat, Inc. QEMU PCIe Root port \[1b36:000c\]
	Subsystem: Red Hat, Inc. QEMU PCIe Root port \[1b36:0000\]
	Kernel driver in use: pcieport
00:04.0 Serial controller \[0700\]: Red Hat, Inc. QEMU PCI 16550A Adapter \[1b36:0002\] (rev 01)
	Subsystem: Red Hat, Inc. QEMU Virtual Machine \[1af4:1100\]
	Kernel driver in use: serial
01:00.0 Ethernet controller \[0200\]: Red Hat, Inc. Virtio 1.0 network device \[1af4:1041\] (rev 01)
	Subsystem: Red Hat, Inc. Virtio 1.0 network device \[1af4:1100\]
	Kernel driver in use: virtio-pci
	Kernel modules: virtio\_pci
02:00.0 USB controller \[0c03\]: Red Hat, Inc. QEMU XHCI Host Controller \[1b36:000d\] (rev 01)
	Subsystem: Red Hat, Inc. QEMU XHCI Host Controller \[1af4:1100\]
	Kernel driver in use: xhci\_hcd
03:00.0 Communication controller \[0780\]: Red Hat, Inc. Virtio 1.0 console \[1af4:1043\] (rev 01)
	Subsystem: Red Hat, Inc. Virtio 1.0 console \[1af4:1100\]
	Kernel driver in use: virtio-pci
	Kernel modules: virtio\_pci
04:00.0 Unclassified device \[00ff\]: Red Hat, Inc. Virtio 1.0 memory balloon \[1af4:1045\] (rev 01)
	Subsystem: Red Hat, Inc. Virtio 1.0 memory balloon \[1af4:1100\]
	Kernel driver in use: virtio-pci
	Kernel modules: virtio\_pci
05:00.0 Unclassified device \[00ff\]: Red Hat, Inc. Virtio 1.0 RNG \[1af4:1044\] (rev 01)
	Subsystem: Red Hat, Inc. Virtio 1.0 RNG \[1af4:1100\]
	Kernel driver in use: virtio-pci
	Kernel modules: virtio\_pci
06:00.0 SCSI storage controller \[0100\]: Red Hat, Inc. Virtio 1.0 SCSI \[1af4:1048\] (rev 01)
	Subsystem: Red Hat, Inc. Virtio 1.0 SCSI \[1af4:1100\]
	Kernel driver in use: virtio-pci
	Kernel modules: virtio\_pci

`root #``lsusb -vt````
/:  Bus 02.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/4p, 5000M
    ID 1d6b:0003 Linux Foundation 3.0 root hub
/:  Bus 01.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/4p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
    |__ Port 1: Dev 2, If 0, Class=Human Interface Device, Driver=usbhid, 480M
        ID 0627:0001 Adomax Technology Co., Ltd QEMU Tablet
    |__ Port 2: Dev 3, If 0, Class=Human Interface Device, Driver=usbhid, 480M
        ID 0627:0001 Adomax Technology Co., Ltd QEMU Tablet
```
`root #``lsmod`
Module                  Size  Used by
ipmi\_ssif              24576  0
ipmi\_devintf           20480  0
ipmi\_msghandler        49152  2 ipmi\_devintf,ipmi\_ssif
sd\_mod                 45056  0
t10\_pi                 16384  1 sd\_mod
crc64\_rocksoft\_generic    16384  1
sr\_mod                 24576  0
cdrom                  32768  1 sr\_mod
crc64\_rocksoft         16384  1 t10\_pi
crc64                  20480  2 crc64\_rocksoft,crc64\_rocksoft\_generic
sg                     32768  0
sha2\_ce                16384  0
sha256\_arm64           24576  1 sha2\_ce
virtio\_scsi            20480  0
virtio\_balloon         20480  0
virtio\_rng             16384  0
virtio\_console         28672  0
button                 16384  0
evdev                  20480  2
binfmt\_misc            20480  1
jc42                   16384  0
regmap\_i2c             16384  1 jc42
fuse                  106496  1
dm\_mod                106496  0
configfs               36864  1
efivarfs               20480  1
qemu\_fw\_cfg            16384  0
ip\_tables              24576  0
x\_tables               28672  1 ip\_tables
autofs4                28672  2
virtio\_net             45056  0
net\_failover           20480  1 virtio\_net
failover               16384  1 net\_failover
virtio\_pci             24576  0
virtio\_pci\_legacy\_dev    16384  1 virtio\_pci
virtio\_pci\_modern\_dev    16384  1 virtio\_pci
virtio\_mmio            16384  0

`root #``dmidecode`
\# dmidecode 3.4
Getting SMBIOS data from sysfs.
SMBIOS 3.0.0 present.
Table at 0x135EC0000.
Handle 0x0000, DMI type 0, 24 bytes
BIOS Information
	Vendor: Hetzner
	Version: 20171111
	Release Date: 11/11/2017
	Address: 0xE8000
	Runtime Size: 96 kB
	ROM Size: 64 kB
	Characteristics:
		BIOS characteristics not supported
		Targeted content distribution is supported
		UEFI is supported
		System is a virtual machine
	BIOS Revision: 1.0
Handle 0x0100, DMI type 1, 27 bytes
System Information
	Manufacturer: Hetzner
	Product Name: vServer
	Version: 20171111
	Serial Number: 43607703
	UUID: 5316b371-b196-4a2e-9bcd-3488e8f3e8a7
	Wake-up Type: Power Switch
	SKU Number: TM
	Family: Hetzner\_vServer
Handle 0x0200, DMI type 2, 15 bytes
Base Board Information
	Manufacturer: KVM
	Product Name: KVM Virtual Machine
	Version: virt-6.2
	Serial Number: Not Specified
	Asset Tag: Not Specified
	Features:
		Board is a hosting board
	Location In Chassis: Not Specified
	Chassis Handle: 0x0300
	Type: Motherboard
	Contained Object Handles: 0
Handle 0x0300, DMI type 3, 22 bytes
Chassis Information
	Manufacturer: QEMU
	Type: Other
	Lock: Not Present
	Version: NotSpecified
	Serial Number: Not Specified
	Asset Tag: Not Specified
	Boot-up State: Safe
	Power Supply State: Safe
	Thermal State: Safe
	Security Status: Unknown
	OEM Information: 0x00000000
	Height: Unspecified
	Number Of Power Cords: Unspecified
	Contained Elements: 0
	SKU Number: Not Specified
Handle 0x0400, DMI type 4, 42 bytes
Processor Information
	Socket Designation: CPU 0
	Type: Central Processor
	Family: Other
	Manufacturer: QEMU
	ID: 00 00 00 00 00 00 00 00
	Version: NotSpecified
	Voltage: Unknown
	External Clock: Unknown
	Max Speed: 2000 MHz
	Current Speed: 2000 MHz
	Status: Populated, Enabled
	Upgrade: Other
	L1 Cache Handle: Not Provided
	L2 Cache Handle: Not Provided
	L3 Cache Handle: Not Provided
	Serial Number: Not Specified
	Asset Tag: Not Specified
	Part Number: Not Specified
	Core Count: 2
	Core Enabled: 2
	Thread Count: 1
	Characteristics: None
Handle 0x1000, DMI type 16, 23 bytes
Physical Memory Array
	Location: Other
	Use: System Memory
	Error Correction Type: Multi-bit ECC
	Maximum Capacity: 4000 MB
	Error Information Handle: Not Provided
	Number Of Devices: 1
Handle 0x1100, DMI type 17, 40 bytes
Memory Device
	Array Handle: 0x1000
	Error Information Handle: Not Provided
	Total Width: Unknown
	Data Width: Unknown
	Size: 4000 MB
	Form Factor: DIMM
	Set: None
	Locator: DIMM 0
	Bank Locator: Not Specified
	Type: RAM
	Type Detail: Other
	Speed: Unknown
	Manufacturer: QEMU
	Serial Number: Not Specified
	Asset Tag: Not Specified
	Part Number: Not Specified
	Rank: Unknown
	Configured Memory Speed: Unknown
	Minimum Voltage: Unknown
	Maximum Voltage: Unknown
	Configured Voltage: Unknown
Handle 0x2000, DMI type 32, 11 bytes
System Boot Information
	Status: No errors detected
Handle 0xFEFF, DMI type 127, 4 bytes
End Of Table

## Installation

Hetzner solutions do not provide the option to boot from a Gentoo installation disk (although it is possible to contact them to add a custom ISO to the menu <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>), but Gentoo can be installed from the [Hetzner Rescue System](https://docs.hetzner.com/robot/dedicated-server/troubleshooting/hetzner-rescue-system/), which is based on Debian, so it doesn't matter which distribution is chosen when creating the server. Before creating the server, it would be wise to [configure the firewall](https://wiki.gentoo.org#Hetzner_Cloud_Firewall). Once the firewall is configured, [create an SSH key](https://community.hetzner.com/tutorials/howto-ssh-key) (or [create a GPG key](https://wiki.gentoo.org#Usage_of_GPG_keys_instead_of_SSH_keys)). The created key and firewall should be specified during the [server creation process](https://docs.hetzner.com/cloud/servers/getting-started/creating-a-server/). After creating the server, go to the server menu. Click on the *Rescue* tab and click on the button labeled *Enable rescue & power cycle*. Select the previously created SSH key from the list and click on the button labeled *Enable rescue & power cycle*. The server will reboot into the Rescue System and it will be possible to [connect to it via SSH](https://docs.hetzner.com/cloud/servers/getting-started/connecting-via-private-ip/). The installation process is straightforward, [Handbook:AMD64](https://wiki.gentoo.org/wiki/Handbook:AMD64) is usable even for ARM virtual machines. The system should be installed on /dev/sda which contains another operating system, so the disk needs to be wiped.

### Hetzner Cloud Firewall

Hetzner provides a way to configure the [Hetzner Could Firewall](https://docs.hetzner.com/cloud/firewalls/faq/) before server creation. The firewall is free of charge and allows to create a whitelist for incoming traffic, so only allowed IP addresses will be able to connect to the server. This is useful because the server will be protected from attacks until it is ready for public release (or to keep the server completely private). The [official guide](https://docs.hetzner.com/cloud/firewalls/getting-started/creating-a-firewall/) can be used to configure the firewall.

### Server IP address

The *Networking* tab shows the IPv6 address as `7777:777:7777:7777::/64`, which is a bit confusing since the IP address to connect to is `7777:777:7777:7777::1` (click on the button with the three dots to the right of the IP address and click *Show Instructions* to see it). Hetzner assigns the first address (`::1`) by default <sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>.

### Usage of GPG keys instead of SSH keys

It is possible to use [GnuPG](https://wiki.gentoo.org/wiki/GnuPG) to create and store authentication keys.

#### Client-side actions

##### GPG key generation

Generate a master key as described [here](https://wiki.gentoo.org/wiki/GnuPG#Primary_key) and an authentication key as described [here](https://wiki.gentoo.org/wiki/GnuPG#Add_an_authentication_key). The articles describe *Ed25519*, but *RSA-4096* is also acceptable. However, moving past *RSA-2048* leads to the inability to use some smartcards and other devices. [\[3\]](https://wiki.gentoo.org#cite_note-3)

To export the public SSH key, execute the following command:

`user $``gpg --export-ssh-key KEY_ID`
The key can be treated as a regular SSH key and can be used in Hetzner web forms.

##### Configuration of gpg-agent

It is necessary to tell gpg-agent which key to use for SSH. To do so, it is necessary to know the *keygrip* of the authentication key:

`user $``gpg --list-keys --with-keygrip`
Once the *keygrip* is known, gpg-agent can be informed (replace *7777777777777777777777777777777777777777* with the *keygrip*):

`user $``gpg-connect-agent 'KEYATTR 7777777777777777777777777777777777777777 Use-for-ssh: true' /bye`
gpg-agent will add the corresponding line to \~/.gnupg/private-keys-v1.d/\<keygrip>.key, so the above actions need to be performed only once.

Next, it is necessary to tell SSH to use gpg-agent and run it if it is not already running:

**`~/.bashrc`**

SSH does not inform gpg-aget which /dev/pts/\<N> to use <sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>, so it should be done as below:

**`~/.ssh/config`**

The configuration will take effect after a reboot or after gpg-agent is safely <sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup> terminated:

`user $``gpgconf --kill gpg-agent`
### UEFI

The cloud uses UEFI with the following entries:

`root #``efibootmgr`
BootCurrent: 0004
BootOrder: 0004,0005,0006,0007,0003,0001,0000,0002,0008
Boot0000\* UiApp
Boot0001\* UEFI QEMU QEMU CD-ROM
Boot0002\* UEFI Misc Device
Boot0003\* UEFI QEMU QEMU HARDDISK
Boot0004\* UEFI PXEv4 (MAC:96000308A34D)
Boot0005\* UEFI PXEv6 (MAC:96000308A34D)
Boot0006\* UEFI HTTPv4 (MAC:96000308A34D)
Boot0007\* UEFI HTTPv6 (MAC:96000308A34D)
Boot0008\* EFI Internal Shell

If the entries are deleted, they will be recreated after a reboot. The cloud supports the creation of new entries (tested with [EFI stub](https://wiki.gentoo.org/wiki/EFI_stub)).

### Kernel

**Kernel parameters (the root should be modified or deleted depending on the boot method)**

**PCI bus**

**Virtio**

**GPU**

**SSD**

**Ethernet**

**Keyboard**

**Random Number Generator**

**Shutdown button**

### Scripted Kernel Config

Since Hetzner Cloud is run on KVM virtual machines, we can take advantage of some default configurations included in the kernel source tree:

make defconfig
make kvm\_guest.config
for i in \
	DRM\_NOUVEAU \
	DRM\_EXYNOS \
	DRM\_ROCKCHIP \
	DRM\_RCAR\_DU \
	DRM\_RCAR\_DW\_HDMI \
	DRM\_RCAR\_USE\_LVDS \
	DRM\_RCAR\_USE\_MIPI\_DSI \
	DRM\_IMX\_DCSS \
	DRM\_ETNAVIV \
	DRM\_HISI\_HIBMC \
	DRM\_HISI\_KIRIN \
	DRM\_MEDIATEK \
	DRM\_MSM \
	DRM\_MXSFB \
	DRM\_MESON \
	DRM\_PL111 \
	DRM\_TIDSS \
	DRM\_LEGACY \
	DRM\_SUN4I \
	DRM\_TEGRA \
	TEGRA\_HOST1X \
	SCSI\_UFSHCD \
	FPGA \
	RC\_CORE \
	NEW\_LEDS \
	CHROME\_PLATFORMS \
	SURFACE\_PLATFORMS \
	XEN\_BLKDEV\_FRONTEND \
	LOGO \
	SOUND \
	SLIMBUS \
	SOUNDWIRE \
	MEDIA\_SUPPORT \
	MMC \
	BTRFS\_FS \
	OVERLAY\_FS \
	NFS\_FS \
	9P\_FS \
	SUSPEND \
	HIBERNATION \
	BLK\_DEV\_INITRD \
	VIRTUALIZATION \
	WLAN \
	PINCTRL \
	GPIOLIB \
	PWM \
	IPMI\_HANDLER \
	CAN \
	BT \
	WIRELESS \
	MD \
	RFKILL \
	NET\_9P \
	NFC \
	SPI \
	SPMI \
	HWMON \
	THERMAL \
	IIO \
	USB\_NET\_DRIVERS \
	XEN\_NETDEV\_FRONTEND \
	ETHERNET \
	QCOM\_IPA \
	REGULATOR \
	STAGING \
	SQUASHFS \
	DEBUG\_KERNEL \
	XEN \
	MODULES; do ./scripts/config --disable $i; done
./scripts/config --set-str CMDLINE "init=/usr/lib/systemd/systemd root=/dev/sda3 rootwait rootfstype=ext4"
make -j\<n> Image
mkdir -p /boot/EFI/BOOT
cp -a /usr/src/linux/arch/arm64/boot/Image /efi/EFI/BOOT/BOOTAA64.EFI

Adapting init and root

## Configuration

### SSH

#### SSH key

Before leaving the Rescue System, the SSH key should be copied to the installed system:

`root #``mkdir /mnt/gentoo/root/.ssh``root #``chmod 700 /mnt/gentoo/root/.ssh``root #``cp /root/.ssh/authorized_keys /mnt/gentoo/root/.ssh``root #``chmod 600 /mnt/gentoo/root/.ssh/authorized_keys`
#### Removal of unnecessary SSH host keys

Assuming that only *Ed25519* is used, other host keys can be removed:

`root #``rm -rf /etc/ssh/ssh_host_ecdsa_key*``root #``rm -rf /etc/ssh/ssh_host_rsa_key*`
##### Disabling host key regeneration (OpenRC)

To prevent key regeneration, comment out or delete the following line in /etc/init.d/sshd:

```
${SSHD_KEYGEN_BINARY} -A || return 2
```
Restart the SSH daemon:

`root #``rc-service sshd restart`
Check the result from the client machine:

`user $``ssh-keyscan <SERVER IP>`
There should only be one host key in the result.

### Network

Install [Netifrc](https://wiki.gentoo.org/wiki/Netifrc):

`root #``emerge --ask net-misc/netifrc`
Create the [interface symlink](https://wiki.gentoo.org/wiki/Netifrc#Creating_symlinks):

`root #``ln -s /etc/init.d/net.lo /etc/init.d/net.eth0`
Enable the interface [at boot](https://wiki.gentoo.org/wiki/Netifrc#Enable_at_boot):

`root #``rc-update add net.eth0 default`
#### IPv4 only

Replace `XXX.XXX.XXX.XXX` with the real IP. The gateway is provided by Hetzner. <sup>[\[6\]](https://wiki.gentoo.org#cite_note-6)</sup> The DNS servers are official servers provided by Hetzner. [\[7\]](https://wiki.gentoo.org#cite_note-7)

**`/etc/conf.d/net`**

```
config_eth0="XXX.XXX.XXX.XXX/32"
routes_eth0="172.31.1.1
 default via 172.31.1.1"
dns_servers_eth0="185.12.64.1 185.12.64.2"
```
#### IPv6 only

Replace `XXX:XXX:XXX:XXX` with the real IP (found [above](https://wiki.gentoo.org#Server_IP_address)). The gateway is provided by Hetzner. <sup>[\[9\]](https://wiki.gentoo.org#cite_note-9)</sup> The DNS servers are official servers provided by Hetzner. [\[10\]](https://wiki.gentoo.org#cite_note-10)

**`/etc/conf.d/net`**

```
config_eth0="XXXX:XXXX:XXXX:XXXX::1/64"
routes_eth0="default via fe80::1"
dns_servers_eth0="2a01:4ff:ff00::add:1 2a01:4ff:ff00::add:2"
```
#### IPv4 with IPv6

The following configuration is a combination of the above configurations.

**`/etc/conf.d/net`**

```
config_eth0="XXX.XXX.XXX.XXX/32 XXXX:XXXX:XXXX:XXXX::1/64"
routes_eth0="172.31.1.1
 default via 172.31.1.1
 default via fe80::1"
dns_servers_eth0="185.12.64.1 185.12.64.2 2a01:4ff:ff00::add:1 2a01:4ff:ff00::add:2"
```
## Troubleshooting

### f0 respawning

The following message constantly appears in the VNC console:

INIT: Id "f0" respawning too fast: disabled for 5 minutes

To get rid of it, follow [these steps](https://wiki.gentoo.org/wiki/MNT_Reform#f0_respawning).

### jitterentropy initialization failure (unsolved issue)

Sometimes [jitterentropy](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/crypto/jitterentropy.c) initialization fails on boot, but it doesn't cause the kernel to panic, just a failure message in the log. Since the error doesn't always appear, it's most likely a kernel bug. Other ARM machines seem to be affected too.
[\[11\]](https://wiki.gentoo.org#cite_note-11)[\[12\]](https://wiki.gentoo.org#cite_note-12)

`root #``dmesg`
\[    0.172340\] jitterentropy: Initialization failed with host not compliant with requirements: 9

### Losing IPv6 after 20 minutes

If the system keeps losing IPv6 connection after 20 minutes adding following file to the /etc/syslog.d/00\_IPv6.conf directory might solve the issue.

**`/etc/sysctl.d/00_IPv6.conf`**

**Enabling IPv6 privacy extensions**

```
net.ipv6.conf.all.forwarding=1
 #net.ipv6.conf.all.accept_ra=2  << Might be over bearing
 net.ipv6.conf.default.forwarding=1
 net.ipv6.conf.enp1s0.accept_ra=2
```
