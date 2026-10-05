<!-- source: https://wiki.gentoo.org/wiki/WirePlumber | group: Gentoo Wiki (Main) | wiki-title: WirePlumber -->
---
title: WirePlumber
url: https://wiki.gentoo.org/wiki/WirePlumber
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-24"
fingerprint: beacb35b4ca27bc6
license: CC BY-SA 4.0
---

# WirePlumber

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**WirePlumber** is a modular session / policy manager for [PipeWire](https://wiki.gentoo.org/wiki/PipeWire), enabling functionality such as saving and restoring session state. More generally, WirePlumber allows one to:

- enable devices;
- configure devices;
- configure client (e.g. application) access control;
- configure PipeWire nodes;
- manage links between PipeWire nodes;
- manage metadata.



Device Drivers  --->
  \<\*> Sound card support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SOUND\</code> to find this item.  --->
    \<\*> Advanced Linux Sound Architecture [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\</code> to find this item.  --->
      -\*-  Sound Proc FS Support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_PROC\_FS\</code> to find this item.
      \[\*\]    Verbose procfs contents [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_SND\_VERBOSE\_PROCFS\</code> to find this item.


| [+doc](https://packages.gentoo.org/useflags/+doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [elogind](https://packages.gentoo.org/useflags/elogind) | Enable session tracking via sys-auth/elogind | 
| [system-service](https://packages.gentoo.org/useflags/system-service) | Install systemd unit files for running as a system service. Not recommended. | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

`root #``emerge --ask media-video/wireplumber`
Generally, WirePlumber should work "out of the box", without any need for manual configuration.

If manual configuration is required, a sample WirePlumber configuration file is available at /usr/share/wireplumber/wireplumber.conf; this file can be copied to $XDG\_CONFIG\_HOME/wireplumber/ and modified as required. The $XDG\_CONFIG\_HOME/wireplumber/ directory might need to be created manually.

WirePlumber state is stored in $XDG\_STATE\_HOME/wireplumber/<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

`user $``systemctl --user enable --now wireplumber.service`
To enable the WirePlumber service:

`user $``rc-update --user add wireplumber default`
To start the service without enabling it:

`user $``rc-service --user wireplumber start`
WirePlumber is controlled by [wpctl(1)](https://man.archlinux.org/man/wpctl.1.en)[. As of WirePlumber 0.5.15:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``wpctl -h`
Usage:
  wpctl \[OPTION…\] COMMAND \[COMMAND\_OPTIONS\] - WirePlumber Control CLI
Commands:
  status 
  list \[audio|video\] \[devices|sinks|sources\]
  get-volume ID
  inspect ID
  set-default ID
  set-volume ID VOL\[%\]\[-/+\]
  set-mute ID 1|0|toggle
  set-profile ID INDEX
  set-route ID INDEX
  clear-default \[ID\]
  settings \[KEY\] \[VAL\]
  set-log-level \[ID\] LEVEL
  reset 
Help Options:
  -h, --help       Show help options
Pass -h after a command to see command-specific options

The special identifiers `@DEFAULT_SINK@`, `@DEFAULT_AUDIO_SINK@`, `@DEFAULT_SOURCE@`, `@DEFAULT_AUDIO_SOURCE@`, and `@DEFAULT_VIDEO_SOURCE@` can be used when an ID is required; refer to the [wpctl(1)](https://man.archlinux.org/man/wpctl.1.en) [man page for details.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

Get information about current PipeWire state, including node IDs and currently active devices:

`user $``wpctl status`
Get list of available audio sinks:

`user $``wpctl list audio sinks`
Get volume of current default sink:

`user $``wpctl get-volume @DEFAULT_SINK@`
Set volume of current default sink to 90%:

`user $``wpctl set-volume @DEFAULT_SINK@ 90%`
Decrease volume of current default audio sink by 5%:

`user $``wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%-`
Increase volume of current default audio sink by 5%:

`user $``wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%+`
Toggle muting of current default audio sink:

`user $``wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle`
List details of current WirePlumber settings:

`user $``wpctl settings`
List only the `Id` and `Value` fields for current settings:

`user $``wpctl settings | sed -n '/Id:/p;/Value:/p'`
Save current settings:

`user $``wpctl settings --save`
Refer to the [WirePlumber/troubleshooting](https://wiki.gentoo.org/wiki/WirePlumber/troubleshooting) page.

- [PipeWire](https://wiki.gentoo.org/wiki/PipeWire) — low-latency, graph-based, processing engine and server, for interfacing with audio and video devices.

- [WirePlumber, the PipeWire session manager](https://www.collabora.com/news-and-blog/blog/2020/05/07/wireplumber-the-pipewire-session-manager/) - General introduction to WirePlumber
- [WirePlumber](https://wiki.archlinux.org/title/WirePlumber) - ArchWiki page
- ["Well-known features"](https://pipewire.pages.freedesktop.org/wireplumber/daemon/configuration/features.html) - List of some of the WirePlumber features that can be enabled or disabled.
- ["Well-known settings"](https://pipewire.pages.freedesktop.org/wireplumber/daemon/configuration/settings.html) - List of WirePlumber settings that can be configured statically or dynamically.
