<!-- source: https://wiki.gentoo.org/wiki/Trusted_Platform_Module/SSH | group: Gentoo Wiki (Main) | wiki-title: Trusted Platform Module/SSH -->
---
title: Trusted Platform Module/SSH
url: https://wiki.gentoo.org/wiki/Trusted_Platform_Module/SSH
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-02"
fingerprint: be10491ad9b038e4
license: CC BY-SA 4.0
---

# Trusted Platform Module/SSH

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A TPM can be used to store SSH private keys, making them harder to steal. This mechanism is much more secure than using filesystem permissions, and is comparable in security to encrypting the keyfile, with different considerations.

SSH can be configured to read PKCS 11 keys from the TPM using libtpm\_pkcs11.

## Installation

### USE Flags


| [+pie](https://packages.gentoo.org/useflags/+pie) | Build programs as Position Independent Executables (a security hardening technique) | 
| [+ssl](https://packages.gentoo.org/useflags/+ssl) | Enable additional crypto algorithms via OpenSSL | 
| [audit](https://packages.gentoo.org/useflags/audit) | Enable support for Linux audit subsystem using sys-process/audit | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [ldns](https://packages.gentoo.org/useflags/ldns) | Use LDNS for DNSSEC/SSHFP validation. | 
| [legacy-ciphers](https://packages.gentoo.org/useflags/legacy-ciphers) | Enable support for deprecated, soon-to-be-dropped DSA keys. See https://marc.info/?l=openssh-unix-dev>m=170494903207436>w=2. | 
| [libedit](https://packages.gentoo.org/useflags/libedit) | Use the libedit library (replacement for readline) | 
| [livecd](https://packages.gentoo.org/useflags/livecd) | Enable root password logins for live-cd environment. | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [seccomp](https://packages.gentoo.org/useflags/seccomp) | Enable seccomp (secure computing mode) to perform system call filtering at runtime to increase security of programs | 
| [security-key](https://packages.gentoo.org/useflags/security-key) | Include builtin U2F/FIDO support | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [static](https://packages.gentoo.org/useflags/static) | !!do not set this during bootstrap!! Causes binaries to be statically linked instead of dynamically | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [xmss](https://packages.gentoo.org/useflags/xmss) | Enable XMSS post-quantum authentication algorithm | 



| [+fapi](https://packages.gentoo.org/useflags/+fapi) | Enable feature API (requires openssl as crypto backend) | 
| [+openssl](https://packages.gentoo.org/useflags/+openssl) | Use dev-libs/openssl as crypto engine | 
| [+policy](https://packages.gentoo.org/useflags/+policy) | Enable policy library (requires openssl as crypto backend) | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [mbedtls](https://packages.gentoo.org/useflags/mbedtls) | Use net-libs/mbedtls as crypto engine | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask app-crypt/tpm2-pkcs11``root #``emerge --ask app-crypt/tpm2-tss``root #``emerge --ask net-misc/openssh``root #``emerge --ask sys-apps/dbus`
## Configuration

### Service

#### OpenRC

Add *dbus* and *tpm2-abrmd* to the default runlevel on OpenRC systems:

`root #````
rc-update add dbus default
```
`root #````
rc-update add tpm2-abrmd default
```
`root #````
rc-service dbus start
```
`root #``rc-service tpm2-abrmd start`
### User groups

Users must be added to the *tss* group to use the TPM's stored keys:

`root #``gpasswd -a larry tss`
### Key Creation

Then, as the user, create a new primary, token & private key. There is debate about whether it is better to create the private key in software and import it (easier to audit and trust the creation). Or create the private key on the TPM so it never touches the disk. For this example, create the private key via the TPM.

`user $``tpm2_ptool init``user $``tpm2_ptool addtoken --pid=1 --label=ssh --userpin=PasswordRequiredToUsekey --sopin=AdminPasswordForUncommonModifications``user $``tpm2_ptool addkey --label=ssh --userpin=PasswordrequiredToUseKey --algorithm=ecc256`
RSA and different key sizes are available. Find a complete list in [the source](https://github.com/tpm2-software/tpm2-pkcs11/blob/1.9.0/tools/tpm2_pkcs11/tpm2.py#L25-L30).

It's possible to set `--userpin=""` to mimic the behavior of an SSH key that doesn't have password protection. But leaving it empty means the physical theft of the computer can allow an attacker to use the SSH private key through possession of the TPM alone. Setting a password achieves two factors of authentication, something you have (TPM) and something you know (password).

### SSH

#### Using the TPM by default

To configure SSH to try to use TPM keys by default:

**`~/.ssh/config`**

This directive could also be added to specific hosts like:

**`~/.ssh/config`**

## Usage

### Reading public keys

To retrieve the public key from the TPM, run:

`user $``ssh-keygen -D /usr/lib64/libtpm2_pkcs11.so`
These key fingerprints can be copied to the \~/.ssh/authorized\_keys file like with other SSH keys.

### Using keys

To use the TPM key for a single SSH connection:

`user $``ssh -I /usr/lib64/libtpm2_pkcs11.so user@remote.host.tld`
#### SSH agent

To load the key into the SSH agent:

`user $``ssh-add -s /usr/lib64/libtpm2_pkcs11.so`
#### Signing Git commits with ssh-agent

Load your key into SSH agent (if you haven't already)

`user $````
ssh-add -s /usr/lib64/libtpm2_pkcs11.so
```
Copy your SSH public key to disk

`user $````
ssh-keygen -D /usr/lib64/libtpm2_pkcs11.so > ~/.ssh/tpm_key.pub
```
Within your git repo run

`user $````
git config gpg.format ssh
```
`user $````
git config user.signingkey ~/.ssh/tpm_key.pub
```
Optionally, to sign all commits instead of `commit -S` only.

`user $````
git config commit.gpgsign true
```
## See Also

- [SSH](https://wiki.gentoo.org/wiki/SSH) — the ubiquitous tool for logging into and working on remote machines securely.
- [YubiKey/SSH](https://wiki.gentoo.org/wiki/YubiKey/SSH) — YubiKeys can be configured to authenticate SSH connections
