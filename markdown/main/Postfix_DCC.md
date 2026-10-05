<!-- source: https://wiki.gentoo.org/wiki/Postfix/DCC | group: Gentoo Wiki (Main) | wiki-title: Postfix/DCC -->
---
title: Postfix/DCC
url: https://wiki.gentoo.org/wiki/Postfix/DCC
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2016-06-08"
fingerprint: e219fc4a7cb96f16
license: CC BY-SA 4.0
---

# Postfix/DCC

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**The Distributed Checksum Clearinghouses** (or **DCC**) is an anti-spam content filter based upon the exchange between mail servers of fuzzy checksums identifying message content and the number of such messages received. The checksums are constant across common variations in bulk messages, including "personalizations." The official project website is at [rhyolite.com/dcc/](http://www.rhyolite.com/dcc/).

DCC detects bulk mail. It cannot distinguish between solicited and unsolicited bulk mail. This is the biggest drawback to DCC, and the main mitigation is manual whitelisting.

## Background

The idea of DCC is that if mail recipients could compare the mail they receive, they could recognize unsolicited bulk mail. A DCC server totals reports of checksums of messages from clients and answers queries about the total counts for checksums of mail messages. A DCC client reports the checksums for a mail message to a server and is told the total number of recipients of mail with each checksum. If one of the totals is higher than a threshold set by the client and according to local whitelists the message is unsolicited, the DCC client can log, discard, or reject the message.

Because simplistic checksums of spam would not be effective, the main DCC checksums are fuzzy and ignore aspects of messages. The fuzzy checksums are changed as spam evolves. Since DCC started being used in late 2000, the fuzzy checksums have been modified several times.

Normal users only run the client.

DCC traffic is DNS-like but appears on UDP port 6276 (greylisting traffic) and 6277 (normal DCC traffic). Inbound and outbound firewall rules should be set to allow this traffic.

## Installation

First, you probably do not want to enable `milter` support for DCC, so disable that `USE` flag as follows.

`root #``echo "mail-filter/dcc -milter" >>/etc/portage/package.use/dcc`
Next, emerge the package. This should not pull in the `milter` library.

`root #``emerge --ask mail-filter/dcc`
Because the ebuild currently installs everything to run as `root` we are going to do some permissions cleanup.

`root #``useradd -U dcc``root #``mkdir /var/run/dcc``root #``chgrp dcc /var/run/dcc``root #``chmod g+w /var/run/dcc``root #``chmod o-rwx /var/run/dcc``root #``chgrp -R dcc /etc/dcc``root #``chmod o-rwx /etc/dcc`
## Setup

### Client only (normal scenario)

The most important binaries installed by the package are `cdcc` (Control Distributed Checksum Clearinghouse) and `dccifd` (Distributed Checksum Clearinghouse Interface Daemon), which is the preferred interface between postfix and DCC.

We want to add `dccifd` as a Before-Queue Content Filter.

First, verify that `DCCIFD_ENABLE=on` in the `/etc/dcc/dcc_conf` file.

**`/etc/dcc/dcc_conf`**

**Ensure the interface daemon is enabled**

Next, we explicitly specify paths to the sockets that the daemon will create and use to communicate. For more information on this, see `man dccifd`.

**`/etc/dcc/dcc_conf`**

**Specify port numbers**

Next, add the following values to `master.cf`.

**`/etc/postfix/master.cf`**

**Tell postfix to use DCC's interface daemon**

### Server (only if you run a big mailserver)

Before continuing, see [Do I need to run a server?](http://www.dcc-servers.net/dcc/FAQ.html#need-server) in the DCC FAQ. Probably not.

The server binary is called `dccd`. Please contribute documentation regarding configuration here. At a guess, probably the process is basically as follows.

**`/etc/dcc/dcc_conf`**

**Enable DCCD**

It will now be necessary to connect postfix to the DCC server in some DCC-server specific way, please refer to the DCC documentation for this. There is a start within the server section of `man dcc`. Finally...

`root #``rc-update add dcc default``root #``/etc/init.d/dcc start`
## Deployment

To make your changes take effect, reload the postfix configuration.

`root #``/etc/init.d/postfix reload`
