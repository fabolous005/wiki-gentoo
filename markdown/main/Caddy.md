<!-- source: https://wiki.gentoo.org/wiki/Caddy | group: Gentoo Wiki (Main) | wiki-title: Caddy -->
---
title: Caddy
url: https://wiki.gentoo.org/wiki/Caddy
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-24"
fingerprint: a6a165d809b720b0
license: CC BY-SA 4.0
---

# Caddy

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Caddy** is a fast and extensible multi-platform HTTP/1-2-3 [web server](https://wiki.gentoo.org/wiki/Category:Web_servers) with automatic HTTPS. It provides an extensible platform which can be used as a HTTP server, reverse proxy, or static file server. Caddy and its extensions are written in [Go](https://wiki.gentoo.org/wiki/Go).

## Installation

### USE flags


### USE flags for
            [www-servers/caddy](https://packages.gentoo.org/packages/www-servers/caddy)
            
            Fast and extensible multi-platform HTTP/1-2-3 web server with automatic HTTPS

| [+filecaps](https://packages.gentoo.org/useflags/+filecaps) | Use Linux file capabilities to control privilege rather than set\*id (this is orthogonal to USE=caps which uses capabilities at runtime e.g. libcap) | 
| [dns-alidns](https://packages.gentoo.org/useflags/dns-alidns) | Adds module which allows to manage Aliyun DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.alidns | 
| [dns-azure](https://packages.gentoo.org/useflags/dns-azure) | Adds module which allows to manage Azure hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.azure | 
| [dns-cloudflare](https://packages.gentoo.org/useflags/dns-cloudflare) | Adds module which allows to manage Cloudflare hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.cloudflare | 
| [dns-cloudns](https://packages.gentoo.org/useflags/dns-cloudns) | Adds module which allows to manage ClouDNS hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.cloudns | 
| [dns-digitalocean](https://packages.gentoo.org/useflags/dns-digitalocean) | Adds module which allows to manage DigitalOcean hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.digitalocean | 
| [dns-duckdns](https://packages.gentoo.org/useflags/dns-duckdns) | Adds module which allows to manage Duck DNS hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.duckdns | 
| [dns-dynv6](https://packages.gentoo.org/useflags/dns-dynv6) | Adds module which allows to manage Dynv6 hosted DNS zones using Caddy | 
| [dns-gandi](https://packages.gentoo.org/useflags/dns-gandi) | Adds module which allows to manage Gandi hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.gandi | 
| [dns-godaddy](https://packages.gentoo.org/useflags/dns-godaddy) | Adds module which allows to manage Godaddy hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.godaddy | 
| [dns-googleclouddns](https://packages.gentoo.org/useflags/dns-googleclouddns) | Adds module which allows to manage Google Cloud hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.googleclouddns | 
| [dns-he](https://packages.gentoo.org/useflags/dns-he) | Adds module which allows to manage Hurricane Electric hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.he | 
| [dns-hetzner](https://packages.gentoo.org/useflags/dns-hetzner) | Adds module which allows to manage Hetzner hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.hetzner | 
| [dns-huaweicloud](https://packages.gentoo.org/useflags/dns-huaweicloud) | Adds module which allows to manage Huawei Cloud hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.huaweicloud | 
| [dns-linode](https://packages.gentoo.org/useflags/dns-linode) | Adds module which allows to manage Linode hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.linode | 
| [dns-mailinabox](https://packages.gentoo.org/useflags/dns-mailinabox) | Adds module which allows to manage Mail-in-a-Box hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.mailinabox | 
| [dns-namecheap](https://packages.gentoo.org/useflags/dns-namecheap) | Adds module which allows to manage Namecheap hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.namecheap | 
| [dns-netcup](https://packages.gentoo.org/useflags/dns-netcup) | Adds module which allows to manage netcup hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.netcup | 
| [dns-netlify](https://packages.gentoo.org/useflags/dns-netlify) | Adds module which allows to manage Netlify hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.netlify | 
| [dns-oraclecloud](https://packages.gentoo.org/useflags/dns-oraclecloud) | Adds module which allows to manage Oracle cloud hosted DNS zones using Caddy https://github.com/caddy-dns/oraclecloud | 
| [dns-ovh](https://packages.gentoo.org/useflags/dns-ovh) | Adds module which allows to manage OVHcloud hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.ovh | 
| [dns-porkbun](https://packages.gentoo.org/useflags/dns-porkbun) | Adds module which allows to manage porkbun hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.porkbun | 
| [dns-powerdns](https://packages.gentoo.org/useflags/dns-powerdns) | Adds module which allows to manage DNS zones of net-dns/pdns using Caddy https://caddyserver.com/docs/modules/dns.providers.powerdns | 
| [dns-rfc2136](https://packages.gentoo.org/useflags/dns-rfc2136) | Adds module which allows to manage DNS zones using RFC2136 Dynamic Updates within Caddy https://caddyserver.com/docs/modules/dns.providers.rfc2136 | 
| [dns-route53](https://packages.gentoo.org/useflags/dns-route53) | Adds module which allows to manage AWS route53 hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.route53 | 
| [dns-unifi](https://packages.gentoo.org/useflags/dns-unifi) | Adds module which allows to manage UniFi networks DNS zones using Caddy https://github.com/caddy-dns/unifi | 
| [dns-vultr](https://packages.gentoo.org/useflags/dns-vultr) | Adds module which allows to manage Vultr hosted DNS zones using Caddy https://caddyserver.com/docs/modules/dns.providers.vultr | 
| [dynamicdns](https://packages.gentoo.org/useflags/dynamicdns) | Adds module which allows querying an endpoint to get dynamic public IP and updating records with DNS providers https://caddyserver.com/docs/modules/dynamic\_dns | 
| [events-handlers-exec](https://packages.gentoo.org/useflags/events-handlers-exec) | Adds module which lets user exec command on Caddy events https://caddyserver.com/docs/modules/events.handlers.exec https://caddyserver.com/docs/caddyfile/options#event-options | 
| [security](https://packages.gentoo.org/useflags/security) | Authentication, Authorization, and Accounting. LDAP, OAuth, SAML, MFA, 2FA, JWT etc.. https://caddyserver.com/docs/modules/security | 
| [webdav](https://packages.gentoo.org/useflags/webdav) | Adds module which implements an HTTP handler for responding to WebDAV clients https://caddyserver.com/docs/modules/http.handlers.webdav | 

### Emerge

`root #``emerge --ask www-servers/caddy`
## Configuration

Caddy's default config file when using the Gentoo package is /etc/caddy/Caddyfile.

### Simple text response

To configure a basic HTTP server that responds with simple text, using a Caddyfile <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> is the easiest way to get started. The config for this looks like:

**`Caddyfile`**

```
localhost {
    respond "Hello, Gentoo!"
}
```
### Using a domain

For a more real-world setup where you have a domain you would like to host a web server on, the config will look like the following:

**`Caddyfile`**

```
example.com {
    root * /var/www/example.com # the root of the website
    tls webmaster@example.com # email on the HTTPS certificate
    file_server # enables a static file server
}
```
### Reverse proxies

Caddy allows you to use a reverse proxy to allow HTTPS connections over the Internet without having port conflicts. For reverse proxying a web server listening on port `8080` using Caddy, the following config will allow you to do so:

**`Caddyfile`**

```
sub.example.com {
    reverse_proxy :8080
}
```
## Usage

### Services

#### OpenRC

To start Caddy using OpenRC, run

`root #``rc-service caddy start`
and to have it run on boot, run

`root #``rc-update add caddy default`
#### Systemd

To start Caddy on Systemd, run

`root #``systemctl start caddy`
and to have it start on boot, run

`root #``systemctl enable caddy`
### Command line usage

#### Using a specified config file

To start Caddy with a config file that is not /etc/caddy/Caddyfile, run

`user $``caddy run --config /path/to/Caddyfile`
#### Reloading

Caddy config can be reloaded in a zero-downtime fashion. To reload Caddy from the command-line, you can run

`user $``caddy reload`
or for reloading the service:

`root #``rc-service caddy reload``root #``systemctl reload caddy`
## See also

- [Apache](https://wiki.gentoo.org/wiki/Apache) — an efficient, extensible [web server](https://wiki.gentoo.org/wiki/Category:Web_Servers). It is one of the most popular web servers used the Internet.
- [Nginx](https://wiki.gentoo.org/wiki/Nginx) — a robust, small, high performance [web server](https://wiki.gentoo.org/wiki/Category:Web_servers) and reverse proxy server.

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [The Caddyfile — Caddy Documentation](https://caddyserver.com/docs/caddyfile), caddyserver. Retrieved on May 24, 2024
