<!-- source: https://wiki.gentoo.org/wiki/Privoxy | group: Gentoo Wiki (Main) | wiki-title: Privoxy -->
---
title: Privoxy
url: https://wiki.gentoo.org/wiki/Privoxy
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-29"
fingerprint: "9c323b1aee34388d"
license: CC BY-SA 4.0
---

# Privoxy

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Privoxy** is a non-caching web proxy server with advanced filtering capabilities which can improve privacy. It works by removing or modifying elements of a HTTP request and its response, either on the headers or on the body of the request.

Although comparable to some browser extensions, being a server, it allows different programs to use it, removing the need to add extensions to each browser and with it, its potential associated problems like incompatibilities between extensions. It also helps to reduce browser fingerprinting thanks to the reduction of installed extensions.

It may be combined with caching proxies like [squid](https://wiki.gentoo.org/wiki/Squid) to improve its overall speed.

## Installation

### USE flags


| [+acl](https://packages.gentoo.org/useflags/+acl) | Add support for Access Control Lists | 
| [+fast-redirects](https://packages.gentoo.org/useflags/+fast-redirects) | Support fast redirects | 
| [+force](https://packages.gentoo.org/useflags/+force) | Allow single-page disable (force load) | 
| [+image-blocking](https://packages.gentoo.org/useflags/+image-blocking) | Allows the +handle-as-image action, to send "blocked" images instead of HTML | 
| [+jit](https://packages.gentoo.org/useflags/+jit) | Enable PCRE jit (recommended) | 
| [+mbedtls](https://packages.gentoo.org/useflags/+mbedtls) | Use net-libs/mbedtls for HTTPS filtering | 
| [+openssl](https://packages.gentoo.org/useflags/+openssl) | Use dev-libs/openssl for HTTPS filtering | 
| [+stats](https://packages.gentoo.org/useflags/+stats) | Keep statistics | 
| [+threads](https://packages.gentoo.org/useflags/+threads) | Enable POSIX threads. Highly recommended, otherwise both build and run-time features may not work properly. | 
| [+zlib](https://packages.gentoo.org/useflags/+zlib) | Decompress zlib compressed data using virtual/zlib before filtering | 
| [+zstd](https://packages.gentoo.org/useflags/+zstd) | Enable support for ZSTD compression | 
| [brotli](https://packages.gentoo.org/useflags/brotli) | Decompress brotli compressed data using app-arch/brotli before filtering | 
| [client-tags](https://packages.gentoo.org/useflags/client-tags) | Enable support for client-specific tags | 
| [compression](https://packages.gentoo.org/useflags/compression) | Allow privoxy to compress buffered content before sending to the client, if it supports it | 
| [editor](https://packages.gentoo.org/useflags/editor) | Enable the web-based actions file editor | 
| [extended-host-patterns](https://packages.gentoo.org/useflags/extended-host-patterns) | Enable and require PCRE syntax in host patterns. You must convert action files to PCRE, see privoxy-url-pattern-translator.pl (see tools USE flag). Use at your own risk! | 
| [extended-statistics](https://packages.gentoo.org/useflags/extended-statistics) | Gather extended statistics | 
| [external-filters](https://packages.gentoo.org/useflags/external-filters) | Allow to filter content with scripts and programs. Experimental | 
| [fuzz](https://packages.gentoo.org/useflags/fuzz) | Exposes Privoxy internals to input from files or stdout. Intended for fuzzing testing | 
| [graceful-termination](https://packages.gentoo.org/useflags/graceful-termination) | Allow to shutdown Privoxy through the webinterface | 
| [ipv6](https://packages.gentoo.org/useflags/ipv6) | Add support for IP version 6 | 
| [lfs](https://packages.gentoo.org/useflags/lfs) | Support large files (>2GB) on 32-bit systems | 
| [png-images](https://packages.gentoo.org/useflags/png-images) | Use PNG format instead of GIF for built-in images | 
| [sanitize](https://packages.gentoo.org/useflags/sanitize) | Enable asan, msan and usan sanitizers. Your compiler must support them | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | HTTPS inspection support. Enables privoxy to perform SSL MITM filtering, see docs, use with care | 
| [toggle](https://packages.gentoo.org/useflags/toggle) | Support temporary disable toggle via web interface | 
| [tools](https://packages.gentoo.org/useflags/tools) | Install log parser, regression tester and user agent generator tools | 
| [whitelists](https://packages.gentoo.org/useflags/whitelists) | Support trust files (white lists) | 

### Emerge

To install [net-proxy/privoxy](https://packages.gentoo.org/packages/net-proxy/privoxy):

`root #``emerge --ask net-proxy/privoxy`
### Service

#### OpenRC

To have privoxy start at boot:

`root #``rc-update add privoxy default`
To start manually:

`root #``rc-service privoxy start`
#### systemd

To have privoxy start at boot:

`root #``systemctl enable privoxy.service`
To start manually:

`root #``systemctl start privoxy.service`
## Configuration

Once the server is running, clients have to be made aware of it, to do so, the proxy configuration can be adjusted on each program. But many applications, such as browsers, [Wget](https://wiki.gentoo.org/wiki/Wget), [Yt-dlp](https://wiki.gentoo.org/wiki/Yt-dlp), and others, evaluate a system variable and no longer need to be manually configured to use the web proxy. Create a new file, e.g., /etc/env.d/99proxy with:

**`/etc/env.d/99proxy`**

and activate it:

`root #``env-update`
### Enable logging

These are the recommended settings:

**`/etc/privoxy/config`**

### Clients

#### Firefox

Edit > Settings > Network Settings > Settings > Manual proxy configuration

**HTTP Proxy** 127.0.0.1
**Port** 8118

Mark the checkbox *Also use this proxy for HTTPS*

#### Chromium

`user $``chromium --proxy-server="localhost:8118"`
### Advanced configuration

The default values on Privoxy should work well for most cases, but further configuration can be made using the following methods.

#### Web configuration

Pointing a browser like [Firefox](https://wiki.gentoo.org/wiki/Firefox) to [status and configuration page](http://config.privoxy.org/show-status).

#### Editing configuration files manually

All the configuration files are located at /etc/privoxy.

The following are two common cases for modifying the base configuration:

##### To change the default port where Privoxy listens

Look for:

**`/etc/privoxy/config`**

Change it to the desired port, for instance, if the desired port is 8080:

**`/etc/privoxy/config`**

##### To block specific sites

Using a text editor as root edit /etc/privoxy/user.action, and add this at the end:

**`/etc/privoxy/user.action`**

## Testing

Once the server is running, different tools and methods can be used to test if it is working properly.

### lsof

If no client has been used yet, only the first line will be present. If a client has issued a request then more results will be present on the output of the command:

`root #``lsof -i | grep privoxy`
privoxy    5482 privoxy    4u  IPv4  15135      0t0  TCP localhost:8118 (LISTEN)
privoxy    5482 privoxy    7u  IPv4 246374      0t0  TCP localhost:8118->localhost:54486 (ESTABLISHED)
privoxy    5482 privoxy    9u  IPv4 246376      0t0  TCP localhost:55012->localhost:9050 (ESTABLISHED)

### nmap

`root #``nmap -sS 127.0.0.1 -p 8118`
Starting Nmap 7.92 ( https://nmap.org ) at 2021-10-20 10:07 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.000050s latency).
PORT     STATE SERVICE
8118/tcp open  privoxy
Nmap done: 1 IP address (1 host up) scanned in 0.09 seconds

### On a browser

Following the link [config.privoxy.org](http://config.privoxy.org/) will trigger a default filter on privoxy which will serve a page with the text:

**This is Privoxy 3.0.32 on localhost (127.0.0.1), port 8118, enabled**

a section with the Privoxy Menu and a section with support links.

## Usage

### By itself

Once clients are made aware of the proxy by adjusting their settings to point to the privoxy server they will start using it, including any changes made to the configuration files since the server doesn't have to be restarted to update its behaviour

### Forwarding traffic through Tor

[Tor](https://wiki.gentoo.org/wiki/Tor) is a powerful tool for the anonymity seekers and for many years has been used in combination with privoxy.

As root edit /etc/privoxy/config, look for:

**`/etc/privoxy/`**

Change it to:

**`/etc/privoxy/`**

### Using Squid as cache proxy

After installing [Squid](https://wiki.gentoo.org/wiki/Squid) and Privoxy, set the clients to use Squid and set Squid to forward traffic to Privoxy.

For instance, change the proxy configuration on [Firefox](https://wiki.gentoo.org/wiki/Firefox) following [the steps mentioned above](https://wiki.gentoo.org#Firefox) but instead of port 8118 set it to 3128. After that, as root edit /etc/squid/squid.conf and add the following lines:

**`/etc/squid/squid.conf`**

### Using Squid + Privoxy + Tor

After following all the steps above, the full chain should be working. To confirm that everything is working fine, visit the following 2 URLs.

The first URL should get the same result as the [browser test done before](https://wiki.gentoo.org#On_a_browser).

The second URL should end in a page with the text **Congratulations. This browser is configured to use Tor**.

## Caveats

Although removing elements from the page makes it lighter and the privoxy process tiself is not big or slow, if too many filters are working at the same time, the final result may be a bit slower than browsing without privoxy.

## See also

- [Squid](https://wiki.gentoo.org/wiki/Squid) — a web cache and a proxy server application used speed up web browsing.
- [Tor](https://wiki.gentoo.org/wiki/Tor) — an onion routing Internet anonymity system.
