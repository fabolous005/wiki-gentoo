<!-- source: https://wiki.gentoo.org/wiki/Logitech_G533 | group: Gentoo Wiki (Main) | wiki-title: Logitech G533 -->
---
title: Logitech G533
url: https://wiki.gentoo.org/wiki/Logitech_G533
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-01-21"
fingerprint: b922341252ef099c
license: CC BY-SA 4.0
---

# Logitech G533

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a wiki entry about using [Logitech G533](https://www.logitechg.com/en-roeu/products/gaming-audio/g533-wireless-gaming-headset.html) wireless headset on Gentoo.

## Requirements

### Software

pavucontrol is recommended.

**dts** USE is recommended for virtual sound functionality.

### Kernel

## Troubleshooting

### USB Receiver

USB receiver has green LED under "G" logo we assume that USB receiver is plugged in system's USB port.

#### LED is blinking

1. There is no drivers for USB receiver detected.
  - Check requirements above.
  - If you confirmed that software + kernel configuration is present share your experience in talk page.
2. USB Receiver is not paired.
  - There are two "holes" on USB receiver and headset. Take a paper clip and insert it in USB receiver and headset.
    - Holes are located:
    - USB receiver = above LED and under "G" logo.
    - Headset = Left reproducer above "G" logo. Note that it is hidden under plastic piece of skeleton, NO disassembly is required.
    - Press and hold until both USB receiver and headset are blinking rapidly which indicated pairing mode.

#### LED is ON

1. Connection with headset is successful and there is no issue from headset and USB receiver.
  - If the headset is still not working check \`pavucontrol\`. We can eliminate issue with USB receiver and headset from troubleshooting.

#### LED is OFF

1. Corrupted firmware.
  - Logitech provides software called "Logitech Gaming Software" which support only windows. If you install it using wine or VirtualBox on windows there is "FWupdate" folder in it's root directory which has firmware recovery software for all supported hardware saved in .exe to recover your firmware. Warning: if using VirtualBox you need to remount the USB connection for USB receiver and headset multiple times. Installation will freeze until remount it completed.
2. Hardware damage.
  - USB receiver - New USB receiver costs around 50$ on websites like eBay, etc.. If you have experience with micro-solder you can probably fix it yourself.

**Photos of stripped USB receiver:**

![G533 USB Reciever front.jpg](https://wiki.gentoo.org/images/c/cd/G533_USB_Reciever_front.jpg) 

![G533 USB Reciever back.jpg](https://wiki.gentoo.org/images/9/96/G533_USB_Reciever_back.jpg)


**Recommended approach:** Best practice is to attack from USB side to split open the plastic casing which has the least possibility of damaging the receiver and it's casing. Note photos above to avoid any components during this attack.

Receivers are made by Avnera you can probably get same receiver from other headset (which are widely-used across multiple vendors) and reverse-engineer firmware from mentioned software above to reprogram it.
