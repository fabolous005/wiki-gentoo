<!-- source: https://wiki.gentoo.org/wiki/Warning_about_insecure_obsolete_network_hardware | group: Gentoo Wiki (Main) | wiki-title: Warning about insecure obsolete network hardware -->
---
title: Warning about insecure obsolete network hardware
url: https://wiki.gentoo.org/wiki/Warning_about_insecure_obsolete_network_hardware
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-30"
fingerprint: "37cf7f12c02b91c3"
license: CC BY-SA 4.0
---

# Warning about insecure obsolete network hardware

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Most network hardware requires regular updates to patch security issues, just as operating systems do. Sadly, some network hardware is under-supported by manufacturers, or supported for a limited amount of time. If such outdated network hardware is used without appropriate updates, security issues arise.

Because hardware often sees continued use even without the required security updates, users should be aware of potential security issues, and be mindful of using potentially obsolete and insecure network hardware.

Over the years, whole security protocols have been found to be insecure and have been deprecated, however these insecure implementations can sometimes remain in operation on old hardware. Users should be aware of what to avoid.

## Insecure protocols

### WEP

A major design flaw was discovered in WEP in 2001, it should no longer be used. Devices from this era often do not have updates available, and sadly have to be be discarded.

[net-wireless/wpa\_supplicant](https://packages.gentoo.org/packages/net-wireless/wpa_supplicant) has a [wep](https://packages.gentoo.org/useflags/wep) [flag, though this should be avoided if possible.](https://wiki.gentoo.org/wiki/USE_flag)

### TKIP (WPA or "WPA+WPA2")

WPA was a temporary measure to avoid WEP's security issues, flaws have since been discovered in WPA (TKIP), and it was deprecated in 2012.

Routers should deactivate the insecure "TKIP" or "WPA/WPA2 mixed mode" methods, and prefer for example "WPA2-AES" or "WPA3". Devices which do not allow this, and only support the insecure method, should be replaced, if they cannot be updated.

#### Insecure workaround

Those using [wpa\_supplicant](https://wiki.gentoo.org/wiki/Wpa_supplicant) can activate the following use flags, if they are fully aware of the risks outlined in the rest of this article:

**`/etc/portage/package.use/wpa_supplicant`**

Re-emerge wpa\_supplicant:

`root #``emerge --ask net-wireless/wpa_supplicant`
Portage keeps source tarballs for all currently installed packages in /var/cache/distfiles, which will include wpa\_suppilcant. Thus these flags can be enabled even without a working network connection, and there should be no need to use live media to fix this from a chroot, on a correctly maintained system.

See the previous section for information about the [wep](https://packages.gentoo.org/useflags/wep) [flag, though this should be avoided if possible.](https://wiki.gentoo.org/wiki/USE_flag)

## Updated open firmware

If the manufacturer does not provide updated, secure firmware, for some devices, alternative firmware such as [OpenWrt](https://openwrt.org/) or [OPNsense](https://opnsense.org) may be available. Be aware that installing these solutions can be technically demanding (sometimes extremely so for some devices).

## See also

- [Wi-Fi](https://wiki.gentoo.org/wiki/Wi-Fi) — describes the setup of a [Wi-Fi](https://en.wikipedia.org/wiki/Wi-Fi) (wireless) network device.
- [wpa\_supplicant](https://wiki.gentoo.org/wiki/Wpa_supplicant) — an app for [Wi-Fi](https://wiki.gentoo.org/wiki/Wi-Fi) authentication
