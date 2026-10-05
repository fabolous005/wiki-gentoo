<!-- source: https://wiki.gentoo.org/wiki/Trusted_Platform_Module/LUKS | group: Gentoo Wiki (Main) | wiki-title: Trusted Platform Module/LUKS -->
---
title: Trusted Platform Module/LUKS
url: https://wiki.gentoo.org/wiki/Trusted_Platform_Module/LUKS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: a715680aef83b4fc
license: CC BY-SA 4.0
---

# Trusted Platform Module/LUKS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Installation

**Enable support for TPM**

### USE flags


| [+fapi](https://packages.gentoo.org/useflags/+fapi) | Enable feature API (requires openssl as crypto backend) | 
| [+openssl](https://packages.gentoo.org/useflags/+openssl) | Use dev-libs/openssl as crypto engine | 
| [+policy](https://packages.gentoo.org/useflags/+policy) | Enable policy library (requires openssl as crypto backend) | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [mbedtls](https://packages.gentoo.org/useflags/mbedtls) | Use net-libs/mbedtls as crypto engine | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 


| [+argon2](https://packages.gentoo.org/useflags/+argon2) | Enable password hashing algorithm from app-crypt/argon2 | 
| [+openssl](https://packages.gentoo.org/useflags/+openssl) | Use dev-libs/openssl crypto backend | 
| [+udev](https://packages.gentoo.org/useflags/+udev) | Enable virtual/udev integration (device discovery, power and storage device support, etc) | 
| [fips](https://packages.gentoo.org/useflags/fips) | Enable FIPS mode restrictions | 
| [gcrypt](https://packages.gentoo.org/useflags/gcrypt) | Use dev-libs/libgcrypt crypto backend | 
| [kernel](https://packages.gentoo.org/useflags/kernel) | Use kernel crypto backend (mainly for embedded systems) | 
| [nettle](https://packages.gentoo.org/useflags/nettle) | Use dev-libs/nettle crypto backend | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [passwdqc](https://packages.gentoo.org/useflags/passwdqc) | Use sys-auth/passwdqc for password quality checking | 
| [pwquality](https://packages.gentoo.org/useflags/pwquality) | Use dev-libs/libpwquality for password quality checking | 
| [ssh](https://packages.gentoo.org/useflags/ssh) | Build cryptsetup-ssh for experimental support of token via SSH-server | 
| [static](https://packages.gentoo.org/useflags/static) | !!do not set this during bootstrap!! Causes binaries to be statically linked instead of dynamically | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [urandom](https://packages.gentoo.org/useflags/urandom) | Use /dev/urandom instead of /dev/random | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

`root #``emerge --ask app-crypt/tpm2-tss``root #``emerge --ask sys-fs/cryptsetup`
#### Clevis

**app-crypt/clevis** is available in the [GURU](https://wiki.gentoo.org/wiki/Project:GURU) repository which can be added with [Eselect/Repository](https://wiki.gentoo.org/wiki/Eselect/Repository).

`root #``eselect repository enable guru`
Then **app-crypt/clevis** can be emerged:

`root #``emerge --ask app-crypt/clevis`
## Usage

The TPM can be used to decrypt LUKS drives using programs like Clevis. Clevis supports many methods to encrypt and decrypt data, but this guide will focus on using TPM to decrypt LUKS-encrypted drives.

The TPM can be used to check the integrity of the system at boot. If it has been tampered with, then the root partition will not be unlocked and will require the passphrase to be entered. If no tampering has been detected, it unlocks the root partition without any user input.

#### Adding a TPM LUKS key

The **cryptsetup luksDump {device}** command can be used to view keys associated with that device.

`root #``cryptsetup luksDump /dev/nvme0n1p3`
LUKS header information
Version:       	2
Epoch:         	3
Metadata area: 	16384 \[bytes\]
Keyslots area: 	16744448 \[bytes\]
UUID:          	aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee
Label:         	(no label)
Subsystem:     	(no subsystem)
Flags:       	(no flags)
Data segments:
  0: crypt
	offset: 16777216 \[bytes\]
	length: (whole device)
	cipher: aes-xts-plain64
	sector: 512 \[bytes\]
Keyslots:
  0: luks2
	Key:        512 bits
	Priority:   normal
	Cipher:     aes-xts-plain64
	Cipher key: 512 bits
	PBKDF:      argon2id
	Time cost:  15
	Memory:     1048576
	Threads:    4
	Salt:       00 11 22 33 44 55 66 77 88 99 00 aa bb cc dd ee 
	           00 11 22 33 44 55 66 77 88 99 00 aa bb cc dd ee
	AF stripes: 4000
	AF hash:    sha256
	Area offset:111111 \[bytes\]
	Area length:111111 \[bytes\]
	Digest ID:  0
Tokens:
Digests:
  0: pbkdf2
	Hash:       sha256
	Iterations: 111111
	Salt:       00 11 22 33 44 55 66 77 88 99 00 aa bb cc dd ee
	           00 11 22 33 44 55 66 77 88 99 00 aa bb cc dd ee
	Digest:     00 11 22 33 44 55 66 77 88 99 00 aa bb cc dd ee
	           00 11 22 33 44 55 66 77 88 99 00 aa bb cc dd ee

Now, using clevis, add a key to our LUKS header that will be stored in the TPM:

`root #``clevis luks bind -d /dev/nvme0n1p3 tpm2 '{"pcr_bank":"sha256","pcr_ids":"0,2,3,5,6,7"}'`
Enter existing LUKS password: 
Warning: Value 512 is outside of the allowed entropy range, adjusting it.

Finally, cryptsetup luksDump can be used to confirm that the new key has been added to the LUKS header:

`root #``cryptsetup luksDump /dev/nvme0n1p3`
LUKS header information
Version:       	2
Epoch:         	5
Metadata area: 	16384 \[bytes\]
Keyslots area: 	16744448 \[bytes\]
UUID:          	af9789b4-39fc-4e3c-aeba-4b9542a5d4e7
Label:         	(no label)
Subsystem:     	(no subsystem)
Flags:       	(no flags)
Data segments:
  0: crypt
	offset: 16777216 \[bytes\]
	length: (whole device)
	cipher: aes-xts-plain64
	sector: 512 \[bytes\]
Keyslots:
  0: luks2
	Key:        512 bits
	Priority:   normal
	Cipher:     aes-xts-plain64
	Cipher key: 512 bits
	PBKDF:      argon2id
	Time cost:  15
	Memory:     1048576
	Threads:    4
	Salt:       00 11 22 33 44 55 66 77 88 99 00 aa bb cc dd ee
	           00 11 22 33 44 55 66 77 88 99 00 aa bb cc dd ee
	AF stripes: 4000
	AF hash:    sha256
	Area offset:111111 \[bytes\]
	Area length:111111 \[bytes\]
	Digest ID:  0
  1: luks2
	Key:        512 bits
	Priority:   normal
	Cipher:     aes-xts-plain64
	Cipher key: 512 bits
	PBKDF:      argon2id
	Time cost:  16
	Memory:     1048576
	Threads:    4
	Salt:       00 11 22 33 44 55 66 77 88 99 00 aa bb cc dd ee
	           00 11 22 33 44 55 66 77 88 99 00 aa bb cc dd ee
	AF stripes: 4000
	AF hash:    sha256
	Area offset:111111 \[bytes\]
	Area length:111111 \[bytes\]
	Digest ID:  0
Tokens:
  0: clevis
	Keyslot:    1
Digests:
  0: pbkdf2
	Hash:       sha256
	Iterations: 111111
	Salt:       00 11 22 33 44 55 66 77 88 99 00 aa bb cc dd ee
	           00 11 22 33 44 55 66 77 88 99 00 aa bb cc dd ee
	Digest:     00 11 22 33 44 55 66 77 88 99 00 aa bb cc dd ee
	           00 11 22 33 44 55 66 77 88 99 00 aa bb cc dd ee

Now, a second keyslot is shown : the one that's been created by **clevis**.

#### Rebuilding the initramfs

##### Dracut

[app-crypt/clevis](https://packages.gentoo.org/packages/app-crypt/clevis) installs a hook to allow clevis to work at boot time. When it's installed, dracut will detected it and automatically add the clevis module to the initramfs. Therefore, this is as simple as running the usual dracut command. You can check the output to confirm it added the clevis module:

`root #``dracut`
...
dracut: \*\*\* Including module: clevis \*\*\*
dracut: \*\*\* Including module: clevis-pin-sss \*\*\*
dracut: \*\*\* Including module: clevis-pin-tpm2 \*\*\*
dracut: \*\*\* Including module: crypt \*\*\*
dracut: \*\*\* Including module: dm \*\*\*
...

Once the initramfs is built and deployed, the system is ready to reboot and automatically decrypt the root partition, as long as the system has not been tampered with.

## Troubleshooting

### pcr-input-file filesize does not match pcr set-list

If the TPM has multiple banks, such as SHA1 and SHA256, clevis will fail to encrypt data when given only the pcr\_ids:

`root #``echo "Super Secret Password" | clevis encrypt tpm2 '{"pcr_ids":"1,7"}' > pass.jwe`
ERROR: pcr-input-file filesize does not match pcr set-list
ERROR: Could not build pcr policy
ERROR: Unable to run tpm2\_createpolicy

To remedy to this, specify which bank to read the pcr\_ids from. For example :

`root #``echo "Super Secret Password" | clevis encrypt tpm2 '{"pcr_bank":"sha256","pcr_ids":"1,7"}' > pass.jwe`
### TPM is in DA lockout mode

If the TPM fails to boot and dracut repeatedly logs some errors about being unable to unseal:

WARNING:esys:src/tss2-esys/api/Esys\_Unseal.c:295:Esys\_Unseal\_Finish() Received TPM Error
ERROR:esys:src/tss2-esys/api/Esys\_Unreal.c:98:Esys\_Unseal\_Finish() Received TPM Error
ERROR: Esys\_Unseal(0x99D) - tpm:session(1):a policy check failed
ERROR: Unable to run tpm2\_unseal
Unsealing jwk from TPM failed!
/dev/nvme0n1p3 could not be opened.
Unable to unlock /dev/nvme0n1p3 (UUID=...)

This means dracut failed to unseal the key for one reason or another (most likely, the PCR IDs returned different values), and the TPM locked itself to prevent tampering.

The easiest way to recover from this error is to power off the computer and wait for 10 minutes until the TPM unlocks itself. Another alternative is to reboot on a live CD and rebuild the initramfs without the clevis module, though that may very well take more than 10 minutes.

To avoid this error reoccurring in the future, try to figure out which PCR ID has changed, remove the TPM LUKS key, then add a new one without that ID.

## See also

- [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot) — an enhancement of the security of the pre-boot process of a [UEFI](https://wiki.gentoo.org/wiki/UEFI) system.
