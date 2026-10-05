<!-- source: https://wiki.gentoo.org/wiki/Varnish | group: Gentoo Wiki (Main) | wiki-title: Varnish -->
---
title: Varnish
url: https://wiki.gentoo.org/wiki/Varnish
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2017-05-11"
fingerprint: b2731cc32f9469c7
license: CC BY-SA 4.0
---

# Varnish

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Varnish** is a webcache and HTTP accelerator. It can either serve cached content, or retrieve content from a server and cache it. This helps to reduce I/O pressure for web servers that are serving many clients or have many requests.

## Installation

### USE flags

*www-servers/varnish*correct?

### Emerge

Install [www-servers/varnish](https://packages.gentoo.org/packages/www-servers/varnish)

`root #``emerge --ask www-servers/varnish`
## Configuration

### Files

#### Global

Configuration is controlled by the /etc/varnish/default.vcl file.

**`/etc/varnish/example.vcl`**

Any traffic pointed at port 8080 will travel through varnish.

### Service

#### OpenRC

To start varnish immediately:

`root #``rc-service varnishd start`
To start varnish at boot:

`root #``rc-update add varnishd default`
#### systemd

To start varnish on boot:

`root #``systemctl enable varnishd`
To start varnish immediately:

`root #``systemctl start varnishd`
## Troubleshooting

### Verification

The curl command ([net-misc/curl](https://packages.gentoo.org/packages/net-misc/curl)) can be used to verify that HTTP traffic is successfully traveling through the varnish proxy:

`user $``curl -I https://wiki.gentoo.org/wiki`
