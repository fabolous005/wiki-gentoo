<!-- source: https://wiki.gentoo.org/wiki/Mount_Encrypted_Ubuntu_Home/Guide | group: Gentoo Wiki (Main) | wiki-title: Mount Encrypted Ubuntu Home/Guide -->
---
title: Mount Encrypted Ubuntu Home/Guide
url: https://wiki.gentoo.org/wiki/Mount_Encrypted_Ubuntu_Home/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-07-10"
fingerprint: "3c88f01836e70764"
license: CC BY-SA 4.0
---

# Mount Encrypted Ubuntu Home/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Ubuntu allows users to encrypt their home directories upon installation. In case of hardware failure it is easy to decrypt and access these files with Gentoo so they can be recovered. The encrypted home directory and either the login password, or the decryption passphrase are all that is required.

## Installation

### Kernel

The files and filenames are individually encrypted and decrypted on the fly using [eCryptfs](https://wiki.gentoo.org/wiki/ECryptfs). eCryptfs needs to be enabled in the kernel:

`root #``cd /usr/src/linux``root #``make menuconfig``root #``make && make modules_install``root #``mount /boot``root #``make install`
Reboot!

Install the ecrypt file system utilities:

`root #``emerge --ask sys-fs/ecryptfs-utils`
## Locating the files

Locate the Ubuntu encrypted home directory for decryption. If the home directory is on an external hard drive Gentoo may have automagically mounted it at:

/run/media/{username}/{UUID}

As an example we will use:

/run/media/anon/27a70809-cb85-43eb-908f-ecb759dd4c99/

The decryption target would then be the users home directory:

/run/media/anon/27a70809-cb85-43eb-908f-ecb759dd4c99/home/user

That folder is, however, empty; except for some symbolic links. Ubuntu puts the encrypted home directory files in a different directory; which is then decrypted and mounted on the fly to the users home directory by ecryptfs. All of the encrypted files for our example are located here:

/run/media/anon/27a70809-cb85-43eb-908f-ecb759dd4c99/home/.ecryptfs/user/.Private

## Decryption passphrase

The passphrase is a 16-byte hexadecimal number that Ubuntu asks the user to record after installation is complete. The example passphrase is: `7069ca27397aa8ac9163fe7a703257f7`

If the decryption passphrase is known move on to [the next step](https://wiki.gentoo.org#Filename_Encryption).

If the decryption passphrase is unknown it can be discovered by using the logon password to decrypt the ***wrapped-passphrase*** file:

/run/media/anon/27a70809-cb85-43eb-908f-ecb759dd4c99/home/.ecryptfs/user/.ecryptfs/wrapped-passphrase

Unwrap the passphrase:

`user $``ecryptfs-unwrap-passphrase /run/media/anon/27a70809-cb85-43eb-908f-ecb759dd4c99/home/.ecryptfs/user/.ecryptfs/wrapped-passphrase`
Passphrase: 
7069ca27397aa8ac9163fe7a703257f7

## Filename encryption

The filename encryption key is needed before the files can be accessed. Also the decryption passphrase needs to be added to the user session keyring. Accomplish both of these things with the following command:

`root #``ecryptfs-add-passphrase --fnek`
Passphrase:
Inserted auth tok with sig \[fe4b983ff729814b\] into the user session keyring
Inserted auth tok with sig \[cd7b5893b93c0920\] into the user session keyring

The filename encryption key is output as a hexadecimal number in the second set of brackets.
The example filename encryption key is `cd7b5893b93c0920`

## Decrypt and mount

Give the mount command with three options: type ecryptfs, the location of the encrypted files, and the location to mount the decrypted files at. The example command is:

`root #``mount -t ecryptfs /run/media/anon/27a70809-cb85-43eb-908f-ecb759dd4c99/home/.ecryptfs/user/.Private /run/media/anon/27a70809-cb85-43eb-908f-ecb759dd4c99/home/user`
At the interactive prompt make the following eight entries/choices:

1. Passphrase
2. Cipher: AES
3. Key bytes: 16
4. plaintext passthrough: n
5. Filename encryption: y
6. Filename Encryption Key
7. Proceed?: yes
8. Append sig?: no

Passphrase: 
Select cipher: 
 1) aes: blocksize = 16; min keysize = 16; max keysize = 32
 2) blowfish: blocksize = 16; min keysize = 16; max keysize = 56
 3) des3_ede: blocksize = 8; min keysize = 24; max keysize = 24
 4) twofish: blocksize = 16; min keysize = 16; max keysize = 32
 5) cast6: blocksize = 16; min keysize = 16; max keysize = 32
 6) cast5: blocksize = 8; min keysize = 5; max keysize = 16
Selection [aes]: aes
Select key bytes: 
 1) 16
 2) 32
 3) 24
Selection [16]: 16
Enable plaintext passthrough (y/n) [n]: n
Enable filename encryption (y/n) [n]: y
Filename Encryption Key (FNEK) Signature [fe4b983ff729814b]: cd7b5893b93c0920
Attempting to mount with the following options:
  ecryptfs_unlink_sigs
  ecryptfs_fnek_sig=cd7b5893b93c0920
  ecryptfs_key_bytes=16
  ecryptfs_cipher=aes
  ecryptfs_sig=fe4b983ff729814b
WARNING: Based on the contents of [/root/.ecryptfs/sig-cache.txt],
it looks like you have never mounted with this key 
before. This could mean that you have typed your 
passphrase wrong.
Would you like to proceed with the mount (yes/no)? : yes
Would you like to append sig [fe4b983ff729814b] to
[/root/.ecryptfs/sig-cache.txt] 
in order to avoid this warning in the future (yes/no)? : no
Not adding sig to user sig cache file; continuing with mount.

The decrypted files are now available for recovery or backup. In the example they are at:

/run/media/anon/27a70809-cb85-43eb-908f-ecb759dd4c99/home/user

## Troubleshooting

### Mount failures

mount: mount(2) failed: No such file or directory Error mounting eCryptfs: \[-1\] Operation not permitted Check your system logs; visit \<http://ecryptfs.org/support.html>

This usually means the key was not added to the user session keyring. Try running dmesg for a more detailed error message:

\[17955.991447\] Could not find key with description: \[91f6e7ae96b0047e\] \[17955.991449\] process\_request\_key\_err: No key \[17955.991451\] Could not find valid key in user session keyring for sig specified in mount option: \[91f6e7ae96b0047e\] \[17955.991452\] One or more global auth toks could not properly register; rc = \[-2\] \[17955.991453\] Error parsing options; rc = \[-2\]

To fix make sure that ecryptfs-add-passphrase --fnek is run by the same user that is mounting the filesystem.

Also check syslog for more errors:

Jul 10 22:54:37 gentoo mount.ecryptfs\[4402\]: could not resolve full path for source /run/media/anon/27a70809-cb85-43eb-908f-ecb759dd4c99/home/user/.Private \[-2\]

Using a symbolic link as the source for the mount will result in the above error. Make sure to use /run/media/anon/27a70809-cb85-43eb-908f-ecb759dd4c99/home/**.ecryptfs**/user/.Private when mounting.
