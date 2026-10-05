<!-- source: https://wiki.gentoo.org/wiki/WeeChat/Glowing_Bear_Guide | group: Gentoo Wiki (Main) | wiki-title: WeeChat/Glowing Bear Guide -->
---
title: WeeChat/Glowing Bear Guide
url: https://wiki.gentoo.org/wiki/WeeChat/Glowing_Bear_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-25"
fingerprint: cbfbf55914024057
license: CC BY-SA 4.0
---

# WeeChat/Glowing Bear Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Glowing Bear is a web frontend relay for the WeeChat IRC client. Although the [Glowing Bear site itself](https://www.glowing-bear.org/) can be used to securely connect to a WeeChat relay, it is also possible to host Glowing Bear from a home server. This article will walk users through the process of setting up a local Glowing Bear instance on Gentoo Linux.

Version >=0.4.2 of WeeChat is necessary in order to obtain proper relay support. Older versions are incompatible with this guide.

## Concepts

Some knowledge on following topics will be needed in order to complete this guide:

- [WeeChat](https://wiki.gentoo.org/wiki/WeeChat) - As expected, WeeChat is the base Glowing Bear will connect to. Without it this guide is meaningless.
- Web servers - There are a few choices here: [Apache](https://wiki.gentoo.org/wiki/Apache), [Lighttpd](https://wiki.gentoo.org/wiki/Lighttpd), or [Nginx](https://wiki.gentoo.org/wiki/Nginx). Any one of them will get the job done.
- [Git](https://wiki.gentoo.org/wiki/Git) - Git is version control software. It is the primary means of obtaining and then maintaining a local copy of Glowing Bear.
- Domain name management (optional) - Used to forward information computer looking the local Glowing Bear copy that is behind a firewall.
- SSL certificate generation (highly recommended) - Used to establish a secure login page.
- A web browser that can accept self-signed certificates - Firefox is easiest, although it is possible to accept self-signed certs with Chromium.

WeeChat, the software back-end of Glowing Bear, can be hosted locally or by another server on the network. For many in the Gentoo crowd, being connected to IRC *all* the time is a must. For more information concerning the IRC user mindset check out the [IRC guide](https://wiki.gentoo.org/wiki/IRC/Guide).

## Installation

### The web server

Setting up the web server with SSL encryption is the most difficult portion of this guide. Thankfully there are other articles that have been written to handle this task. For a the speediest setup using nginx is recommended. Glowing Bear does not need a large, dependency heavy HTTP server in order to present a bit of HTML and JavaScript for a single user use-case.

The goal here is to configure the web server to be reachable at least on the local area network (LAN), but ideally from a wide area network (WAN), which typically is the internet. Remember: it does not matter what web server is used; just that one can be configured to host the Glowing Bear front-end source code.

Travel over to the [nginx article](https://wiki.gentoo.org/wiki/Nginx) if further instructions are needed on setting up a simple web server.

### Getting Glowing Bear

Now that the web server is ready to rock and roll a simple git clone is needed to pull the Glowing Bear sources from the hosting service to the server's default HTTP directory. In the example below /var/www is being used as the main HTTP directory. This location may vary depending on which web server has been chosen to serve Glowing Bear.

`user $````
cd /var/www
```
`user $````
sudo mkdir glowingbear
```
`user $````
sudo chown nginx:nginx glowingbear
```
## Configuration

### Starting the web server

Once the git repository has been cloned the next step is to start the web server and connect via the [local area network (LAN)](https://en.wikipedia.org/wiki/Local_area_network).

Use the appropriate command to start the web server. This will very based on which server was chosen. Presuming nginx is being used:

OpenRC:

`root #``service nginx start`
systemd:

`root #``systemctl start nginx`
### Access from the WAN

Opening a port through the firewall to the WAN is only necessary if access to Glowing Bear is needed when outside the LAN (home network). Instructions for this will vary from situation to situation, however for the majority of home users this will most likely consist of accessing the home router in order to open the port and send the Glowing Bear traffic to the system running the WeeChat relay. This means the IP address of the machine that is hosting Glowing Bear will need to be known.

Open the port from the WAN (Internet) to the IP address of the host machine. Make sure TCP port 80 is forwarded since that is the standard web traffic port.

If everything was successful Glowing Bear should be accessible by opening a browser of choice and entering the WAN IP as the URL.

### Setting up WeeChat

The final step in the process is setting up WeeChat to connect with the relay. If this step is not performed the rest of this article will not be of much practical use since WeeChat serves as the software back-end. If necessary run through the [official WeeChat quick start guide](http://www.weechat.org/files/doc/stable/weechat_quickstart.en.html) before continuing.

Once user information has been configured, WeeChat will need to be set as a relay in order for Glowing Bear to connect. Run the following commands from within a running WeeChat instance to set a relay password and instruct the relay to run on port `9001`:

```
/set relay.network.password <password>
```
```
/relay add weechat 9001
```
Be sure to replace `<password>` in the first command with a non-guessable password. This is important for ensuring secure usage.

### Accepting self-signed SSL certificates

Firefox makes the task of accepted self-signed SSL certificates somewhat simple. Simply visit the Glowing Bear URL. A warning should be displayed from Firefox that the SSL certificate is "not secure" because it is self-signed and cannot be validated against a trusted certificate authority.

[Chromium](https://wiki.gentoo.org/wiki/Chromium) (unbranded Google [Chrome](https://wiki.gentoo.org/wiki/Chrome) has a more tedious process of accepting self signed certificates.

Be sure to visit the `<PORT>/weechat` URL as well as the base URL.

## See also

- [WeeChat](https://wiki.gentoo.org/wiki/WeeChat) - The "big dog" IRC client capable of doing most things gracefully.
- [Irssi](https://wiki.gentoo.org/wiki/Irssi) - Another text mode IRC client.
- [IRC](https://wiki.gentoo.org/wiki/IRC) - An article that lists some IRC software available through the main Gentoo repository.
