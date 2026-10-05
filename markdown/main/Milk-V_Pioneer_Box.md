<!-- source: https://wiki.gentoo.org/wiki/Milk-V_Pioneer_Box | group: Gentoo Wiki (Main) | wiki-title: Milk-V Pioneer Box -->
---
title: Milk-V Pioneer Box
url: https://wiki.gentoo.org/wiki/Milk-V_Pioneer_Box
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-05"
fingerprint: "1e009a1b0965b9e9"
license: CC BY-SA 4.0
---

# Milk-V Pioneer Box

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gentoo on the [Milk-V Pioneer Box](https://milkv.io/pioneer).

## Hardware

The best resource for the hardware inside the Pioneer Box is [https://milkv.fyi/](https://milkv.fyi/). We will avoid repeating the information there, and note only the default partition layout on the 1TB NVMe drive.

| Partition | Description | Fedora 38 mount point | Size | 
|---|---|---|---|
| /dev/nvme0n1p1 | FAT32 UEFI partition | /boot/efi | 122M | 
| /dev/nvme0n1p2 | ext4 boot partition | /boot | 488M | 
| /dev/nvme0n1p3 | ext4 Fedora 38 root | / | 14G | 

This leaves roughly 939.2G at the end of the drive unused. The perfect place to install Gentoo.

### Serial console

The serial console on the box is exposed via the USB-C port on the front of the case. It is connected to UART Bridge controller on the motherboard. If you attach to it using a USB **data** cable, you should see something like the following on the client machine:

**`/var/log/dmesg`**

To make use of this, you need two kernel modules (again on the client):

**Serial-to-USB modules for the Pioneer**

The resulting modules are called `usbserial` and `cp210x`. If you have them loaded, you should see e.g. /dev/ttyUSB0 created when you connect your USB cable to the Pioneer:

**`/var/log/dmesg`**

Now just run screen (this requires root or sudo unless you've added yourself to the [acct-group/dialout](https://packages.gentoo.org/packages/acct-group/dialout) group),

`user $``screen /dev/ttyUSB0 115200`
and reboot the Pioneer.

### RISC-V and MCU consoles

But wait, there's more. On the Pioneer Box, the front-of-the-box serial port/cable is already attached to the pins on the motherboard. But there are at two other serial consoles available, both having three pins and running at 3.3 volts:

1. The "RISC-V" console. This is labeled and you can see it on the motherboard.
2. The "MCU" console. Again, labeled. Its pins are right next to the "RISC-V" console pins.

To connect to these, you'll need another adapter. I'm sure there are others that will work, but I've used the three-pin [TTL-232R-RPI from FTDI](https://ftdichip.com/products/ttl-232r-rpi/). It's well-documented, inexpensive, and the kernel drivers for it are upstream:

This will build a module called `ftdi_sio` that will detect the adapter and give you a /dev/ttyUSBX to connect to with `screen` on the client:

**`/var/log/dmesg`**

### Mainboard console

But wait, there's more. There's *also* a secret (undocumented) [mainboard console](https://github.com/milkv-pioneer/issues/issues/33#issuecomment-1919711235). This shows output even before the RISC-V console does.

## Early boot process

Out of the box, the Pioneer uses LinuxBoot to launch a customized version of Fedora 38, but there are actually several possible [boot paths](https://github.com/sophgo/sophgo-doc/blob/main/SG2042/HowTo/bootflow.rst). The process is summarized below. Later we go into more detail about [changing the  boot flow](https://wiki.gentoo.org#Changing_the_boot_flow).

1. 
The board determines whether or not a usable SD card is inserted. For example, the following output from the [mainboard console](https://wiki.gentoo.org#Mainboard_console) shows an SD card being detected: NOTICE: BOOT: 0x7000140000/0x1/0x5 NOTICE: Booting Trusted Firmware NOTICE: BL1: v2.7(release):83c4f9e3e NOTICE: BL1: Built : 10:39:27, Jul 12 2022 INFO: BL1: RAM 0x7010002000 - 0x7010011000 NOTICE: SD initializing 100000000Hz Hit key to stop autoboot: 00 NOTICE: BOOT: 0x7000140000/0x1/0x5 NOTICE: Booting Trusted Firmware NOTICE: BL1: v2.7(release):83c4f9e3e NOTICE: BL1: Built : 10:39:27, Jul 12 2022 INFO: BL1: RAM 0x7010002000 - 0x7010011000 NOTICE: SD initializing 100000000Hz NOTICE: boot from SD If an SD card is inserted, then from now on, everything will be relative to the the first FAT32 partition on the SD card. Otherwise, they'll refer to "files" stored on the SPI flash (/dev/mtd0), which does not have a nice filesystem on it (more on this later).
2. 
fip.bin is loaded from either the SD card or the SPI flash (see the first step). This is a Firmware Image Package (FIP), presumably generated using [fiptool](https://github.com/sophgo/fiptool). It is most likely signed and encrypted according to the [SOPHGO Secure Boot User Guide](https://doc.sophgo.com/cvitek-develop-docs/master/docs_latest_release/CV180x_CV181x/en/01.software/BSP/Secure_Boot_User_Guide/build/html/index.html), making it difficult to replace. [An updated copy of fip.bin](https://github.com/sophgo/edk2-non-osi/blob/devel-sg2042/Silicon/Sophgo/SG2042/Boot/fip.bin) lives in SOPHGO's [edk2-non-osi](https://github.com/sophgo/edk2-non-osi/) repository. We see from the [mainboard console](https://wiki.gentoo.org#Mainboard_console) that fip.bin is indeed loaded from an inserted SD card: INFO: BL1: Loading BL2 NOTICE: Locate FIP in SD FAT INFO: Loading image id=1 at address 0x7010020000 INFO: Image id=1 loaded: 0x7010020000 - 0x701005ca41 NOTICE: BL1: Booting BL2
3. 
The [zero stage bootloader](https://github.com/sophgo/zsbl) (ZSBL) is loaded from zsbl.bin either on the SD card or from the SPI flash. ZSBL reads a configuration file riscv64/conf.ini (either from SPI flash or from the SD card), collects the relevant information, and then hands it off to OpenSBI. This process is handled by the [boot() function](https://github.com/sophgo/zsbl/blob/master/plat/sg2042/boot.c).
4. [OpenSBI](https://github.com/sophgo/opensbi) runs.
5. 
Depending on riscv64/conf.ini, one of [LinuxBoot](https://www.linuxboot.org/), [U-Boot](https://www.denx.de/project/u-boot/), or [EDK2](https://github.com/sophgo/sophgo-edk2)/GRUB are launched next.
  - LinuxBoot
  - This is the stock bootloader. It loads its own early-stage initramfs (riscv64/initrd.img) and Linux kernel (riscv64/riscv64\_Image), both of which are included in the stock Fedora under /boot/efi/riscv64. Those copies are not actually used, however. The real copies are stored in the SPI flash where you can't easily access them. Before LinuxBoot loads, a serial console is needed to see the output from the boot process, but eventually it initializes the display and you can see what's going on. Hardware support is likely why this is the default boot method. Linux already has drivers for your hardware, but e.g. U-Boot does not.
  - EDK2/GRUB
  - 
This process uses a file called riscv64/SRA1-20.fd that is the result of building EDK2, and EDK2 is the "official" implementation of UEFI. The file riscv64/SRA1-20.fd knows to hand-off the rest of the process to a GRUB EFI image living at some predetermined path, which is why we call this the "EDK2/GRUB" process and not just "EDK2." The file shipped with the Pioneer is named riscv64/SG2042.fd, but newer versionsof EDK2 have renamed it. (The name doesn't actually matter, so long as you point riscv64/conf.ini to the correct file.) **Of the three, this is the recommended boot path, as it can easily be built on Gentoo and works with newer kernels.**
  - U-Boot
  - U-Boot is a more minimal UEFI implementation. You build U-Boot to get u-boot.bin, which in turn is what you point conf.ini at. U-Boot can then itself launch the GRUB EFI image, or any other image. This is a little bit faster (and a lot easier to build) than its EDK2 counterpart.

## Installing Gentoo

The easiest (and safest) way to install Gentoo is to take advantage of the empty space at the end of the NVMe drive. This leaves the stock Fedora available if you shoot yourself in the foot. The process roughly follows the Gentoo installation handbook, with a few modifications. There is no RISC-V handbook, but you should read and understand (say) the [amd64 handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64) before attempting this. We mention only the steps that differ.

### Preparing the disks (the easy way)

1. Instead of using a LiveUSB, boot into the Fedora 38 that ships with the box.
2. Log in, open a terminal, and `su` to root.
3. Run `cfdisk /dev/nvme0n1`, and create a new primary partition of type 83/Linux using the rest of the disk. Keep the existing UEFI and boot partitions, ignoring swap entirely :)
4. Save your changes and format the new partition, /dev/nvme0n1p4, with your favorite filesystem.
5. `mkdir /mnt/gentoo`

### Installing the Gentoo installation files

1. Download an rv64gc/lp64d stage3 from [https://www.gentoo.org/downloads/#riscv](https://www.gentoo.org/downloads/#riscv). These are the architecture and floating point ABI [supported by the C920 processors](https://github.com/sophgocommunity/Pioneer_Doc/blob/main/resources/T-Head_XuanTie_C920_Processor_Datasheet.pdf?raw=true) in the Pioneer.
2. Follow the handbook.

### Installing the Gentoo base system

### CFLAGS

According to /proc/cpuinfo, the C920 processors in the Pioneer box have an ISA of `rv64gcv`, which is [shorthand](https://en.wikipedia.org/wiki/RISC-V#ISA_base_and_extensions) for `rv64imafdcv`. The "v" however refers not to the final, ratified v1.0 of the vector extension, but instead to a draft v0.7.1 that is known by the name `XTheadVector`. So long as you have [gcc-14 or newer](https://gcc.gnu.org/gcc-14/changes.html#riscv) though, the draft extension is supported as well. To enable it within GCC, you can add (for example)

**`/etc/portage/make.conf`**

```
COMMON_FLAGS="$COMMON_FLAGS -march=rv64gcv"
```
to your make.conf. Beware however that this can make you vulnerable to [GhostWrite](https://wiki.gentoo.org#GhostWrite), which exploits `XTheadVector`. A safer choice is to omit the "v":

**`/etc/portage/make.conf`**

```
COMMON_FLAGS="$COMMON_FLAGS -march=rv64gc"
```
In addition to `-march`, there are `-mabi` and `-mtune` flags that may help:

**`/etc/portage/make.conf`**

```
COMMON_FLAGS="$COMMON_FLAGS ${CFLAGS_lp64d} -mtune=thead-c906"
```
At the time of writing, the variable `${CFLAGS_lp64d}` is defined in the riscv profiles (profiles/arch/riscv/make.defaults) and is equivalent to `-mabi=lp64d`. The `-mtune=thead-c906` tells GCC to optimize for the T-Head C906, a close relative of the C920s in the Pioneer Box. As of GCC 16 `-mtune=xt-c920` can be used (xt-c920 is for systems that implement RVV 0.7.1, while xt-c920v2 is for RVV 1.0).

References:

1. [https://gcc.gnu.org/onlinedocs/gcc/RISC-V-Options.html](https://gcc.gnu.org/onlinedocs/gcc/RISC-V-Options.html)
2. [https://www.sifive.com/blog/all-aboard-part-1-compiler-args](https://www.sifive.com/blog/all-aboard-part-1-compiler-args)
3. [https://www.reddit.com/r/RISCV/comments/1c7fyd6/differences\_between\_the\_xuantie\_c906\_c910\_and\_c920/](https://www.reddit.com/r/RISCV/comments/1c7fyd6/differences_between_the_xuantie_c906_c910_and_c920/)
4. [https://www.phoronix.com/news/GCC-XuanTie-RISC-V-CPUs](https://www.phoronix.com/news/GCC-XuanTie-RISC-V-CPUs)

### VIDEO\_CARDS

This is our video card,

`user $````
lspci -k | head -n5
```
```
0000:00:00.0 PCI bridge: Device 1e30:2042
0000:01:00.0 VGA compatible controller: Advanced Micro Devices, Inc. [AMD/ATI] Caicos [Radeon HD 6450/7450/8450 / R5 230 OEM]
        Subsystem: Hightech Information System Ltd. Device 3000
        Kernel driver in use: radeon
        Kernel modules: radeon, amdgpu
```
And according to the [Radeon](https://wiki.gentoo.org/wiki/Radeon) page, the value of `VIDEO_CARDS` corresponding to the [Caicos R5 230 card](https://wiki.gentoo.org#Hardware) should be,

**`/etc/portage/make.conf`**

```
VIDEO_CARDS="radeon r600"
```
### Configuring the Linux kernel

We're going to build our own kernel, if for no other reason than to mitigate [GhostWrite](https://wiki.gentoo.org#GhostWrite). Your best bet is to use the latest [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) with my [sg2042-patches](https://dev.gentoo.org/~mjo/distfiles/sg2042-patches-20260905.tar.xz) stored under [/etc/portage/patches](https://wiki.gentoo.org/wiki//etc/portage/patches). Upstream activity for the SG2042 has died down, so the patches may remain necessary indefinitely.

[a patch](https://lore.kernel.org/sophgo/20260421143154.1590156-1-guoren@kernel.org/T)on top of linux-next (as of 2026-05-17).

[Ghostwrite](https://wiki.gentoo.org#GhostWrite).

[SV39](https://docs.kernel.org/arch/riscv/vm-layout.html)-compatible MMU.

[Radeon](https://wiki.gentoo.org/wiki/Radeon)CAICOS card. Needed for the framebuffer console, among other things.

[a temporary clock skew issue](https://wiki.gentoo.org#Clock_skew_detected.21).

[dev-util/perf](https://packages.gentoo.org/packages/dev-util/perf).

[hacking the kernel source code](https://github.com/sophgo/sophgo-doc/blob/main/SG2042/HowTo/How%20to%20adjust%20CPU%20operating%20frequency.rst).

[the device tree](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/arch/riscv/boot/dts/sophgo/sg2042.dtsi), this is the GPIO controller we have.

[the device tree](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/arch/riscv/boot/dts/sophgo/sg2042.dtsi), this is the I2C controller we have. Again I don't know how this helps, but it does seem to work. With the module loaded, I can run e.g.

`user $``cat /sys/devices/platform/soc/7030005000.i2c/i2c-8/name`
Compiling it in plays a part in [avoiding clock skew](https://wiki.gentoo.org#Clock_skew_detected.21).

### Configuring the bootloader

Unfortunately, in order to use an upstream kernel, we must [build our own bootloader](https://wiki.gentoo.org#Building_the_bootloader). You should build ZSBL, OpenSBI, and EDK2/GRUB. These can all (make sure you get the correct branch!) be built from within Gentoo. Afterwards, you'll need to [change the  boot flow](https://wiki.gentoo.org#Changing_the_boot_flow) to use EDK2/GRUB, since it is not the default. You should do this on an SD card to
avoid breaking the default boot path.

**Update**: recent binaries of ZSBL (zsbl.bin), OpenSBI (fw\_dynamic.bin), and fip.bin are now available [in one of the SOPHGO repositories](https://github.com/sophgo/edk2-non-osi/tree/devel-sg2042/Silicon/Sophgo/SG2042/Boot).

## Firmware

During installation, you'll have the chance to install [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware). Gentoo allows fine-grained control of which [Linux firmware](https://wiki.gentoo.org/wiki/Linux_firmware) are installed. In particular, the [savedconfig](https://wiki.gentoo.org/wiki/Savedconfig) feature can be used to choose only the necessary blobs. The following is a short but comprehensive list; only the video card and onboard NIC require firmware blobs.

### Video card

Provided by [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware)

- radeon/BTC\_rlc.bin
- radeon/CAICOS\_mc.bin
- radeon/CAICOS\_me.bin
- radeon/CAICOS\_pfp.bin
- radeon/CAICOS\_smc.bin
- radeon/SUMO\_uvd.bin

#### Dracut

If you use dracut, then you should instruct it to include the video card's firmware into the initial ramdisk:

**`/etc/dracut.conf.d/radeon.conf`**

Otherwise your system may boot successfully, e.g., reachable via network, but the display will stay black, because Linux is not able to initialize the video card.

#### References

### Onboard NIC

1. rtl\_nic/rtl8125b-2.fw

References:

## Microarchitectural vulnerabilities

### GhostWrite

The Pioneer Box is vulnerable to [GhostWrite](https://ghostwriteattack.com/). To mitigate the vulnerability, you have choices. You can do it in userspace, by ensuring that you don't compile any code with support for the vector instructions. Take for example the [GhostWrite proof-of-concept](https://github.com/cispa/ghostwrite), contained in ghostwrite.c. If we try to compile it with the vector instructions enabled via `-march=rv64gcv`, it works:

`user $````
gcc -march=rv64gcv -o ghostwrite ghostwrite.c
```
`user $````
sudo ./ghostwrite
```
Virtual address:        3f83478000
Physical address:       1f6805f000
Value before:                 caaa
Value after:                  cafe

If instead we compile it with `-march=rv64gc`, omitting the "v", then it fails:

`user $````
gcc -march=rv64gc -o ghostwrite ghostwrite.c 
```
ghostwrite.c: Assembler messages:
ghostwrite.c:58: Error: unrecognized opcode \`vsetvli zero,zero,e8,m1', extension \`v' or \`zve64x' or \`zve32x' required
ghostwrite.c:59: Error: unrecognized opcode \`vmv.v.x v0,a0', extension \`v' or \`zve64x' or \`zve32x' required

The second way to mitigate GhostWrite is wholesale, in the kernel.

**Disable RISCV\_ISA\_V**

The description of the `RISCV_ISA_V` option is "Say N here if you want to disable all vector related procedure in the kernel." Disabling it does *not* affect the ISA string visible in /proc/cpuinfo, but it does disable the dangerous instructions, even in userspace. With `RISCV_ISA_V=n`, the GhostWrite proof-of-concept will compile (with `-march=rv64gcv`), but fail at runtime:

`user $````
gcc -march=rv64gcv -o ghostwrite ghostwrite.c
```
`user $````
sudo ./ghostwrite
```
Virtual address:        3fa7f07000
Physical address:       100da74000
Value before:                 caaa
Illegal instruction

### Security RISCs

Researchers at CISPA have identified several hardware vulnerabilities that affect the Pioneer Box:

- IEEE Symposium on Security and Privacy presentation
- [https://www.youtube.com/watch?v=3-c4C\_L2PRQ](https://www.youtube.com/watch?v=3-c4C_L2PRQ)
- Paper
- [A Security RISC: Microarchitectural Attacks on Hardware RISC-V CPUs](https://misc0110.net/files/riscv_attacks_sp23.pdf)
- Github repository
- [https://github.com/cispa/Security-RISC](https://github.com/cispa/Security-RISC)

I've opened a bug about these: [https://github.com/milkv-pioneer/issues/issues/38](https://github.com/milkv-pioneer/issues/issues/38)

#### Cache+Time (new)

*Cache+Time* exploits branch prediction in combination with the ability to flush the instruction cache using the standard `fence.i` instruction, and possibly the proprietary `icache.iva` instruction. It is related to [Spectre](<https://en.wikipedia.org/wiki/Spectre_(security_vulnerability)>).

The Pioneer box is vulnerable to this and there are presently no mitigations.

#### Flush+Fault (new)

This is a variant of the usual *Flush+Reload* attack that works even when the instruction and data caches are separate. Both proofs of concept use `fence.i`, but the proprietary `dcache.civa` and `icache.iva` might be usable instead.

The Pioneer is vulnerable to this and there are presently no mitigations.

#### CycleDrift (new)

*CycleDrift* relies on unprivileged access to the `rdcycle` and `rdinstret` instructions. In newer linux kernels, this is mitigated by default:

After that patch, the instructions are privileged by default, and can only be made unprivileged with the `perf_user_access` sysctl. There does not seem to be an easy way to do this using the SOPHGO kernel.

#### Flush+Flush / Fence+Flush (old)

This is a known attack that depends on a second "flush" operation being faster if the cache is still empty. We mention it only because the standard `fence.i` instruction is *not* vulnerable—it takes the same amount of time either way. The proprietary `dcache.civa` however reintroduces the exploit, because it is unprivileged and has variable timing.

The Pioneer box is vulnerable to this as well, with no mitigation yet.

## Building the bootloader

Before we  start messing with the boot flow, we should know how to build the bootloader ourselves. The "bootloader" consists of several independent projects, most of which have been forked by SOPHGO. Rather than cross-compile them, we can build them on the Pioneer itself. This simplifies things a bit, but we still mostly follow the [envsetup.sh script](https://github.com/sophgo/bootloader-riscv/blob/master/scripts/envsetup.sh) in SOPHGO's [bootloader-riscv](https://github.com/sophgo/bootloader-riscv) repository.

**Update**: recent binaries of ZSBL (zsbl.bin), OpenSBI (fw\_dynamic.bin), and fip.bin are now available [in one of the SOPHGO repositories](https://github.com/sophgo/edk2-non-osi/tree/devel-sg2042/Silicon/Sophgo/SG2042/Boot).

### LinuxBoot

I don't even know where to begin with this one. The only way I know to obtain riscv64\_Image and initrd.img is to steal them from the [pre-built OS images provided by SOPHGO](https://github.com/sophgo/sophgo-doc/blob/main/SG2042/HowTo/FAQ.rst), but those are outdated and will not work with an upstream kernel.

It's better to avoid this method entirely.

### EDK2

This should be your preferred bootloader, since it actually works.

The only trick here is that there are *three* forked SOPHGO repositories that you need to clone, and that all of them have a special branch you need to check out. Their build script doesn't mention this!

`user $````
git clone https://github.com/sophgo/edk2.git edk2.git
```
`user $````
git clone https://github.com/sophgo/edk2-platforms.git edk2-platforms.git
```
`user $````
git clone https://github.com/sophgo/edk2-non-osi.git edk2-non-osi.git
```
`user $````
cd edk2-platforms.git; git checkout devel-sg2042; cd ..
```
`user $````
cd edk2-non-osi.git; git checkout devel-sg2042; cd ..
```
`user $``cd edk2.git; git checkout devel-sg2042; cd ..`
Now to build:

`user $````
cd edk2.git
```
`user $````
git submodule sync
```
`user $````
git submodule update --init --recursive
```
`user $````
export PACKAGES_PATH="$(pwd)/../edk2-platforms.git:$(pwd)/../edk2-non-osi.git"
```
`user $````
. edksetup.sh
```
`user $````
make -j64 -C BaseTools
```
`user $``build -a RISCV64 -t GCC5 -b RELEASE -p Platform/Sophgo/SG2042Pkg/SRA1-20/SRA1-20.dsc`
When all is said and done, you should be in possession of Build/SRA1-20/RELEASE\_GCC5/FV/SRA1-20.fd.

### zsbl

There are two tricks that you may need to build the zero-stage boot loader:

1. Installing [sys-apps/dtc](https://packages.gentoo.org/packages/sys-apps/dtc). This is required for the build.
2. Adding `-znotext` to "LDFLAGS" by appending `-Wl,-znotext` to `CC`. This avoids a RELRO linking issue.

We also typically use `make V=1` and `make CROSS_COMPILE=""` to ensure that the build is verbose, and that the right toolchain names are used, respectively. In summation,

`user $````
git clone https://github.com/sophgo/zsbl.git zsbl.git
```
`user $````
cd zsbl.git
```
`user $````
mkdir build
```
`user $````
make V=1 O=build ARCH=riscv sg2042_defconfig
```
`user $````
cd build
```
`user $``make -j64 V=1 CROSS_COMPILE="" ARCH=riscv CC="gcc -Wl,-znotext"`
### OpenSBI

Somewhat recently, SOPHGO have changed the default branch of their repo to the upstream master branch. Be sure to check out the `sg2042-dev` branch before you build it; otherwise, everything will appear to work up until the point where you [reboot and it hangs](https://github.com/sophgo/opensbi/issues/36#issuecomment-2623136490).

`user $````
git clone https://github.com/sophgo/opensbi.git opensbi.git
```
`user $````
cd opensbi.git
```
`user $````
git checkout sg2042-dev
```
`user $``make -j64 V=1 PLATFORM_RISCV_ABI=lp64 PLATFORM_RISCV_ISA=rv64gc CROSS_COMPILE= PLATFORM=generic FW_PIC=y`
### U-Boot

It looks like SOPHGO has given up on this one. The original git repo has disappeared, and the fork at [https://github.com/sophgo/u-boot-2021.10](https://github.com/sophgo/u-boot-2021.10) doesn't support the SG2042. I was never able to get it to boot the newer kernels anyway. Use EDK2 instead.

### GRUB

You can use the [sys-boot/grub](https://packages.gentoo.org/packages/sys-boot/grub) from Gentoo. Set up `GRUB_PLATFORMS`,

**`/etc/portage/package.use`**

and install it. If you really want to, you can add [sys-boot/efibootmgr](https://packages.gentoo.org/packages/sys-boot/efibootmgr) to [/etc/portage/profile/package.provided](https://wiki.gentoo.org/wiki//etc/portage/profile/package.provided). It gets pulled in by GRUB, but won't work on the Pioneer.

Now, rather than mess with grub-install, what we want to do is build a single EFI image. First we need to create a "default" config file that will tell GRUB where to look for grub.cfg. If we skip this step, it may not know where to look.

**`defaults.cfg`**

The UUID of your disk can be found under /dev/disk/by-uuid. Now we generate the EFI image:

`user $````
GRUB_MODULES="acpi boot cat configfile disk echo elf exfat ext2 fat fdt file gettext gzio halt hashsum help hexdump keystatus linux lsacpi lsefimmap lsefi lsefisystab lsmmap ls minicmd mmap msdospart normal part_gpt part_msdos probe progress read reboot regexp search_fs_file search_fs_uuid search_label search serial setjmp xzio"
```
`user $``grub-mkimage -v -o BOOTRISCV64.EFI -O riscv64-efi -p efi -c defaults.cfg $GRUB_MODULES`
(You can probably reduce that list of modules even further if you feel like it.) Now, to "install" GRUB, we simply copy BOOTRISCV64.EFI to the location where U-Boot or EDK will be expecting it. For U-Boot, it's the root of the device, so most likely the first partition on your SD card.

## Changing the boot flow

The boot flow can be altered with a configuration file called conf.ini. The two methods for supplying it are laid out in a [HowTo by SOPHGO](https://github.com/sophgo/sophgo-doc/blob/main/SG2042/HowTo/Configuration%20Info%20in%20INI%20file.rst).

The first is to create and write conf.ini to the SPI flash via /dev/mtd0. This is the more difficult method for two reasons. First, /dev/mtd0 is a [Memory Technology Device](http://linux-mtd.infradead.org/doc/general.html), and does not have a normal filesystem that can be mounted somewhere. But more importantly, this is the "baked in" configuration file that the box will use if there is no SD card inserted. It's probably best not to mess with it right away, so we will focus on the second method.

### Using an SD card

Which is, to create and write conf.ini to an SD card. The first thing to know is that it's not quite as easy as the HowTo suggests. Your SD card needs the following files, at a minimum:

- fip.bin ([downloaded from Github](https://github.com/sophgo/edk2-non-osi/blob/devel-sg2042/Silicon/Sophgo/SG2042/Boot/fip.bin))
- zsbl.bin ([built from ZSBL](https://wiki.gentoo.org#ZSBL), or [downloaded from Github](https://github.com/sophgo/edk2-non-osi/blob/devel-sg2042/Silicon/Sophgo/SG2042/Boot/zsbl.bin))
- riscv64/fw\_dynamic.bin ([built from OpenSBI](https://wiki.gentoo.org#OpenSBI), or [downloaded from Github](https://github.com/sophgo/edk2-non-osi/blob/devel-sg2042/Silicon/Sophgo/SG2042/Boot/fw_dynamic.bin))
- riscv64/conf.ini (you write this from scratch)
- riscv64/mango-milkv-pioneer.dtb (a device tree blob, built from zsbl)
- riscv64/SRA1-20.fd ([built from EDK2](https://wiki.gentoo.org#EDK2))
- EFI/BOOT/BOOTRISCV64.EFI ([built with GRUB](https://wiki.gentoo.org#GRUB))

And finally, if you want to put your kernel on the SD card too,

- kernel-gentoo-6.19.0+ (the name of the kernel I'll be using)
- sg2042-milkv-pioneer.dtb (another device tree blob, built with the kernel)

Yes, there are two separate DTBs: the one that needs to be fed to the bootloader via riscv64/conf.ini, and the one that needs to be passed to the kernel via GRUB. No, you can't use the same one for both; if you try to use the kernel DTB with zsbl, you'll get a page fault in EDK2.

#### GRUB example

Let's switch from the default LinuxBoot process to GRUB, assuming:

1. You already have an SD card whose first partition has been formatted as FAT32.
2. You have [built GRUB](https://wiki.gentoo.org#GRUB) and can build a `BOOTRISCV64.EFI` image.
3. You have [built SRA1-20.fd](https://wiki.gentoo.org#EDK2) from EDK2.

We're going to build a new GRUB image, but we have to be careful to point it to the right grub.cfg. You have some choices here, but I'm going to put grub.cfg on the SD card along with everything else, so that booting from the card is largely independent of the rest of the system. To do that, refer back to how we built GRUB using a file called defaults.cfg. Inside that file, we specify a "root" partition by UUID, and in this case, I want to use the UUID of the SD card. Yours will be different, but it can be found the same way:

`user $````
ls -lh /dev/disk/by-uuid/
```
total 0
lrwxrwxrwx 1 root root 15 2024-09-09 15:36 104ad877-cc35-4e61-9e63-0ceea0d353ed -> ../../nvme0n1p3
lrwxrwxrwx 1 root root 15 2024-09-09 16:06 1E9C-55A8 -> ../../mmcblk1p1
lrwxrwxrwx 1 root root 15 2024-09-09 15:36 1EE7-06EB -> ../../nvme0n1p1
lrwxrwxrwx 1 root root 15 2024-09-09 16:06 4b98d180-42f7-4bc1-b25f-2871b610a9a9 -> ../../mmcblk1p2
lrwxrwxrwx 1 root root 15 2024-09-09 15:36 4d9f57b4-cf22-4c6e-a13f-ead74716b706 -> ../../nvme0n1p4
lrwxrwxrwx 1 root root 15 2024-09-09 15:36 d9518f6d-f175-4331-83de-cb10ef6d8885 -> ../../nvme0n1p2

I want `1E9C-55A8` because it corresponds to /dev/mmcblk1p1, the first partition on my SD card. Anyway, put that in your defaults.cfg and [build a new BOOTRISCV64.EFI](https://wiki.gentoo.org#GRUB). Once you have it, move it to EFI/BOOT/BOOTRISCV64.EFI on your SD card.

Finally, we need the GRUB configuration file, which (thanks to our defaults.cfg), we know should live at grub/grub.cfg on the SD card:

**`grub/grub.cfg`**

The entry above refers to a device tree blob, /sg2042-milkv-pioneer.dtb, so you should copy that onto the SD card from your (compiled) kernel source tree as well.

Finally, we will need to create a conf.ini to tell ZSBL to tell OpenSBI to use SRA1-20.fd. This should live at riscv64/conf.ini on your SD card:

**`riscv64/conf.ini`**

```
[sophgo-config]
[devicetree]
name = mango-milkv-pioneer.dtb
[kernel]
name = SRA1-20.fd
[eof]
```
Once again this refers to mango-milkv-pioneer.dtb that is built with newer versions of ZSBL, so don't forget to put that in the riscv64 directory on your SD card.

If you are very lucky, you can now reboot the machine, and it will load GRUB and then, after two more seconds, Linux. **Keep in mind that you can't see any of this except through a serial console.** To recap, this example requires the following files on your SD card:

- fip.bin
- zsbl.bin
- riscv64/fw\_dynamic.bin
- riscv64/conf.ini
- riscv64/mango-milkv-pioneer.dtb
- riscv64/SRA1-20.fd
- EFI/BOOT/BOOTRISCV64.EFI
- kernel-gentoo-6.19.0+
- sg2042-milkv-pioneer.dtb

}}

### Overwriting the SPI flash

This process isn't for the faint of heart. Proceed at your own risk! There's not too much risk if you have a bootable SD card. You should be familiar with [Using an SD card](https://wiki.gentoo.org#Using_an_SD_card) first anyway.

#### The short version

There's no real filesystem on the SPI flash and thus no file names. What there is, is a convention: ZSBL expects conf.ini to live at the very beginning of /dev/mtd0, and a "partition table" to be present at a specific address on /dev/mtd1. That partition table is more like a directory list in that it contains information about the rest of the "files" on the SPI flash. And how to those "files" and the "partition table" get there? Well, there is a utility called [gen\_spi\_flash](https://github.com/sophgo/bootloader-riscv/blob/master/scripts/gen_spi_flash.c) that follows the same convention as ZSBL, and builds you a file called spi\_flash.bin. That file gets written to /dev/mtd1 with `flashcp` and then when you reboot, everything is where ZSBL expects it to be. There is an example (but no documentation) of using the script in [envsetup.sh](https://github.com/sophgo/bootloader-riscv/blob/master/scripts/envsetup.sh#L1714) in one of the SOPHGO repos.

#### The long version

The SPI flash is accessible via /dev/mtd0 and /dev/mtd1... sort of. But they're not normal block devices, and don't contain normal filesystems. So you can't just mount them and write files to it. The boot process likewise cannot just read files off of these devices—there is a convention that needs to be followed. So let's reverse engineer it. ZSBL is what loads the files, and there are a few that it's expecting. What follows is from plat/sg2042/boot.c in the ZSBL git repository.

First, there are two flash devices, corresponding to /dev/mtd0 and /dev/mtd1. The driver included with ZSBL knows about both of these:

**`include/driver/spi-flash/mango_spif.h`**

```
#define FLASH0_BASE		0x7000180000
#define FLASH1_BASE		0x7002180000
```
And just like when booting from an SD card, there are a few "files" that ZSBL will be looking for:

- conf.ini
- Your config file.
- fw\_dynamic.bin
- The OpenSBI firmware. Its name can be changed with the `[firmware]` property in conf.ini.
- mango.dtb
- A device tree blob. Its name can be changed with the `[devicetree]` property in conf.ini.
- riscv64\_Image
- This is the name of the EFI image to launch (for example, a LinuxBoot kernel). This can can be changed via `[kernel]` in conf.ini.
- initrd.img
- If you are using LinuxBoot, you'll need one of these. It can be set via `[ramfs]` in conf.ini.

You might be thinking: if the names of the other files can be changed by conf.ini, then don't we have to read conf.ini first? And you would be right. ZSBL has a special function `read_conf_and_parse()` that reads conf.ini from the first flash device. In the branch for the SPI flash (as opposed to an SD card), we find

These are low-level commands to read bytes from /dev/mtd0, and store them at the address pointed to by `boot_file[conf_file_index].addr`. But ultimately, that address is just the address of a buffer called `conf_file`,

So it's reading 1023 bytes from /dev/mtd0 into the `conf_file` buffer. So far so good. If you compare what we now know with the [HowTo](https://github.com/sophgo/sophgo-doc/blob/main/SG2042/HowTo/Configuration%20Info%20in%20INI%20file.rst#spi-nor-flash), it makes sense that we can just

`user $``flashcp conf.ini /dev/mtd0`
to update/overwrite the configuration.

But once the configuration has been parsed, there are the other "files" to worry about, and their names can change depending on the entries in conf.ini. This gets weird because there is no filesystem and therefore no file names on the SPI flash! (Remember, we read "conf.ini" by reading a fixed number of bytes from a fixed hardware address.)

The function `build_bootfile_info()` is what uses the contents of conf.ini. Basically it fills an array, called `boot_file`, with the names and addresses from either conf.ini or the defaults:

**`include/driver/io/io.h`**

```
typedef struct boot_file {
	uint32_t id;
	char *name;
	uint64_t addr;
	int len;
	uint32_t offset;
} BOOT_FILE;
```
**`plat/sg2042/boot.c`**

```
BOOT_FILE boot_file[ID_MAX] = {
	[ID_CONFINI] = {
		.id = ID_CONFINI,
		.name = "0:riscv64/conf.ini",
		.addr = (uint64_t)conf_file,
	},
        ...
	[ID_DEVICETREE] = {
		.id = ID_DEVICETREE,
		.name = "0:riscv64/mango.dtb",
		.addr = DEVICETREE_ADDR,
	},
};
```
Defaults for `.len` and `.offset` are not provided; they depend on the files themselves and need to be read from the "disk" at some point. The default addresses are supplied by a header:

**`plat/sg2042/include/platform.h`**

```
#define OPENSBI_ADDR	0x0
#define KERNEL_ADDR	0x2000000
#define RAMFS_ADDR	0x30000000
#define DEVICETREE_ADDR	0x20000000
```
And it looks like the default names are right there in `.name`, but they're not. The real default names differ on an SD card and on the SPI flash. Here they are:

Back in `build_bootfile_info()`, we can now look at a representative example:

When booting from flash, `imgs[ID_OPENSBI]` will refer to "fw\_dynamic.bin" in `img_name_spi`, so this is checking to see if the configuration file provided a value, and if not, falling back to the default. Okay.

But how are the filenames being used? Remember, no filesystem. This sends us down another rabbit hole, in `read_all_img()`, which is responsible for reading the "files." It uses an I/O abstraction that can be found under `drivers/io` in the ZSBL tree, and if we look at the flash implementation, it's doing a few things. First, it initializes the device. The underlying implementation here just calls,

**`drivers/io/io_flash.c`**

```
int flash_device_init(void)
{
	bm_spi_init(FLASH1_BASE);
	return 0;
}
```
which is helpful, because it tells us that everything we're doing is relative to /dev/mtd1. In other words, conf.ini is the only thing stored on /dev/mtd0, and everything else goes on /dev/mtd1.

After initializing the device, what we ultimately want to do is read a file, and once we get there, that part is simple: it's reading a certain amount of bytes from an offset. Namely, it's reading the-length-of-the-file bytes from where the file is supposed to start. But we don't know those! Which brings us to the intermediate step: we have to obtain some "file info" based on a name:

This, in turn, defers to the flash implementation in `flash_device_get_file_info()`, where the real magic happens:

**`drivers/io/io_flash.c`**

```
int flash_device_get_file_info(BOOT_FILE *boot_file, int id, FILINFO *f_info)
{
	int ret;
	struct part_info info;
	ret = mango_get_part_info_by_name(DISK_PART_TABLE_ADDR, boot_file[id].name, &info);
	if (ret) {
		pr_debug("failed to get %s partition info\n", boot_file[id].name);
		return ret;
	}
	f_info->fsize = info.size;
	boot_file[id].offset = info.offset;
	pr_debug("load %s image from sf 0x%x to memory 0x%lx size %d\n",
		 boot_file[id].name, info.offset, info.lma, info.size);
	return 0;
}
```
This is reading a "partition table" to search for a file by name. It turns out that the "partition table" is just a struct that gets dumped to disk:

**`include/driver/spi-flash/mango_spif.h`**

```
#define DISK_PART_TABLE_ADDR      0x600000
struct part_info {
	/* disk partition table magic number */
	uint32_t magic;
	char name[32];
	uint32_t offset;
	uint32_t size;
	char reserve[4];
	/* load memory address*/
	uint64_t lma;
};
```
**`driver/spi-flash/mango_spif.c`**

```
static int mango_get_part_info(uint32_t addr, struct part_info *info)
{
	int ret;
	ret = plat_spif_read((uint64_t)info, addr, sizeof(struct part_info));
	if (ret)
		return ret;
	if (info->magic != DPT_MAGIC) {
		pr_debug("bad partition magic\n");
		return -EINVAL;
	}
	return ret;
}
int mango_get_part_info_by_name(uint32_t addr, const char *name, struct part_info *info)
{
	int ret;
	do {
		ret = mango_get_part_info(addr, info);
		addr += sizeof(struct part_info);
	} while (!ret && strcmp(info->name, name));
	return ret;
}
```
So after all that, it's expecting there to be a "partition table" stored at a fixed address, where in reality that just means some struct (containing file metadata) dumped to disk.

Now the problem becomes: if we have a bunch of files and want to put them on /dev/mtd1, where do we put them, and then how do we generate the "partition table" that will let ZSBL find them?

Well, thankfully, SOPHGO have written a utility to do it for us. It's
called [gen\_spi\_flash](https://github.com/sophgo/bootloader-riscv/blob/master/scripts/gen_spi_flash.c), and it compiles to a simple CLI program that can generate the flash image for us. Right away, we see that it shares the `DISK_PART_TABLE_ADDR` and `struct part_info` definitions with ZSBL:

**`gen_spi_flash.c`**

```
#define DISK_PART_TABLE_ADDR	0x600000
...
/* disk partition table */
struct part_info {
	/* disk partition table magic number */
	uint32_t magic;
	char name[32];
	uint32_t offset;
	uint32_t size;
	char reserve[4];
	/* load memory address*/
	uint64_t lma;
};
```
If we look inside `main()` we see that the program takes (on the CLI) four arguments at a time: a partition name, a file name, an offset, and a load memory address (LMA).

**`gen_spi_flash.c`**

```
for (i = 0; i < part_num; i++) {
	part_name = argv[i * NUM_PAR_PER_PART + 1];
	file_name = argv[i * NUM_PAR_PER_PART + 2];
	ret = sscanf(argv[i * NUM_PAR_PER_PART + 3], "0x%lx",
						&flash_offset);
	...
	ret = sscanf(argv[i * NUM_PAR_PER_PART + 4], "0x%lx", &lma);
```
For each of these, the `file_name` is opened and read. Then its contents are written to a partition named `part_name`,
stored at `flash_offset` with the given LMA. The output is an image file called spi\_flash.bin that can be
`flashcp`ed to /dev/mtd1. Typically, the file name and partion name will agree, and the LMA should match the
corresponding `address` in your conf.ini, or more likely, agree with the defaults. Remember these?

**`plat/sg2042/include/platform.h`**

```
#define OPENSBI_ADDR	0x0
#define KERNEL_ADDR	0x2000000
#define RAMFS_ADDR	0x30000000
#define DEVICETREE_ADDR	0x20000000
```
They were used as the defaults in ZSBL, and the envsetup.sh script that SOPHGO runs to generate the flash image uses those same
values. (SG2042.fd, like u-boot.bin, is an alternate "kernel" that will be used *instead* of riscv64\_Image, so
it's OK to load those all into the same address.) Here's an excerpt from the SOPHGO script:

`user $````
dtb_group=$(ls *.dtb | awk '{print ""$1" "$1" 0x601000 0x020000000 "}')
```
`user $``./gen_spi_flash $dtb_group \`
fw_dynamic.bin fw_dynamic.bin 0x660000 0x00000000 \
		riscv64_Image riscv64_Image 0x6b0000 0x02000000 \
		SG2042.fd SG2042.fd 0x02000000 0x02000000 \
		zsbl.bin zsbl.bin 0x2a00000 0x40000000 \
		initrd.img initrd.img 0x2b00000 0x30000000

The flash offsets, however, look to be up to you. ZSBL will find them by partition name, so there are only a few constraints:

1. Leave space at the beginning for two copies of fip.bin. Although it doesn't appear on the command line, `gen_spi_flash` will store two of them right at the beginning of the output image.
2. The partition table starts at `DISK_PART_TABLE_ADDR` (0x600000), and takes up whatever the size of a `struct part_info` is. The DTB files are written after that (there's a comment in the source to that effect), ignoring the offset you specify. So the actual files should probably start far enough after 0x600000 to leave room for the struct and your device tree blobs.
3. The LMA of zsbl seems important, because it can't be fudged in conf.ini. Maybe it's hard-coded in the firmware (in fip.bin)?
4. Each file should leave enough room for the previous one (ha ha).

Anyway, I hope you're familiar with hexadecimal. With that out of the way, here are the steps to make this all work:

1. Copy all of the files that you want to write to flash into one directory. I'm going to use /boot/efi. You'll need fip.bin, zsbl.bin, conf.ini, and anything else referred to by conf.ini.
2. Compile gen\_spi\_flash
3. Figure out what arguments to pass to `gen_spi_flash`, and in what order
4. Run it
5. Write conf.ini to /dev/mtd0 using `flashcp`
6. Write spi\_flash.bin to /dev/mtd1 using `flashcp`
7. Reboot and pray

We can't do a U-Boot example, because [U-Boot doesn't support the SPI flash](https://github.com/sophgo/u-boot/issues/8) on the Pioneer yet. So instead we'll do [LinuxBoot](https://wiki.gentoo.org#LinuxBoot) and [EDK2/GRUB](https://wiki.gentoo.org#EDK2).

Make a new directory to work in:

`user $````
mkdir new-firmware
```
`user $``cd new-firmware`
The first thing we need to do is obtain fip.bin from SOPHGO's edk2-non-osi repository.

`user $``wget https://github.com/sophgo/edk2-non-osi/raw/refs/heads/devel-sg2042/Silicon/Sophgo/SG2042/Boot/fip.bin`
Next we'll copy over the files that we decided are necessary from the local repos where we built them.

`user $````
cp /path/to/zsbl.git/build/arch/riscv/boot/dts/mango-milkv-pioneer.dtb ./
```
`user $````
cp /path/to/zsbl.git/build/zsbl.bin ./
```
`user $````
cp /path/to/opensbi.git/build/platform/generic/firmware/fw_dynamic.bin ./
```
`user $````
cp /path/to/edk2.git/Build/SRA1-20/RELEASE_GCC5/FV/SRA1-20.fd ./
```
`user $````
cp /path/to/pre-built/riscv64_Image ./
```
`user $````
cp /path/to/pre-built/initrd.img ./
```
`user $``/path/to/gen_spi_flash \`
mango-milkv-pioneer.dtb mango-milkv-pioneer.dtb 0x601000 0x020000000 \
		fw_dynamic.bin fw_dynamic.bin 0x660000 0x00000000 \
		riscv64_Image riscv64_Image 0x6b0000 0x02000000 \
		SRA1-20.fd SRA1-20.fd 0x02000000 0x02000000 \
		zsbl.bin zsbl.bin 0x2a00000 0x40000000 \
		initrd.img initrd.img 0x2b00000 0x30000000

Create a conf.ini:

**`conf.ini`**

```
[sophgo-config]
[devicetree]
name = mango-milkv-pioneer.dtb
[kernel]
name = riscv64_Image
[ramfs]
name = initrd.img
[eof]
```
Make a backup of the existing flash contents:

`user $````
dd if=/dev/mtd0 of=mtd0-backup.bin
```
`user $``dd if=/dev/mtd1 of=mtd1-backup.bin`
And finally, `flashcp` the new stuff to the SPI flash:

`user $````
flashcp conf.ini /dev/mtd0
```
`user $``flashcp spi_flash.bin /dev/mtd1`
The time has come: remove your SD card, reboot, and pray.

### Miscellaneous references

- [Pioneer boot process (forum thread)](https://community.milkv.io/t/pioneer-boot-process/1488)
- [How to build bsp (official how-to)](https://github.com/sophgo/sophgo-doc/blob/main/SG2042/HowTo/How%20to%20build%20SG2042%20bsp.rst)
- [Source code for generating fip.bin? (Github issue)](https://github.com/sophgo/bootloader-riscv/issues/74)

## Solved problems

### The boot process periodically hangs

The stock boot process occasionally hangs, although at slightly different places. This happens with both the LinuxBoot flow on the SPI flash, and with the EDK2/GRUB flow on an SD card with SG2042.fd obtained from a Fedora image. A few examples of these are included below.

This is "solved" because it stopped happening after I rebuilt the bootloader components and switched to U-Boot on an SD card as my primary boot flow. On an SD card, U-Boot seems like the obvious choice for a bootloader anyway. Since the hangs aren't predictable, I can't say for sure if the LinuxBoot hangs are gone now that I've replaced most of the bootloader components on the flash.

#### LinuxBoot flow

An example of LinuxBoot hanging when booting from the SPI flash:

\[    8.339299\] usb 1-2.3.2: new low-speed USB device number 6 using xhci\_hcd
\[    8.571905\] usb 1-2.3.2: New USB device found, idVendor=0c45, idProduct=22bc, bcdDevice= 1.00
\[    8.586162\] usb 1-2.3.2: New USB device strings: Mfr=1, Product=2, SerialNumber=0
\[    8.599408\] usb 1-2.3.2: Product: Majestouch Convertible3 TKL
\[    8.610911\] usb 1-2.3.2: Manufacturer:      
\[    8.638602\] input:       Majestouch Convertible3 TKL as /devices/platform/4c00000000.pcie/pci1
\[    8.729299\] hid-generic 0003:0C45:22BC.0002: input: USB HID v1.00 Keyboard \[      Majestouch 0
\[    8.753479\] input:       Majestouch Convertible3 TKL Consumer Control as /devices/platform/4c2
\[    8.849360\] input:       Majestouch Convertible3 TKL System Control as /devices/platform/4c003
\[    8.877345\] hid-generic 0003:0C45:22BC.0003: input: USB HID v1.00 Device \[      Majestouch Co1
\[    9.849941\] u-root init: error creating mount -t "efivarfs" -o  "efivarfs" "/sys/firmware/efiy
\[   10.026558\] ISOFS: Unable to identify CD-ROM format.
\<HANG>

#### EDK2/GRUB flow

Two examples of EDK2/GRUB hanging. The first is more obviously a problem...

```
Emulated images:
            x64 Image 0xBFB4A000-0xBFB61FFF (Entry 0xBFB4EA34)
            x64 Image 0xBFAF1000-0xBFB49FFF (Entry 0xBFB1965C)
            x64 Image 0xBFA98000-0xBFAF0FFF (Entry 0xBFAC065C)
Wrapped EFI_EVENTs:
            x64 ImageBase 0xBFAF1000 Event BF2F6C18 Fn BFB19A60 Context 0
            x64 ImageBase 0xBFA98000 Event BF244D18 Fn BFAC0A60 Context 0
!!!! RISCV64 Exception Type - 0000000000000003(EXCEPT_RISCV_BREAKPOINT) !!!!
     t0 = 0x00000000000000003        t1 = 0x000000000BFB95976
     t2 = 0x0000000000000002C        t3 = 0x000000000BFC272A8
     t4 = 0x00000000000000000        t5 = 0x00000000000000002
     t6 = 0x00000000000000002        s0 = 0x000000000047FF5C8
     s1 = 0x00000000000000013        s2 = 0x00000000000000000
     s3 = 0x00000000000000000        s4 = 0x00000000000000000
     s5 = 0x00000000000000000        s6 = 0x00000000000000002
     s7 = 0x00000000000000040        s8 = 0x00000000000004000
     s9 = 0x0000000000004EAC0       s10 = 0x00000000000000000
    s11 = 0x00000000000000000        a0 = 0x00000000000000000
     a1 = 0x0000000000000005A        a2 = 0x00000000000000000
     a3 = 0x0000000000000000A        a4 = 0x00000000000000001
     a5 = 0x00000000000000000        a6 = 0x000000000000000FF
     a7 = 0x00000000000000000      zero = 0x00000000000000000
     ra = 0x000000000BFE100D4        sp = 0x000000000BFC2F220
     gp = 0x00000000000000000        tp = 0x0000000000014E000
   sepc = 0x000000000BFB95A32   sstatus = 0x08000000200006100
  stval = 0x00000000000000000
<HANG>
```
whereas in the second, the kernel simply stops loading:

```
  Booting `Gentoo (Sophgo 6.1.80+ nomod)'
loader/efi/linux.c:102:linux: UEFI stub kernel:.80  40.64MiB  100%  1.24GiB/s ]
loader/efi/linux.c:103:linux: PE/COFF header @ 00000040
loader/efi/linux.c:132:linux: LoadFile2 initrd loading enabled
loader/efi/linux.c:515:linux: kernel file size: 98123776
loader/efi/linux.c:517:linux: kernel numpages: 23956
                            [ kernel-sophgo-6.1.80  37.70MiB  92%  13.08GiB/s ]
<HANG>
```
### Box stops powering on

Unplug the power cord (yes, the physical power cord), wait a few minutes, and try again.

### Nothing happens when booting from an SD card

Ensure that you have all of the right files in all of the right places, and that there are no typos in riscv64/conf.ini. The first partition must be FAT32. When in doubt, remove riscv64/conf.ini from the SD card to see if that was the problem. You should be able to see the SD card being initialized over a serial console, and as an extreme measure, you can try wiping its partition table and rebooting.

Even though the card (obviously) needs to be initialized to read riscv64/conf.ini off of its first partition, the boot process hangs (and displays nothing about the SD card) if your riscv64/conf.ini or one of the files it needs are messed up.

### Flaky console keyboard

After rebooting into Gentoo (using the default LinuxBoot flow), the keyboard is flaky and starts dropping keys and/or sending keys that were not pressed. This happens with the stock Fedora kernel as well as the latest git kernel. Both `showkey` and `evtest` confirm that (only) the correct keystrokes *are* being sent... but something goes wrong and they don't make it to the screen.

The solution? Well, it seems to be a problem with the default firmware stored in flash. If you boot from an [SD card with GRUB on it](https://wiki.gentoo.org#GRUB_Example), the problem disappears. Though it is far more difficult, the problem also disappears if you overwrite the firmware in the SPI flash with newer stuff.

### No hardware clock

First, did you buy a CR-1220 battery and put it in the socket? The board doesn't come with one. Okay.

After the default boot flow, your clock will be all wrong, because there won't be a device corresponding to the hardware clock (/dev/rtcN). By all means it looks like there should be one; the device tree contains e.g.

**`mango-milkv-pioneer.dts`**

...but there just isn't. Even after putting a CR-1220 battery in the socket on the motherboard, nothing.

Well, this turns out to be another problem with the default (flash) firmware or device tree, just like the flaky console keyboard. If you boot from GRUB on an SD card, the clock device will appear. In fact, two of them will appear, which creates a new problem: if you are using an initramfs, sometimes you wind up with the wrong one as /dev/rtc0, and Gentoo (the `hwclock` service) still won't know what to do with it. Compare

**`/var/log/dmesg (initramfs)`**

with

**`/var/log/dmesg (no initramfs)`**

If you really want to use an initramfs, you can pass `-f /dev/rtc1` to `hwclock` in /etc/conf.d/hwclock. The systemd equivalent is left as an exercise.

### No sound

If you still have no sound even after building the right modules into the kernel, maybe you have weird device numbering and need to tell ALSA about it. For example,

`user $````
aplay -l
```
\*\*\*\* List of PLAYBACK Hardware Devices \*\*\*\*
card 0: HDMI \[HDA ATI HDMI\], device 3: HDMI 0 \[HDMI 0 \*\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0

The *only* device I have is `device 3`? ALSA is not expecting this. To tell ALSA to use my (only) device, I had to create the following:

**`/etc/asound.conf`**

Afterwards, I also had to run `alsamixer` from [media-sound/alsa-utils](https://packages.gentoo.org/packages/media-sound/alsa-utils) to unmute the `S/PDIF` source.

### Clock skew detected!

Even with the RTC clock working and set to the correct time, OpenRC likes to tell me that clock skew is detected every time I reboot:

\* WARNING: clock skew detected!
 \* Restoring Mixer Levels ...
 \[ ok \]

dmesg tells me that the system clock is being set during boot:

**`/var/log/dmesg`**

But several of the files under /run/openrc that are created very early are being created with the wrong time (system clock appears set close to the epoch). The system time then stays wrong (epoch plus a few seconds) until the udev-trigger service runs, and, I am guessing, the default udev rule that calls `hwclock` finally fixes the time.

I don't *really* know what's happening here, but it looks like the console output is out of order. I added `printk` statements to the kernel, and I can see that e.g. hwclock and ntpd are starting in the middle of the kernel output. That can't be possible if the console output is in order, and *that* means that I can't really be sure when the system clock was set.

The current theory is that the console output is ordered incorrectly, and that the RTC clock is probed much later than we want it to be. In any case, based on that assumption and the fact that the `ds1307` is on the `i2c0` in arch/riscv/boot/dts/sophgo/mango-milkv-pioneer.dts, I've fixed it by compiling both the RTC and I2C drivers into the kernel:

- `CONFIG_RTC_DS1307=y`
- `CONFIG_I2C_DESIGNWARE_PLATFORM=y`



### SD card stops working

Sometimes, after a crash, the machine will stop booting, with nothing being sent to the serial console. This turns out to be an issue with the SD card that can only be diagnosed via the [mainboard console](https://wiki.gentoo.org#Mainboard_console):

NOTICE:  Booting Trusted Firmware
NOTICE:  BL1: v2.7(release):83c4f9e3e
NOTICE:  BL1: Built : 10:39:27, Jul 12 2022
INFO:    BL1: RAM 0x7010002000 - 0x7010011000
NOTICE:  SD initializing 100000000Hz
NOTICE:  sdhci read data timeout
ASSERT: lib/fatfs/diskio.c:57

I've had luck sticking the SD card in a Windows machine to run chkdsk on the FAT partition, and then putting it back. (There aren't any errors on the disk, so who knows why it works.)

The last time this happened, chkdsk didn't fix it, and I wound up replacing my SD card with this one for $10: [Micro Center 32GB microSDHC Card Class 10 Flash Memory Card with Adapter](https://www.microcenter.com/product/658457/micro-center-32gb-microsdhc-card-class-10-flash-memory-card-with-adapter). That fixed the problem straight away.

### GPU Lockups

Every few days, the radeon GPU locks up, and goes into a reset loop:

**`/var/log/dmesg`**

This turned out to be a webkit-gtk problem, and not a hardware/kernel issue. Newer versions of webkit-gtk no longer build on risc-v, "solving" this. (Firefox ESR should work instead.)

## Unsolved problems

### USB keyboard/mouse stop working

I thought I had this fixed, but I don't. Sometimes after rebooting, the keyboard and/or mouse will be dead. Usually unplugging them and plugging them back in again solves the issue. This is what dmesg shows:

`user $````
dmesg -T | grep -i USB
```
\[Mon Oct 14 09:03:31 2024\] \[    T1\] \_\_power\_supply\_register: Expected proper parent device for 'test\_usb'
\[Mon Oct 14 09:03:34 2024\] \[  T859\] usbcore: registered new interface driver usbfs
\[Mon Oct 14 09:03:34 2024\] \[  T859\] usbcore: registered new interface driver hub
\[Mon Oct 14 09:03:34 2024\] \[  T859\] usbcore: registered new device driver usb
\[Mon Oct 14 09:03:34 2024\] \[  T850\] xhci\_hcd 0002:c4:00.0: new USB bus registered, assigned bus number 1
\[Mon Oct 14 09:03:34 2024\] \[  T850\] xhci\_hcd 0002:c4:00.0: new USB bus registered, assigned bus number 2
\[Mon Oct 14 09:03:34 2024\] \[  T850\] xhci\_hcd 0002:c4:00.0: Host supports USB 3.1 Enhanced SuperSpeed
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb1: New USB device found, idVendor=1d6b, idProduct=0002, bcdDevice= 6.01
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb1: New USB device strings: Mfr=3, Product=2, SerialNumber=1
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb1: Product: xHCI Host Controller
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb1: Manufacturer: Linux 6.1.80+ xhci-hcd
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb1: SerialNumber: 0002:c4:00.0
\[Mon Oct 14 09:03:34 2024\] \[  T850\] hub 1-0:1.0: USB hub found
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb2: We don't know the algorithms for LPM for this host, disabling LPM.
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb2: New USB device found, idVendor=1d6b, idProduct=0003, bcdDevice= 6.01
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb2: New USB device strings: Mfr=3, Product=2, SerialNumber=1
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb2: Product: xHCI Host Controller
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb2: Manufacturer: Linux 6.1.80+ xhci-hcd
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb2: SerialNumber: 0002:c4:00.0
\[Mon Oct 14 09:03:34 2024\] \[  T850\] hub 2-0:1.0: USB hub found
\[Mon Oct 14 09:03:34 2024\] \[  T850\] xhci\_hcd 0002:c6:00.0: new USB bus registered, assigned bus number 3
\[Mon Oct 14 09:03:34 2024\] \[  T850\] xhci\_hcd 0002:c6:00.0: new USB bus registered, assigned bus number 4
\[Mon Oct 14 09:03:34 2024\] \[  T850\] xhci\_hcd 0002:c6:00.0: Host supports USB 3.0 SuperSpeed
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb3: New USB device found, idVendor=1d6b, idProduct=0002, bcdDevice= 6.01
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb3: New USB device strings: Mfr=3, Product=2, SerialNumber=1
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb3: Product: xHCI Host Controller
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb3: Manufacturer: Linux 6.1.80+ xhci-hcd
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb3: SerialNumber: 0002:c6:00.0
\[Mon Oct 14 09:03:34 2024\] \[  T850\] hub 3-0:1.0: USB hub found
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb4: New USB device found, idVendor=1d6b, idProduct=0003, bcdDevice= 6.01
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb4: New USB device strings: Mfr=3, Product=2, SerialNumber=1
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb4: Product: xHCI Host Controller
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb4: Manufacturer: Linux 6.1.80+ xhci-hcd
\[Mon Oct 14 09:03:34 2024\] \[  T850\] usb usb4: SerialNumber: 0002:c6:00.0
\[Mon Oct 14 09:03:34 2024\] \[  T850\] hub 4-0:1.0: USB hub found
\[Mon Oct 14 09:03:34 2024\] \[  T273\] usb 1-1: new high-speed USB device number 2 using xhci\_hcd
\[Mon Oct 14 09:03:34 2024\] \[  T981\] usb 3-1: new high-speed USB device number 2 using xhci\_hcd
\[Mon Oct 14 09:03:34 2024\] \[  T273\] usb 1-1: device descriptor read/64, error -71
\[Mon Oct 14 09:03:34 2024\] \[  T981\] usb 3-1: New USB device found, idVendor=2109, idProduct=3431, bcdDevice= 4.20
\[Mon Oct 14 09:03:34 2024\] \[  T981\] usb 3-1: New USB device strings: Mfr=0, Product=1, SerialNumber=0
\[Mon Oct 14 09:03:34 2024\] \[  T981\] usb 3-1: Product: USB2.0 Hub
\[Mon Oct 14 09:03:34 2024\] \[  T981\] hub 3-1:1.0: USB hub found
\[Mon Oct 14 09:03:34 2024\] \[  T273\] usb 1-1: device descriptor read/64, error -71
\[Mon Oct 14 09:03:35 2024\] \[  T273\] usb 1-1: new high-speed USB device number 3 using xhci\_hcd
\[Mon Oct 14 09:03:35 2024\] \[  T273\] usb 1-1: device descriptor read/64, error -71
\[Mon Oct 14 09:03:35 2024\] \[  T273\] usb 1-1: device descriptor read/64, error -71
\[Mon Oct 14 09:03:35 2024\] \[  T273\] usb usb1-port1: attempt power cycle
\[Mon Oct 14 09:03:36 2024\] \[ T1047\] usb 2-1: new SuperSpeed USB device number 2 using xhci\_hcd
\[Mon Oct 14 09:03:36 2024\] \[ T1047\] usb 2-1: device descriptor read/8, error -71
\[Mon Oct 14 09:03:36 2024\] \[ T1047\] usb 2-1: new SuperSpeed USB device number 2 using xhci\_hcd
\[Mon Oct 14 09:03:36 2024\] \[ T1047\] usb 2-1: device descriptor read/8, error -71
\[Mon Oct 14 09:03:36 2024\] \[ T1047\] usb 2-1: new SuperSpeed USB device number 3 using xhci\_hcd
\[Mon Oct 14 09:03:36 2024\] \[ T1047\] usb 2-1: device descriptor read/8, error -71
\[Mon Oct 14 09:03:36 2024\] \[ T1047\] usb 2-1: new SuperSpeed USB device number 3 using xhci\_hcd
\[Mon Oct 14 09:03:36 2024\] \[ T1047\] usb 2-1: device descriptor read/8, error -71
\[Mon Oct 14 09:03:36 2024\] \[ T1047\] usb usb2-port1: attempt power cycle
\[Mon Oct 14 09:03:37 2024\] \[  T273\] usb 1-1: new high-speed USB device number 4 using xhci\_hcd
\[Mon Oct 14 09:03:37 2024\] \[  T273\] usb 1-1: device descriptor read/8, error -71
\[Mon Oct 14 09:03:37 2024\] \[  T273\] usb 1-1: device descriptor read/8, error -71
\[Mon Oct 14 09:03:37 2024\] \[  T273\] usb 1-1: new high-speed USB device number 5 using xhci\_hcd
\[Mon Oct 14 09:03:37 2024\] \[  T273\] usb 1-1: device descriptor read/8, error -71
\[Mon Oct 14 09:03:38 2024\] \[  T273\] usb 1-1: device descriptor read/8, error -71
\[Mon Oct 14 09:03:38 2024\] \[  T273\] usb usb1-port1: unable to enumerate USB device
\[Mon Oct 14 09:03:38 2024\] \[ T1047\] usb 2-1: new SuperSpeed USB device number 4 using xhci\_hcd
\[Mon Oct 14 09:03:38 2024\] \[ T1047\] usb 2-1: device descriptor read/8, error -71
\[Mon Oct 14 09:03:38 2024\] \[ T1047\] usb 2-1: new SuperSpeed USB device number 4 using xhci\_hcd
\[Mon Oct 14 09:03:38 2024\] \[ T1047\] usb 2-1: device descriptor read/8, error -71
\[Mon Oct 14 09:03:38 2024\] \[ T1047\] usb 2-1: new SuperSpeed USB device number 5 using xhci\_hcd
\[Mon Oct 14 09:03:38 2024\] \[ T1047\] usb 2-1: device descriptor read/8, error -71
\[Mon Oct 14 09:03:38 2024\] \[ T1047\] usb 2-1: new SuperSpeed USB device number 5 using xhci\_hcd
\[Mon Oct 14 09:03:39 2024\] \[ T1047\] usb 2-1: device descriptor read/8, error -71
\[Mon Oct 14 09:03:39 2024\] \[ T1047\] usb usb2-port1: unable to enumerate USB device
\[Mon Oct 14 09:03:39 2024\] \[  T273\] usb 1-2: new high-speed USB device number 6 using xhci\_hcd
\[Mon Oct 14 09:03:39 2024\] \[  T273\] usb 1-2: device descriptor read/64, error -71
\[Mon Oct 14 09:03:39 2024\] \[  T273\] usb 1-2: device descriptor read/64, error -71
\[Mon Oct 14 09:03:40 2024\] \[  T273\] usb 1-2: new high-speed USB device number 7 using xhci\_hcd
\[Mon Oct 14 09:03:40 2024\] \[  T273\] usb 1-2: device descriptor read/64, error -71
\[Mon Oct 14 09:03:40 2024\] \[  T273\] usb 1-2: device descriptor read/64, error -71
\[Mon Oct 14 09:03:40 2024\] \[  T273\] usb usb1-port2: attempt power cycle
\[Mon Oct 14 09:03:40 2024\] \[ T1047\] usb 2-2: new SuperSpeed USB device number 6 using xhci\_hcd
\[Mon Oct 14 09:03:40 2024\] \[ T1047\] usb 2-2: device descriptor read/8, error -71
\[Mon Oct 14 09:03:40 2024\] \[ T1047\] usb 2-2: new SuperSpeed USB device number 6 using xhci\_hcd
\[Mon Oct 14 09:03:40 2024\] \[ T1047\] usb 2-2: device descriptor read/8, error -71
\[Mon Oct 14 09:03:41 2024\] \[ T1047\] usb 2-2: new SuperSpeed USB device number 7 using xhci\_hcd
\[Mon Oct 14 09:03:41 2024\] \[ T1047\] usb 2-2: device descriptor read/8, error -71
\[Mon Oct 14 09:03:41 2024\] \[ T1047\] usb 2-2: new SuperSpeed USB device number 7 using xhci\_hcd
\[Mon Oct 14 09:03:41 2024\] \[ T1047\] usb 2-2: device descriptor read/8, error -71
\[Mon Oct 14 09:03:41 2024\] \[ T1047\] usb usb2-port2: attempt power cycle
\[Mon Oct 14 09:03:42 2024\] \[  T273\] usb 1-2: new high-speed USB device number 8 using xhci\_hcd
\[Mon Oct 14 09:03:42 2024\] \[  T273\] usb 1-2: device descriptor read/8, error -71
\[Mon Oct 14 09:03:42 2024\] \[  T273\] usb 1-2: device descriptor read/8, error -71
\[Mon Oct 14 09:03:42 2024\] \[  T273\] usb 1-2: new high-speed USB device number 9 using xhci\_hcd
\[Mon Oct 14 09:03:42 2024\] \[  T273\] usb 1-2: device descriptor read/8, error -71
\[Mon Oct 14 09:03:42 2024\] \[  T273\] usb 1-2: device descriptor read/8, error -71
\[Mon Oct 14 09:03:43 2024\] \[  T273\] usb usb1-port2: unable to enumerate USB device
\[Mon Oct 14 09:03:43 2024\] \[ T1047\] usb 2-2: new SuperSpeed USB device number 8 using xhci\_hcd
\[Mon Oct 14 09:03:43 2024\] \[ T1047\] usb 2-2: device descriptor read/8, error -71
\[Mon Oct 14 09:03:43 2024\] \[ T1047\] usb 2-2: new SuperSpeed USB device number 8 using xhci\_hcd
\[Mon Oct 14 09:03:43 2024\] \[ T1047\] usb 2-2: device descriptor read/8, error -71
\[Mon Oct 14 09:03:43 2024\] \[ T1047\] usb 2-2: new SuperSpeed USB device number 9 using xhci\_hcd
\[Mon Oct 14 09:03:43 2024\] \[ T1047\] usb 2-2: device descriptor read/8, error -71
\[Mon Oct 14 09:03:43 2024\] \[ T1047\] usb 2-2: new SuperSpeed USB device number 9 using xhci\_hcd
\[Mon Oct 14 09:03:43 2024\] \[ T1047\] usb 2-2: device descriptor read/8, error -71
\[Mon Oct 14 09:03:44 2024\] \[ T1047\] usb usb2-port2: unable to enumerate USB device

### NVMe is flaky

The boot often hangs while waiting for the NVMe. It usually succeeds after a while, but it holds up the boot process:

**`/var/log/dmesg`**

and occasionally, it does fail, and the boot is stalled indefinitely:

**`/var/log/dmesg`**

The NVMe just seems generally flaky, even after booting:

**`/var/log/dmesg`**
