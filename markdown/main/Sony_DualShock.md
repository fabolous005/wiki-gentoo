<!-- source: https://wiki.gentoo.org/wiki/Sony_DualShock | group: Gentoo Wiki (Main) | wiki-title: Sony DualShock -->
---
title: Sony DualShock
url: https://wiki.gentoo.org/wiki/Sony_DualShock
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-22"
fingerprint: "4fa5d70e60a41441"
license: CC BY-SA 4.0
---

# Sony DualShock

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the use of Sony DualShock 3 / [Sixaxis](https://en.wikipedia.org/wiki/Sixaxis), DualShock 4, and [DualSense](https://en.wikipedia.org/wiki/DualShock#DualSense) [PlayStation](https://en.wikipedia.org/wiki/PlayStation) controllers via USB and Bluetooth.

## Prerequisites

This article presumes the system has been configured to use [USB](https://wiki.gentoo.org/wiki/USB) and [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth). If these communication buses have not been configured, please take time to do so now.

## Hardware

The following devices can be queried when connected via USB with the lsusb command (available via the [sys-apps/usbutils](https://packages.gentoo.org/packages/sys-apps/usbutils) package).

| Device | Vendor ID / Product ID | Vendor Name | Product Name | Supported | Kernel Version | Notes | 
|---|---|---|---|---|---|---|
| DualShock 3 / Sixaxis | `054c:0268` | Sony Corp. | Batoh Device / PlayStation 3 Controller |  | >=3.15 <sup>[\[1\]](https://wiki.gentoo.org#cite_note-ds4-support-1)</sup> |  | 
| DualShock 4 | `054c:05c4` | Sony Corp. | DualShock 4 \[CUH-ZCT1x\] |  | >=3.15 <sup>[\[1\]](https://wiki.gentoo.org#cite_note-ds4-support-1)</sup> | New revisions have a modified HID descriptor that require Linux 4.5+ <sup>[\[2\]](https://wiki.gentoo.org#cite_note-ds4-revision-2)</sup>. | 
| DualShock 4 (2nd Gen) | `054c:09cc` | Sony Corp. | DualShock 4 \[CUH-ZCT2x\] |  | >=4.10 <sup>[\[3\]](https://wiki.gentoo.org#cite_note-ds4v2-support-3)</sup> | Since kernel 6.2 hid-playstation is needed (which depends on LED Multicolor Class Support) | 
| DualSense | `054c:0ce6` | Sony Corp. | Wireless Controller |  | >=5.12 <sup>[\[4\]](https://wiki.gentoo.org#cite_note-:0-4)</sup> |  | 

## Installation

### Kernel

The recommended minimum version of Linux to use is 3.15. This release adds full support for the DualShock 4. This release also fixes the DualShock 3 blinking LED issue when connected via Bluetooth<sup>[\[1\]](https://wiki.gentoo.org#cite_note-ds4-support-1)</sup>. New revisions of the DualShock 4 have a modified HID descriptor and require Linux 4.5<sup>[\[2\]](https://wiki.gentoo.org#cite_note-ds4-revision-2)</sup>. The 2nd generation DualShock 4 included with the PlayStation 4 Slim and Pro requires Linux 4.10<sup>[\[3\]](https://wiki.gentoo.org#cite_note-ds4v2-support-3)</sup>. The PlayStation 5 DualSense controller requires Linux 5.12<sup>[\[4\]](https://wiki.gentoo.org#cite_note-:0-4)</sup>.

Enable the following configuration options: *CONFIG\_BT\_HIDP,* `CONFIG_INPUT_JOYDEV`, `CONFIG_INPUT_EVDEV`, `CONFIG_INPUT_UINPUT`, `CONFIG_HID_BATTERY_STRENGTH`, `CONFIG_UHID`, `CONFIG_HIDRAW`, `CONFIG_HID_SONY`, `CONFIG_SONY_FF`, `CONFIG_HID_GENERIC`, `CONFIG_USB_HID`, `NEW_LEDS`, `LEDS_CLASS`:

**Enabling Sony DualShock/DualSense support**

### Xorg

To have the DualShock 4 touchpad behave as a mouse using the kernel driver, the [Xorg](https://wiki.gentoo.org/wiki/Xorg) input driver should be set to `libinput`, and >=[sys-fs/eudev](https://packages.gentoo.org/packages/sys-fs/eudev)-3.2.6 should be used for libinput to recognize the touchpad correctly:

**`/etc/X11/xorg.conf.d/30-dualshock-4-touchpad.conf`**

```
Section "InputClass"
        Identifier   "ds4-touchpad"
        Driver       "libinput"
        MatchProduct "Sony Interactive Entertainment Wireless Controller Touchpad"
        Option       "Mode" "Relative"
EndSection
```
The built-in gyro can be set as a joystick, but users should note that some games will not work properly with this functionality enabled:

**`/etc/X11/xorg.conf.d/30-dualshock-4-gyro.conf`**

```
Section "InputClass"
        Identifier   "ds4-gyro"
        Driver       "joystick"
        MatchProduct "Sony Interactive Entertainment Wireless Controller Motion Sensors"
        Option       "Ignore" "False" # Switch the last value in this line to "True" to disable using DS4's gyro as a joystick.
EndSection
```
### BlueZ

To have the DualSense controller connect using Bluetooth and properly load the drivers, it is necessary to [enable](https://wiki.gentoo.org/wiki/Bluetooth_input_devices) user-space HID support in BlueZ. This can be done by uncommenting the associated line:

**`/etc/bluetooth/input.conf`**

```
# Enable HID protocol handling in userspace input profile
# Defaults to false (HIDP handled in HIDP kernel module)
UserspaceHID=true
```
Be sure to restart the bluetooth service so that the configuration file change is recognized and applied:

OpenRC:

`root #``rc-service bluetooth restart`
systemd:

`root #``systemctl restart bluetooth`
## Usage

### USB

- Connect the DualShock to the system using a USB cable and press the `PlayStation` button on the controller.

- Turn the DualShock off when it's no longer in use by pressing and holding the `PlayStation` button for 10 seconds.

- Press the `PlayStation` button to use the DualShock again.

### Bluetooth

The recommended minimum version of BlueZ to use is 5.14. This release adds full support for the DualShock 4<sup>[\[5\]](https://wiki.gentoo.org#cite_note-bluez-5.14-5)</sup>. This release also incorporates DualShock 3 setup and pairing support which was added in BlueZ 5.12<sup>[\[6\]](https://wiki.gentoo.org#cite_note-bluez-5.12-6)</sup>.

#### DualShock 3

Start bluetoothctl:

`user $``bluetoothctl`
Enable the agent and set it as default:

`[bluetooth]#````
agent on
```
`[bluetooth]#````
default-agent
```
Power on the Bluetooth controller, and set it as discoverable and pairable:

`[bluetooth]#````
power on
```
`[bluetooth]#````
discoverable on
```
`[bluetooth]#````
pairable on
```
Connect the DualShock 3 to the system using a USB cable and press the `PlayStation` button.

Allow the service authorization request:

`[agent]``Authorize service` *service_uuid* (yes/no): yes
Discover the DualShock 3 MAC address:

`[bluetooth]#``devices`
Trust the DualShock 3:

`[bluetooth]#``trust` *device_mac_address*
Disconnect the USB cable from the DualShock 3.

The DualShock 3 is now paired:

`[bluetooth]#``quit`
Turn the DualShock 3 off when it's no longer in use by pressing and holding the `PlayStation` button for 10 seconds.

Press the `PlayStation` button to use the DualShock 3 again.

#### DualShock 4

Start bluetoothctl:

`user $``bluetoothctl`
Enable the agent and set it as default:

`[bluetooth]#````
agent on
```
`[bluetooth]#````
default-agent
```
Power on the Bluetooth controller, and set it as discoverable and pairable:

`[bluetooth]#````
power on
```
`[bluetooth]#````
discoverable on
```
`[bluetooth]#````
pairable on
```
Scan for devices:

`[bluetooth]#``scan on`
Put the DualShock 4 into pairing mode by pressing and holding the `PlayStation` and `Share` buttons until the light bar starts flashing.

Discover the DualShock 4 MAC address:

`[bluetooth]#``devices`
Pair with the DualShock 4:

`[bluetooth]#``pair` *device_mac_address*
Allow the service authorization request:

`[agent]``Authorize service` *service_uuid* (yes/no): yes
Trust the DualShock 4:

`[bluetooth]#``trust` *device_mac_address*
The DualShock 4 is now paired:

`[bluetooth]#``quit`
Turn the DualShock 4 off when it's no longer in use by pressing and holding the `PlayStation` button for 10 seconds, or by disconnecting it from bluetoothctl or any GUI Bluetooth device manager.

Press the `PlayStation` button to use the DualShock 4 again.

### Battery charge level

The DualShock battery charge level can checked with [sysfs](https://wiki.gentoo.org/wiki/Sysfs):

`user $````
cat "/sys/class/power_supply/sony_controller_battery_04:76:6e:9a:98:fc/capacity"
```
25

The battery charge level for the DualShock 3 will reported as either 100%, 75%, 50% or 25% remaining. These values correspond to 3, 2, 1, and 0 bars respectively when checked with the PlayStation 3. The battery charge level for the DualShock 4 will be reported in the range of 100% to 0% remaining, at intervals of 10%.

The battery charge level can also be checked with [sys-power/upower](https://packages.gentoo.org/packages/sys-power/upower). First, list available devices:

`user $````
upower -e
```
/org/freedesktop/UPower/devices/battery\_sony\_controller\_battery\_04o76o6eo9ao98ofc
/org/freedesktop/UPower/devices/DisplayDevice

Next, display the DualShock power information:

`user $````
upower -i /org/freedesktop/UPower/devices/battery_sony_controller_battery_04o76o6eo9ao98ofc
```
```
  native-path:          sony_controller_battery_04:76:6e:9a:98:fc
  power supply:         no
  updated:              Tue 01 Dec 2015 00:00:00 UTC (1 seconds ago)
  has history:          yes
  has statistics:       yes
  battery
    present:             yes
    rechargeable:        yes
    state:               discharging
    warning-level:       none
    energy:              0 Wh
    energy-empty:        0 Wh
    energy-full:         0 Wh
    energy-full-design:  0 Wh
    energy-rate:         0 W
    percentage:          25%
    capacity:            100%
    icon-name:          'battery-full-symbolic'
  History (charge):
    1449283773	25.000	discharging
    1449283773	0.000	unknown
  History (rate):
    1449283773	0.000	unknown
```
## Troubleshooting

### Imitation DualShock 3 controllers

Imitation DualShock 3 controllers are not supported by BlueZ and will *not* work via Bluetooth<sup>[\[7\]](https://wiki.gentoo.org#cite_note-ds3-imitation-7)</sup>. However, Linux 4.15<sup>[\[8\]](https://wiki.gentoo.org#cite_note-ds3-imitation-4.15-8)</sup> and BlueZ 5.48<sup>[\[9\]](https://wiki.gentoo.org#cite_note-bluez-5.48-9)</sup> will have full support for imitation DualShock 3 controllers via USB and Bluetooth.

### Steam doesn't detect Dualshock PS4 over bluetooth

If Steam doesn't detect Dualshock PS4 over bluetooth, the following may help:

`user $``echo -e "# Valve HID devices over bluetooth hidraw\nKERNEL==\"hidraw*\", KERNELS==\"*28DE:*\", MODE=\"0666\"" | sudo tee -a /etc/udev/rules.d/99-steam-controller-perms.rules``user $``echo -e "# Dualshock 4 over bluetooth hidraw\nKERNEL==\"hidraw*\", KERNELS==\"*054C:05C4*\", MODE=\"0666\"" | sudo tee -a /etc/udev/rules.d/99-steam-controller-perms.rules``user $``echo -e "# Dualshock 4 Slim over bluetooth hidraw\nKERNEL==\"hidraw*\", KERNELS==\"*054C:09CC*\", MODE=\"0666\"" | sudo tee -a /etc/udev/rules.d/99-steam-controller-perms.rules``user $``sudo udevadm trigger`
### DualSense does not work in certain games

If the controller works in applications such as inside of Big Picture mode in Steam, but not inside of certain games running Wine or Proton, verify that the `CONFIG_INPUT_UINPUT` kernel symbol has been enabled in the running kernel:

**Enable`CONFIG_INPUT_UINPUT` for user level driver support**

### Dualshock PS4 not working on kernel >= 6.2

If the DS4 controller is not working with hid-sony driver, try the hid-playstation driver. It added support for DS4 controller in Kernel 6.2.

To enable the hid-playstation driver "LED Multicolor Class Support" must be selected.

### When connecting over bluetooth, Dualshock 3 controller asks for a PIN code

`[agent] Enter PIN code:`
Set ClassicBondedOnly=false in input.conf and restart bluez.[\[10\]](https://wiki.gentoo.org#cite_note-ds3-pin-10)

**`/etc/bluetooth/input.conf`**

```
# Limit HID connections to bonded devices
# The HID Profile does not specify that devices must be bonded, however some
# platforms may want to make sure that input connections only come from bonded  
# device connections. Several older mice have been known for not supporting
# pairing/encryption.
# Defaults to true for security.
ClassicBondedOnly=false
```
## See also

- [Steam Controller](https://wiki.gentoo.org/wiki/Steam_Controller) — a game controller developed by [Valve](https://en.wikipedia.org/wiki/Valve_Corporation).

## External resources

## References

1. ↑ <sup>[1.0](https://wiki.gentoo.org#cite_ref-ds4-support_1-0)</sup> <sup>[1.1](https://wiki.gentoo.org#cite_ref-ds4-support_1-1)</sup> <sup>[1.2](https://wiki.gentoo.org#cite_ref-ds4-support_1-2)</sup> Jiri Kosina. [\[GIT\] HID](https://lkml.org/lkml/2014/4/2/264), [LKML](https://lkml.org/), April 2nd, 2014. Retrieved on October 24th, 2014.
2. ↑ <sup>[2.0](https://wiki.gentoo.org#cite_ref-ds4-revision_2-0)</sup> <sup>[2.1](https://wiki.gentoo.org#cite_ref-ds4-revision_2-1)</sup> Frank Praznik. [HID: sony: Remove the size check for the Dualshock 4 HID Descriptor](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git/commit/?h=v4.5&id=b71b5578a84d297954e4812ba0ca2d466e61cf42), [Linux kernel stable tree](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git/), November 19th, 2015. Retrieved on July 8th, 2016.
3. ↑ <sup>[3.0](https://wiki.gentoo.org#cite_ref-ds4v2-support_3-0)</sup> <sup>[3.1](https://wiki.gentoo.org#cite_ref-ds4v2-support_3-1)</sup> Roderick Colenbrander. [HID: sony: Update device ids](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git/commit/?h=v4.10&id=cf1015d65d7c8a5504a4c03afb60fb86bff0f032), [Linux kernel stable tree](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git/), October 10th, 2016. Retrieved on March 21st, 2017.
4. ↑ <sup>[4.0](https://wiki.gentoo.org#cite_ref-:0_4-0)</sup> <sup>[4.1](https://wiki.gentoo.org#cite_ref-:0_4-1)</sup> Bobby Borisov. [Linux Kernel 5.12 Released With Many Essential Addons](https://linuxiac.com/linux-kernel-5-12-released-with-many-essential-additions/), April 26, 2021. Retrieved on November 10, 2021.
5. [↑](https://wiki.gentoo.org#cite_ref-bluez-5.14_5-0) Johan Hedberg. [Release of BlueZ 5.14](http://www.bluez.org/bluez-5-14/), [BlueZ](http://www.bluez.org/), January 21st, 2014. Retrieved on October 24th, 2014.
6. [↑](https://wiki.gentoo.org#cite_ref-bluez-5.12_6-0) Johan Hedberg. [Release of BlueZ 5.12](http://www.bluez.org/release-of-bluez-5-12/), [BlueZ](http://www.bluez.org/), December 10th, 2013. Retrieved on October 24th, 2014.
7. [↑](https://wiki.gentoo.org#cite_ref-ds3-imitation_7-0) DjMadness. [Sixaxis via bluetooth](https://forums.gentoo.org/viewtopic-p-7712574.html#7712574), [Gentoo Forums](https://forums.gentoo.org/), March 4th, 2015. Retrieved on March 12th, 2015.
8. [↑](https://wiki.gentoo.org#cite_ref-ds3-imitation-4.15_8-0) Bastien Nocera. [HID: sony: Fix SHANWAN pad rumbling on USB](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?h=v4.15-rc1&id=492ca83c3d19fba1622164f07cd7b775596a7db2), [Linux kernel stable tree](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git/), November 9th, 2017. Retrieved on December 20th, 2017.
9. [↑](https://wiki.gentoo.org#cite_ref-bluez-5.48_9-0) Bastien Nocera. [plugins/sixaxis: Provide DualShock 3 SDP record while adding new device](https://git.kernel.org/pub/scm/bluetooth/bluez.git/commit/?h=5.48&id=1629c39ededef07988a5403b27331e0e317f1e08), [Bluetooth protocol stack for Linux](https://git.kernel.org/pub/scm/bluetooth/bluez.git/), November 9th, 2017. Retrieved on December 20th, 2017.
10. [↑](https://wiki.gentoo.org#cite_ref-ds3-pin_10-0) [Bug #2045931](https://bugs.launchpad.net/ubuntu/+source/bluez/+bug/2045931), bugs.launchpad.net, December 8th, 2023. Retrieved on January 22th, 2025.
