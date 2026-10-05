<!-- source: https://wiki.gentoo.org/wiki/PipeWire/troubleshooting | group: Gentoo Wiki (Main) | wiki-title: PipeWire/troubleshooting -->
---
title: PipeWire/troubleshooting
url: https://wiki.gentoo.org/wiki/PipeWire/troubleshooting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-27"
fingerprint: be9c81324d6089d1
license: CC BY-SA 4.0
---

# PipeWire/troubleshooting

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## PipeWire troubleshooting

Refer also to the [the "Troubleshooting" section of the WirePlumber page](https://wiki.gentoo.org/wiki/WirePlumber#Troubleshooting).

If PipeWire is not detecting audio input/output devices even though the `pipewire` and `wireplumber` services are running, this might be because ACL support is missing. Make sure the [acl](https://packages.gentoo.org/useflags/acl) [USE flag is not disabled in](https://wiki.gentoo.org/wiki/USE_flag) [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf), and re-emerge if a change was made.

If [seatd](https://wiki.gentoo.org/wiki/Seatd) is being used, make sure the user is in the `audio` group.

On Intel Tiger Lake-H HD systems, the [sys-firmware/sof-firmware](https://packages.gentoo.org/packages/sys-firmware/sof-firmware) package might need to be installed.

In chrome://flags, set "WebRTC PipeWire support" to "Enabled".

If clients report being unable to lock memory, raise the value of RLIMIT\_MEMLOCK:

**`/etc/security/limits.d/50-custom.conf`**

Crackling and stuttering might be reduced or eliminated by setting `default.clock.min-quantum` appropriately in pipewire.conf<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>:

**`/etc/pipewire/pipewire.conf`**

If a sound - such as a 'waterdrop' sound - is played after certain actions, e.g. after pressing `Tab` in [XTerm](https://wiki.gentoo.org/wiki/XTerm), this might be the result of the PipeWire configuration enabling the `x11-bell` PipeWire module (e.g. because the [X](https://packages.gentoo.org/useflags/X) [USE flag is enabled).](https://wiki.gentoo.org/wiki/USE_flag)

To disable this, either disable the [X](https://packages.gentoo.org/useflags/X) [USE flag and re-emerge, or modify the relevant configuration file, e.g.:](https://wiki.gentoo.org/wiki/USE_flag)

**`~/.config/pipewire/pipewire.conf.d/disable-bell.conf`**

**Disable x11-bell module**

If there's no sound after resuming from sleep, it might be that the monitor was still in a 'power save' mode when the system resumed. Ensure that the monitor is not in such a mode before resuming.
