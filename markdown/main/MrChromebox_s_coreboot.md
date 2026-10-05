<!-- source: https://wiki.gentoo.org/wiki/MrChromebox%27s_coreboot | group: Gentoo Wiki (Main) | wiki-title: MrChromebox's coreboot -->
---
title: MrChromebox's coreboot
url: https://wiki.gentoo.org/wiki/MrChromebox%27s_coreboot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-27"
fingerprint: "1a0cbcb581f3a3ec"
license: CC BY-SA 4.0
---

# MrChromebox's coreboot

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **MrChromebox's coreboot** is a [coreboot](https://wiki.gentoo.org/wiki/Coreboot) fork maintained by one of the coreboot leaders <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, Matt DeVillier (MrChromebox) <sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>. The fork targets Chrome OS devices based on x86 architecture. ARM is not supported <sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>.

## Firmware Utility Script

MrChromebox provides a script that automatically detects the motherboard, downloads the compiled coreboot as a blob, injects the [VPD](https://en.wikipedia.org/wiki/Vital_Product_Data) into that blob, disables write protection, and flashes it to the device. The script can be executed as follows:

`root #``./firmware-util.sh`
The current state of the script running on various live images:

| Distribution | Version | Date | Status | Notes | 
|---|---|---|---|---|
| Gentoo LiveGUI USB Image | 20240707T170407Z | 2024-07-14 |  | The kernel is not permissive enough. | 
| Linux Mint Cinnamon Edition Live Image | 21.3 | 2024-07-14 |  | Works out of the box. | 
| Ubuntu Desktop Live Image | 24.04 LTS | 2024-07-14 |  | Requires curl to be installed. | 

## Manual installation

### Device list

This is a list of Chrome OS devices on which manual installation has been successfully performed. Feel free to add to the list!

| Device | Motherboard | coreboot version | Owner(s) | Status | Notes | 
|---|---|---|---|---|---|
| [Lenovo IdeaPad Flex 5 13IML05 Chromebook](https://wiki.gentoo.org/wiki/Lenovo_IdeaPad_Flex_5_13IML05_Chromebook) | akemi | 2405.0 | [Lars Hint](https://wiki.gentoo.org/index.php?title=User:Lars_Hint&action=edit&redlink=1)  |  |  | 

### Linux Mint environment

To achieve a more reproducible environment, the compilation will be performed from a [Linux Mint](https://linuxmint.com) system (version 21.3).

Install crossgcc dev-dependencies:

`root #``apt-get install git g++ zlib1g-dev gnat`
Install coreboot dev-dependencies:

`root #``apt-get install libssl-dev uuid-dev nasm imagemagick`
Install menuconfig dev-dependencies:

`root #``apt-get install libncurses-dev`
Install flashrom dev-dependencies:

`root #``apt-get install meson libpci-dev`
Create a symlink to python:

`root #``ln -s /usr/bin/python3 /usr/bin/python`
### Compilation

Clone the repository:

Select a version (all versions can be seen by executing `git branch --all`):

`user $``git checkout remotes/origin/MrChromebox-2405`
Compile the cross-compiler:

`user $``make crossgcc-i386 CPUS=$(nproc)`
Detect the name of the motherboard:

`root #``dmidecode --string system-product-name`
Starting with version 4.22.0, there is a script in the repository to simplify the build <sup>[\[11\]](https://wiki.gentoo.org#cite_note-11)</sup>:

`user $``./build-uefi.sh <MOTHERBOARD_NAME_IN_LOWER_CASE>`
To see the compiled binary file, run the command:

`user $``ls ../roms/*.rom`
### VPD injection

#### BIOS region extraction

Compile the flashrom:

`user $````
cd flashrom
```
`user $````
git switch --detach v1.5.1
```
`user $````
meson setup builddir
```
`user $````
meson compile -C builddir
```
`user $``cd builddir`
Extract the BIOS region into a file:

##### Intel-based device

`root #``./flashrom -p internal --ifd -i bios -r /tmp/bios.bin`
##### Non-Intel-based device

`root #``./flashrom -p internal -r /tmp/bios.bin`
#### VPD extraction and injection

Compile the cbfstool:

`user $````
cd coreboot
```
`user $````
git switch --detach 24.05
```
`user $``make -C util/cbfstool`
Extract the VPD from the BIOS region extracted earlier:

`user $``./util/cbfstool/cbfstool /tmp/bios.bin read -r RO_VPD -f /tmp/vpd.bin`
Ensure that the VPD is present:

`user $``hexdump -C /tmp/vpd.bin`
Inject the VPD into the firmware file:

`user $``./util/cbfstool/cbfstool <FIRMWARE FILE PATH> write -r RO_VPD -f /tmp/vpd.bin`
### Flashing the firmware

#### Intel-based device

`root #``./flashrom -p internal --ifd -i bios -N -w <FIRMWARE FILE PATH>``root #``echo $?`
#### Non-Intel-based device

`root #``./flashrom -p internal -N -w <FIRMWARE FILE PATH>``root #``echo $?`
### Customization

Find the location of the configuration file:

`user $``find ./configs -name "config.<MOTHERBOARD_NAME_IN_LOWER_CASE>.uefi"`
Copy the configuration file to the root directory of the coreboot repository:

`user $``cp <PATH_TO_CONFIGURATION_FILE> ./.config`
The configuration file has the same structure as the kernel configuration file and can be edited via menuconfig:

`user $``make menuconfig`
After editing the configuration, the original configuration file needs to be replaced:

`user $``make savedefconfig``user $``mv ./defconfig <PATH_TO_CONFIGURATION_FILE>`
After replacement, it is necessary to (re)build the firmware by (re)running the build-uefi.sh script.

#### Custom BIOS name

The name is defined through `CONFIG_LOCALVERSION`, which can be changed in the build-uefi.sh file to, for example, this:

#### Decreasing the boot timeout

To decrease the menu prompt display time from two seconds to one second:

```
Payload  --->
  [ ] Don't add a payload
  (1)  Set the timeout for boot menu prompt 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_EDK2_BOOT_TIMEOUT</code> to find this item.
#### Larry the Cow as the splash screen image

Download the image to the root directory of the repository:

`user $``wget wiki.gentoo.org/images/3/3d/Larry_color.svg`
And set the path via menuconfig:

```
Payload  --->
  [ ] Don't add a payload
  (Larry_color.svg) edk2 Bootsplash path and filename 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_EDK2_BOOTSPLASH_FILE</code> to find this item.
## See also

- [Coreboot](https://wiki.gentoo.org/wiki/Coreboot) — a free and open-source hardware initializing firmware which supports multiple boot ROM payloads.
- [Chromebook](https://wiki.gentoo.org/wiki/Chromebook) — installing Gentoo on a Chromebook
