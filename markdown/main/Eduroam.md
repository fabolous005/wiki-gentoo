<!-- source: https://wiki.gentoo.org/wiki/Eduroam | group: Gentoo Wiki (Main) | wiki-title: Eduroam -->
---
title: eduroam
url: https://wiki.gentoo.org/wiki/Eduroam
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-29"
fingerprint: "961900fc6da6a184"
license: CC BY-SA 4.0
---

# eduroam

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This article will describe how to configure Gentoo to connect to **eduroam**. eduroam (*edu*cation *roam*ing) is an international [Wi-Fi](https://wiki.gentoo.org/wiki/Wifi) service based on [802.1x](https://en.wikipedia.org/wiki/IEEE_802.1X) for users at many educational institutions.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>. There is a map available to see where eduroam networks exist.[\[2\]](https://wiki.gentoo.org#cite_note-2)

## Configuration

### Configuration assistant tool

The eduroam Configuration Assistant Tool (CAT) collects information about RADIUS/EAP deployments and generates secure installation programs for a range of popular PC and smartphone platforms.<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> The installer can be downloaded at [cat.eduroam.org](https://cat.eduroam.org/). On Linux, it supports PEAP-MSCHAPv2, TLS, TTLS-MSCHAPv2, TTLS-PAP, and Managed IdP.<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup> The CAT installer primarily supports NetworkManager, but can also be used to generate standalone configuration files for use with [wpa\_supplicant](https://wiki.gentoo.org/wiki/Wpa_supplicant) and [iwd](https://wiki.gentoo.org/wiki/Iwd).

### NetworkManager (nmcli)

nmcli can be used to manually establish eduroam connections with [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager). The connection-specific configuration files are stored in /etc/NetworkManager/system-connections/.

**`eduroam-setup.sh`**

```
#!/bin/bash
 
CONNAME="eduroam"
USERNAME="firstname.surname@tuni.fi"
PASSWORD=""
 
nmcli connection add type wifi con-name $CONNAME        \
        connection.permissions $LOGNAME                 \
        802-11-wireless.ssid $CONNAME                   \
        802-11-wireless-security.key-mgmt wpa-eap       \
        802-11-wireless-security.group ccmp,tkip        \
        802-11-wireless-security.pairwise ccmp          \
        802-11-wireless-security.proto rsn              \
        802-1x.altsubject-matches DNS:wifi.tuni.fi      \
        802-1x.anonymous-identity anonymous@tuni.fi     \
        802-1x.eap peap                                 \
        802-1x.identity $USERNAME                       \
        802-1x.password $PASSWORD                       \
        802-1x.phase2-auth mschapv2                     \
        ipv4.method auto                                \
        ipv6.addr-gen-mode stable-privacy               \
        ipv6.method auto
```
The above is specific to Tampere University in Finland. Configuration may differ across institutions, especially parameters like `802-1x.altsubject-matches DNS:wifi.tuni.fi` and `802-1x.anonymous-identity anonymous@tuni.fi`.

#### Troubleshooting

On [systemd](https://wiki.gentoo.org/wiki/Systemd) profiles, a conflict may arise between NetworkManager and [systemd-networkd](https://wiki.gentoo.org/wiki/Systemd/systemd-networkd).service which results in eduroam connections continually disconnecting after a short time and then reconnecting. In order to ensure that only NetworkManager is managing the eduroam connection, run

`root #``systemctl stop systemd-networkd.service`
and

`root #``systemctl disable systemd-networkd.service`
unless this service is needed for something else.

#### Roam.fi

[https://www.roam.fi/](https://www.roam.fi/) is a similar networking project like *eduroam* in Finland. The above script works also for *roam.fi*, only the SSID is different. Please set the variable `CONNAME="roam.fi"`.

### IWD

This section details eduroam configuration for standalone iwd network management.

Run the CAT installer with the `--iwd_conf` option:

`root #``./eduroam-linux-<your-org>.py --iwd_conf`
This will generate a eduroam.8021x file in the current directory. Move it to the runtime directory (see also man 5 iwd.network):

`root #``mv eduroam.8021x /var/lib/iwd/`
Provided that the username and password are correct, connecting with eduroam should now work.

### wpa\_supplicant

#### Configuration

Generate configuration file using the installation script:

`user $` `./eduroam-linux<your-org>.py` Alternatively it is possible to write the configuration file from scratch or edit existing templates.

#### Usage

`root #` `wpa_supplicant -B -i interface_name -c /path/to/conf`
### KDE Plasma settings

Below are screenshots from [KDE](https://wiki.gentoo.org/wiki/KDE) Plasma desktop environment system settings for eduroam wi-fi configuration.

- ![](https://wiki.gentoo.org/images/thumb/f/f7/KDE_Plasma_eduroam_general_configuration.png/300px-KDE_Plasma_eduroam_general_configuration.png) General configuration
- ![](https://wiki.gentoo.org/images/thumb/7/73/KDE_Plasma_eduroam_wi-fi.png/300px-KDE_Plasma_eduroam_wi-fi.png) Wi-Fi settings
- ![](https://wiki.gentoo.org/images/thumb/c/c6/KDE_Plasma_eduroam_wi-fi_security.png/300px-KDE_Plasma_eduroam_wi-fi_security.png) Wi-Fi security settings. Cyan dots indicate settings which probably vary from one institution to another.
- ![](https://wiki.gentoo.org/images/thumb/a/a2/KDE_Plasma_eduroam_ipv4.png/300px-KDE_Plasma_eduroam_ipv4.png) IPv4 settings
- ![](https://wiki.gentoo.org/images/thumb/d/d8/KDE_Plasma_eduroam_ipv6.png/300px-KDE_Plasma_eduroam_ipv6.png) IPv6 settings

## Site-specific tips

For institution-specific guidance, please contact the institution's support team. Some tips have been proposed here, and may be useful for some individual users.

### University of Bristol

The University of Bristol [has pages](https://www.wireless.bris.ac.uk/eduroam/instructions/) on configuring eduroam using NetworkManager, wpa\_supplicant, netctl and more.

### Technical University of Łódź

The Technical University of Łódź (Politechnika Łódzka) does not provide any official guidance on how to configure eduroam.[\[5\]](https://wiki.gentoo.org#cite_note-5)

Below is a config that allowed at least one student to use NetworkManager to connect to eduroam.

1. Download the `tuLodzPem.pem` file from [the University CA's site](https://ca.p.lodz.pl/tulodz/) and save it to `/etc/ca-certificates/trust-source/`
2. Copy this file to `/var/lib/iwd/eduroam.8021x` and replace the stuff \[in brackets\]

**`/var/lib/iwd/eduroam.8021x`**

This should work already.
However just to be safe go to `nmtui` and check the eduroam connection.
Once you save it the file above should begin with `# Auto-generated from NetworkManager connection "eduroam"`.

### Umeå University

Umeå University has official guidance for setting up Eduroam through Ubuntu, which can be found on [their website](https://manual.umu.se/en/install-eduroam-manually) under the "Ubuntu" tab. The instructions there can be applied to Gentoo, for example using NetworkManager and `nmtui`. To note, the institution uses TLS 1.0 for authentication rather than EAP (even though the authentication domain is `eap.ad.umu.se`), meaning logging in to download a user and root certificate from the website before setting the connection up is required.

### Simon Fraser University

Simon Fraser University [has generic pages](https://www.sfu.ca/information-systems/services/wireless-connectivity/sfunet-secure/ubuntu.html) for setting up eduroam and their secure network SFUNET-SECURE. Below is a script informed by those pages that allowed at least one student to use NetworkManager to connect to eduroam and SFUNET-SECURE. Be sure to replace the items inside the square brackets. If you are unsure about your wireless\_device, you can check with ip a, your wireless\_device name typically start with a 'w'.

A connection only needs to be added once (NetworkManager stores all the information needed to establish this connection under the 'con-name'), then it can be connected to until it is deleted. You can list all of the connections currently configured with nmcli connection show.

## See also

- [Category:Network\_management](https://wiki.gentoo.org/wiki/Category:Network_management)
- [iwd](https://wiki.gentoo.org/wiki/Iwd) — a wireless daemon intended to replace wpa\_supplicant
- [resolv.conf](https://wiki.gentoo.org/wiki/Resolv.conf) — used to configure hostname resolution.
- [WireGuard](https://wiki.gentoo.org/wiki/WireGuard) — a modern, simple, and secure VPN that utilizes state-of-the-art cryptography.
- [wpa\_supplicant](https://wiki.gentoo.org/wiki/Wpa_supplicant) — an app for [Wi-Fi](https://wiki.gentoo.org/wiki/Wi-Fi) authentication

## External resources

- [https://unix.stackexchange.com/questions/145366/how-to-connect-to-an-802-1x-wireless-network-via-nmcli](https://unix.stackexchange.com/questions/145366/how-to-connect-to-an-802-1x-wireless-network-via-nmcli) — How to connect to an 802.1x wireless network via nmcli
- [eduroam Privacy Notice](https://eduroam.org/eduroam-privacy-notice/)
- [https://monitor.eduroam.org/](https://monitor.eduroam.org/) - eduroam services status
- [CAT Diagnostics](https://cat.eduroam.org/diag/diag.php)
- [RITlug eduroam Guide](https://wiki.ritlug.com/eduroam/)
