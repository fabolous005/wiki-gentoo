<!-- source: https://wiki.gentoo.org/wiki/Wpa_supplicant | group: Gentoo Wiki (Main) | wiki-title: Wpa supplicant -->
---
title: wpa_supplicant
url: https://wiki.gentoo.org/wiki/Wpa_supplicant
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-13"
fingerprint: e99fd58d80219c9
license: CC BY-SA 4.0
---

# wpa\_supplicant

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**wpa\_supplicant** is an app for [Wi-Fi](https://wiki.gentoo.org/wiki/Wi-Fi) authentication (a ["supplicant"](<https://en.wikipedia.org/wiki/Supplicant_(computer)>)  in the technical jargon).

## Installation

As a precondition, wireless support might need to be activated in the kernel as described in [IEEE 802.11 section](https://wiki.gentoo.org/wiki/Wi-Fi#IEEE_802.11) as well as necessary [wireless device drivers](https://wiki.gentoo.org/wiki/Wi-Fi).[\[1\]](https://wiki.gentoo.org#cite_note-1)

### USE flags


| [+ap](https://packages.gentoo.org/useflags/+ap) | Add support for access point mode | 
| [+fils](https://packages.gentoo.org/useflags/+fils) | Add support for Fast Initial Link Setup (802.11ai) | 
| [+mbo](https://packages.gentoo.org/useflags/+mbo) | Add support Multiband Operation | 
| [+mesh](https://packages.gentoo.org/useflags/+mesh) | Add support for mesh mode | 
| [broadcom-sta](https://packages.gentoo.org/useflags/broadcom-sta) | Flag to help users disable features not supported by broadcom-sta driver | 
| [dbus](https://packages.gentoo.org/useflags/dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [eap-sim](https://packages.gentoo.org/useflags/eap-sim) | Add support for EAP-SIM authentication algorithm | 
| [eapol-test](https://packages.gentoo.org/useflags/eapol-test) | Build and install eapol\_test binary | 
| [gui](https://packages.gentoo.org/useflags/gui) | Enable support for a graphical user interface | 
| [macsec](https://packages.gentoo.org/useflags/macsec) | Add support for wired macsec | 
| [p2p](https://packages.gentoo.org/useflags/p2p) | Add support for Wi-Fi Direct mode | 
| [privsep](https://packages.gentoo.org/useflags/privsep) | Enable wpa\_priv privledge separation binary | 
| [readline](https://packages.gentoo.org/useflags/readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [smartcard](https://packages.gentoo.org/useflags/smartcard) | Add support for smartcards | 
| [tkip](https://packages.gentoo.org/useflags/tkip) | Add support for WPA TKIP (deprecated due to security flaws in 2009) | 
| [uncommon-eap-types](https://packages.gentoo.org/useflags/uncommon-eap-types) | Add support for GPSK, SAKE, GPSK\_SHA256, IKEV2 and EKE | 
| [wep](https://packages.gentoo.org/useflags/wep) | Add support for Wired Equivalent Privacy (deprecated due to security flaws in 2004) | 
| [wps](https://packages.gentoo.org/useflags/wps) | Add support for Wi-Fi Protected Setup | 

### Emerge

After USE flags have been reviewed, install [net-wireless/wpa\_supplicant](https://packages.gentoo.org/packages/net-wireless/wpa_supplicant) using Portage's emerge command:

`root #``emerge --ask net-wireless/wpa_supplicant`
## Direct connect

### Quick Connect

`root #``set +o history``root #``wpa_supplicant -i wlp0s20f3 -c <(wpa_passphrase ssid password) &``root #``set -o history`
### Connection for two interfaces

wpa\_supplicant  can  control  multiple  interfaces  (or *radios*)  either by running a separate process for each interface, or one process for all interfaces with a list of options on the command line. Each interface is separated with a `-N` argument. The following command would start wpa\_supplicant for two interfaces:

`user $``wpa_supplicant -c wpa1.conf -i wlan0 -D nl80211 -N -c wpa2.conf -i ath0 -D wext`
## Configuration

### Files

#### Minimal configuration

wpa\_supplicant includes a tool to quickly write a network block from the command line for pre-shared key (WPA-PSK aka password) networks, wpa\_passphrase.

`root #``wpa_passphrase ssid password >> /etc/wpa_supplicant/wpa_supplicant.conf`
#### Setup for wireless interface

For usage with a single wireless interface only one configuration file will be needed.

**`/etc/wpa_supplicant/wpa_supplicant.conf`**

```
# Allow users in the 'wheel' group to control wpa_supplicant
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=wheel
 
# Make this file writable for wpa_gui / wpa_cli
update_config=1
```
To allow unprivileged users to control the connection using wpa\_gui / wpa\_cli, make sure the users are in the wheel [group](https://wiki.gentoo.org/wiki/Knowledge_Base:Adding_a_user_to_a_group).

This file does not exist by default; a well documented template configuration file can be copied from /usr/share/doc/${P}/wpa\_supplicant.conf.bz2 where the value of the `P` variable is the name and version of the currently emerged wpa\_supplicant:

`root #``bzcat /usr/share/doc/${P}/wpa_supplicant.conf.bz2 > /etc/wpa_supplicant/wpa_supplicant.conf`
#### WPA2 with wpa\_supplicant

Connecting to any wireless access point serving *YourSSID*

**`/etc/wpa_supplicant/wpa_supplicant.conf`**

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=wheel
#ap_scan=0
#update_config=1
 
network={
        ssid="YourSSID"
        psk="your-secret-key"
        scan_ssid=1
        proto=RSN
        key_mgmt=WPA-PSK
        group=CCMP
        pairwise=CCMP
        priority=5
}
```
#### Configuration file with dynamic WEP keys

**`/etc/wpa_supplicant/wpa_supplicant_wired.conf`**

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=wheel
network={
	ssid="1x-test"
	scan_ssid=1
	key_mgmt=IEEE8021X
	eap=TLS
	identity="user@example.com"
	ca_cert="/etc/cert/ca.pem"
	client_cert="/etc/cert/user.pem"
	private_key="/etc/cert/user.prv"
	private_key_passwd="password"
	eapol_flags=3
}
```
#### Allows more or less all configuration modes

**`/etc/wpa_supplicant/wpa_supplicant_wired.conf`**

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=wheel
network={
	ssid="example"
	scan_ssid=1
	key_mgmt=WPA-EAP WPA-PSK IEEE8021X NONE
	pairwise=CCMP TKIP
	group=CCMP TKIP WEP104 WEP40
	psk="very secret passphrase"
	eap=TTLS PEAP TLS
	identity="user@example.com"
	password="foobar"
	ca_cert="/etc/cert/ca.pem"
	client_cert="/etc/cert/user.pem"
	private_key="/etc/cert/user.prv"
	private_key_passwd="password"
	phase1="peaplabel=0"
	ca_cert2="/etc/cert/ca2.pem"
	client_cert2="/etc/cer/user.pem"
	private_key2="/etc/cer/user.prv"
	private_key2_passwd="password"
}
```


#### Setup wired 802.1X

It's possible to have wired connections handled via wpa\_supplicant, which is useful for networks using 802.1X. Create a separate configuration file containing the wired configuration. Below example use certificates for authentication, check the wpa\_supplicant.conf man page for examples of other methods.

**`/etc/wpa_supplicant/wpa_supplicant_wired.conf`**

```
ctrl_interface=/var/run/wpa_supplicant
eapol_version=1
ap_scan=0
fast_reauth=1
 
network={
	key_mgmt=IEEE8021X
	eap=TLS
	identity="COMPUTERAACT$@DOMAIN"
	ca_cert="/etc/wpa_supplicant/ca.pem"
	client_cert="/etc/wpa_supplicant/COMPUTERACCT.pem"
	private_key="/etc/wpa_supplicant/COMPUTERAACT.key"
	private_key_passwd="secret_password"
	eapol_flags=0
}
```
Since the configuration file contains sensitive information, chmod accordingly.

`root #``chmod 600 /etc/wpa_supplicant/wpa_supplicant_wired.conf`
wpa\_supplicant needs some extra parameters to apply above configuration to the wired interface (eth0) Note that below wpa\_supplicant arguments assumes wpa\_supplicant is version >=2.6-r2 (-M, CONFIG\_MATCH\_IFACE=y)

**`/etc/conf.d/wpa_supplicant`**

```
wpa_supplicant_args="-ieth0 -Dwired -c/etc/wpa_supplicant/wpa_supplicant_wired.conf -M -c/etc/wpa_supplicant/wpa_supplicant.conf"
```
Let wpa\_supplicant handle start/stop of the interfaces by removing them from /etc/init.d and enabling the wpa\_supplicant daemon

`root #````
/etc/init.d/net.eth0 stop
```
`root #````
/etc/init.d/net.wlan0 stop
```
`root #````
rm /etc/init.d/net.wlan0 /etc/init.d/net.eth0
```
`root #````
rc-update add wpa_supplicant
```
`root #````
/etc/init.d/wpa_supplicant start
```
Check the status of the wired interface via wpa\_cli

Connect directly to the wireless access point from the command line



`root #````
wpa_cli
```
wpa\_cli v2.8
Copyright (c) 2004-2019, Jouni Malinen \<j@w1.fi> and contributors
 
This software may be distributed under the terms of the BSD license.
See README for more details.
 
 
Selected interface 'p2p-dev-wlan0'
 
Interactive mode
 
> interface eth0
Connected to interface 'eth0.
> status
bssid=00:00:00:00:00:00
freq=0
ssid=
id=0
mode=station
pairwise\_cipher=NONE
group\_cipher=NONE
key\_mgmt=IEEE 802.1X (no WPA)
wpa\_state=COMPLETED
ip\_address=10.10.10.100
p2p\_device\_address=bb:bb:bb:bb:bb:bb
address=aa:aa:aa:aa:aa:aa
Supplicant PAE state=AUTHENTICATED
suppPortStatus=Authorized
EAP state=SUCCESS
selectedMethod=13 (EAP-TLS)
eap\_tls\_version=TLSv1
EAP TLS cipher=ECDHE-RSA-AES256-SHA
...

## Setup the network manager

Be sure to choose the corresponding setup.

### Setup for dhcpcd as network manager

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
In case the deprecated [WEXT](https://wiki.gentoo.org/wiki/Wifi#WEXT) driver is needed, changing the wireless driver can help resolve cases where it associates then immediately disconnects with **reason 3**. Run wpa\_supplicant -h to see a list of the available drivers that were built at compile-time.

**`/etc/conf.d/wpa_supplicant`**

**set the driver to wext**

```
wpa_supplicant_args="-D wext"
```
### Setup for Netifrc

To configure [Netifrc](https://wiki.gentoo.org/wiki/Netifrc) to use wpa\_supplicant:

**`/etc/conf.d/net`**

```
modules_wlan0="wpa_supplicant"
config_wlan0="dhcp"
```
After configuration above it is a good idea to change the permissions to ensure that WiFi passwords can not be viewed in plaintext by anyone using the computer:[\[2\]](https://wiki.gentoo.org#cite_note-2)

`root #``chmod 600 /etc/wpa_supplicant/wpa_supplicant.conf`
### Setup for NetworkManager

[NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager) configured with wpa\_supplicant as WiFi backend is able to use [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) to start wpa\_supplicant when needed. Therefore it is recommended to keep the wpa\_supplicant service itself stopped at boot time.

## Usage

### Using wpa\_gui

The simplest way to use wpa\_supplicant is by using its interface called wpa\_gui. To enable it, build wpa\_supplicant with the `qt5` USE flag enabled.

### Using wpa\_cli

Wpa\_supplicant also has a command-line user interface. Typing wpa\_cli starts its interactive mode with tab-completion. Typing `help` at this prompt will list the commands available (click "Expand" to view the output for the wpa\_cli command below):

`user $````
wpa_cli
```
wpa\_cli v2.5
 Copyright (c) 2004-2015, Jouni Malinen \<j@w1.fi> and contributors
 
 This software may be distributed under the terms of the BSD license.
 See README for more details.
 
 
 Selected interface 'wlan0'
 
 Interactive mode
 
 > scan
 OK
 > scan\_results
 bssid / frequency / signal level / flags / ssid
 01:23:45:67:89:ab       2437    0       \[WPA-PSK-CCMP+TKIP\]\[WPA2-PSK-CCMP+TKIP\]\[ESS\]    hotel-free-wifi
 > add\_network
 0
 > set\_network 0 ssid "hotel-free-wifi"
 OK
 > set\_network 0 psk "password"
 OK
 > enable\_network 0
 OK
 \<3>CTRL-EVENT-SCAN-RESULTS 
 \<3>WPS-AP-AVAILABLE 
 \<3>Trying to associate with 01:23:45:67:89:ab (SSID='hotel-free-wifi' freq=2437 MHz)
 \<3>Associated with 01:23:45:67:89:ab
 \<3>WPA: Key negotiation completed with 01:23:45:67:89:ab \[PTK=CCMP GTK=TKIP\]
 \<3>CTRL-EVENT-CONNECTED - Connection to 01:23:45:67:89:ab completed \[id=0 id\_str=\]
 > save\_config 
 OK
 > quit

For switching to another Wi-Fi:

`user $````
wpa_cli
```
wpa\_cli v2.5
 Copyright (c) 2004-2015, Jouni Malinen \<j@w1.fi> and contributors
 
 This software may be distributed under the terms of the BSD license.
 See README for more details.
> list\_networks
network id / ssid / bssid / flags
0	TAMO	any	
1	ORBI705	any	
2	ORBI	any	
3	Tangerine	any	
4	271	any	
5	POCO X3 Pro	any	
6	Orbi Guest	any	
7	hackerspace	any	
8	HUAWEI-25 a-2	any	
9	A1-13	any	
 
> select\_network 1

More details on how to connect can be found in the Arch Linux wiki.[\[3\]](https://wiki.gentoo.org#cite_note-3)

### Editing manually

Of course, the configuration file /etc/wpa\_supplicant/wpa\_supplicant.conf could also be edited manually. However this can be very laborious if the computer needs to connect to many different access points.

Examples can be found in [wpa\_supplicant.conf(5)](https://man.archlinux.org/man/wpa_supplicant.conf.5.en) [man page and /usr/share/doc/wpa\_supplicant-2.4-r3/wpa\_supplicant.conf.bz2.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

**`/etc/wpa_supplicant/wpa_supplicant.conf`**

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=wheel
ap_scan=1
 
network={
        bssid=00:50:17:31:1a:11
        ssid="YourSSID"
        psk="your-secret-key"
        scan_ssid=1
        proto=RSN
        key_mgmt=WPA-PSK
        group=CCMP TKIP
        pairwise=CCMP TKIP
        priority=5
}
```
After editing wpa\_supplicant.conf, to get wpa\_cli to see the new network, run: `wpa_cli -i <interface> reconfigure`.

#### Auto-connect to any unsecured network

**`/etc/wpa_supplicant/wpa_supplicant.conf`**

```
network={
        key_mgmt=NONE
        priority=-999
}
```
## Troubleshooting

### Name can have spaces postfix

See point in the scan but not connecting? Check SSID for spaces: in wpa\_cli scan\_results select that point - so you will "see" the space at the end.

In case it does not work as expected try some of the following and analyze the output.

### Check for known bugs

### Can't see all SSIDs

SSIDs that use deprecated insecure protocols will be unavailable by default, this can sometimes explain why some SSIDs may seem to be "missing".

There is an [insecure workaround](https://wiki.gentoo.org/wiki/Warning_about_insecure_obsolete_network_hardware#Insecure_workaround) to enable connecting to these insecure networks, though users should be aware of the security implications.

### rfkill: WLAN soft blocked

If [rfkill](https://wiki.gentoo.org/wiki/Util-linux#rfkill) is blocking the interface, first find the interface number with:

`user $``rfkill list`
0: ideapad\_wlan: Wireless LAN
	Soft blocked: yes
	Hard blocked: no
1: ideapad\_bluetooth: Bluetooth
	Soft blocked: yes
	Hard blocked: no
2: hci0: Bluetooth
	Soft blocked: yes
	Hard blocked: no
3: phy0: Wireless LAN
	Soft blocked: yes
	Hard blocked: no

Then the interface can be unblocked with:

`root #``rfkill unblock 3`
### Configuration for a captive portal

Using a captive portal involves setting the `key_mgmt` parameter for the relevant network to `NONE`:

**`/etc/wpa_supplicant/wpa_supplicant.conf`**

### Run wpa\_supplicant in debug mode

Be sure to stop any running instance of the supplicant:

`root #``killall wpa_supplicant`
Something like the following options can be used for debugging (click "Expand" to view the output below):

`root #````
wpa_supplicant -Dnl80211 -iwlan0 -C/var/run/wpa_supplicant/ -c/etc/wpa_supplicant/wpa_supplicant.conf -dd
```
wpa\_supplicant v2.2
random: Trying to read entropy from /dev/random
Successfully initialized wpa\_supplicant
Initializing interface 'wlp8s0' conf '/etc/wpa\_supplicant/wpa\_supplicant.conf' driver 'nl80211' ctrl\_interface '/var/run/wpa\_supplicant' bridge 'N/A'
Configuration file '/etc/wpa\_supplicant/wpa\_supplicant.conf' -> '/etc/wpa\_supplicant/wpa\_supplicant.conf'
Reading configuration file '/etc/wpa\_supplicant/wpa\_supplicant.conf'
ctrl\_interface='DIR=/var/run/wpa\_supplicant GROUP=wheel'
update\_config=1
Line: 6 - start of a new network block

### Enable logging

#### Enable logging for Gentoo net.\* scripts

**`/etc/conf.d/net`**

**for usage with the setup for Gentoo net.\* scripts**

```
modules_wlan0="wpa_supplicant"
wpa_supplicant_wlan0="-Dnl80211 -d -f /var/log/wpa_supplicant.log"
config_wlan0="dhcp"
```
Now, within one terminal issue a tail command to monitor output and restart the net.wlan0 device in another:

`root #````
tail -f /var/log/wpa_supplicant.log
```
`root #````
/etc/init.d/net.wlan0 restart
```
## References

## See also

- [iwd](https://wiki.gentoo.org/wiki/Iwd) — a wireless daemon intended to replace wpa\_supplicant

## External resources

- [wpa\_supplicant / hostapd Developers' documentation for wpa\_supplicant and hostapd](https://w1.fi/wpa_supplicant/devel/)
- [sample config for wpa\_supplicant](https://w1.fi/cgit/hostap/plain/wpa_supplicant/wpa_supplicant.conf)
- [HOWTO: Remote access point with wpa\_supplicant](https://forums.gentoo.org/viewtopic-t-1007254.html) (Gentoo Forums)
- [Extensible Authentication Protocol](https://en.wikipedia.org/wiki/Extensible_Authentication_Protocol) (Wikipedia)
- [Extensible Authentication Protocol](https://wiki.freeradius.org/protocol/EAP) (wiki.freeradius.org)
- [wpa\_supplicant upstream just accepted patch to allow interface matching](https://forums.gentoo.org/viewtopic-t-1036958-start-4.html)
- [https://www.kb.cert.org/vuls/id/CHEU-AQNN3Z](https://www.kb.cert.org/vuls/id/CHEU-AQNN3Z)
