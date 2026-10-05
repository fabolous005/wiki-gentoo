<!-- source: https://wiki.gentoo.org/wiki/System_time | group: Gentoo Wiki (Main) | wiki-title: System time -->
---
title: System time
url: https://wiki.gentoo.org/wiki/System_time
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-17"
fingerprint: c7893c7aa346ab66
license: CC BY-SA 4.0
---

# System time

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The **system time**, backed by the system clock, is used in Unix systems to keep track of time. It can be set by an onboard hardware clock or by an external time server.

The [system clock](https://wiki.gentoo.org/wiki/System_time#System_clock), provided by the kernel, is the amount of time that has elapsed since the 1 January 1970 00:00:00 UTC [epoch](<https://en.wikipedia.org/wiki/Epoch_(computing)>). This is called [Unix time](https://en.wikipedia.org/wiki/Unix_time).

The [hardware clock](https://wiki.gentoo.org/wiki/System_time#Hardware_clock) (also known as real-time clock or RTC) is typically a component on the mainboard, which keeps time while the computer is switched off. The accuracy of most RTCs varies, it is typical for them to gain or lose several seconds per day. While adequate for most purposes, systems connected to the Internet usually use Network Time Protocol which achieves accuracy to within a few milliseconds.

In this case, the system will have the slightly incorrect time from the RTC only briefly, with NTP correcting it shortly after networking is established. The RTC can then be corrected, and in this manner will only drift significantly if the machine is left switched off for prolonged periods.

Some systems such as the Raspberry Pi (models up to 4) lack an RTC altogether. As such, these rely on NTP to start up with the correct time automatically.

System time is always set to local time as determined by the user's [time zone](https://wiki.gentoo.org/wiki/System_time#Time_zone), taking [daylight saving time](https://en.wikipedia.org/wiki/Daylight_saving_time) (DST) into account.

The hardware clock can represent either local time or [Coordinated Universal Time](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) (UTC). UTC is preferred because it is independent of time zones and DST. Some operating systems (most notably Windows) use local time by default, which can cause conflicts on dual-boot systems. Windows can be reconfigured to use UTC, allowing it to stay in sync with Linux; see section [Dual booting with Windows](https://wiki.gentoo.org/wiki/System_time#Dual_booting_with_Windows).

In order to keep time properly, select the proper time zone so the system knows where it is located.

#### OpenRC

See [Timezone (AMD64 Handbook)](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Timezone).

#### systemd

[systemd](https://wiki.gentoo.org/wiki/Systemd) comes with the timedatectl command to manage the time zone:

To check the current zone:

`user $``timedatectl`
To list available zones:

`user $``timedatectl list-timezones`
To change the time zone, e.g. for Germany:

`root #``timedatectl set-timezone Europe/Berlin`
This [environment variable](https://wiki.gentoo.org/wiki/Localization/Guide#Environment_variables_for_locales) defines formatting of dates and times. For more details see [The GNU C Library](https://www.gnu.org/software/libc/manual/html_node/Locale-Categories.html#index-LC_005fTIME)

Typically the system clock time is set up by the hardware clock on boot. Alternatively it is possible to manually set the system clock or use a network time server.

The date command can be used to manage the system clock time:

To check the current software clock time:

`user $``date`
To set the system clock, e.g. 12:34, May 6, 2016:

`root #``date 050612342016`
See the [Chrony](https://wiki.gentoo.org/wiki/Chrony) or [Network Time Protocol](https://wiki.gentoo.org/wiki/Network_Time_Protocol) articles for information concerning the use of time servers.

#### systemd

systemd comes with the timedatectl command to manage the system clock:

To check the current software clock:

`user $``timedatectl`
To set the system clock:

`root #``timedatectl set-time "2012-12-17 12:30:59"`
To have a hardware clock, the following kernel options must be activated:

**Necessary kernel options for a hardware clock**

At runtime, to check the current hardware clock:

`root #``hwclock --show`
To set the hardware clock to the current system clock:

`root #``hwclock --systohc`
Typically the hardware clock is used to setup the system clock on boot. This can be done by the kernel itself or by a boot service (init script). Also on shutdown the kernel or a service can write the software clock to the hardware clock. This aids the system in having the correct time on boot.

On a sufficiently modern kernel (3.9 or newer), Linux can be configured to handle setting the system time automatically. To do so, enable the **Set system time from RTC on startup and resume** (`CONFIG_RTC_HCTOSYS`) and **Set the RTC time based on NTP synchronization** (`CONFIG_RTC_SYSTOHC`) kernel options:

**Letting the kernel sync the system clock**

The **Set the RTC time based on NTP synchronization** kernel option is currently supported by [chrony](https://wiki.gentoo.org/wiki/Chrony)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, [NTP](https://wiki.gentoo.org/wiki/NTP) and [OpenNTPD](https://wiki.gentoo.org/wiki/OpenNTPD) since version 5.9p1<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>.

To check the hardware time was updated, install [net-misc/adjtimex](https://packages.gentoo.org/packages/net-misc/adjtimex) and run:

`root #``adjtimex --print | grep status`
Bit 6 of the reported number should be unset (0). More information in hwclock man pages (search '11 minute mode').

#### OpenRC

When using OpenRC the hwclock init script can set the system clock on boot and sync system time to the hardware clock on shutdown. The service is enabled by default and should be *disabled* in favor of the above mentioned in-kernel method. The hwclock script should *not* be run when using the kernel's RTC.

`root #``rc-update delete hwclock boot`
If however there is a need for using the OpenRC, set both `clock_hctosys` and `clock_systohc` to `YES` in /etc/conf.d/hwclock. By default the service is configured for UTC. To change to local time add `clock="local"`.

**`/etc/conf.d/hwclock`**

**Adding hardware clock sync**

```
clock_hctosys="YES" 
clock_systohc="YES"
# clock="local"
```
Restart the hwclock service and have the hardware clock init script run on system boot:

`root #````
rc-service hwclock restart
```
`root #````
rc-update add hwclock boot
```
#### systemd

[systemd](https://wiki.gentoo.org/wiki/Systemd) can be used to set the system clock on boot. Use timedatectl to manage the hardware clock:

To check the current hardware clock:

`user $``timedatectl | grep "RTC time"`
To set the hardware clock to the current system clock (in UTC):

`root #``timedatectl set-local-rtc 0`
To set the hardware clock to the current system clock (in the local time):

`root #``timedatectl set-local-rtc 1`
## Troubleshooting

Historically, Unix systems have set their RTC to represent UTC. Other operating system, such as Windows, expect the RTC to represent local time by default.

This can lead to a difficulty when dual-booting: the time being correct on one operating system causes it to be incorrect on the other. When either uses NTP to obtain the time, they then 'correct' the RTC, only for the situation to revert when the other does the same, seemingly 'fighting' over the RTC, and resulting in the clock being incorrect by several hours.

To avoid Windows adjusting the hardware clock back to local time, add the following registry entry:

For 64-bit Windows, open regedit then browse to HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation. Create a new QWORD entry called `RealTimeIsUniversal`, then set its value to `1`. Reboot the system. The clock should now be in UTC time.  For 32-bit Windows, follow the 64-bit instructions except use DWORD instead of QWORD.

## See also

- [Network Time Protocol](https://wiki.gentoo.org/wiki/Network_Time_Protocol) — used to synchronize the [system time] with other devices over the network.
- [Chrony](https://wiki.gentoo.org/wiki/Chrony) — a versatile implementation of the [Network Time Protocol](https://wiki.gentoo.org/wiki/Network_Time_Protocol) (NTP).
- [OpenNTPD](https://wiki.gentoo.org/wiki/OpenNTPD) — a lightweight [NTP](https://wiki.gentoo.org/wiki/Network_Time_Protocol) server ported from OpenBSD.

## External resources

- [https://lifehacker.com/5742148/fix-windows-clock-issues-when-dual-booting-with-os-x](https://lifehacker.com/5742148/fix-windows-clock-issues-when-dual-booting-with-os-x) - Dual booting with MS Windows, set RealTimeIsUniversal. Also tested with [Windows 10](https://wiki.gentoo.org/wiki/UEFI_Dual_boot_with_Windows_7/8).
- [http://tldp.org/HOWTO/Clock-2.html](http://tldp.org/HOWTO/Clock-2.html) - The Clock Mini-HOWTO.
