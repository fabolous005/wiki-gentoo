<!-- source: https://wiki.gentoo.org/wiki/PPP | group: Gentoo Wiki (Main) | wiki-title: PPP -->
---
title: PPP
url: https://wiki.gentoo.org/wiki/PPP
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-04-20"
fingerprint: aa938950148339cd
license: CC BY-SA 4.0
---

# PPP

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**PPP** (**P**oint-to-**P**oint **P**rotocol) is commonly used in establishing a direct connection between two networking nodes. It can provide connection authentication, transmission encryption, and compression.

## Installation


| [activefilter](https://packages.gentoo.org/useflags/activefilter) | Enables active filter support | 
| [atm](https://packages.gentoo.org/useflags/atm) | Enable Asynchronous Transfer Mode protocol support | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 

Portage has a USE flag `ppp` for enabling support for PPP for other packages.

**`/etc/portage/make.conf`**

After setting global USE flags update your system to the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
Or emerge [net-dialup/ppp](https://packages.gentoo.org/packages/net-dialup/ppp) package manually:

`root #``emerge --ask net-dialup/ppp`
### Kernel

Following kernel options need to be enabled, to support **PPPoE**, which is used in most cases.

| Optional PPP options |  |  | 
|---|---|---|
| Option | Driver | Description | 
|---|---|---|
| PPP BSD-Compress compression | ppp\_bsdcomp | (Not recommended) Support for data compression. "PPP Deflate compression" is preferable. | 
| PPP filtering | - | Support for packet filtering. | 
| PPP MPPE compression (encryption) | ppp\_mppe | Driver for [Microsoft Point-to-Point Encryption](https://en.wikipedia.org/wiki/Microsoft_Point-to-Point_Encryption). | 
| PPP multilink support | - | Support for PPP multilink to combine serveral lines. | 
| PPP over Ethernet | pppoe | Driver for [PPPoE](https://wiki.gentoo.org/wiki/PPPoE). | 
| PPP support for sync tty ports | ppp\_sync\_tty | Support for synchronous devices. | 

Finally you need to rebuild linux, install and boot new kernel with PPP support.

## Configuration

Provided eth0 following lines should be added for PPPoE connection:

**`/etc/conf.d/net`**

**Add:**

Create an init script for the PPP device by symlinking to net.lo:

`root #``ln -s /etc/init.d/net.lo /etc/init.d/net.ppp0``root #``/etc/init.d/net.ppp0 start`
## Example setup with systemd and automatic connection

First, create the configuration file:

**`/etc/ppp/peers/provider`**

```
plugin pppoe.so
# network interface
enp41s0
# login name
name "you_login_to_ISP"
usepeerdns
persist
# Uncomment this to enable dial on demand
#demand
#idle 180
defaultroute
defaultroute-metric 1023
hide-password
noauth
#linkname eth0
ifname eth0
```
enp41s0 is the network interface card to use. It can be found with ip link command.

Next, create the password secrets file:

**`/etc/ppp/chap-secrets`**

```
# Secrets for authentication using CHAP
# client        server  secret                  IP addresses
your_login_to_ISP   *       your_secret_password
```
For this to work at system startup, add the following unit in /etc/systemd/system/pppoe.service:

**`/etc/systemd/system/pppoe.service`**

```
[Unit]
Description=PPPoE connection
BindsTo=sys-subsystem-net-devices-enp41s0.device
After=sys-subsystem-net-devices-enp41s0.device
 
[Service]
Type=forking
PIDFile=/var/run/eth0.pid
#RemainAfterExit=true
ExecStart=/usr/sbin/pon
ExecStop=/usr/sbin/poff provider
 
[Install]
WantedBy=multi-user.target
```

sys-subsystem-net-devices-enp41s0.device is the network device via which pppoe will connect on. To find out the exact name you can use the following bash command

`root #``systemctl list-units | grep device | grep net`
Optionally, create a service unit to handle wake up after sleep:

**`/etc/systemd/system/pppoe-after-wakeup.service`**

```
[Install]
WantedBy=sleep.target
 
[Unit]
After=systemd-suspend.service systemd-hybrid-sleep.service systemd-hibernate.service
 
[Service]
Type=simple
ExecStart=/bin/systemctl restart pppoe
```

Finally, test it with:

`root #````
systemctl start pppoe
```
`root #````
systemctl start pppoe-after-wakeup
```
If it works as expected, enable them on a permanent basis:

`root #````
systemctl enable pppoe
```
`root #````
systemctl enable pppoe-after-wakeup
```
