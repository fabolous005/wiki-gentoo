<!-- source: https://wiki.gentoo.org/wiki/Join_FreeIPA | group: Gentoo Wiki (Main) | wiki-title: Join FreeIPA -->
---
title: Join FreeIPA
url: https://wiki.gentoo.org/wiki/Join_FreeIPA
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-06"
fingerprint: f338bf582f076fff
license: CC BY-SA 4.0
---

# Join FreeIPA

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gentoo is not a supported distribution of the FreeIPA server or client tools, but its possible to manually provision a Gentoo system. Setting up the server will not be discussed here.

## Prerequisites

### Check hostname

The hostname must be set up on the client and match the hostname (not FQDN) used for enrollment.

#### OpenRC

The DNS hostname (NOT the FQDN) must in /etc/hostname

`root #``printf "gentoo" > /etc/hostname`
#### systemd

Use hostnamectl to set static hostname (NOT the FQDN)

`root #``hostnamectl hostname --static "gentoo"`
### Synchronize time

By default, FreeIPA does NOT provide a time server for clients. FreeIPA's preferred ntp client is [net-misc/chrony](https://packages.gentoo.org/packages/net-misc/chrony). Any NTP client should work.

#### OpenRC

Install an NTP client. The default configuration should be fine. Don't forget to enable and start the service in OpenRC!

#### systemd

Use timedatectl to enable NTP:

`root #``timedatectl set-ntp true`
## Installation

### USE flags

The following per-package USE flags are required:

**`/etc/portage/package.use/sssd.conf`**

The following per-package USE flags are recommended:

**`/etc/portage/package.use/sssd.conf (Excerpt)`**

### IPA Server part

The provisioning for the new **gentoo** client must be done the server. The web ui can create the host entry, but the command line **on the FreeIPA server** must be used to create the Kerberos keytab.

`root #````
kinit admin
```
`root #````
ipa host-add --force gentoo.internal.example.com
```
`root #``ipa-getkeytab -s ipa1.internal.example.com -p host/gentoo.internal.example.com -k /tmp/gentoo.keytab`
Copy the keytab file to the client's /etc/krb5.keytab. This may be difficult. If the client has a local user and has an ssh server, the FreeIPA server admin can "push" the keytab to the client with scp. If the client has no local users, because OpenSSH doesn't permit password authentication for root by default, this won't work. The root user on the FreeIPA server will need to choose user with ssh access to FreeIPA server (usually "admin"), copy the ticket to somewhere that user can access (like their home directory) and *chown* the keytab so that user can access the file. Then, on the client, "pull" the keytab from the server with scp.

### Emerge

`root #``emerge --ask sys-auth/sssd``root #``emerge --ask -auvDU @world`
### Get the CA certificate

The CA certificate is be needed for *sssd*, and may be required for other applications too (like Firefox)

`root #````
mkdir /etc/ipa
```
## Essential Configuration

The 4 most important things that need to be configured are Kerberos, SSSD, PAM and NSS. Because the configuration between hosts is same or similar, once the first host is configured, those files can be copied to a distribution point (webserver, thumb drive), then copied to the other machines, and edited.

### Kerberos

All the Kerberos file are identical on every host. First, a few directories need to be created

`root #````
mkdir /etc/krb5.conf.d
```
`root #``mkdir /var/lib/ipa-client`
On the client, use the **scp** utility to copy 2 files (these are the same for every host, and be added to the configuration ditrobution)

`root #``scp -r admin@ipa1.internal.example.com:/var/lib/ipa-client/pki /var/lib/ipa-client`
The server's Kerberos configuration is different from the client, so it cannot be copied over. Either enroll on a client (like a Fedora Workstation VM) that supports *ipa-client-install* and copy the /etc/krb5.conf file and /etc/krb5.conf.d directory; or use the ones below:

**`/etc/krb5.conf`**

**`/etc/krb5.conf.d/enable_sssd_conf_dir`**

**`/etc/krb5.conf.d/freeipa`**

**`/etc/krb5.conf.d/freeipa-realm`**

The next file is only effective if [sys-auth/sssd](https://packages.gentoo.org/packages/sys-auth/sssd) was compiled with the [openid](https://packages.gentoo.org/useflags/openid) [USE flag](https://wiki.gentoo.org/wiki/USE_flag)

**`/etc/krb5.conf.d/sssd_enable_idp`**

The next file is only effective if [sys-auth/sssd](https://packages.gentoo.org/packages/sys-auth/sssd) was compiled with the [passkey](https://packages.gentoo.org/useflags/passkey) [USE flag](https://wiki.gentoo.org/wiki/USE_flag)

**`/etc/krb5.conf.d/sssd_enable_passkey`**

### sssd

Note the **ipa\_hostname** and **dyndns\_iface** need to be customized for each client in the file below.

**`/etc/sssd/sssd.conf`**

### PAM

If [sssd](https://packages.gentoo.org/useflags/sssd) [is set on](https://wiki.gentoo.org/wiki/USE_flag) [sys-auth/pambase](https://packages.gentoo.org/packages/sys-auth/pambase), PAM will be automatically configured for sssd. However, it does not create home directories on login. IF that is desired, the pam\_mkhomedir module may be added to the appropriate spot in the PAM configuration (SELinux users: Use [app-misc/oddjob](https://packages.gentoo.org/packages/app-misc/oddjob) instead). PAM is very fragile, manual configuration of PAM is not recommended.

### NSS

Each host /etc/nsswitch.conf will look different, so the excerpt below will need to be adapted to the host, for example, "systemd" won't be present on non-systemd systems. Any nonpresent entries, like "sudoers" should be added.

**`/etc/nsswitch.conf (excerpt)`**

## Starting sssd service

### OpenRC

`root #````
rc-update add sssd default
```
`root #``rc-service sssd start`
### systemd

`root #``systemctl enable --now sssd`
## Ancillary services

### ssh

**`/etc/ssh/ssh_config.d/04-ipa.conf`**

**`/etc/ssh/sshd_config.d/04-ipa.conf`**

### ssl

#### OpenSSL and gnutls

`root #````
cp /etc/ipa/ca.crt /usr/local/share/ca-certificates/INTERNAL.EXAMPLE.COM_IPA_CA.crt
```
`root #``update-ca-certififcates`
#### NSS

This assumes you have a (password protected) NSS database already created.

`root #``ertutil -d sql:/etc/ipa/nssdb -A -n 'INTERNAL.EXAMPLE.COM IPA CA' -t 'CT,C,C' -a -f /etc/ipa/nssdb/pwdfile.txt < /etc/ipa/ca.crt`
### LDAP

**`/etc/openldap/ldap.conf`**

## Usage

`root #``kinit username`
This should aquire a ticket for *username*

`root #``id username`
Will print membership of *username* in both FreeIPA and local groups.

`root #``sudo -ll -U username`
Will print sudo rules (if any) for *username*.

## Troubleshooting

As root, the **sssctl** command can be used to temporaily increase debugging. The logging may be increased on one subsystem or globally in sssd,

`root #````
sssctl debug-level --pam 0x00F0 # Additonally log pam_sss minor failures 
```
`root #``sssctl debug-level 0x0070 # default level`
## External resources

- [\[1\]](https://www.freeipa.org/page/Main_Page) FreeIPA project
