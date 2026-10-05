<!-- source: https://wiki.gentoo.org/wiki/Wpa_supplicant/Setup_for_dhcpcd_as_network_manager | group: Gentoo Wiki (Main) | wiki-title: Wpa supplicant/Setup for dhcpcd as network manager -->
---
title: Wpa supplicant/Setup for dhcpcd as network manager
url: https://wiki.gentoo.org/wiki/Wpa_supplicant/Setup_for_dhcpcd_as_network_manager
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-22"
fingerprint: be4973193b859beb
license: CC BY-SA 4.0
---

# Wpa supplicant/Setup for dhcpcd as network manager

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

First follow the setup guide for [dhcpcd](https://wiki.gentoo.org/wiki/Network_management_using_DHCPCD#Setup).

Emerge wpa\_supplicant (Version >=2.6-r2 is needed in order to get the [CONFIG\_MATCH\_IFACE](https://forums.gentoo.org/viewtopic-t-1036958-start-4.html) option [added in April 2017](https://gitweb.gentoo.org/repo/gentoo.git/commit/net-wireless/wpa_supplicant?id=423af2686b225f76bb8ae52221690581a56a9625)):

`root #``emerge --ask net-wireless/wpa_supplicant`
#### Using [OpenRC](https://wiki.gentoo.org/wiki/OpenRC)

Complete its conf.d file with the `-M` option for the wireless network interface:

**`/etc/conf.d/wpa_supplicant`**

```
wpa_supplicant_args="-B -M -c /etc/wpa_supplicant/wpa_supplicant.conf"
```
In case authentication for the [wired interface](https://wiki.gentoo.org/wiki/Wpa_supplicant#Setup_wired_802.1X) is needed, this configuration file should look like:

**`/etc/conf.d/wpa_supplicant`**

```
wpa_supplicant_args="-ieth0 -Dwired -c/etc/wpa_supplicant/wpa_supplicant_wired.conf -B -M -c/etc/wpa_supplicant/wpa_supplicant.conf"
```
With the configuration done, run it as a service:

`root #``rc-update add wpa_supplicant default``root #``rc-service wpa_supplicant start`
#### Using [Systemd](https://wiki.gentoo.org/wiki/Systemd)

[Systemd](https://wiki.gentoo.org/wiki/Systemd) allows a simpler per-device setup without needing to create the above conf.d files. As explained under *wpa\_supplicant* item in the [Native services](https://wiki.gentoo.org/wiki/Systemd#Native_services) section, a service symlink such as `wpa_supplicant@wlan0.service` looks for a separate configuration file to manage the device `wlan0` in this case.

To configure a specific device this way, first copy or rename the /etc/wpa\_supplicant/wpa\_supplicant.conf file as /etc/wpa\_supplicant/wpa\_supplicant-DEVNAME.conf where `DEVNAME` should be the name of the device, such as `wlan0`.

Then, navigate to /etc/systemd/system/multi-user.target.wants and create the symlink:

`root #``ln -s /lib/systemd/system/wpa_supplicant@.service wpa_supplicant@DEVNAME.service`
where `DEVNAME` is *same* device name as in the conf file above.

Test the system:

`root #````
systemctl daemon-reload
```
`root #````
systemctl start wpa_supplicant@DEVNAME
```
`root #````
systemctl status wpa_supplicant@DEVNAME
```
