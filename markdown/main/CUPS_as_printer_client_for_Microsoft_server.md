<!-- source: https://wiki.gentoo.org/wiki/CUPS_as_printer_client_for_Microsoft_server | group: Gentoo Wiki (Main) | wiki-title: CUPS as printer client for Microsoft server -->
---
title: CUPS as printer client for Microsoft server
url: https://wiki.gentoo.org/wiki/CUPS_as_printer_client_for_Microsoft_server
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-25"
fingerprint: "96da8e58f5316bd8"
license: CC BY-SA 4.0
---

# CUPS as printer client for Microsoft server

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Many Microsoft servers started using encryption for their shares. This is also the case for Windows printer shares. In order to print from a Gentoo installation Samba and CUPS will be needed.

## Installation

Edit /etc/portage/package.use to enable active directory support for CIFS and Samba. The following USE flags will pull in the required encryption needed for newer Windows servers:

`root #````
echo "net-fs/cifs-utils ads creds upcall" >> /etc/portage/package.use
```
`root #``echo "net-fs/samba caps addns ads" >> /etc/portage/package.use`
Next (re)install [net-fs/samba](https://packages.gentoo.org/packages/net-fs/samba) and [net-print/cups](https://packages.gentoo.org/packages/net-print/cups):

`root #``emerge --ask --oneshot net-fs/samba net-print/cups`
## Configuration

Setting up printers is fairly simple through the web-interface of CUPS. Point a browser to [https://localhost:631](https://localhost:631)

`smb://USERNAME:PASSWORD@DOMAIN/URL/PRINTERSHARE`

Where the values for `USERNAME`, `PASSWORD`, `DOMAIN`, `URL` and `PRINTERSHARE` are properly substituted with the appropriate values for each use case. When not on a network with a domain-server leave out the `DOMAIN/` section of the string.

## Troubleshooting

If users cannot print try to connect to the print server using the Samba client:

`user $``smbclient -W DOMAIN -U USERNAME //SERVER/PRINTER`
Password: PASSWORD
Domain=\[DOMAIN\] OS=\[Windows Server 2008 R2 Datacenter 7601 Service Pack 1\] Server=\[Windows Server 2008 R2 Datacenter 6.1\]
smb: \> print test.ps
printing file test.ps as test.ps (196,6 kb/s) (average 196,6 kb/s)
smb: \> quit

Where `test.ps` is a postscript file located in the local current working directory.

If this test is working, then something went bad when setting up CUPS.
