<!-- source: https://wiki.gentoo.org/wiki/Printer_mfcj6710dw | group: Gentoo Wiki (Main) | wiki-title: Printer mfcj6710dw -->
---
title: Printer mfcj6710dw
url: https://wiki.gentoo.org/wiki/Printer_mfcj6710dw
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-02-28"
fingerprint: ddf9c9f75b6ce62
license: CC BY-SA 4.0
---

# Printer mfcj6710dw

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

### Brother mfcj6710dw

Install drivers and integrate with CUPS:

- Create spooler directory:

- `user $``mkdir -p /var/spool/lpd/mfcj6710dw``user $``chown lp:lp /var/spool/lpd/mfcj6710dw``user $``chmod 700 /var/spool/lpd/mfcj6710dw`

- Create spooler directory:

- `user $``rpm -ivh --nodeps mfcj6710dwlpr-3.0.0-1.i386.rpm``user $``rpm -ivh --nodeps mfcj6710dwcupswrapper-3.0.0-1.i386.rpm`

- Link to cupswrapper:

- `user $``ln -s /opt/brother/Printers/mfcj6710dw/lpd/filtermfcj6710dw /usr/libexec/cups/filter/brother_lpdwrapper_mfcj6710dw`

- Restart cups:

- `root #``/etc/init.d/cups restart`

Then go to the CUPS admin interface under [http://localhost:631/](http://localhost:631/) and select lpd://\<IP ADDRESS>/BINARY\_P1
