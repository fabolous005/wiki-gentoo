<!-- source: https://wiki.gentoo.org/wiki/YubiKey/GPG | group: Gentoo Wiki (Main) | wiki-title: YubiKey/GPG -->
---
title: YubiKey/GPG
url: https://wiki.gentoo.org/wiki/YubiKey/GPG
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-25"
fingerprint: "2e81d8dc2daa22f4"
license: CC BY-SA 4.0
---

# YubiKey/GPG

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Many YubiKeys can be configured to store OpenPGP keys. Most support RSA2048 keys while more modern ones support ECC and RSA4096 keys.  The **ykman** tool is not required, but is helpful for configuring the OpenPGP module.

## Introduction

The OpenPGP interface on a YubiKey can be used to store *signing*, *encryption*, and *authentication* keys.  Once these private keys are written to the device, they cannot be read/exported, but can be used.

The *authentication* key can be used for SSH authentication with the **gpg-agent**.

## Installation

### USE flags


| [+alternatives](https://packages.gentoo.org/useflags/+alternatives) | Install renamed, for compatibility with app-alternatives/gpg. | 
| [+smartcard](https://packages.gentoo.org/useflags/+smartcard) | Build scdaemon software. Enables usage of OpenPGP cards. For other type of smartcards, try app-crypt/gnupg-pkcs11-scd. Bring in dev-libs/libusb as a dependency; enable scdaemon. | 
| [+tofu](https://packages.gentoo.org/useflags/+tofu) | Enable support for Trust on First use trust model; requires dev-db/sqlite. | 
| [bzip2](https://packages.gentoo.org/useflags/bzip2) | Enable bzip2 compression support | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [readline](https://packages.gentoo.org/useflags/readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [tools](https://packages.gentoo.org/useflags/tools) | Install extra tools (including gpgsplit and gpg-zip). | 
| [tpm](https://packages.gentoo.org/useflags/tpm) | Enable TPM support via app-crypt/tpm2-tss and build tpm2d. | 
| [usb](https://packages.gentoo.org/useflags/usb) | Build direct CCID access for scdaemon; requires dev-libs/libusb. | 
| [user-socket](https://packages.gentoo.org/useflags/user-socket) | try a socket directory which is not removed by init manager at session end | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [wks-server](https://packages.gentoo.org/useflags/wks-server) | Install the wks-server | 


### Emerge

`root #``emerge --ask app-crypt/gnupg``root #``emerge --ask app-crypt/yubikey-manager`
## Configuration

### Generating Keys

It is generally best to generate the GPG keys using GnuPG, then move them to the YubiKey. This allows the keys to be backed up, and offers more control over how the keys are created and defined. A guide on this process is available here: [Generating GPG keys](https://wiki.gentoo.org/wiki/GnuPG#Key_management)

### Setting PINs

The OpenPGP pins can be changed using **ykman** or **gpg**

#### gpg

First, connect to the card using:

`user $``gpg --card-edit`
Once connected, use **admin** to enable admin mode:

`gpg/card>``admin`
Admin commands are allowed

Then enter the password changing menu using **passwd**:

`gpg/card>``passwd`
gpg: OpenPGP card no. D00000000000000000000000 detected
1 - change PIN
2 - unblock PIN
3 - change Admin PIN
4 - set the Reset Code
Q - quit

- **1** - Changes the user PIN, which is used to unlock the stored keys for usage.
- **2** - Unblocks the user PIN using the Admin PIN if it has been locked. This will also prompt for a new PIN to be set.
- **3** - Changes the Admin PIN, which is used to unblock the card and used to protect the card from modification.
- **4** - Sets the Reset Code, which is used to wipe the card to factory defaults.

#### ykman

To change the User PIN:

`user $``ykman openpgp access change-pin`
Enter PIN: 
New PIN: 
Repeat for confirmation:

To change the Admin PIN:

`user $``ykman openpgp access change-admin-pin`
Enter PIN: 
New PIN: 
Repeat for confirmation:

### Forcing presence detection for key usage

**ykman** can be used to force the YubiKey to be touched to use any of the loaded keys.

Simply run the following command for each key type (**enc**, **sig**, and **aut**):

`user $``ykman openpgp keys set-touch enc on`
Enter Admin PIN: 
Set touch policy of ENC key to on? \[y/N\]: y

### Resetting the OpenPGP module

If PINs to the YubiKey's OpenPGP module are lost, or it is locked, it must be reset to be used again, this can be done by using:

#### Using ykman

`user $``ykman openpgp reset`
#### Using GnuPG

`user $``gpg --card-edit``gpg/card>``admin`
Admin commands are allowed

`gpg/card>``factory-reset`
### Loading Keys

Once each key is created, they can be loaded onto the YubiKey using **keytocard**:

Begin by editing the relevant key with:

`user $``gpg --edit-key {key name}````
gpg (GnuPG) 2.2.41; Copyright (C) 2022 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Secret key is available.
sec  ed25519/0x638C17B8E66933AE
     created: 2023-05-22  expires: 2025-07-30  usage: C   
     trust: ultimate      validity: ultimate
ssb  ed25519/0xCCAF9BFBFCA8DE6C
     created: 2023-05-22  expires: 2024-06-25  usage: S   
     card-no: 0006 12345678
ssb  cv25519/0xD4CA9B1A823F23CD
     created: 2023-05-22  expires: 2024-06-25  usage: E   
     card-no: 0006 12345678
ssb  ed25519/0xADCD9EB4C88DDD76
     created: 2023-05-22  expires: 2024-06-25  usage: A   
     card-no: 0006 12345678
[ultimate] (1). larry <larry@gentoo.org>
```
Then select the signing key with **key 1**:

`gpg>``key 1````
sec  ed25519/0x638C17B8E66933AE
     created: 2023-05-22  expires: 2025-07-30  usage: C   
     trust: ultimate      validity: ultimate
ssb* ed25519/0xCCAF9BFBFCA8DE6C
     created: 2023-05-22  expires: 2024-06-25  usage: S   
     card-no: 0006 12345678
ssb  cv25519/0xD4CA9B1A823F23CD
     created: 2023-05-22  expires: 2024-06-25  usage: E   
     card-no: 0006 12345678
ssb  ed25519/0xADCD9EB4C88DDD76
     created: 2023-05-22  expires: 2024-06-25  usage: A   
     card-no: 0006 12345678
[ultimate] (1). larry <larry@gentoo.org>
```
With the key selected, it can be moved to the card with **keytocard**:

`gpg>``keytocard`
Please select where to store the key:
   (1) Signature key
   (3) Authentication key
Your selection? 1

Once the key has been loaded, the key can be selected with **key 1** again, and the process can be continued for the other keys:

`gpg>``key 1````
sec  ed25519/0x638C17B8E66933AE
     created: 2023-05-22  expires: 2025-07-30  usage: C   
     trust: ultimate      validity: ultimate
ssb ed25519/0xCCAF9BFBFCA8DE6C
     created: 2023-05-22  expires: 2024-06-25  usage: S   
     card-no: 0006 12345678
ssb  cv25519/0xD4CA9B1A823F23CD
     created: 2023-05-22  expires: 2024-06-25  usage: E   
     card-no: 0006 12345678
ssb  ed25519/0xADCD9EB4C88DDD76
     created: 2023-05-22  expires: 2024-06-25  usage: A   
     card-no: 0006 12345678
[ultimate] (1). larry <larry@gentoo.org>
```
`gpg>``key 2````
sec  ed25519/0x638C17B8E66933AE
     created: 2023-05-22  expires: 2025-07-30  usage: C   
     trust: ultimate      validity: ultimate
ssb  ed25519/0xCCAF9BFBFCA8DE6C
     created: 2023-05-22  expires: 2024-06-25  usage: S   
     card-no: 0006 12345678
ssb* cv25519/0xD4CA9B1A823F23CD
     created: 2023-05-22  expires: 2024-06-25  usage: E   
     card-no: 0006 12345678
ssb  ed25519/0xADCD9EB4C88DDD76
     created: 2023-05-22  expires: 2024-06-25  usage: A   
     card-no: 0006 12345678
[ultimate] (1). larry <larry@gentoo.org>
```
### Setting the Key URL

The Key URL can be used to allow GPG to find the YubiKey's public key using a URL.

The key can be uploaded to keys.openpgp.org with:

`user $``gpg --keyserver keys.openpgp.org --send-keys {key_fingerprint}`
If [https://www.openpgp.org](https://www.openpgp.org) was used for the keys, the following URL can be used: [https://keys.openpgp.org/vks/v1/by-fingerprint/{key\_fingerprint}](https://keys.openpgp.org/vks/v1/by-fingerprint/{key_fingerprint})

To set the URL, first enable admin mode, then set the URL parameter:

`gpg/card>``admin`
admin commands are allowed

`gpg/card>``url`
URL to retrieve public key: https://keys.openpgp.org/vks/v1/by-fingerprint/13EBBDBEDE7A12775DFDB1BABB572E0E2D182910

### Using PCSCD with GPG

[app-crypt/gnupg](https://packages.gentoo.org/packages/app-crypt/gnupg), if built with the `smartcard` *USE* flag, will include the scdaemon service. This can act as a smartcard reader if built with `usb`.

scdaemon can be configured to use pcscd as the card reader by configuring \~/.gnupg/scdaemon.conf:

**`~/.gnupg/scdaemon.conf`**

## Usage

Once keys have been loaded onto the YubiKey, they can be used with **gpg**, **gpg-agent**, or any other tool which knows how to read the YubiKey's OpenPGP module.

### SSH Auth

The Authentication key can be used for SSH authentication.

To configure gpg-agent to use the Authentication key loaded on a YubiKey, the *keygrip* must be obtained. This value can be obtained by running:

`user $``gpg -K --with-keygrip````
/home/larry/.gnupg/pubring.kbx
-----------------------------
sec   ed25519/0x638C17B8E66933AE 2023-05-22 [C] [expires: 2025-07-30]
      7751D62F9F9A0454B86871CE64FA651BB8850B48
      Keygrip = BB9CDAF7AFB27EBCC7E99CEECCF3EA8FD118BB5E
uid                   [ultimate] Larry <larry@gentoo.org>
ssb   ed25519/0xCCAF9BFBFCA8DE6C 2023-05-22 [S] [expires: 2024-06-25]
      Keygrip = F6BB075BBBDB8982CDA1824FBB6CE0AEFC7AD64D
ssb   cv25519/0xD4CA9B1A823F23CD 2023-05-22 [E] [expires: 2024-06-25]
      Keygrip = 3BAAA536BC72EAF10A79C772AB8DF2B8A3D76D0E
ssb   ed25519/0xADCD9EB4C88DDD76 2023-05-22 [A] [expires: 2024-06-25]
      Keygrip = 967FB799AF01C54EFB57EED49D882B0D05BDDDBA
```
With the keygrip, which is **967FB799AF01C54EFB57EED49D882B0D05BDDDBA** in this case, **gpg-agent** can be configured according to: [GPG agent configuration](https://wiki.gentoo.org/wiki/GnuPG#GPG_Agent)

Once the agent has been configured, the ssh public keys can be viewed using **ssh-add -L**, where the YubiKey's serial number should appear next to the corresponding Authentication key.

## Troubleshooting

### PC/SC Not available

The following error is a result of GPG reading the YubiKey, and can be resolved by killing gpg-agent and starting the pcscd service.

`root #``rc-service pcscd start`
## See Also

[GnuPG](https://wiki.gentoo.org/wiki/GnuPG) — a free implementation of the OpenPGP standard (RFC 4880).
