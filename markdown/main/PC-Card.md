<!-- source: https://wiki.gentoo.org/wiki/PC-Card | group: Gentoo Wiki (Main) | wiki-title: PC-Card -->
---
title: PC-Card
url: https://wiki.gentoo.org/wiki/PC-Card
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-04-25"
fingerprint: e601a15e1daa098e
license: CC BY-SA 4.0
---

# PC-Card

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the setup of *PC-Card* (also known as PCMCIA) controllers.

## Installation

### Kernel Configuration

The following kernel options must be activated for PCMCIA controllers:

### Portage Configuration

Portage knows the global USE flag `pcmcia` for enabling support for PC-Cards in other packages. Enabling this USE flag will pull in [sys-apps/pcmciautils](https://packages.gentoo.org/packages/sys-apps/pcmciautils) automatically:

**`/etc/portage/make.conf`**

```
USE="... pcmcia ..."
```
After setting the USE flag in the step above be sure to update the system so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
To install pcmciautils manually (if it is not already pulled in):


| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [staticsocket](https://packages.gentoo.org/useflags/staticsocket) | Add support for static sockets | 

`root #``emerge --ask pcmciautils`
### Software Configuration

#### Card Information Structure

A *PC-Card* has a firmware called **Card Information Structure** (CIS). Some CISs are flawed, but can be patched at boot time. For those types of cards install [sys-apps/pcmcia-cs-cis](https://packages.gentoo.org/packages/sys-apps/pcmcia-cs-cis):

`root #``emerge --ask pcmcia-cs-cis`
#### Resource database

Some old 16-bit *PCMCIA* cards works only if a resource database is set up. This effects only some x86 and x86\_64 computers; not all are effected. To find out if the database is needed look at the controllers PCI-ID. [The lspci tool](https://wiki.gentoo.org/wiki/Hardware_detection) works nicely for this task:

`root #````
lspci | egrep --color -i "pcmcia|cardbus"
```
**03**:01.0 CardBus bridge: O2 Micro, Inc. Cardbus bridge (rev 21)

If the first two numbers are not zeros (here in bold: 03), then no resource database is needed. If they are zeros, then the database will need copied to /etc/pcmcia/config.opts:

`root #````
cd /var/db/repos/gentoo/sys-apps/pcmciautils
```
`root #````
ebuild pcmciautils-017.ebuild unpack
```
`root #````
cp -p /var/tmp/portage/sys-apps/pcmciautils-017/work/pcmciautils-017/config/config.opts /etc/pcmcia/
```
Edit the file as needed.

## Usage

The pccardctl command (part of the [sys-apps/pcmciautils](https://packages.gentoo.org/packages/sys-apps/pcmciautils) package) can be used to show information about the PC-Card controller and the inserted cards. It can also be used to load and unload PC cards.
