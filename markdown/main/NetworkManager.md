<!-- source: https://wiki.gentoo.org/wiki/NetworkManager | group: Gentoo Wiki (Main) | wiki-title: NetworkManager -->
---
title: NetworkManager
url: https://wiki.gentoo.org/wiki/NetworkManager
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-13"
fingerprint: fe1a52780ab639a6
license: CC BY-SA 4.0
---

# NetworkManager

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**NetworkManager** is a [network management software](https://wiki.gentoo.org/wiki/Network_management) for [Ethernet](https://wiki.gentoo.org/wiki/Ethernet), [WiFi](https://wiki.gentoo.org/wiki/Wifi), DSL, dialup, VPN, [WiMAX](https://wiki.gentoo.org/wiki/WiMAX), and mobile broadband network connections.

## Installation

NetworkManager requires an implementation of [udev](https://wiki.gentoo.org/wiki/Udev) and [D-Bus](https://wiki.gentoo.org/wiki/D-Bus). On laptops and desktops, it is typically built with [polkit](https://wiki.gentoo.org/wiki/Polkit) to enable local users to configure it. It also optionally integrates with [systemd](https://wiki.gentoo.org/wiki/Systemd), [upower](https://wiki.gentoo.org/index.php?title=Upower&action=edit&redlink=1), and others.

The [networkmanager](https://packages.gentoo.org/useflags/networkmanager) [USE flag will enable support for NetworkManager in other packages. Enabling this USE flag will make those packages pull in](https://wiki.gentoo.org/wiki/USE_flag) [net-misc/networkmanager](https://packages.gentoo.org/packages/net-misc/networkmanager) automatically:

**`/etc/portage/make.conf`**

Alternatively, the euse tool from [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit) can do this from the command-line:

`root #``euse -E networkmanager`
### Kernel

For WiFi devices enable also the following options:

Look at the [udev](https://wiki.gentoo.org/wiki/Udev) page for kernel configuration needed for this NetworkManager dependency.

### USE flags


| [+concheck](https://packages.gentoo.org/useflags/+concheck) | Enable connectivity checking support (needed for Captive portals detection) | 
| [+introspection](https://packages.gentoo.org/useflags/+introspection) | Add support for GObject based introspection | 
| [+modemmanager](https://packages.gentoo.org/useflags/+modemmanager) | Enable support for mobile broadband devices using net-misc/modemmanager | 
| [+nss](https://packages.gentoo.org/useflags/+nss) | Use dev-libs/nss for cryptography | 
| [+ppp](https://packages.gentoo.org/useflags/+ppp) | Enable support for mobile broadband and PPPoE connections using net-dialup/ppp | 
| [+tools](https://packages.gentoo.org/useflags/+tools) | Build cli tools such as nmcli, nmtui and nm\_cloud\_setup | 
| [+wext](https://packages.gentoo.org/useflags/+wext) | Enable support for the deprecated Wext (Wireless Extensions) API; needed for some older drivers (e.g. ipw2200, ndiswrapper) | 
| [+wifi](https://packages.gentoo.org/useflags/+wifi) | Enable support for wifi and 802.1x security | 
| [audit](https://packages.gentoo.org/useflags/audit) | Enable support for Linux audit subsystem using sys-process/audit | 
| [bluetooth](https://packages.gentoo.org/useflags/bluetooth) | Enable Bluetooth Support | 
| [clat](https://packages.gentoo.org/useflags/clat) | Enable CLAT (464XLAT) support for IPv4 over IPv6-only networks | 
| [connection-sharing](https://packages.gentoo.org/useflags/connection-sharing) | Support connection sharing (uses net-dns/dnsmasq) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [dhclient](https://packages.gentoo.org/useflags/dhclient) | Use dhclient from net-misc/dhcp for getting an IP via DHCP | 
| [dhcpcd](https://packages.gentoo.org/useflags/dhcpcd) | Use net-misc/dhcpcd for getting an IP | 
| [elogind](https://packages.gentoo.org/useflags/elogind) | Use sys-auth/elogind for session tracking | 
| [gnutls](https://packages.gentoo.org/useflags/gnutls) | Prefer net-libs/gnutls as SSL/TLS provider (ineffective with USE=-ssl) | 
| [gtk-doc](https://packages.gentoo.org/useflags/gtk-doc) | Build and install gtk-doc based developer documentation for dev-util/devhelp, IDE and offline use | 
| [iptables](https://packages.gentoo.org/useflags/iptables) | Use net-firewall/iptables for connection sharing | 
| [iwd](https://packages.gentoo.org/useflags/iwd) | Use net-wireless/iwd instead of net-wireless/wpa\_supplicant for wifi support by default | 
| [libedit](https://packages.gentoo.org/useflags/libedit) | Use the libedit library (replacement for readline) | 
| [nbft](https://packages.gentoo.org/useflags/nbft) | Enable NBFT support in the initrd generator | 
| [nftables](https://packages.gentoo.org/useflags/nftables) | Use net-firewall/nftables for connection sharing | 
| [ofono](https://packages.gentoo.org/useflags/ofono) | Use net-misc/ofono for telephony support. | 
| [ovs](https://packages.gentoo.org/useflags/ovs) | Enable OpenVSwitch support | 
| [policykit](https://packages.gentoo.org/useflags/policykit) | Enable PolicyKit (polkit) authentication support | 
| [psl](https://packages.gentoo.org/useflags/psl) | Use public suffix list via net-libs/libpsl | 
| [resolvconf](https://packages.gentoo.org/useflags/resolvconf) | Use net-dns/openresolv for managing DNS information in /etc/resolv.conf. Generally, a symlink to /run/NetworkManager/resolv.conf is simpler. On systems running systemd-resolved, disable this flag and create a symlink to /run/systemd/resolve/stub-resolv.conf. | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [syslog](https://packages.gentoo.org/useflags/syslog) | Enable support for syslog | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [teamd](https://packages.gentoo.org/useflags/teamd) | Enable Teamd control support | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [vala](https://packages.gentoo.org/useflags/vala) | Enable bindings for dev-lang/vala | 

### Emerge

After changing use flags run the following command to update the system so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
To manually install NetworkManager, if not already pulled in automatically from above command:

`root #``emerge --ask net-misc/networkmanager`
It may be required to add the [dbus](https://packages.gentoo.org/useflags/dbus) [use flag to](https://wiki.gentoo.org/wiki/USE_flag) [net-wireless/wpa\_supplicant](https://packages.gentoo.org/packages/net-wireless/wpa_supplicant) also:

**`/etc/portage/package.use/networkmanager`**

### Additonal software

#### VPN plugins

The following packages can be used to add VPN support to the base NetworkManager agent:

- [net-vpn/networkmanager-openconnect](https://packages.gentoo.org/packages/net-vpn/networkmanager-openconnect) - VPN connection using [OpenConnect](https://wiki.gentoo.org/wiki/OpenConnect)
- [net-vpn/networkmanager-libreswan](https://packages.gentoo.org/packages/net-vpn/networkmanager-libreswan) - VPN connection using [net-vpn/libreswan](https://packages.gentoo.org/packages/net-vpn/libreswan)
- [net-vpn/networkmanager-openvpn](https://packages.gentoo.org/packages/net-vpn/networkmanager-openvpn) - VPN connection using OpenVPN server
- [net-vpn/networkmanager-pptp](https://packages.gentoo.org/packages/net-vpn/networkmanager-pptp) - VPN connection to a PPTP server
- [net-vpn/networkmanager-sstp](https://packages.gentoo.org/packages/net-vpn/networkmanager-sstp) - VPN connection to a SSTP server
- [net-vpn/networkmanager-vpnc](https://packages.gentoo.org/packages/net-vpn/networkmanager-vpnc) - VPN connection using [net-vpn/vpnc](https://packages.gentoo.org/packages/net-vpn/vpnc)

After emerging a plugin, it will be available when adding new connections to NetworkManager.

#### GTK GUIs

![](https://wiki.gentoo.org/images/thumb/a/a3/Nm-applet-png.png/300px-Nm-applet-png.png)

There is a systray applet working in classic Xembed-based systrays provided by [gnome-extra/nm-applet](https://packages.gentoo.org/packages/gnome-extra/nm-applet).

If a systray is not included as a part of the desktop environment in use, a standalone systray like [x11-misc/stalonetray](https://packages.gentoo.org/packages/x11-misc/stalonetray) can be installed. The connection editor GUI in the same package as the applet. Note that this package serves all sorts of [desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment) and panels with systrays but it is no longer used by [Gnome](https://wiki.gentoo.org/wiki/Gnome) which has its own implementations in [Gnome Shell](https://wiki.gentoo.org/index.php?title=Gnome_Shell&action=edit&redlink=1) and [Gnome Control Center](https://wiki.gentoo.org/index.php?title=Gnome_Control_Center&action=edit&redlink=1).

`root #``emerge --ask gnome-extra/nm-applet`
Also note that the current upstream version [doesn't support the appindicator API](https://bugzilla.gnome.org/show_bug.cgi?id=740574) and thus does not work in some systray implementations like those in current versions of [KDE](https://wiki.gentoo.org/wiki/KDE) and [Unity](https://wiki.gentoo.org/wiki/Unity) or the development versions of [Enlightenment](https://wiki.gentoo.org/wiki/Enlightenment).

In LXQt when setting panel to autohide and [mouse hover on Wi-Fi ico hides panel](https://wiki.gentoo.org/wiki/File:Lxqt-tray-widget-obsolote.webp), the following are needed:

1. Emerge [gnome-extra/nm-applet](https://packages.gentoo.org/packages/gnome-extra/nm-applet) with USE flag **appindicator**
2. Emerge [lxqt-base/lxqt-panel](https://packages.gentoo.org/packages/lxqt-base/lxqt-panel) with USE flag **statusnotifier**
3. Edit autostart in LXQt: change **nm-applet** to **nm-applet --indicator**.

See [related bug](https://bugs.gentoo.org/show_bug.cgi?id=768666).

#### KDE GUIs

- [kde-plasma/plasma-nm](https://packages.gentoo.org/packages/kde-plasma/plasma-nm) - KDE Plasma frontend.

## Configuration

### User permission

On Gentoo, NetworkManager uses the plugdev group to specify which non-root users can manage system network connections (treated as pluggable devices). Be sure to add each user who should be permitted to manage the network connections to that group.

Adding user `larry` can be achieved issuing:

`root #``gpasswd -a larry plugdev`
### Service

#### OpenRC

Remove any existing network management services (if activated).

For example, to remove any netifrc scripts from controlling network interfaces (assuming they are all in the default runlevel), issue the following command:

`root #``for x in /etc/runlevels/default/net.* ; do rc-update del $(basename $x) default ; rc-service --ifstarted $(basename $x) stop; done`
To remove dhcpcd:

`root #``rc-update del dhcpcd default`
Start NetworkManager:

`root #``rc-service NetworkManager start`
To start NetworkManager at boot time add it to the default runlevel:

`root #``rc-update add NetworkManager default`
#### systemd

To enable and start NetworkManager immediately:

`root #``systemctl enable --now NetworkManager`
With NetworkManager older than 0.9.10 or with services that order themselves after network.service instead of network-online.service, enabling the NetworkManager-wait-online.service for `multi-user.target` may be necessary:

`root #``systemctl enable NetworkManager-wait-online.service`
When writing custom systemd services, they can wait for NetworkManager to configure the boot time connections:

**`/etc/systemd/system/*.service`**

```
[Unit]
After=network-online.service
Wants=network-online.service
```
With NetworkManager 0.9.10 and later it works even without explicitly enabling the network-online.service.

### Setting a hostname

If NetworkManager was built with the [dhclient](https://packages.gentoo.org/useflags/dhclient) [USE flag enabled a hostname can be set using the following command:](https://wiki.gentoo.org/wiki/USE_flag)

**`/etc/dhcp/dhclient.conf`**

```
 host-name "customhostname";
```
### Checking connectivity

NetworkManager can try to reach a page on Internet when connecting to a network. For those behind a captive portal, the desktop manager can automatically open a window asking for credentials. It's automatically done since NetworkManager 1.8, but it has to be configured manually for earlier versions. To enable this feature, edit (or create) the /etc/NetworkManager/NetworkManager.conf file to look something like this:

**`/etc/NetworkManager/NetworkManager.conf`**

```
[connectivity]
uri=http://nmcheck.gnome.org/check_network_status.txt
```
### nm-applet and X session startup

To be able to get nm-applet started when starting a light X session or light desktop environment, just put the following line in the relevant user's \~/.xinitrc file:

**`~/.xinitrc`**

```
 &
```
For [gnome-base/gnome-keyring](https://packages.gentoo.org/packages/gnome-base/gnome-keyring) support, add the following lines **before** the previous line. This will ease password management for GnuPG, ssh and WiFi:

**`~/.xinitrc`**

```
# Ensure dbus is either already running, or safely start it
if [[ -z "${DBUS_SESSION_BUS_ADDRESS}" ]];
then
    eval $(dbus-launch --sh-syntax --exit-with-session)
fi
# Make the keyring daemon ready to communicate with nm-applet
export $(gnome-keyring-daemon --start --components=pkcs11,secrets,ssh,gpg)
```
### Dnsmasq

#### NetworkManager way

NetworkManager can be set up to use [Dnsmasq](https://wiki.gentoo.org/wiki/Dnsmasq) as a local DNS server that passes the DNS queries on to a provider's DNS server. /etc/resolv.conf will be set to point to 127.0.0.1, where dnsmasq runs and processes the queries. This can be useful for example if an application chroots for security reasons and before doing so copies /etc/resolv.conf. Then it would never be informed about changes to the DNS servers as the device moves from one WiFi network to another.

Setup of dnsmasq is simple:

**`/etc/NetworkManager/NetworkManager.conf`**

```
[main]
plugins=keyfile
dns=dnsmasq
```
[Dnsmasq](https://wiki.gentoo.org/wiki/Dnsmasq) can be configured with files in /etc/NetworkManager/dnsmasq.d, for more information see the wiki page or the man pages of [Dnsmasq](https://wiki.gentoo.org/wiki/Dnsmasq).

Then restart NetworkManager.

##### DNSSEC

Dnsmasq can optionally validate DNSSEC data while passing through queries (must be compiled with the [dnssec](https://packages.gentoo.org/useflags/dnssec) [USE flag). This can be accomplished by adding these lines to the NetworkManager dnsmasq config file:](https://wiki.gentoo.org/wiki/USE_flag)

**`/etc/NetworkManager/dnsmasq.d/dnssec`**

```
# DNSSEC setup
dnssec
trust-anchor=.,19036,8,2,49AAC11D7B6F6446702E54A1607371607A1A41855200FD2CE1CDDE32F24E8FB5
trust-anchor=.,20326,8,2,E06D44B80B8F1D39A95C0B0D7C65D08458E880409BBC683457104237C7F8EC8D
dnssec-check-unsigned
```
The trusted anchor can be found [here](https://data.iana.org/root-anchors/root-anchors.xml). After this change dnsmasq will return SERVFAIL and no DNS data if the validation fails. If the validation succeeds it sets the Authenticated Data (AD) flag. In case the domain does not support DNSSEC dnsmasq behaves as before.

If an ISP's DNS server does not forward DNSSEC data then this will fail. Uncomment the last line, but it will defy the purpose of DNSSEC. Google's server 8.8.8.8 provides DNSSEC data.

#### Service way

In certain system network setups, [Dnsmasq](https://wiki.gentoo.org/wiki/Dnsmasq) should not be controlled by NetworkManager. An alterantive way they both can be used together:

**`/etc/NetworkManager/NetworkManager.conf`**

```
[main]
plugins=keyfile
dns=none
```
And add localhost to the /etc/resolv.conf file:

**`/etc/resolv.conf`**

```
# This should be the first nameserver entry in resolv.conf!
nameserver=127.0.0.1
```
Set up [Dnsmasq](https://wiki.gentoo.org/wiki/Dnsmasq), see man pages and the wiki page about [Dnsmasq](https://wiki.gentoo.org/wiki/Dnsmasq) for details.

Don't forget to start [Dnsmasq](https://wiki.gentoo.org/wiki/Dnsmasq).

On systemd systems:

`root #``systemctl enable --now dnsmasq`
On OpenRC systems:

`root #````
rc-update add dnsmasq default
```
`root #````
rc-service dnsmasq start
```
## Usage

### Plugins

#### WireGuard

See the [NetworkManager section](https://wiki.gentoo.org/wiki/WireGuard#NetworkManager) of the [WireGuard](https://wiki.gentoo.org/wiki/WireGuard) article for more information on adding a WireGuard tunnel to NetworkManager.

### Networks

#### eduroam

eduroam is an international Wi-Fi network for universities. Please see article about [Eduroam](https://wiki.gentoo.org/wiki/Eduroam).

## Troubleshooting

### Fixing nm-applet insufficient privileges

If nm-applet fails to create new networks with the error "Insufficient Privileges," then it could be a policy kit issue. Create the following file:

**`/etc/polkit-1/rules.d/50-org.freedesktop.NetworkManager.rules`**

```
polkit.addRule(function(action, subject) {
    if (action.id.indexOf("org.freedesktop.NetworkManager.") == 0 && subject.isInGroup("plugdev")) {
        return polkit.Result.YES;
    }
});
```
This lets all users in the plugdev group control network manager.

### Hostname problems

The standard "keyfile" plugin does not forward the hostname in default configuration - to avoid having it changed upon network connection, add the following section to NetworkManager.conf and enter hostname accordingly:

**`/etc/NetworkManager/NetworkManager.conf`**

```
[main]
plugins=keyfile
hostname-mode=none
```
Alternatively, if a hostname is set which NetworkManager considers valid (Mainly anything other than "localhost" or similar default values), the hostname fetching from DHCP servers is skipped. To set a new hostname, edit the file /etc/conf.d/hostname:

**`/etc/conf.d/hostname`**

```
# Set to the hostname of this machine
hostname="my-hostname"
```
### Connection sharing

Connection sharing is not working on an Ethernet connection when set to shared via nmtui.

Ensure the [connection-sharing](https://packages.gentoo.org/useflags/connection-sharing) [USE flag has been enabled for](https://wiki.gentoo.org/wiki/USE_flag) [net-misc/networkmanager](https://packages.gentoo.org/packages/net-misc/networkmanager), then reemerge the package:

`root #``emerge --ask --newuse --deep net-misc/networkmanager`
### DHCPv6 Unique IDentifier (DUID)

The [DUID](https://en.wikipedia.org/wiki/DHCPv6#DHCP_unique_identifier) will be generated by NetworkManager and stored as the first line in the following file:

**`/var/lib/NetworkManager/dhclient6-*.lease`**

```
 "\000\001\000\001\031\012D\036<\331+m3\004";
lease6 {
...
```
To generate a DUID NetworkManager relies on the following file, which is created by systemd. This should be unique to any system.

**`/etc/machine-id`**

For non-systemd users, it is possible to use the following command from lubko on #nm irc channel @libera.chat:

`root #``uuidgen | sed 's/-//g' > /etc/machine-id`
### NetworkManager messing with X authentication

When NetworkManager connects to a WiFi access point, it might change the system hostname. If it does, it might mess with X authentication and prevent launching X applications. Verify this with xauth list.

To fix this, set hostname-mode = none in the config.

### Wifi card driver and firmware are correctly loaded but interface is not available in NetworkManager

In case dmesg | grep wifi shows the kernel and the firmware of the network card has been properly loaded and e.g. beautifully appears in hwinfo | grep wifi but nmcli device show shows a line like `GENERAL.DEVICE: wifi0 GENERAL.STATE: 20 (unavailable)`. This likely means the kernel and firmware are working correctly; the problem is somewhere in userspace.

A possible solution is to load iwd prior to NetworkManager. It is now recommended to replace wpa\_supplicant by [Iwd](https://wiki.gentoo.org/wiki/Iwd). To do so, remove wpa\_supplicant from the init system (OpenRC/systemd) and add iwd, which then loads the Wifi card. Note that wireless network password store from wpa\_supplicant will not be preserved, so WiFi authentication passwords will be need to be reentered in order to (re)connect to each SSID.

Review the [iwd page](https://wiki.gentoo.org/wiki/Iwd) for further information on setting up iNet wireless daemon backend.

### Failed to add new connection: 802.1x connections must have IWD provisioning files

When using iwd backend for wifi connections, NetworkManager cannot autogenerate the provisioning file for 802.1x connections.

In this situation, you can just write the provisioning file manually.

**`/var/lib/iwd/[SSID].8021x`**

```
[Security]
EAP-Method=PEAP
EAP-Identity=your-username
EAP-Password=your-password
EAP-CACert=path/to/ca-cert.pem # (delete this line if you dont need CA certification)
EAP-Phase2-Method=GTC
```
This example is for WPA/WPA2 Enterprise, PEAP-GTC certification. For other connection method, refer [iwd#configuration-per-connection](https://wiki.gentoo.org/wiki/Iwd#configuration-per-connection).

### systemd-networkd

NetworkManager and [systemd-networkd](https://wiki.gentoo.org/wiki/Systemd-networkd) are generally incompatible and should not be run together. They both attempt to manage the same network resources, leading to potential conflicts and unpredictable behavior.

To disable the systemd-networkd services, run

`root #````
systemctl disable systemd-networkd.service
```
`root #````
systemctl disable systemd-networkd.socket
```
`root #````
systemctl disable systemd-networkd-wait-online.service
```
## See also

- [Iwd](https://wiki.gentoo.org/wiki/Iwd) — a wireless daemon intended to replace wpa\_supplicant
- [Resolv.conf](https://wiki.gentoo.org/wiki/Resolv.conf) — used to configure hostname resolution.
