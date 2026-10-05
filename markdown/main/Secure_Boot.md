<!-- source: https://wiki.gentoo.org/wiki/Secure_Boot | group: Gentoo Wiki (Main) | wiki-title: Secure Boot -->
---
title: Secure Boot
url: https://wiki.gentoo.org/wiki/Secure_Boot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-28"
fingerprint: "1f194d1210e8b4f4"
license: CC BY-SA 4.0
---

# Secure Boot

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Secure Boot** is an enhancement of the security of the pre-boot process of a [UEFI](https://wiki.gentoo.org/wiki/UEFI) system. When enabled, the UEFI firmware verifies the signature of every component used in the boot process. This results in boot files which are easily readable, but tamper evident.

This wiki page describes how to set up Secure Boot by registering custom keys in the system's firmware directly. For an alternative method of booting with Secure Boot enabled that (usually) does not require modifying the UEFI, see the wiki page on [Shim](https://wiki.gentoo.org/wiki/Shim).

## Introduction

Implementing Secure Boot can significantly enhance the security of a system. The integrity of the boot chain is extremely important. If malicious code is able to interfere with the boot process, many other security measures are effectively nullified.

### Simple setup

This article explains in detail how the user may directly interact with each component of the Secure Boot process. Users wishing to simply get Secure Boot working may prefer automated setup using [sbctl](https://wiki.gentoo.org/wiki/Sbctl).

### Cryptography

UEFI Secure Boot typically uses **RSA-2048** and **sha256** to perform *public key cryptography*.

Secure Boot public keys should be stored in the **X.509** format.

### Components

Secure Boot typically implements the following keys and lists: [\[1\]](https://wiki.gentoo.org#cite_note-1)[\[2\]](https://wiki.gentoo.org#cite_note-2)<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>:

- **PK** - *Platform Key* - Composed of two parts, *PKpub* (the public key) and *PKpriv* (the private key), used to sign the KEK.
- **KEK** - *Key Exchange Key* - The key used to sign the *Signatures* and *Forbidden Signatures* database, there can be more than one.
- **db** - *Signature Database* - Contains lists of public keys, signatures, and hashes which are allowed as part of the boot chain.
- **dbx** - *Forbidden Signature Database* - The opposite of the signature database, public keys, signatures, and hashes which should never be allowed to boot.

### Key Formats

Several key formats and extensions are used with Secure Boot:

- **.key** - *PEM* - Used for private keys.
- **.crt** - *PEM* - Used for public keys.
- **.cer** - *DER* - Used for public keys.
- **.esl** - *EFI Signature List* - Used by EFI, a collection of public keys and hashes.
- **.auth** - *Signed EFI Signature List* - Used by EFI, signed form of an esl.

### Implementation

Secure Boot is only as strong as the implementation. The motherboard's UEFI firmware interface must be protected by a password, otherwise an attacker could simply turn Secure Boot off and/or bypass it. Additionally, weak key storage can render any protections Secure Boot provides useless.

#### Key Storage

Secure Boot stores the public keys and *Signature Lists*, typically stored on the mainboard's NVRAM. This memory region is typically readable once booted, but can only be written using the EFI firmware. An example of this is efibootmgr being used to adjust the boot order while the system is running. Memory for Secure Boot keys is typically read-only, and depending on the implementation, variables may only be writable one time in *Setup Mode*. UEFI firmware may support updating using properly *.auth* files, but this is not universal.

## Installation

### USE flags

On packages with the `secureboot` global USE flag this flag can be enabled to automatically sign any EFI binaries installed by the package. When this flag is enabled the `SECUREBOOT_SIGN_KEY` and `SECUREBOOT_SIGN_CERT` environment variables must be used to specify the path (or pkcs11 URI) of the db key and certificate to use for signing in PEM format.

**`/etc/portage/make.conf`**

**make.conf**

```
SECUREBOOT_SIGN_KEY="..."
SECUREBOOT_SIGN_CERT="..."
```
In addition to the kernel itself, the kernel modules must also be signed to boot successfully with Secure Boot enabled. For this purpose the `modules-sign` global USE flag can be used in addition to the `MODULES_SIGN_KEY` and `MODULES_SIGN_CERT` environment variables.

#### app-crypt/efitools


#### app-crypt/sbsigntools


#### dev-libs/openssl


| [+asm](https://packages.gentoo.org/useflags/+asm) | Enable using assembly for optimization | 
| [+quic](https://packages.gentoo.org/useflags/+quic) | Enable support for QUIC (RFC 9000); a UDP-based protocol intended to replace TCP | 
| [fips](https://packages.gentoo.org/useflags/fips) | Enable FIPS provider | 
| [ktls](https://packages.gentoo.org/useflags/ktls) | Enable support for Kernel implementation of TLS (kTLS) | 
| [rfc3779](https://packages.gentoo.org/useflags/rfc3779) | Enable support for RFC 3779 (X.509 Extensions for IP Addresses and AS Identifiers) | 
| [sctp](https://packages.gentoo.org/useflags/sctp) | Support for Stream Control Transmission Protocol | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [tls-compression](https://packages.gentoo.org/useflags/tls-compression) | Enable support for discouraged TLS compression | 
| [vanilla](https://packages.gentoo.org/useflags/vanilla) | Do not add extra patches which change default behaviour; DO NOT USE THIS ON A GLOBAL SCALE as the severity of the meaning changes drastically | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [weak-ssl-ciphers](https://packages.gentoo.org/useflags/weak-ssl-ciphers) | Build support for SSL/TLS ciphers that are considered "weak" | 

### Emerge

`root #``emerge --ask app-crypt/efitools``root #``emerge --ask app-crypt/sbsigntools``root #``emerge --ask dev-libs/openssl`
## Backing up existing keys

The efi-readvar command can be used to view the **public** contents of the UEFI signature database.

The keys can be saved using:

`~/secure_boot/factory_config $``for key_type in PK KEK db dbx; do efi-readvar -v $key_type -o ${key_type}.esl; done`
Or each var can manually be saved using:

`~/secure_boot/factory_config $``efi-readvar -v PK -o PK.esl``~/secure_boot/factory_config $``efi-readvar -v KEK -o KEK.esl``~/secure_boot/factory_config $``efi-readvar -v db -o db.esl``~/secure_boot/factory_config $``efi-readvar -v dbx -o dbx.esl`
## Generating new keys

OpenSSL can be used to generate the Secure Boot keys.

At a minimum, the **PK**, **KEK**, and **db** keys must be created. Each of these keys can be created similarly.

### Generating a UUID

To generate a *UUID* using **uuidgen**, and write it to uuid.txt:

`~/secure_boot/custom_config $``uuidgen > uuid.txt`
### Batch key creation

By default, openssl will create keys which are individually protected with symmetric encryption. It's possible to use GPG to encrypt keyfiles without letting them touch storage using named pipes, described below.

#### Symmetrically protected keyfile creation

To create keyfiles protected with openssl:

`~/secure_boot/custom_config $``for key_type in PK KEK db; do openssl req -new -x509 -newkey rsa:2048 -subj "/CN=Larry's ${key_type}" -keyout ${key_type}.key -out ${key_type}.crt -days 9999 -sha256; done`
This is equivalent to:

`~/secure_boot/custom_config $````
openssl req -new -x509 -newkey rsa:2048 -subj "/CN=Larry's PK" -keyout PK.key -out PK.crt -days 9999 -sha256
```
`~/secure_boot/custom_config $````
openssl req -new -x509 -newkey rsa:2048 -subj "/CN=Larry's KEK" -keyout KEK.key -out KEK.crt -days 9999 -sha256
```
`~/secure_boot/custom_config $``openssl req -new -x509 -newkey rsa:2048 -subj "/CN=Larry's db" -keyout db.key -out db.crt -days 9999 -sha256`
#### Public key encryption

To create a GPG encrypted file for each key type (*PK*, *KEK*, *db*):

`~/secure_boot/custom_config $``mkfifo key_pipe & sleep 1 && for key_type in PK KEK db; do openssl req -new -x509 -newkey rsa:2048 -subj "/CN=Larry's ${key_type}" -keyout key_pipe -out ${key_type}.crt -days 9999 -noenc -sha256 & gpg --output ${key_type}.key.gpg --recipient larry@gentoo.org --encrypt < key_pipe ; done ; rm key_pipe`
This is equivalent to:

`~/secure_boot/custom_config $``mkfifo key_pipe &``~/secure_boot/custom_config $``openssl req -new -x509 -newkey rsa:2048 -subj "/CN=Larry's PK" -keyout key_pipe -out PK.crt -days 9999 -noenc -sha256 &``~/secure_boot/custom_config $``gpg --output PK.key.gpg --recipient larry@gentoo.org --encrypt < key_pipe``~/secure_boot/custom_config $``openssl req -new -x509 -newkey rsa:2048 -subj "/CN=Larry's KEK" -keyout key_pipe -out KEK.crt -days 9999 -noenc -sha256 &``~/secure_boot/custom_config $``gpg --output KEK.key.gpg --recipient larry@gentoo.org --encrypt < key_pipe``~/secure_boot/custom_config $``openssl req -new -x509 -newkey rsa:2048 -subj "/CN=Larry's db" -keyout key_pipe -out db.crt -days 9999 -noenc -sha256 &``~/secure_boot/custom_config $``gpg --output db.key.gpg --recipient larry@gentoo.org --encrypt < key_pipe``~/secure_boot/custom_config $``rm key_pipe`
### Manual key creation

To create a *Platform Key*:

`~/secure_boot/custom_config $``openssl req -new -x509 -newkey rsa:2048 -subj "/CN=Larry's Platform Key" -keyout PK.key -out PK.crt -days 9999 -sha256`
#### Unprotected key creation

To create a *Platform Key* pair without protecting the *PKpriv*:

`~/secure_boot/custom_config $``openssl req -new -x509 -newkey rsa:2048 -subj "/CN=Larry's Platform Key" -keyout PK.key -out PK.crt -days 9999 -noenc -sha256`
#### GPG protected key creation

To immediately GPG encrypt the *PKpriv* for additional protection, a named pipe can be used:

`~/secure_boot/custom_config $``mkfifo key_pipe & sleep 1; openssl req -new -x509 -newkey rsa:2048 -subj "/CN=Larry's Platform Key" -keyout key_pipe -out PK.crt -days 9999 -noenc -sha256 & gpg --output PK.key.gpg --recipient larry@gentoo.org --encrypt < key_pipe && rm key_pipe`
This process can be repeated for the *Key Exchange Key* (**KEK**), and *Signature Database Key* (**db**).

## Preparing the signature lists

Once the secure boot keys have been generated, they can be used to create signature lists which are loaded by the EFI.

### Creating the new Signature Lists

**cert-to-efi-list** can be used to create an *.esl* file using a *.crt* file, that signature list can then be signed using **sign-efi-sig-list** and the private key. To perform these actions on the *Platform Key* which has been GPG encrypted:

#### Batch creation

This process can be done in a batch using:

`~/secure_boot/custom_config $````
touch dbx.esl
```
`~/secure_boot/custom_config $``for key_type in PK KEK db; do cert-to-efi-sig-list -g $(< uuid.txt) ${key_type}.crt ${key_type}.esl; done`
#### Manual creation

The *Signature List* for the *Platform Key* can be created using:

`~/secure_boot/custom_config $``cert-to-efi-sig-list -g $(< uuid.txt) PK.crt PK.esl`
### DER file creation (optional)

To create a *DER* (*.cer*) file for each of the generated keys:

`~/secure_boot/custom_config $``for key_type in PK KEK db; do openssl x509 -outform DER -in ${key_type}.crt -out ${key_type}.cer ; done`
With the new keys created, they must be combined with existing keys, formatted and signed to be used.

### Combining the Signature Lists (optional)

The created *Signature Lists* are not populated, it often makes sense to use the factory keys as a base; these can later be removed if desired.

#### Batch concatenation

To concatenate all new and old *Signature Lists*:

`~/secure_boot $``for key_type in KEK db dbx; do cat factory_config/${key_type}.esl custom_config/${key_type}.esl > ${key_type}.esl; done`
#### Manual concatenation

The **KEK** *Signature Lists* can be concatenated using:

`~/secure_boot $``cat factory_config/KEK.esl custom_config/KEK.esl > KEK.esl`
This process can be repeated for the **db *and* dbx** *Signature Lists*

### Copying the signature lists

If the last step was not followed, all *esl* files from the custom\_config directory should be copied to the parent directory:

`~/secure_boot $``cp custom_config/*.esl .`
#### Copying the Platform Key

If the factory signature list was combined with the custom one, only the PK must be copied:

`~/secure_boot $``cp custom_config/PK.esl .`
## Signing the Signature Lists

With the *Signature Lists* finalized, they must be signed.

### Platform Key

The signed **PK** *Signature List* can be created with:

`~/secure_boot/ $``sign-efi-sig-list -k custom_config/PK.key -c custom_config/PK.crt PK PK.esl PK.auth`
#### GPG protected PK

If GPG was used to encrypt the keyfiles:

`~/secure_boot/ $``mkfifo key_pipe & sleep 1 && gpg --decrypt custom_config/PK.key.gpg > key_pipe & sign-efi-sig-list -k key_pipe -c custom_config/PK.crt PK PK.esl PK.auth ; rm key_pipe`
### Key Exchange Keys

The signed **KEK** *Signature List* can be created with:

`~/secure_boot/ $``sign-efi-sig-list -k custom_config/PK.key -c custom_config/PK.crt KEK KEK.esl KEK.auth`
#### GPG protected PK

If GPG was used to encrypt the keyfiles:

`~/secure_boot/ $``mkfifo key_pipe & sleep 1 && gpg --decrypt custom_config/PK.key.gpg > key_pipe & sign-efi-sig-list -k key_pipe -c custom_config/PK.crt KEK KEK.esl KEK.auth ; rm key_pipe`
### Signature Databases

Finally, this process must be used with the **db** and **dbx** *Signature Lists*:

`~/secure_boot/ $``for db_type in db dbx; do sign-efi-sig-list -k custom_config/KEK.key -c custom_config/KEK.crt $db_type ${db_type}.esl ${db_type}.auth ; done`
#### GPG protected KEK

If GPG was used to protect the KEK private key:

`~/secure_boot/ $``mkfifo key_pipe & sleep 1 && for db_type in db dbx; do gpg --decrypt custom_config/KEK.key.gpg > key_pipe & sign-efi-sig-list -k key_pipe -c custom_config/KEK.crt $db_type ${db_type}.esl ${db_type}.auth ; done ; rm key_pipe`
#### Reset Key

If an empty file is passed as an *esl*, it creates an *auth* file which can be leaded into the *Platform Key* slot, disabling it:

`~/secure_boot/ $````
rm -f noPK.esl
```
`~/secure_boot/ $````
touch noPK.esl
```
`~/secure_boot/ $````
mkfifo key_pipe
```
`~/secure_boot/ $````
mkfifo nopk_pipe
```
`~/secure_boot/ $````
gpg --decrypt custom_config/PK.key.gpg > key_pipe & sign-efi-sig-list -k key_pipe -c custom_config/PK.crt PK noPK.esl nopk_pipe &
```
`~/secure_boot/ $````
gpg --recipient larry@gentoo.org --output noPK.auth.gpg --encrypt < nopk_pipe 
```
`~/secure_boot/ $````
rm nopk_pipe
```
`~/secure_boot/ $``rm key_pipe`
## Installing the keys

On most systems, the keys and signature lists can be installed using **efi-updatevar**. If this is not possible, the keys must be loaded using the UEFI.

Before installing the keys, the system must be put into *Setup Mode*. This process differ widely across systems, but generally, there is an option under the *Security* tab allowing secure boot to be disabled, and for the keys to be cleared/setup mode to be entered.

### Installing the Key Exchange Key

While in *Setup Mode*, the **KEK** can be installed with either the *.auth*, or *.esl* file:

`~/secure_boot/ $``sudo efi-updatevar -e -f KEK.esl KEK`
While in *User Mode*, the **KEK** can be installed with a valid *KEK.auth*:

`~/secure_boot/ $``sudo efi-updatevar -f KEK.auth KEK`
### Installing the Database Keys

While in *Setup Mode*, the **db** and **dbx** can be installed with either the *.auth*, or *.esl* file:

`~/secure_boot/ $``for db_type in db dbx; do sudo efi-updatevar -e -f ${db_type}.esl $db_type; done`
While in *User Mode*, the **KEK** can be installed with a valid *KEK.auth*:

`~/secure_boot/ $``for db_type in db dbx; do sudo efi-updatevar -f ${db_type}.auth $db_type; done`
### Installing the Platform Key

While in *Setup Mode*, the **PK** can be installed using:

`~/secure_boot/ $``sudo efi-updatevar -f PK.auth PK`
Successful execution of this command exits *Setup Mode* and enters *User Mode*

## Signing Boot Files

Before activating secure boot, all boot files must be signed using the **db** key.

### Signing

Advanced usage, with GPG encrypted keys:

`~/secure_boot/ $``mkfifo key_pipe & sleep 1 && gpg --decrypt custom_config/db.key.gpg > key_pipe & sudo sbsign --key key_pipe --cert custom_config/db.crt --output /efi/signed-vmlinuz /boot/vmlinuz; rm key_pipe`
Basic usage with an unencrypted **db** private key:

`~/secure_boot/ $``sudo sbsign --key custom_config/db.key --cert custom_config/db.crt --output /efi/signed-vmlinuz /boot/vmlinuz`
**efibootmgr** can be used to add a boot entry for this new signed file, or this signed kernel image can be moved to /boot/EFI/BOOT/BOOTx64.efi.

#### Creating an EFI Boot Manager entry

To create an EFI boot entry for /efi/vmlinuz-6.3.4-gentoo-r1-initramfs.signed which is under the root of /dev/sda1:

`root #``efibootmgr --disk /dev/sda --create --label "Signed Gentoo 6.3.4" --loader vmlinuz-6.3.4-gentoo-r1-initramfs.signed`
### Verifying Signatures

Files signed using this process can be checked using:

`~/secure_boot/ $``sbverify --cert custom_config/db.crt /efi/vmlinuz-6.3.4-gentoo-r1-initramfs.signed`
Signature verification OK

### Listing Signatures

Signatures can be listed using:

`user $``sbverify --list /efi/vmlinuz-6.3.4-gentoo-r1-initramfs.signed`
signature 1
image signature issuers:
 - /CN=Larry's db
image signature certificates:
 - subject: /CN=Larry's db
   issuer:  /CN=Larry's db

## Checking Secure Boot Status

When Secure Boot is enabled, **dmesg** should report:

`root #``dmesg | grep -i "secure"`
\[    0.010667\] Secure boot enabled

Additionally, several tools are available to read and interpret the EFI variables associated with Secure Boot:

### Directly reading from sysfs

od can be used to read the *Secure Boot* status from /sys/firmware/efi/efivars/:

`user $``od --address-radix n --format x1 /sys/firmware/efi/efivars/SecureBoot-8be4df61-93ca-11d2-aa0d-00e098032b8c`
06 00 00 00 01

A value of 1 in the last portion indicates that *Secure Boot* is set to be active in the *EFI variables*.

### efivar

**efivar** can be used to read and change the values of EFI variables:

#### Listing variables

The **--list** parameter can be used to read current *EFI* variables.

`user $``efivar --list | grep -i secure`
aa1305b9-01f3-4afb-920e-c9b979a852fd-SecureBootData
8be4df61-93ca-11d2-aa0d-00e098032b8c-SecureBoot
59d1c24f-50f1-401a-b101-f33e0daed443-SecureBootEnforce
382af2bb-ffff-abcd-aaee-cce099338877-SecureFlashInfo
59d1c24f-50f1-401a-b101-f33e0daed443-AdministerSecureBoot

#### Checking Secure Boot Status

The status of secure boot can be checked with:

`user $``efivar --print --name 8be4df61-93ca-11d2-aa0d-00e098032b8c-SecureBoot`
Name: "SecureBoot"
Attributes:
	Boot Service Access
	Runtime Service Access
Value:
00000000  01                                                |.               |

An output of 1 indicates Secure Boot is enabled.

### sbctl

sbctl status outputs the current Secure Boot state, including the state of Setup Mode. A typical output after successful setup:

`user $``sbctl status`
Installed:	✓ sbctl is installed
Owner GUID:	a9fbbdb7-a05f-48d5-b63a-08c5df45ee70
Setup Mode:	✓ Disabled
Secure Boot:	✓ Enabled
Vendor Keys:	microsoft

`Setup Mode` and `Secure Boot` refer directly to the current Secure Boot state. Other fields may vary depending on how the user set up Secure Boot.

### bootctl

bootctl outputs the current Secure Boot state, among other information. This command is available via [sys-apps/systemd-utils](https://packages.gentoo.org/packages/sys-apps/systemd-utils). A typical output after successful setup:

`user $``bootctl````
System:
      Firmware: UEFI 2.80 (American Megatrends 5.26)
 Firmware Arch: x64
   Secure Boot: enabled (user)
  TPM2 Support: yes
  Measured UKI: yes
  Boot into FW: supported
...
```
`Secure Boot` refers to the current Secure Boot state. Other fields may vary.

## Resetting the Platform Key

Using a *noPK.auth*, to update the **PK** disables it:

`~/secure_boot/ $``mkfifo auth_pipe & sleep 1 && gpg --decrypt noPK.auth.gpg > auth_pipe & sudo efi-updatevar -f auth_pipe PK`
## See also

- [Secure Boot/GRUB](https://wiki.gentoo.org/wiki/Secure_Boot/GRUB) — This article explains how to set up [GRUB](https://wiki.gentoo.org/wiki/GRUB) to work with [Secure Boot].
- [Full Disk Encryption From Scratch Simplified](https://wiki.gentoo.org/wiki/Full_Disk_Encryption_From_Scratch_Simplified) — a guide which covers the process of configuring a drive to be encrypted using LUKS and btrfs.
- [Security Handbook/Boot Path Security](https://wiki.gentoo.org/wiki/Security_Handbook/Boot_Path_Security) — boot path security.
- [Trusted Boot](https://wiki.gentoo.org/wiki/Trusted_Boot)
- [Trusted Platform Module](https://wiki.gentoo.org/wiki/Trusted_Platform_Module) — The **Trusted Platform Module**, or **TPM** for short, is a secure, isolated, cryptographic processor that is typically built into most modern computers.
- [Unified Kernel Image](https://wiki.gentoo.org/wiki/Unified_Kernel_Image) — a single executable which can be [booted directly from UEFI firmware](https://wiki.gentoo.org/wiki/EFI_stub), or automatically sourced by boot-loaders with little or no configuration.
- [Shim](https://wiki.gentoo.org/wiki/Shim) — an alternative method of managing accepted [Secure Boot] keys without touching the [UEFI](https://wiki.gentoo.org/wiki/UEFI) firmware settings
- [sbctl](https://wiki.gentoo.org/wiki/Sbctl) — a user-friendly secure boot key manager written in GO.
