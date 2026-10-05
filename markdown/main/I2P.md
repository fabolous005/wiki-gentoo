<!-- source: https://wiki.gentoo.org/wiki/I2P | group: Gentoo Wiki (Main) | wiki-title: I2P -->
---
title: I2P
url: https://wiki.gentoo.org/wiki/I2P
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-08"
fingerprint: b6b25313b32131c1
license: CC BY-SA 4.0
---

# I2P

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Invisible Internet Project (**I2P**) is an anonymous network, similar to [Tor](https://wiki.gentoo.org/wiki/Tor). The key difference is that I2P is internal, focusing on providing anonymous services within the network rather than proxying traffic to the regular internet (although some proxy services do exist).

## Installation

### Java

`root #``emerge --ask net-vpn/i2p`
#### USE flags


### i2pd (C++)

`root #``emerge --ask net-vpn/i2pd`
#### USE flags


## Setup

### Services

Examples below are for java implementation of the i2p. When using the C++ version, substitute **i2p** for **i2pd.**

#### OpenRC

To start the i2p service when the system boots:

`root #``rc-update add i2p default`
To start the i2p service now:

`root #``rc-service i2p start`
#### systemd

To start the i2p service when the system boots:

`root #``systemctl enable i2p.service`
To start the i2p service now:

`root #``systemctl start i2p.service`
### Configuration

Most I2P configuration is done in the Router Console, accessible via web browser at `localhost:7657` once the router service has been started.

Default configuration for i2pd is available at `localhost:7070`.

#### Firewall

I2P selects a random port between 9000 and 31000 for inbound traffic when the router is first run.  This port is forwarded automatically by UPnP, but if your gateway/firewall does not support UPnP, it will need to be manually forwarded (both TCP and UDP) for best performance. Visit [http://localhost:7657/confignet](http://localhost:7657/confignet) to find out which port.

#### Browser

You can use a pac file to delegate browser requests to different proxies. Here connections to localhost are handled directly (no proxy). Eepsites are handled by I2P proxy on port 4444. Other traffic goes via Tor SOCKS proxy on running on port 9050.

**`/usr/local/proxy.pac`**

```
function FindProxyForURL(url, host)
{
   if(host.match(/^(localhost|127[.]0[.]0[.]1|192[.]168[.]1[.]1)$/))
       return 'DIRECT';
   if(host.match(/[.]i2p$/))
       return 'PROXY 127.0.0.1:4444';
   return 'SOCKS 127.0.0.1:9050';
}
```
Save this file as /usr/local/proxy.pac, and point your browser to it. Most browsers accept Proxy configuration URL, where you can specify `file:///usr/local/proxy.pac`.

I2P's CSS can make browsers sluggish. You can add the following to your profile\_dir/chrome/userContent.css to speed up rendering:

**`profile_dir/chrome/userContent.css`**

```
@-moz-document url-prefix('http://localhost:7657/')
{
   * { filter: none !important; background-image: none !important; }
}
```
## Usage

### Eepsites

To access websites hosted on the I2P network, a web browser must be configured to use a proxy at `localhost:4444` for HTTP and `localhost:4445` for HTTPS.  This can be accomplished globally in most browsers' proxy settings, or specifically for sites with the .i2p TLD using a plugin like FoxyProxy for [Firefox](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard) or [Chrome](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp)
See also: [https://geti2p.net/en/about/browser-config](https://geti2p.net/en/about/browser-config)

### Bittorrent

I2PSnark, the I2P Bittorrent client, is accessible at `localhost:7657/i2psnark` with no additional configuration.  However, the above Eepsite configuration is necessary to reach the trackers on which the torrents are found.

### IRC

Using any IRC client, set up a connection to `localhost:6668`
No account creation is required.  If using Pidgin, be sure to fill in the **Ident name** and **Real name** fields in addition to **Username**, otherwise Pidgin may expose identity information from other configured accounts.

### SSH

*openssh* doesn't have any native support for SOCKS5, so you will need to install *openbsd-netcat*. You'll need to modify your SSH config too. It is possible with *netcat' also but the configuration below uses flags specific to the OpenBSD variant.*

`root #``emerge --ask net-analyzer/openbsd-netcat`
This enables proxying through a SOCKS5 I2P tunnel for all *.i2p* hosts. You will need to go to [http://localhost:7657/i2ptunnelmgr](http://localhost:7657/i2ptunnelmgr) and create a SOCKS5 **client** tunnel. Note the port you have used and replace '1234' in the below config with it.

**`~/.ssh/config`**

## See also

- [Tor](https://wiki.gentoo.org/wiki/Tor) - An onion routing internet anonymity system.
