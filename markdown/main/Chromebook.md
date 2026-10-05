<!-- source: https://wiki.gentoo.org/wiki/Chromebook | group: Gentoo Wiki (Main) | wiki-title: Chromebook -->
---
title: Chromebook
url: https://wiki.gentoo.org/wiki/Chromebook
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-16"
fingerprint: "2dd79459e0dfa138"
license: CC BY-SA 4.0
---

# Chromebook

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide details the generic part of installing Gentoo on a Chromebook.

## Installation

### ARM

Check [ASUS Chromebook C201/Installing\_Gentoo](https://wiki.gentoo.org/wiki/ASUS_Chromebook_C201/Installing_Gentoo).

### x86

There are two main ways to run Gentoo Linux on a Chromebook - relying on stock firmware or relying on custom firmware. By default, all Chromebooks use Chrome OS (stock) firmware, which varies by Chromebook generation and processor architecture. Depending on the stock firmware, a Chromebook may be able to boot another operating system without the need to flash a custom firmware. <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> The stock firmware should only be used in case if there is a plan to use Chrome OS or if there is no custom firmware for the Chromebook. The reason is that all Chrome OS devices have an **expiration date**. After the expiration date, the stock firmware will not receive **security** updates or **bug** fixes.

#### Installation relying on custom firmware

Flashing requires disabling hardware write protection and enabling developer mode.

##### Disabling hardware write protection

Depending on the model, there are five possible ways to disable hardware write protection:

- screw
- jumper
- switch
- CR50 (battery removal or SuzyQable)
- CR50 (SuzyQable)

To find the appropriate method for a certain Chromebook model, [MrChromebox's table](https://mrchromebox.tech/#devices) can be used, or in the case of SuzyQable, [another table](https://www.chromium.org/chromium-os/developer-information-for-chrome-os-devices). It is important to keep in mind that some models have incorrect CR50 implementation, for them SuzyQable will not work, only battery removal (or soldering). [\[3\]](https://wiki.gentoo.org#cite_note-3)

##### Enabling Developer Mode

To enable developer mode, follow instructions provided [here](https://mrchromebox.tech/#devmode).

##### Flashing the custom firmware

See [MrChromebox's Firmware Utility Script](https://wiki.gentoo.org/wiki/MrChromebox%27s_coreboot#Firmware_Utility_Script).

After flashing the UEFI firmware, installing Gentoo is rather straightforward: boot on a liveUSB and follow the [Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) as if installing on a regular machine.

## Keyboard

### Xorg

#### Layout

The layout is supported by Xorg:

**`/etc/X11/xorg.conf.d/10-keyboard.conf`**

```
Section "InputClass"
  Identifier "Keyboard"
  MatchProduct "AT Translated Set 2 keyboard"
  MatchIsKeyboard "on"
  Option "XkbModel" "chromebook"
EndSection
```
The `MatchProduct` section might not fit the hardware, to check the correct name, use:

`user $``grep "Using input driver" /var/log/Xorg.0.log`
(...)
\[617682.560\] (II) Using input driver 'libinput' for 'AT Translated Set 2 keyboard'
(...)

#### Missing keys

Since some keys are missing, they are emulated with `Right Alt`:

- `Right Alt`+`Backspace` = `Delete`
- `Right Alt`+`Left` = `Home`
- `Right Alt`+`Right` = `End`
- `Right Alt`+`Up` = `PgUp`
- `Right Alt`+`Down` = `PgDn`
- `Right Alt`+`Search` = `Caps Lock`
- `Right Alt`+`F1 to F10` = `F1 to F10`

#### Extra keys

`Search` is treated as a super key (Super\_L)

#### Multimedia keys

The multimedia keys should works as expected, except:

- `fullscreen` (in the F4 spot) will be treated as `F11`
- `next tab/window` (in the F5 spot) will be treated as `F5`

When used with `Ctrl` or `Alt` or `Shift`, these keys will behave as `F1` to `F10`.

Example: `Alt`+`Refresh` = `Alt`+`F3`

## Troubleshooting

### Sound does not work

Check  [this article](https://wiki.gentoo.org/wiki/Chromebook/Sound_configuration).

### How to reboot

Since there is typically no `Delete` key, it's usually impossible to use `Ctrl`+`Alt`+`Delete`. There is also no `Sys` key, making it impossible to use the Magic Keys.

Fortunately the firmware has a few keyboard shortcuts available:

- `Power` for several seconds = power off
- `Refresh`+`Power` = instant reboot

### Stuck at the warning screen

Try using `Esc`+`Refresh`+`Power` to force a firmware reset

If that is not enough, follow the [official procedure](https://support.google.com/chromebook/answer/1080595)

### Unexpected reboot when coming out of suspend to RAM

This can be caused by a missing TPM (Trusted Platform Module) driver in the kernel, see in Drivers → Character Devices

## See also

- [MrChromebox's coreboot](https://wiki.gentoo.org/wiki/MrChromebox%27s_coreboot) — a [coreboot](https://wiki.gentoo.org/wiki/Coreboot) fork maintained by one of the coreboot leaders

## External resources

- [MrChromebox - Firmwares and firmware flashing tool](https://mrchromebox.tech/)
- [Chrultrabook - A useful resource for converting your Chromebook to linux](https://docs.chrultrabook.com/)
- [GalliumOS - Linux distribution for Chromebooks](https://wiki.galliumos.org/Welcome_to_the_GalliumOS_Wiki)
- [Arch Linux Wiki - Similar page on the Arch Linux Wiki](https://wiki.archlinux.org/index.php/Chrome_OS_devices)
