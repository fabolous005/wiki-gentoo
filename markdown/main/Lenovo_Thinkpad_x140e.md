<!-- source: https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_x140e | group: Gentoo Wiki (Main) | wiki-title: Lenovo Thinkpad x140e -->
---
title: Lenovo Thinkpad x140e
url: https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_x140e
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: d29337c84105a0de
license: CC BY-SA 4.0
---

# Lenovo Thinkpad x140e

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

The **Lenovo ThinkPad x140e** is an 11.6 inch laptop made by Lenovo. Like other members of the ThinkPad line it is semi-rugged and business needs take priority over design aesthetics. As such it is thicker than many modern laptops but has a lot of I/O connectivity options to show for it. Additionally, the laptop is quite easy to service and both the RAM and the fixed disk are user upgradable.

It makes quite a good "grab and go" laptop for casual and office work. It's chief drawbacks are its overly sensitive touchpad and buggy WiFi chipset.

## Installation

Installation of Gentoo is straightforward with both [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) and [systemd](https://wiki.gentoo.org/wiki/Systemd). The only gotcha is the WiFi module. The WiFi module that ships with the ThinkPad x140e is part of the B43XX. In order to get the WiFi working the following package must be installed:

`root #``emerge --ask sys-firmware/b43-firmware`
## Troubleshooting

### I installed Gentoo but my WiFi Module Isn't Working

This is a common issue. Most likely the B43XX firmware needs to be installed.

`root #``emerge --ask sys-firmware/b43-firmware`
### My WiFi suffers frequent drop outs

If you suffer frequent dropouts with commands such as [rsync](https://wiki.gentoo.org/wiki/Rsync), [scp](https://wiki.gentoo.org/wiki/Scp) and similar it's most likely a limitation of the WiFi chipset. The B43XX is known to be very buggy at the chipset level. In more typical usage, such as web browsing, it's easy not to notice these issues. With usage more typical of a Gentoo user WiFi stability issues are much harder to miss.

### I Installed a Better WiFi Card and Now my System Won't Boot

The ThinkPad x140e has a user hostile anti-feature in its firmware: it will not boot with non-Lenovo WiFi modules installed. This would not be so bad except for the issues inherent in the WiFi chipset. A error looks like this:

1802: Unauthorized network card is plugged in - Power off and remove the miniPCI network card.

Absent a third-party firmware patch to disable this "feature" there is no way around this vendor imposed limitation.
