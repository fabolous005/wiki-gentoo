<!-- source: https://wiki.gentoo.org/wiki/Dell_XPS_13_2-in-1_(7390) | group: Gentoo Wiki (Main) | wiki-title: Dell XPS 13 2-in-1 (7390) -->
---
title: Dell XPS 13 2-in-1 (7390)
url: https://wiki.gentoo.org/wiki/Dell_XPS_13_2-in-1_(7390)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: "9798fc57d3e791af"
license: CC BY-SA 4.0
---

# Dell XPS 13 2-in-1 (7390)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The Dell XPS 13 2-in-1 (7390, 2019) is an Intel Ice Lake (10th gen Core i processor, 10nm lithography) convertible, ultra portable laptop.

Hardware highlights

- Up to Core i7-1065G7 CPU, 15W TDP
  - Contains some hardware mitigations against [Speculative Execution vulnerabilities](https://wiki.gentoo.org/wiki/Project:Security/Vulnerabilities/Meltdown_and_Spectre)
- 11th Gen GPU, up to Iris Plus graphics
- Up to 32GB of LPDDR4 RAM, clocked at 3773MHz
- Up to 1TB NVMe SSD storage
- Up to a 4K (3840x2400), 16:10 aspect ratio touchscreen with Wacom Active ES 2.0 Pen support
- UEFI boot only

### Hardware not working

- Fingerprint reader (Goodix)
- Camera (Intel IPU4)

## Target setup

My personal system boots Gentoo as the only operating system (the included Windows 10 installation was wiped). Using Secure Boot and systemd-boot and full-disk (except ESP) LUKS encryption, with the sway window manager. My keyboard layout is United Kingdom (82-key).

## make.conf

**`/etc/portage/make.conf`**

## Wifi

Model is Intel (Killer Wireless) AX1650i and requires firmware from [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware). Because of the firmware, build the driver as a module, or include the firmware in your kernel build.

- Wifi firmware: `iwlwifi-Qu-c0-hr-b0-59.ucode`
- Bluetooth firmware `intel/ibt-19-32-4.sfi`, `intel/ibt-19-32-4.ddc`

## Display

**`/etc/portage/make.conf`**

You'll also need firmware from [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) in order to use the GPU. It is not recommended to build the GPU firmware into the kernel.

Firmware: `i915/icl_dmc_ver1_09.bin`

See also: [Intel](https://wiki.gentoo.org/wiki/Intel)

### Touchscreen

In kernels before 5.5, the Linux kernel may crash when loading the `intel-lpss-pci` module.
When booting live media, pass the kernel parameter `modprobe.blacklist=intel-lpss-pci`. This will disable the touchscreen and pen input.

The patches to fix this are included in kernel versions after 5.5.0-rc1 and should be released into the stable 5.5.

If you're using sources from git, you can fetch the patches from the [mfd tree](https://git.kernel.org/pub/scm/linux/kernel/git/lee/mfd.git/), [ib-mfd-doc-sparc-libdevres-5.5 branch](https://git.kernel.org/pub/scm/linux/kernel/git/lee/mfd.git/log/?h=ib-mfd-doc-sparc-libdevres-5.5)

### Wacom

Need to enable CONFIG\_I2C\_HID\_ACPI, [CONFIG\_HID\_WACOM](https://wiki.gentoo.org/wiki/Wacom) and CONFIG\_PINCTRL\_ICELAKE to get a Wacom Active ES 2.0 pen to work.

**Kernel 5.12 and later**

Before kernel 5.12, CONFIG\_I2C\_HID\_ACPI was called CONFIG\_I2C\_HID.

**Kernel 5.11 and before**

If using Xorg, set `INPUT_DEVICES="wacom"` in `make.conf`. In wayland, the pen is handled by libinput.

#### Bluetooth Buttons

Some active pens support a bluetooth to the laptop to start up applications. Currently, the bluetooth stack only reports that the device has connected and disconnected, not the type of button press (single, double, and long presses). See [xf86-input-wacom "Bluetooth Button"](https://github.com/linuxwacom/xf86-input-wacom/wiki/Bluetooth-Button) for more info.

### (Almost) Flicker-free boot

[sys-boot/plymouth](https://packages.gentoo.org/packages/sys-boot/plymouth) supports "flicker-free" boot, but with a couple of noticeable flickers. Set the plymouth theme to `bgrt`. The `i915.fastboot=1` parameter might not required.

`root #``emerge --ask plymouth``root #``plymouth-set-default-theme bgrt`
### Intel GVT

iGVT-g is not supported by the i915 driver for Ice Lake. Other modes (iGVT-d, iGVT-s) have not been tested.

The developers (Intel) previously indicated that Ice Lake would be supported in the future, however no work enabling Ice Lake has been seen.

- Feature request: [https://github.com/intel/gvt-linux/issues/126](https://github.com/intel/gvt-linux/issues/126)
- Enabling source code: [https://github.com/intel/gvt-linux/blob/gvt-staging/drivers/gpu/drm/i915/intel\_gvt.c#L44-L60](https://github.com/intel/gvt-linux/blob/gvt-staging/drivers/gpu/drm/i915/intel_gvt.c#L44-L60)

#### Experimental GuC/HuC

Since iGVT is disabled, it may be possible to get the performance/power-saving benefits of GuC/HuC since it would otherwise be incompatible with iGVT.

Set `i915.enable_guc=-1` in your kernel command line to enable GuC/HuC firmware loading.

Firmware files: `i915/icl_guc_33.0.0.bin`, `i915/icl_huc_ver9.0.0.bin`

#### VA-API (Video Acceleration API)

Set `USE="vaapi"` in `make.conf` and run:

`root #``emerge --ask media-libs/libva-intel-media-driver`
See [bug #692244](https://bugs.gentoo.org/show_bug.cgi?id=692244) if libva-intel-media-driver has compile errors.

Then set vaapi to use the intel-media-driver, instead of the regular intel-driver:

**`/etc/env.d/11libva-media-driver`**

Applications like mpv, vlc and ffmpeg can then use hardware acceleration via vaapi. Check that the GPU is being used for video with [x11-apps/igt-gpu-tools](https://packages.gentoo.org/packages/x11-apps/igt-gpu-tools)' `intel_gpu_top`. Firefox supports vaapi from version 78 on Wayland<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> and on X11 since release 80<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>.

See [Wikipedia article](https://en.wikipedia.org/wiki/Video_Acceleration_API) for more information.

### Disable keyboard in tablet/tent/stand mode

Other sensors are available (e.g. Ambient Light Sensor, Gyroscope, Inclination and Rotation) in the system, they are not required to disable the keyboard in tablet mode.

### Automatic rotation

You can use [rot8](https://github.com/efernau/rot8) or [iio-sensor-proxy](https://github.com/hadess/iio-sensor-proxy) to automatically rotate your screen. Note, these apps will rotate your screen even in laptop mode, when you may not want it to.

#### Finger and Pen rotates with screen

Rotating the screen won't automatically rotate the touch and pen input.

Add this to your sway config to fix touch (the pen/"tablet\_tool" has issues when rotated, see below):

**`~/.config/sway/config`**

#### Issue: Wacom Pen jitter

If the screen has been rotated 90 degrees, you cannot draw straight vertical or horizontal lines with a pen. There isn't a workaround yet.

Sway issue: [https://github.com/swaywm/sway/issues/4613](https://github.com/swaywm/sway/issues/4613)

## Secure Boot

[app-crypt/efitools](https://packages.gentoo.org/packages/app-crypt/efitools)' `efi-updatevar` can update the 4 secure boot variables when in setup mode. When in user mode, I copied the public databases onto the ESP and loaded them through the UEFI Setup GUI. Tip: `systemctl reboot --firmware-setup` reboots directly to setup without having to press the Setup key on boot (F2).

### Signed UEFI executable by dracut

Once your Secure Boot keys are in `/etc/efikeys`, pop the following into a dracut configuration file:

**`/etc/dracut.conf.d/uefi-secureboot.conf`**

Then when updating your kernel, run (replace where necessary):

`root #``make modules_install``root #``dracut --uefi --kernel-image arch/x86/boot/bzImage --kernel-cmdline "$cmdline" -f /path/to/output.efi $kernel_version`
## TPM2

### OpenSSL engine

Emerge [app-crypt/tpm2-tss-engine](https://packages.gentoo.org/packages/app-crypt/tpm2-tss-engine) to use the TPM as an OpenSSL engine. To use the tpm resource manager as an unprivileged user, add the user to the `tss` group.

`user $``openssl engine -t -c tpm2tss``user $``tpm2tss-genkey myTpmKey.keyfile``user $``openssl req -engine tpm2tss -keyform engine -new -x509 -nodes -sha256 -days 365 -key myTpmKey.keyfile -out self-signed-cert.crt`
#### Optional configuration

The engine should not require further configuration, but if you need to you can add the following configuration to `/etc/ssl/openssl.cnf` and modify to taste.

**`/etc/ssl/openssl.cnf`**

### TOTP measured boot

`root #``emerge --ask app-crypt/tpm2-totp`
Add the tpm2-totp module to dracut's config files:

**`/etc/dracut.conf.d/tpm2-totp.conf`**

By default tpm2-totp uses PCR banks 0, 2, 4 and 6. PCR4 contains a hash of the kernel binary you are running, which will change on every kernel update. If you update often, this will cause the TOTP to break each time. Check your PCR banks with [app-crypt/tpm2-tools](https://packages.gentoo.org/packages/app-crypt/tpm2-tools)' `tpm2_pcrlist`. Adjust which PCR banks are used with the `tpm2-totp generate -p` flag.

| PCR bank | (Suspected) Use | 
|---|---|
| 0 | UEFI code | 
| 1 | UEFI configuration | 
| 2 | Option ROMs | 
| 3 | Option ROM configuration | 
| 4 | Initial program loader code | 
| 5 | Initial program loader code configuration | 
| 6 | State transitions and wake events (Dell Configuration Information 1/2) | 
| 7 | Platform manufacturer specific measurements (combined hash of the PK+KEK+db(+dbx) databases) | 
| 8 | kernel command line | 
| 9 | None (zeros) | 
| 10 | Unknown use (in concert with [IMA](https://wiki.gentoo.org/wiki/Integrity_Measurement_Architecture)) | 

## Sleep

By default, closing the lid will activate S2 sleep (s2idle), which uses a lot of battery while asleep (\~20% overnight).

You can try S3/deep sleep by writing `deep` into `/sys/power/mem_sleep` and try suspending again, however there may be issues with the internal display or the touchscreen failing to work on resume. Try disabling BIOS Setup -> POST Behaviour -> "Sign of Life" -> "Early Dell Logo Display".

Discover which sleep modes are available to the system:

**`/sys/power/disk`**

**`/sys/power/mem_sleep`**

**`/sys/power/state`**

Check the defaults in [/etc/systemd/sleep.conf](https://www.freedesktop.org/software/systemd/man/systemd-sleep.conf.html).

For instance, it's really easy to press the power button when holding in tablet mode, so I selected 's2idle'/'freeze' for the power key (I repurposed hybrid sleep for it). I then use S3 suspend-to-RAM (deep sleep) on lid close, after which a short timeout to suspend-to-disk (S4), to do a full resume through the bootloader:

**`/etc/systemd/sleep.conf`**

**`/etc/systemd/logind.conf`**

**`/etc/kernel/cmdline`**

Note, when the power button is pressed and the system goes into s2idle, it still responds to the lid closing and going into suspend.

### LUKS/resume timeout

If you use hibernation with encrypted swap (with dracut/systemd, but maybe other systems too), the systemd-hibernate-resume service will time out after 90 seconds, and entering your password after that will cause hibernation data loss. Upcoming systemd versions will contain a patch that increases the timeout to infinity, but it'd rather turn the laptop off to save battery power.

Add this to your kernel command line to power off the laptop at the 90 second mark:

**`/etc/kernel/cmdline`**

See also: [Bug report](https://bugzilla.redhat.com/show_bug.cgi?id=1705522) & [systemd-hibernate-resume pull request](https://github.com/systemd/systemd/pull/14241)

## USB-C PD charging

Plugging in a USB-C PD battery may not do the right thing (i.e. the laptop charges the battery) without the Type-C UCSI driver.

The driver exposes `/sys/class/typec` which configures the ports to pull power, and it can be further configured as a power source or sink.

## Dell SMBios tools

`root #``emerge libsmbios[python]`
### Fan Thermal Profile

`root #``smbios-thermal-ctl -i`
Libsmbios version : 2.4.2
smbios-thermal-ctl version : 2.4.2
 Print all the Available Thermal Information of your system:
-------------------------------------------------------------------
Supported Thermal Modes:
	 Balanced
	 Cool Bottom
	 Quiet
	 Performance
Supported Active Acoustic Controller (AAC) modes:
Supported AAC Configuration type:
	Global (AAC enable/disable applies to all supported USTT modes)

`root #``smbios-thermal-ctl -g`
Helper function to Get current Thermal Mode settings
 Print Current Status of Thermal Information:
-------------------------------------------------------------------
Current Thermal Modes:
	 Quiet
Current Active Acoustic Controller (AAC) Mode:
	 AAC mode Disabled
Current Active Acoustic Controller (AAC) Mode:
	Global (AAC enable/disable applies to all supported USTT modes)
Current Fan Failure Mode:

It's pretty self-explanatory except that the 'Cool Bottom' (with a space) mode should be passed as 'cool-bottom' (with a dash):

`root #``smbios-thermal-ctl --set-thermal-mode=cool-bottom`
Helper function to Set Thermal Mode
Thermal Information Set successfully to: cool-bottom

### Filtered calls

You may get some errors while running the smbios tools:

`root #``smbios-token-ctl````
ERROR: Could not execute SMI.
Common problems are:
    -- 'SMM Mitigations' is enabled in BIOS setup.
        Run kernel 4.15 or later with
        dell-smbios-wmi enabled.
         or
        Disable 'SMM mitigations' in BIOS setup.
    -- Insufficient permissions to perform operation.
       Try running as a more privileged account.
          Linux  : run as 'root' user
          Windows: run as 'administrator' user
    -- dell-smbios-wmi driver not loaded
       Try loading the dell-smbios-wmi driver
          Linux : modprobe dell-smbios-wmi
    -- Call filtered by Linux kernel
       Some functionality is natively supported
       by the Linux kernel
    -- dcdbas device driver not loaded.
       Try loading the dcdbas driver
          Linux  : modprobe dcdbas
```
Check dmesg for 'Invalid call'. These calls are filtered by the dell-smbios driver. For some tools, like `smbios-token-ctl`, the filtered calls can be blacklisted so that the tool can continue running (skipping those filtered calls).

## intel-undervolt

All but the 'System Agent' can be undervolted. I experienced GPU glitches below -40mV, and the T<sub>junction</sub> offset goes down to -40°C (100°C-40°C=60°C).

`root #``emerge --ask sys-power/intel-undervolt`
**`/etc/intel-undervolt.conf`**

The intel-undervolt.service will re-apply itself after suspend and hibernation, but not on suspend-then-hibernate. Add this to your service override:

**`/etc/systemd/system/intel-undervolt.service.d/override.conf`**

## BIOS Recovery 3

For more information, see Dell's [How to Recover the BIOS](https://www.dell.com/support/kbdoc/en-uk/000132453/how-to-recover-the-bios-on-a-dell-computer-or-tablet) page.

### Setup

To configure BIOS Recovery 3 with Auto-Recovery, download the System BIOS file from the [Dell support website](https://www.dell.com/support/home/en-uk/product-support/product/xps-13-7390-2-in-1-laptop/drivers). Once downloaded, check the integrity with the provided checksums; the .exe files are the exact same as the .rcv files.

`user $``md5sum XPS_13_7390_2-in-1_*.{exe,rcv}``user $``sha1sum XPS_13_7390_2-in-1_*.{exe,rcv}``user $``sha256sum XPS_13_7390_2-in-1_*.{exe,rcv}`
Once verified, copy the file to the root of your EFI System Partition (ESP), renaming it "BIOS\_IMG.rcv" (FAT32 is case-insensitive, although the documentation insists on this case).

`root #``cp XPS_13_7390_2-in-1_*.exe /efi/BIOS_IMG.rcv || cp XPS_13_7390_2-in-1_*.rcv /efi/BIOS_IMG.rcv`
### Force a recovery

To verify the correct setup, shut down the laptop and unplug any power supply cables. Once off, press and hold the Ctrl and Esc keys and plug in the vendor provided power supply (60W). The laptop should turn on automatically.

If the recovery preparations have been completed correctly, you'll be shown the recovery prompt screen. It is possible to exit and reboot if there is no need to recover the BIOS.

If the recovery file has not been found, the battery status bar should provide a diagnostic code, as listed in the [XPS 13 7390 2-in-1 Service Manual - System diagnostic lights](https://www.dell.com/support/manuals/en-uk/xps-13-7390-2-in-1-laptop/xps-13-7390-2-in-1-servicemanual/system-diagnostic-lights?guid=guid-30f93304-3cfe-483d-9075-ee2d60217562&lang=en-us).
