<!-- source: https://wiki.gentoo.org/wiki/Juniper_Network_Connect | group: Gentoo Wiki (Main) | wiki-title: Juniper Network Connect -->
---
title: Juniper Network Connect
url: https://wiki.gentoo.org/wiki/Juniper_Network_Connect
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-01"
fingerprint: aecb7e31e529f5b8
license: CC BY-SA 4.0
---

# Juniper Network Connect

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Article status**

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

Juniper's Network Connect document to describe the setup running on a 64-bit linux client. This describes a method to setup a client VPN connection using a webbrowser (SSL VPN). Documentation of a working setup as of October 2013 on a target network that requires login via a web page, and it has multiple pages on the portal for different groups. Running client version 7.1. The VPN client would not start automatically or complete when manually invoked using `ncsvc`.

# Configuration

## Stepwise

Go to the network portal web page and examine page source for REALM:

- Login through web portal, attmpt to intiate network connect
- Software downloads and installs into \~/.juniper\_network/network\_connect/
- Examine the cookies for the site and find DSID. *This will have to be refreshed each time*

Change path to the installation path {Path|\~/.juniper\_network/network\_connect/}:

`user $``cd ~/.juniper_network/network_connect`
Get the certificate, e.g.:

`user $``openssl s_client -connect portal.example.net:443 -showcerts < /dev/null 2> /dev/null |  openssl x509 -outform der > cert.der`
Compile the lbncui.so into an executable file:

`user $```gcc -m32 -Wl,-rpath,`pwd` -o ncui libncui.so``
Finally execute `ncui` using following command:

Where [https://portal.example.net/dana-na/auth/url\_0/welcome.cgi](https://portal.example.net/dana-na/auth/url_0/welcome.cgi) is the full path to the login page of the portal.

When the connection has been established be a TUN device will be created. This can be verfied using the ip link show up command.

## Kernel

First make sure that TUN is enabled in your kernel as this is required to be able to create the tunnel to your vpn. Personally, I build this into the kernel and not as a module.

**'make menuconfig' options**

## Prerequisites

Software requirements: SUN Java JRE (both 64 and 32 bit versions) with nsplugin , e.g.:

## Concise Connection Steps

I'm writing this section to explain how I connect to Juniper Network Connect in a more succinct and consolidated manner. Recent versions of Google Chrome block the Java plugin, so it requires a different approach. This method does not use Java and is, personally, a better way.

### Installation Steps

You will need to download ncLinuxApp.jar for your version of Juniper Network Connect. Replace "yoursite" with the address for your vpn website.

Once you have ncLinuxApp.jar download, create a folder somewhere in your home directory. This is where you will be running the network connect client from.

`user $``mkdir ~/juniper_networks`
Now extract the contents of ncLinuxApp.jar

`user $``unzip ncLinuxApp.jar`
Once you have the files extracted, you will need to change the ownership and set file permissions for a couple files. You will need to be root.

Change ownership of ncsv and set to executable

`root #``chown root ncsv && chmod +x ncsv`
Set ncdiag to executable as well. Ownership of this file doesn't seem to need to be root

`root #``chmod +x ncdiag`
As the instructions state in the previous section, you will need to obtain the certificate from your Juniper installation.

`user $``openssl s_client -connect yoursite.net:443 -showcerts < /dev/null 2> /dev/null |  openssl x509 -outform der > cert.der`
Compile libncui.so for your arch. This creates the executable you will need. This must be done as your user.

`user $```gcc -m32 -Wl,-rpath,`pwd` -o ncui libncui.so```user $``chmod +x ncui`
Instructed in the previous section, you will need to obtaini the REALM and DSID from your Juniper installation. The REALM is found in the login form on the front page of your Juniper site and the DSID can be obtained from your cookies after logging into the site.

The one annoying thing about this is that you do have to log into your Juniper site to obtain the DSID everytime. At least it does work! I hope this guide helps others in need! :)

## Split tunneling

[Workaround for JUNIPER VPN split-tunneling restriction](https://www.digitalinternals.com/124/20090430/workaround-for-juniper-vpn-split-tunneling-restriction/) describes methods to achieve split tunneling.

Using `LD_PRELOAD` to preload a custom library to redirect reads to /proc/net/route to another file seem promising, but proved problematic on a 64-bit client. see [https://gist.github.com/anonymous/6777345](https://gist.github.com/anonymous/6777345)

Patching the ncsvc binary can disable the route monitoring function, allowing one to change routes as needed manually or by script. Without patching, a route monitor may be in place that will disconnect if routes are changed.

There are probably many ways to achieve, but one tested is to convert a conditional jump statement in the route monitoring routine:

1. make backup copy of ncsvc
2. open ncsvc in disasembler
3. search for text "no routes to monitor" in the disassembly
4. a few lines up should be something that looks like

.text:0805CC9F                 mov     \[ebp+var\_19\], 0
.text:0805CCA3                 cmp     dword ptr \[eax+60h\], 0
.text:0805CCA7                 jnz     loc\_805CE1A
.text:0805CCAD                 sub     esp, 8
.text:0805CCB0                 push    offset aNoRoutesToMoni ; "no routes to monitor"

1. the jnz (or possibly jne) signals the program to jump if the previous step is not zero (or equal). Change this to invert the conditional, ie jump if zero (or equal).
2. To do so, look at the hexdump for this bit of code. Depending on your debugger, you may be able to change it within the program, or else open up the ncsvc binary in a hexeditor and find the corresponding bits.
3. The bits will likely be either start with 75 ?? ?? or 0F 85 ?? ??
4. change the 75 to 74, or 85 to 84.
5. save and test.

### Sample route

In order to achieve desirerd access to VPN resources, local LAN resources, and internet resources, possible post-connect commands:

Consider ncsvc gave original default gw has a higher metric, added a second default with a lower metirc, and target vpn resources are on 10.0.0.0 and 170.0.0.0, and a tun0 ip of 10.15.15.15 (besides principal resources, check the vpn network's dns servers etc)

`root #````
route del default
```
`root #````
route del default
```
`root #````
route add default gw 192.168.1.1 metric 2
```
`root #````
route del 192.168.1.0 dev eth0
```
`root #````
route del -net 192.168.1.0 netmask 255.255.255.0 dev eth0
```
`root #````
route del -net 192.168.1.0 gw 10.15.15.15 netmask 255.255.255.0
```
`root #````
route add -net 192.168.1.0 netmask 255.255.255.0 dev eth0
```
`root #````
route add -net 170.0.0.0  netmask 255.0.0.0 gw 10.15.15.15 dev tun0
```
`root #````
route add -net 10.0.0.0  netmask 255.0.0.0 gw 10.15.15.15 dev tun0
```
`root #````
echo "nameserver 192.168.1.1" >> /etc/resolv.conf
```
