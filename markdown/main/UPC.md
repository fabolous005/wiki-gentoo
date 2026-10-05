<!-- source: https://wiki.gentoo.org/wiki/UPC | group: Gentoo Wiki (Main) | wiki-title: UPC -->
---
title: UPC
url: https://wiki.gentoo.org/wiki/UPC
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-16"
fingerprint: bc855f3e9922d9cf
license: CC BY-SA 4.0
---

# UPC

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**UPC** is an extension of the C programming language designed for high performance computing on large-scale parallel machines. There is a number of implementations of UPC. Currently only Berkeley implementation is available through eselect-repository (in science overlay).

## Installation of the Berkeley runtime/driver

Berkeley UPC implementation is divided into runtime/driver and UPC-to-C translator. User operates with runtime/driver. If there is no UPC-to-C translator installed localy runtime/driver will use online translator provided by upstream.

First you need to add science overlay:

`root #````
eselect repository enable science
```
`root #``emerge --sync haskell`
There is much more configuration options available for you. To see them visit Berkeley UPC home page and especially read [\[1\]](http://upc.lbl.gov/download/dist/INSTALL.TXT). All these options can be enabled by using EXTRA\_ECONF variable. For example, if you want to use Quadrics/elan network conduit emerge the package as

`root #``EXTRA_ECONF='--enable-elan' emerge -va dev-lang/berkeley_upc`
Some network conduits may need setting other environment variables to be discovered correctly during configure. Consult with documentation.

Also you can use [package.env](https://wiki.gentoo.org/wiki/Package.env) to set EXTRA\_ECONF and other necessary variables.

Portable network conduits are available through setting appropriate USE flags (mpi, single, udp).

If you use portage.env or do not need to set additional options emerge this package just with

`root #``emerge --ask dev-lang/berkeley_upc`
## Installation of the UPC-to-C translator

Install it with

`root #``emerge --ask dev-lang/berkeley_upc_translator`
After installation you will need to point driver/runtime to the translator. You can do it while compiling passing -translator=/usr/libexec/berkeley\_upc\_translator-VERSION/targ option to the upcc. Where VERSION is the version of dev-lang/berkeley\_upc\_translator.

Also you can edit translator option in the file /usr/libexec/berkeley\_upc-VERSION/CONFIGURATION/etc/upcc.conf. VERSION here is the version of dev-lang/berkeley\_upc package and CONFIGURATION is the configuration you use (opt by default). To see what configurations are consult with UPC documentation about multiconf.
