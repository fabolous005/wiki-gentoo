<!-- source: https://wiki.gentoo.org/wiki/PINE64_ROCKPro64/Installing_U-Boot | group: Gentoo Wiki (Main) | wiki-title: PINE64 ROCKPro64/Installing U-Boot -->
---
title: PINE64 ROCKPro64/Installing U-Boot
url: https://wiki.gentoo.org/wiki/PINE64_ROCKPro64/Installing_U-Boot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-03-09"
fingerprint: bfcf7810ddb139c0
license: CC BY-SA 4.0
---

# PINE64 ROCKPro64/Installing U-Boot

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

There are a number of places you can find pre-built images to boot your ROCKPro64. This guide describes how to set up Das U-Boot bootloader on ROCKPro64 from mainline upstream source code.

## Prerequisites

U-Boot can be built on an ARM64 device or cross-compiled on a PC. Regardless, we will need:

- git, to check out the sources
- dtc, to compile device trees
- swig, to build U-Boot
- crossdev, since both a 64- and 32-bit ARM toolchains are required

Let's make sure these are installed:

`root #``emerge --ask --update dev-vcs/git sys-apps/dtc dev-lang/swig sys-devel/crossdev`
Please review the [crossdev](https://wiki.gentoo.org/wiki/Crossdev) article if you don't already have the it set up.

Install the 32-bit toolchain. This is required to build code for Cortex-M0 MCUs in BL31:

`root #``crossdev --target arm-none-eabi`
If cross-compiling, install the 64-bit cross-compiler toolchain:

`root #``crossdev --target aarch64-unknown-linux-gnu`
## Compiling

### Build BL31

Before U-Boot, we need to build the BL31 ("Boot Loader stage 3-1") firmware.

Get the sources:

`user $````
git clone https://github.com/ARM-software/arm-trusted-firmware.git
```
`user $````
cd arm-trusted-firmware
```
`user $````
git tag
```
v2.7.0

Let's select the latest currently stable version:

`user $``git checkout v2.7.0`
The repository contains a few binaries. If this is unacceptable, they may be deleted:

`user $````
git rm '*.bin'
```
rm 'plat/arm/board/common/protpk/arm\_protpk\_rsa\_sha256.bin'
rm 'plat/arm/board/common/rotpk/arm\_rotpk\_ecdsa\_sha256.bin'
rm 'plat/arm/board/common/rotpk/arm\_rotpk\_rsa\_sha256.bin'
rm 'plat/rockchip/rk3368/drivers/ddr/rk3368\_ddr\_reg\_resume\_V1.05.bin'
rm 'plat/rockchip/rk3399/drivers/dp/hdcp.bin'

To compile on ARM64, run:

`user $``make -j$(nproc) PLAT=rk3399 bl31`
If cross-compiling, run:

`user $``make -j$(nproc) PLAT=rk3399 CROSS_COMPILE=aarch64-unknown-linux-gnu- bl31`
Once finished, make a copy of resulting `bl31.elf` binary for safekeeping:

`user $````
mkdir ~/rk3399-atf-2.7.0
```
`user $````
cp build/rk3399/release/bl31/bl31.elf ~/rk3399-atf-2.7.0
```
### Build U-Boot

Now we're ready to build U-Boot itself. Let's get the sources:

`user $````
git clone https://source.denx.de/u-boot/u-boot.git
```
`user $````
cd u-boot
```
`user $````
git tag
```
v2022.07

Check out latest currently stable version:

`user $``git checkout v2022.07`
Load ROCKPro64-specific default configuration:

`user $``make rockpro64-rk3399_defconfig`
Similar to the Linux kernel, `menuconfig` can be used to make changes to the configuration. You don't need to make any changes.

`user $``make menuconfig`
To compile on ARM64, run:

`user $``make -j$(nproc) BL31=~/rk3399-atf-2.7.0/bl31.elf`
If cross-compiling, run:

`user $``make -j$(nproc) BL31=~/rk3399-atf-2.7.0/bl31.elf CROSS_COMPILE=aarch64-unknown-linux-gnu-`
## Installing

When powered on, ROCKPro64 will attempt to find a bootloader in the following order:

- Onboard 128MB SPI Flash
- Removable eMMC
- microSD card

Once U-Boot is loaded from either of these locations, it could then be used to load the kernel from either of these, from peripherals connected through SATA, USB or NVMe, or even through the network.

### Installing on microSD/eMMC

As root, enter the u-boot build directory, and write the images to the card.

`root #````
cd /path/to/u-boot
```
`root #````
dd if=idbloader.img of=/dev/mmcblk1 seek=64
```
`root #````
dd if=u-boot.itb of=/dev/mmcblk1 seek=16384
```
#### Observing boot message with serial console

Before installing a Linux operating system, you may want to ensure that U-Boot is booting correctly. If you have a USB-to-TTL connector (such as [this one](https://www.olimex.com/Products/Components/Cables/USB-Serial-Cable/USB-SERIAL-F/) or [this one](https://pine64.com/product/serial-console-woodpecker-edition/)), connect it to pins 6 (GND), 8 (UART2\_TX) and 10 (UART2\_RX) on the PI2 Bus (consult [the Pine64 RockPro Wiki](https://wiki.pine64.org/wiki/ROCKPro64#GPIO_Pins) for reference) to see the early boot up messages:

`root #``screen /dev/ttyUSB0 1500000`
#### Optional: Adapting baud rate

Serial communication usually requires both sides of the communication to agree on a common [baud rate](https://en.wikipedia.org/wiki/Symbol_rate). By default, Rockchip uses 1500000 bauds, which may be too fast for common USB-to-serial adapters. As a consequence, you may not see any boot-messages from u-boot. A much more common and widely supported, but slower baud rate is 115200 bauds. U-Boot can be configured to use this slower baud rate.

Use `menuconfig` as described in [#Build\_U-Boot](https://wiki.gentoo.org#Build_U-Boot) and reconfigure the symbol `BAUDRATE` to `115200`. Rebuild u-boot and copy it to the SD-card. Now start your program of choice for serial communication

`root #``screen /dev/ttyUSB0 115200`
The [Pine64 RockPro64 Wiki](<https://wiki.pine64.org/wiki/ROCKPro64#Setup_a_Serial_Console_(UART2)>) gives additional information on serial communication.

#### Expected boot messages

Now put the card into the device and power on. If successful, you should be able to see output like this (click to expand):

```
U-Boot TPL 2022.07 (Jul 11 2022 - 20:15:18)
Channel 0: LPDDR4, 50MHz
BW=32 Col=10 Bk=8 CS0 Row=16/15 CS=1 Die BW=16 Size=2048MB
Channel 1: LPDDR4, 50MHz
BW=32 Col=10 Bk=8 CS0 Row=16/15 CS=1 Die BW=16 Size=2048MB
256B stride
lpddr4_set_rate: change freq to 400000000 mhz 0, 1
lpddr4_set_rate: change freq to 800000000 mhz 1, 0
Trying to boot from BOOTROM
Returning to boot ROM...
U-Boot SPL 2022.07 (Jul 11 2022 - 20:15:18 -0400)
Trying to boot from MMC2
U-Boot 2022.07 (Jul 11 2022 - 20:15:18 -0400)
SoC: Rockchip rk3399
Reset cause: POR
Model: Pine64 RockPro64 v2.1
DRAM:  3.9 GiB
PMIC:  RK808
Core:  305 devices, 33 uclasses, devicetree: separate
MMC:   mmc@fe310000: 3, mmc@fe320000: 1, mmc@fe330000: 0
Loading Environment from SPIFlash... SF: Detected gd25q128 with page size 256 Bytes, erase size 4 KiB, total 16 MiB
*** Warning - bad CRC, using default environment
In:    serial
Out:   serial
Err:   serial
Model: Pine64 RockPro64 v2.1
Net:   eth0: ethernet@fe300000
starting USB...
Bus usb@fe380000: ehci_generic usb@fe380000: Failed to get clocks (ret=-19)
Port not available.
Bus usb@fe3a0000: USB OHCI 1.0
Bus usb@fe3c0000: ehci_generic usb@fe3c0000: Failed to get clocks (ret=-19)
Port not available.
Bus usb@fe3e0000: USB OHCI 1.0
Bus usb@fe800000: Register 2000140 NbrPorts 2
Starting the controller
USB XHCI 1.10
Bus usb@fe900000: Register 2000140 NbrPorts 2
Starting the controller
USB XHCI 1.10
scanning bus usb@fe3a0000 for devices... 1 USB Device(s) found
scanning bus usb@fe3e0000 for devices... 1 USB Device(s) found
scanning bus usb@fe800000 for devices... 1 USB Device(s) found
scanning bus usb@fe900000 for devices... 1 USB Device(s) found
       scanning usb for storage devices... 0 Storage Device(s) found
Hit any key to stop autoboot:  0
```
### Installing on SPI flash

The other option is to install U-Boot onto the SPI Flash chip that's on the device itself. This is useful if you want to be able to run the system off of other media without the eMMC or the microSD present.

We need to create a new SPI-specific image. In the u-boot build directory, run:

`user $``./tools/mkimage -n rk3399 -T rkspi -d tpl/u-boot-tpl.bin:spl/u-boot-spl.bin idbloader-spi.img`
Now, copy the required files to the bootable microSD/eMMC we created earlier:

`root #``cp idbloader-spi.img u-boot.itb /mnt/card`
#### Pre-flight checks

Before proceeding, let's be sure we can unbrick the device if something goes wrong.

Make sure you have a:

- microSD (or eMMC) with working U-Boot
- working serial adapter to see early boot messages
- jumper cable to connect two adjacent pins

SPI Flash can be disabled by grounding the SPI clock. Unplug the device from power and connect pin 23 (SPI1\_CLK) to pin 25 (GND) on the PI2 Bus. Connect to UART, and insert the bootable card. Then, power on the device and press a key to interrupt boot:

Now that you're in the U-Boot prompt, scan for SPI Flash. It should not be available:

`=>````
sf probe
```
jedec\_spi\_nor flash@0: unrecognized JEDEC id bytes: ff, ff, ff
Failed to initialize SPI flash at 1:0 (error 0)

Now, disconnect pins 23 and 25 and probe again:

`=>````
sf probe
```
SF: Detected gd25q128 with page size 256 Bytes, erase size 4 KiB, total 16 MiB

To wipe SPI Flash, you can run:

`=>````
sf erase 0 400000
```
SF: 4194304 bytes @ 0x0 Erased: OK

#### Write to SPI flash

Boot using microSD/eMMC created earlier and interrupt boot to enter the U-Boot shell.

Substitute `mmc 1:1` below with the device/partition number containing your images. `mmc 1:1` is the first partition on the microSD. For example, the third partition on eMMC would be `mmc 0:3`.

Run the following commands to flash the images:

`=>````
sf probe
```
`=>````
load mmc 1:1 $kernel_addr_r idbloader-spi.img
```
`=>````
sf erase 0 +$filesize
```
`=>````
sf write $kernel_addr_r 0 $filesize
```
`=>````
load mmc 1:1 ${kernel_addr_r} u-boot.itb
```
`=>````
sf erase 0x60000 +$filesize
```
`=>````
sf write $kernel_addr_r 0x60000 ${filesize}
```
To reboot, run:

`=>``reset`
Output will be similar to booting from microSD/eMMC as above, except in the serial console you should see:

Trying to boot from SPI

## Booting a Linux system

Now that we have U-Boot installed, next step is to boot the Linux system.

By default, U-Boot scans the partitions of each device (except SATA, see next section) to find something it knows how to load. The default supported file systems are `ext2`, `ext4` and `FAT`, but there are a few others you can enable in U-Boot's `menuconfig`. You will want your /boot partition to be formatted with one of those filesystems.

### Creating boot.scr

One thing it can load `boot.scr`, which will contain commands for U-Boot to execute. This will be in your /boot partition along with your kernel and device tree binary.

First, create a file `boot.cmd`:

Then, compile it to `boot.scr`:

`root #``mkimage -C none -A arm64 -T script -d boot.cmd boot.scr`
If the /boot partition is on eMMC or microSD, it should work without additional steps.

### Booting off of SATA

Currently, U-Boot will not try to boot from SATA automatically. If you want your /boot partition to be in SATA, you will need to perform some additional steps.

#### Manually

Expand below to see a sequence of commands to boot from SATA. This is mainly for educational purposes:

```
=> pci
BusDevFun  VendorId   DeviceId   Device Class       Sub-Class
_____________________________________________________________
00.00.00   0x1d87     0x0100     Bridge device           0x04
01.00.00   0x1b4b     0x9215     Mass storage controller 0x06
=> scsi scan
scanning bus for devices...
Target spinup took 0 ms.
Target spinup took 0 ms.
Target spinup took 0 ms.
SATA link 3 timeout.
AHCI 0001.0000 32 slots 4 ports 6 Gbps 0xf impl SATA mode
flags: 64bit ncq led only pmp fbss pio slum part sxs
  Device 0: (0:0) Vendor: ATA Prod.: CT1000MX500SSD1 Rev: M3CR
            Type: Hard Disk
            Capacity: 953869.7 MB = 931.5 GB (1953525168 x 512)
  Device 1: (1:0) Vendor: ATA Prod.: WDC WD10EARS-00Y Rev: 80.0
            Type: Hard Disk
            Capacity: 953869.7 MB = 931.5 GB (1953525168 x 512)
  Device 2: (2:0) Vendor: ATA Prod.: HGST HTS725050A7 Rev: GH2O
            Type: Hard Disk
            Capacity: 476940.0 MB = 465.7 GB (976773168 x 512)
=> part list scsi 0
Partition Map for SCSI device 0  --   Partition Type: EFI
Part    Start LBA       End LBA         Name
        Attributes
        Type GUID
        Partition GUID
  1     0x00000800      0x000207ff      ""
        attrs:  0x0000000000000000
        type:   0fc63daf-8483-4772-8e79-3d69d8477de4
        guid:   dc920591-9516-ed4f-b4b0-67262ae9b42c
  2     0x00020800      0x080207ff      ""
        attrs:  0x0000000000000000
        type:   0fc63daf-8483-4772-8e79-3d69d8477de4
        guid:   82067900-d641-984b-a399-799968d9a850
  3     0x08020800      0x0c0207ff      ""
        attrs:  0x0000000000000000
        type:   0fc63daf-8483-4772-8e79-3d69d8477de4
        guid:   b574080b-f70c-0840-b5ab-aaee17da76d6
=> ls scsi 0:1 /
<DIR>       1024 .
<DIR>       1024 ..
<DIR>      12288 lost+found
             705 boot.cmd
             777 boot.scr
           60690 rk3399-rockpro64.dtb
         8020424 Image.gz
<DIR>       1024 u-boot-2022-07
             370 Makefile
=> load scsi 0:1 $fdt_addr_r rk3399-rockpro64.dtb
60690 bytes read in 3 ms (19.3 MiB/s)
=> load scsi 0:1 $kernel_addr_r Image.gz
8020424 bytes read in 131 ms (58.4 MiB/s)
=> setenv bootargs root=/dev/sda2 rootdelay=2
=> booti $kernel_addr_r - $fdt_addr_r
   Uncompressing Kernel Image
Moving Image from 0x2080000 to 0x2200000, end=3410000
## Flattened Device Tree blob at 01f00000
   Booting using the fdt blob at 0x1f00000
   Loading Device Tree to 00000000f1f09000, end 00000000f1f1ad11 ... OK
Starting kernel ...
[    0.000000] Booting Linux on physical CPU 0x0000000000 [0x410fd034]
[    0.000000] Linux version 5.18.12 (larry@barn) (aarch64-unknown-linux-gnu-gcc (Gentoo 12.1.1_p20220625 p8) 12.1.1 20220625, GNU ld (Gentoo 2.38 p4) 2.38) #17 SMP PREEMPT Sat Jul 16 10:38:56 EDT 2022
...
```
#### Update existing environment script

A better way would be to update the boot script in the existing environment, so that it boots automatically. You can see it if you run

`=>``printenv`
U-Boot's `hush` shell is somewhat reminiscent of a minimal UNIX shell. It does not have a concept of functions, but it can evaluate the values stored in variables using the `run` command. The entry point is `distro_bootcmd`. You can follow from there to see the steps it takes to boot.

In a similar style, let's add steps to boot from the first disk, and prepend the new `scsi0` to `boot_targets`:

`=>````
setenv bootcmd_scsi0 "devnum=0; run boot_pci_enum; run scsi_boot"
```
`=>````
setenv scsi_boot "scsi scan; if scsi dev ${devnum}; then devtype=scsi; run scan_dev_for_boot_part; fi"
```
`=>````
setenv boot_targets "scsi0 mmc1 mmc0 nvme0 usb0 pxe dhcp sf0"
```
`=>````
saveenv
```
Saving Environment to SPIFlash... Erasing SPI flash...Writing to SPI flash...done
OK

Now, if you have your boot partition set up (e.g. it contains `boot.scr`), you should be able to boot:

`=>``boot`
## Accessing U-Boot environment from Linux

You can view and modify U-Boot's environment without having to boot into U-Boot's interactive shell.

`root #``emerge --ask --update dev-embedded/u-boot-tools`
Edit /etc/fw\_env.config and replace the provided examples with the following:

**`/etc/fw_env.config`**

```
# MTD device name       Device offset   Env. size       Flash sector size       Number of sectors
/dev/mtd0               0x3f8000        0x8000          0x2000
```
`root #``fw_printenv`
altbootcmd=setenv boot\_syslinux\_conf extlinux/extlinux-rollback.conf;run distro\_bootcmd
arch=arm
baudrate=1500000
board=rockpro64\_rk3399

…

To set a value, run:

`root #``fw_setenv test_var 'test value'`
