<!-- source: https://wiki.gentoo.org/wiki/VPN_services | group: Gentoo Wiki (Main) | wiki-title: VPN services -->
---
title: VPN services
url: https://wiki.gentoo.org/wiki/VPN_services
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-01"
fingerprint: "61cf2783400aa9e"
license: CC BY-SA 4.0
---

# VPN services

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

VPN services are offered by several companies. They allow users to protect their privacy and security while using the Internet. Guides to using various service providers on Gentoo are below.

## Private Internet Access

The following will auto-start [openvpn](https://wiki.gentoo.org/wiki/OpenVPN) upon boot:

1. Buy a subscription and log in
2. `root #``echo "net-vpn/openvpn passwordsave examples" >> /etc/portage/package.use # optional`
3. `root #``emerge --ask openvpn`
4. Go to [https://www.privateinternetaccess.com/pages/client-control-panel](https://www.privateinternetaccess.com/pages/client-control-panel) and locate the "PPTP/L2TP/SOCKS Username and Password"
5. Generate Username/Password if they are not there
6. In the next step, replace USERNAME and PASSWORD with the located or generated ones. `root #``cd /etc/openvpn``root #``echo "USERNAME" >> login.conf``root #``echo "PASSWORD" >> login.conf`
7. Download the privateinternetaccess configurations and certificates. `root #``mkdir -p pia``root #``cd pia``root #``unzip openvpn.zip``root #``cp US\ East.ovpn ../openvpn.conf # replace with your desired region``root #``cd ..``root #``ln -s pia/ca.rsa.2048.crt``root #``ln -s pia/crl.rsa.2048.pem`
8. Modify the configuration file and permissions. `root #``echo "auth-nocache" >> openvpn.conf``root #``echo "auth-user-pass login.conf" >> openvpn.conf``root #``chmod 600 openvpn.conf login.conf pia/*``root #``chmod 700 pia`
9. Start the service and add to default runlevel. `root #``/etc/init.d/openvpn start``root #``rc-update add openvpn # don't use this line if you don't want openvpn upon boot`

If [Openvpn](https://wiki.gentoo.org/wiki/OpenVPN) doesn't connect you would want to add www.privateinternetaccess.com's DNS servers to your /etc/resolv.conf.head file. 
Create '/etc/resolv.conf.head' file if it doesn't already exist, edit the file by adding the two DNS servers:

**`/etc/resolv.conf.head`**

Go to www.privateinternetaccess.com's to make sure you entered the correct DNS servers:

- Click on 'Client Support Tab' and scroll down to 'DNS leak protection' section

Save and Reboot.

## Free VPN

1. Go to [https://freevpn.me/accounts/](https://freevpn.me/accounts/) and click on "Download OpenVPN Certificate Bundle". You will get a zip file with the ovpn configuration files for all the Free VPN servers. .ovpn files are [Openvpn](https://wiki.gentoo.org/wiki/OpenVPN) configuration files with the client configuration at the beginning of the file, followed by inlined certificates. Choose one of these servers and copy the corresponding directory into /etc/openvpn (You can use mc for that kind of task). I renamed the directory in order to get rid of the spaces in its name.
2. On the website, click on the left menu to choose the server you want. When done click on "Accounts", as example [https://freevpn.me/accounts/](https://freevpn.me/accounts/) . On that page, you can read the user and password needed for that server in the Open VPN section. In the console do, as example for freevpn.me using udp on port 40000:
3. To not get prompted for the username and password: `root #``cd /etc/openvpn/1-FreeVPN.me-FR``root #``echo "<username>" > auth``root #``echo "<password>" >> auth` and change the line: FILE**`/etc/openvpn/1-FreeVPN.me-FR/FreeVPN.me-UDP-40000.ovpn`** into FILE**`/etc/openvpn/1-FreeVPN.me-FR/FreeVPN.me-UDP-40000.ovpn`**
4. You can test it with: `root #``openvpn FreeVPN.me-UDP-40000.ovpn`
5. In order to run it at boot time, follow one of the methods described here: [OpenVPN#Gentoo\_specifics](https://wiki.gentoo.org/wiki/OpenVPN#Gentoo_specifics)

### Troubleshooting

It [Openvpn](https://wiki.gentoo.org/wiki/OpenVPN) fail to connect with something like:

`root #``openvpn FreeVPN.me-UDP-53.ovpn`
...
TLS Error: TLS key negotiation failed to occur within 60 seconds (check your network connectivity)

it can be a firewall blocking you, but other vpn servers will fails too in that case. It other vpn servers are working, just try another configuration file. At that time of writing, FreeVPN.me-UDP-53 fails with that error, but FreeVPN.me-UDP-40000 is running just fine.

## Troubleshooting

The above setup requires that root has write permission on /etc/resolv.conf. To make sure this is the case run

`root #``lsattr /etc/resolv.conf`
If the output looks like this

`root #``lsattr /etc/resolv.conf`
----i---------e---- /etc/resolv.conf

than, since "i" means that the file has the immutable bit, not even the root can write on it. To change that just run

`root #``chattr -i /etc/resolv.conf`
and reboot.

## Windows L2TP/IPsec VPN

Users wishing to connect to their company's L2TP/IPsec network can follow the below guide. Assuming you are already running a gnome profile, follow these steps:

**`/etc/portage/package.use`**

`root #``emerge --ask net-vpn/networkmanager-l2tp`
The USE flag modifications are necessary to bypass sepolicy issues with the charon daemon. These flags are not recommended if you're running an L2TP server (according to the ebuild).

## VPN on your remote machine or cloud

Multiple solutions exists, a few of them:
