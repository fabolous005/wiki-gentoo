<!-- source: https://wiki.gentoo.org/wiki/Scan_modem | group: Gentoo Wiki (Main) | wiki-title: Scan modem -->
---
title: Scan modem
url: https://wiki.gentoo.org/wiki/Scan_modem
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-11-27"
fingerprint: cd954b1b79f621dd
license: CC BY-SA 4.0
---

# Scan modem

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

scanModem a software tool that finds a suitable driver for a connected modem. scanModem is not available in gentoo.git, so it has to be manually downloaded and extracted:

`user $``wget` [https://web.archive.org/web/20190722162705/http://linmodems.technion.ac.il/packages/scanModem.gz](https://web.archive.org/web/20190722162705/http://linmodems.technion.ac.il/packages/scanModem.gz)
`user $````
gunzip scanModem.gz
```
`user $````
chmod +x scanModem
```
`user $````
./scanModem
```
It will create a folder Modem and the file scanout.something contains the wanted information. If a modem is detected, the driver is named next to *Drivers*, e.g. for a HSF modem:

**`./Modem/scanout.something`**
