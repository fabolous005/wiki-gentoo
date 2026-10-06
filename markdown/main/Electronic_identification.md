<!-- source: https://wiki.gentoo.org/wiki/Electronic_identification | group: Gentoo Wiki (Main) | wiki-title: Electronic identification -->
---
title: Electronic identification
url: https://wiki.gentoo.org/wiki/Electronic_identification
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-02-10"
fingerprint: "8bb6470520edb1ad"
license: CC BY-SA 4.0
---

# Electronic identification

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

An **electronic identification** (**eID**) is the core part of e-government implementation, providing a way to identify citizens and organizations.

## Installation

### Smart card reader driver

Most likely, the card reader will be covered by the [app-crypt/ccid](https://packages.gentoo.org/packages/app-crypt/ccid) package (if not, check the [driver list](https://wiki.gentoo.org/wiki/PCSC-Lite#Additional_software)):

`root #``emerge --ask app-crypt/ccid`
And enable the [PCSC-Lite](https://wiki.gentoo.org/wiki/PCSC-Lite) service, as described [here](https://wiki.gentoo.org/wiki/PCSC-Lite#Service).

### Belgium

Install the [app-crypt/eid-mw](https://packages.gentoo.org/packages/app-crypt/eid-mw) package:

`root #``emerge --ask app-crypt/eid-mw`
#### Firefox support

In Firefox's preferences, under Privacy & Security → Security → Certificates → Security Devices, click Load, choose a name such as "Belgian eID", and add /usr/lib64/libbeidpkcs11.so. Installing the add-on isn't necessary.

When using Firefox in a [Firejail](https://wiki.gentoo.org/wiki/Firejail), add this to /etc/firejail/firefox-common.local (create the file if necessary):

FILE **`/etc/firejail/firefox-common.local`****Firejail whitelisting rules for Belgian eID**

```
# Belgian eID
noblacklist /usr/lib/mozilla/pkcs11-modules
noblacklist /usr/lib64/libbeidpkcs11.so*
whitelist /run/pcscd/pcscd.comm
```
### Estonia

See [Web eID](https://wiki.gentoo.org/wiki/Web_eID).

### Germany

`root #``emerge --ask sys-auth/AusweisApp`
See [https://www.ausweisapp.bund.de/en/home](https://www.ausweisapp.bund.de/en/home)
