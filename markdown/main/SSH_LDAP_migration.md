<!-- source: https://wiki.gentoo.org/wiki/SSH/LDAP_migration | group: Gentoo Wiki (Main) | wiki-title: SSH/LDAP migration -->
---
title: SSH/LDAP migration
url: https://wiki.gentoo.org/wiki/SSH/LDAP_migration
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-12-22"
fingerprint: "867d1ffa269b81dc"
license: CC BY-SA 4.0
---

# SSH/LDAP migration

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Why migrate?

Originally, Gentoo used [OpenSSH LDAP public key patch (OpenSSH-LPK patch set)](https://code.google.com/archive/p/openssh-lpk/) from Eric Auge. However, this patch is dead and doesn't work anymore with OpenSSH 7.7 or newer because *auth\_parse\_options()* function was removed in OpenSSH via [commit 7c856857607112a3dfe6414696bf4c7ab7fb0cb3](https://github.com/openssh/openssh-portable/commit/7c856857607112a3dfe6414696bf4c7ab7fb0cb3).

Since the creation of the OpenSSH-LPK patch set, OpenSSH has changed a lot. With the release of [OpenSSH 6.2\_p1](https://www.openssh.com/txt/release-6.2) in 2013-03-22, a new sshd option called "*AuthorizedKeysCommand*" was implemented which supports fetching *authorized\_keys* from a command in addition to (or instead of) from the filesystem. Thanks to this feature, we no longer need to patch OpenSSH itself. Instead we can move LDAP lookup into an own package which is developed and maintained independently of OpenSSH.

In Gentoo we added [sys-auth/ssh-ldap-pubkey](https://packages.gentoo.org/packages/sys-auth/ssh-ldap-pubkey) package which provides a wrapper that can be used by "*AuthorizedKeysCommand*" option and also provides tools to manage keys in LDAP.

## How to migrate

### Step 1: Install wrapper of your choice

`root #``emerge --ask sys-auth/ssh-ldap-pubkey`
### Step 2: Update ldap.conf

Compare the existing /etc/ldap.conf file against ldap.conf provided by the wrapper and update the configuration in case something is missing or needs to be updated:

`user $``diff -u  /etc/ldap.conf /usr/share/doc/ssh-ldap-pubkey-*/examples/ldap.conf`
### Step 3: Verify that your configuration is working

If the current user has keys stored in LDAP, run:

`user $``ssh-ldap-pubkey list`
Or, to verify that the current user or Larry's keys are available like expected, run:

`user $``ssh-ldap-pubkey list -u larry`
### Step 4: Update OpenSSH configuration

Now you need to update your sshd's configuration so that it will use the new wrapper to fetch *authorized\_keys* from LDAP.

`root #``nano /etc/ssh/sshd_config`
Add the following line somewhere:

**`/etc/ssh/sshd_config`**

### Step 5: Restart sshd

As last step don't forget to restart the ssh daemon so that the updated configuration will be used.

With OpenRC:

`root #``/etc/init.d/sshd restart`
With systemd:

`root #``systemctl restart sshd.service`
## Alternatives

### sakcl

Maybe [sys-auth/sakcl](https://packages.gentoo.org/packages/sys-auth/sakcl), written in Rust and created by Gentoo developer  [Doug Goldstein (Cardoe)](https://wiki.gentoo.org/wiki/User:Cardoe) , is a better alternative for your needs. Please follow this guide how to migrate to *sakcl*.

#### Step 1: Install sakcl

`root #``emerge --ask sys-auth/sakcl`
#### Step 2: Create sakcl.conf

An example of this file's contents are:

**`/etc/sakcl.conf`**

#### Step 3: Verify that your configuration is working

To verify that Larry's keys are available like expected, run:

`user $``sakcl larry`
#### Step 4: Update OpenSSH configuration

Now you need to update your sshd's configuration so that it will use the new wrapper to fetch *authorized\_keys* from LDAP.

`root #``nano /etc/ssh/sshd_config`
Add the following line somewhere:

**`/etc/ssh/sshd_config`**

#### Step 5: Restart sshd

As last step don't forget to restart the ssh daemon so that the updated configuration will be used.

With OpenRC:

`root #``/etc/init.d/sshd restart`
With systemd:

`root #``systemctl restart sshd.service`
