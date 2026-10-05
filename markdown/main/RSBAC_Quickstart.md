<!-- source: https://wiki.gentoo.org/wiki/RSBAC/Quickstart | group: Gentoo Wiki (Main) | wiki-title: RSBAC/Quickstart -->
---
title: RSBAC/Quickstart
url: https://wiki.gentoo.org/wiki/RSBAC/Quickstart
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-07"
fingerprint: "3685f55e1cba2d4f"
license: CC BY-SA 4.0
---

# RSBAC/Quickstart

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This document will guide you through the installation of the RSBAC on Gentoo Linux

## Introduction

This guide will help you to install RSBAC on Gentoo Linux. It is assumed that the users have read the [Introduction](https://wiki.gentoo.org/wiki/Project:RSBAC/Introduction) and the [Overview](https://wiki.gentoo.org/wiki/RSBAC/Overview) already, so that they know what is RSBAC and its main concepts.

## Installation of the RSBAC enabled kernel

### Emerging the RSBAC kernel

This step is pretty straight forward, thanks to the way Gentoo handles kernel installations. Start by emerging the [sys-kernel/rsbac-sources](https://packages.gentoo.org/packages/sys-kernel/rsbac-sources) kernel package with Portage:

`root #``emerge --ask sys-kernel/rsbac-sources``root #````
rm /etc/make.profile
```
`root #````
ln -s /usr/portage/profiles/default-linux/x86/2005.0/2.4/ /etc/make.profile
```
`root #````
echo "sys-kernel/hardened-sources rsbac" >> /etc/portage/package.use
```
`root #````
emerge hardened-sources
```
### Configuring the RSBAC kernel

We will now configure the kernel. It is recommended that you enable the following options, in the "Rule Set Based Access Control (RSBAC)" category:

**Configuring and compiling the RSBAC kernel**

{{Note|When planning to run a X Window server (such as X.org or XFree86), please also enable `"[*] X support (normal user MODIFY_PERM access to ST_ioports)"`.

We will now configure PaX which is a complement of the RSBAC hardened kernel. It is also recommended that you enable the following options, in the "Security options ---> PaX" section.

**Configuring PaX kernel options**

`root #````
rsbac_fd_menu /path/to/the/target/item
```
`root #````
attr_set_file_dir FILE /path/to/the/target/item pax_flags [pmerxs]
```
You can now compile and install the kernel as you would do with a normal one concerning the other options.

## Installation of the RSBAC admin utilities

In order to administrate your RSBAC enabled Gentoo, some userspace utilities are required. Those are included in the rsbac-admin package and it needs to be installed.

`root #``emerge --ask rsbac-admin`
Once emerged, the package will have created a new user account on your system (secoff, with uid 400). He will become the security administrator during the first boot. This is the only user, who is able to change the RSBAC configuration. He will commonly be called the Security Officer.

`root #``passwd secoff`
## First boot

At the first boot, login into the system won't be possible, due to the AUTH module *restricting* the programs privileges. To overcome this problem please boot into softmode using the following kernel parameter (in lilo or GRUB configuration):

The login application is managing user logins on the system. It needs rights to setuid, which we will now give:

Login as the Security Officer (secoff) and allow logins to be made by entering the following command:

`root #````
rsbac_fd_menu /bin/login
```
`root #``attr_set_fd AUTH FILE auth_may_setuid 1 /bin/login`
As an alternative, if softmode is not enabled, use the following kernel parameter in order to allow login at boot time:

`root #``rsbac_auth_enable_login`
## Learning mode and the AUTH module

### Creating a policy for OpenSSH

Because there is almost no policy made yet (except the one generated during the first boot), the AUTH module does not allows UID changes.

Thanks to the intelligent learning mode there is an easy way to alleviate this new problem: The AUTH module can automagically generate the necessary policy by watching services while they start up, and note the uids they are trying to switch to. For example to teach the AUTH module about the UIDs needed by sshd (OpenSSH daemon), do the following:

Enable the learning mode for sshd:

`root #```attr_set_file_dir AUTH FILE `which sshd` auth_learn 1``
Start the service:

`root #``/etc/init.d/sshd start`
Disable the learning mode:

`root #```attr_set_file_dir AUTH FILE `which sshd` auth_learn 0``
Now sshd should be working as expected again, *congratulations*, you made your first policy :) The same procedure can be used on every other daemon you will need.

You can enable the global learning mode by issuing this kernel parameter at boot time:

`root #``rsbac_auth_learn`
## Participation

It is also strongly suggested participants subscribe to the [gentoo-hardened mailing-list](https://archives.gentoo.org/gentoo-hardened/). It is generally a low traffic list, and RSBAC announcements for Gentoo will be available there. Connecting to the [#gentoo-hardened](ircs://irc.libera.chat/#gentoo-hardened) ([webchat](https://web.libera.chat/#gentoo-hardened)) channel on Libera.Chat is also a good way to participate. We also recommend subscribing to the [RSBAC mailing-list](http://rsbac.org/mailman/listinfo/rsbac/) and interacting in the [#rsbac](ircs://irc.libera.chat/#rsbac) ([webchat](https://web.libera.chat/#rsbac)) channel on Freecode. Please also check the [hardened FAQ](https://wiki.gentoo.org/wiki/Project:Hardened/FAQ); there is a possibility questions might already be covered in this document.

## Resources
