<!-- source: https://wiki.gentoo.org/wiki/YubiKey/SSH | group: Gentoo Wiki (Main) | wiki-title: YubiKey/SSH -->
---
title: YubiKey/SSH
url: https://wiki.gentoo.org/wiki/YubiKey/SSH
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-16"
fingerprint: cc10495879963044
license: CC BY-SA 4.0
---

# YubiKey/SSH

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

YubiKeys can be configured to authenticate SSH connections in multiple ways.

Many YubiKeys can be configured to provide FIDO/U2F authentication.  OpenSSH [8.2p1](https://www.openssh.com/txt/release-8.2)+ supports **ed25519-sk** and **ecdsa-sk** algorithms.

Most YubiKeys can be configured to provide GPG authentication. In this mode of operation, the **gpg-agent** is used instead of the typical **ssh-agent**.

## Introduction

YubiKeys provide several interfaces which can be used to authentication or encryption. Several of the authentication modules provided by YubiKeys can be used for SSH authentication. U2F is extremely secure, and can be set up in multiple ways without much configuration.

Additionally, SSH authentication is possible using the OpenPGP module on a YubiKey.

## Installation

### USE flags


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

The `security-key` *USE* flag is not enabled by default and must be enabled for U2F/FIDO support.

### Emerge

Emerge [net-misc/openssh\[security-key\]](https://packages.gentoo.org/packages/net-misc/openssh)

`root #``emerge --ask net-misc/openssh`
If using **gpg-agent**, [app-crypt/gnupg\[smartcard\]](https://packages.gentoo.org/packages/app-crypt/gnupg) is required:

`root #``emerge --ask app-crypt/gnupg`
## Configuration

### GPG

To configure GPG keys, the following article can be followed: [Configuring GPG keys on a YubiKey](https://wiki.gentoo.org/wiki/YubiKey/GPG#Generating_Keys)

The general process is to generate the keys, then load them onto the YubiKey.

### U2F

There are two main ways U2F can be used on a YubiKey.

The first method, Non-Discoverable credential mode, requires the generation of **-sk** variant keys outside of the YubiKey.  These keys must be securely stored, but are used in conjunction with the YubiKey for authentication.  This method ensures more separation of factors, but is not suitable for environments where the private key cannot be safely stored.

The second method, Discoverable (resident) mode stores the **-sk** variant key on the YubiKey's storage, and enables the usage of the key in more hostile environments, but also means a attacker could potentially authenticate using the YubiKey given the private key, and U2F key are both in one place.

#### Non-Discoverable

With the YubiKey inserted, execute:

`user $``ssh-keygen -t ed25519-sk`
Generating public/private ed25519-sk key pair.
You may need to touch your authenticator to authorize key generation.
Enter PIN for authenticator: 
You may need to touch your authenticator again to authorize key generation.
Enter file in which to save the key (/home/larry/.ssh/id\_ed25519\_sk): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/larry/.ssh/id\_ed25519\_sk
Your public key has been saved in /home/larry/.ssh/id\_ed25519\_sk.pub
The key fingerprint is:
SHA256:HjLrXXHoW/xllAnBFRw7QecrY97V+ezZTQE69wBrjOcv larry@gentoo
The key's randomart image is:
+\[ED25519-SK 256\]-+
|                 |
|           o     |
|      . . o +    |
|     . + .o\*     |
| .o   + SoBo     |
| +.    .oXoE     |
|..o.   +O =..    |
|.  o+ .o.=oo     |
|    .\*\*=\*\*\*.     |
+----\[SHA256\]-----+

First, ssh-keygen will prompt for the YubiKey's **FIDO** PIN (`Enter PIN for authenticator:`)

Then, it will prompt for a password to protect the private key with `Enter passphrase (empty for no passphrase):` .

Finally, it will prompt for a private key passphrase confirmation with `Enter same passphrase again:` .

### Destination configuration

To export public keys associated with keys in the current ssh agent, **ssh-add -L** can be used:

`user $``ssh-add -L`
sk-ssh-ed25519@openssh.com AAAAW2XXFA0S2f2tHUFyEb6ktQmtjcpfO2McZKg2r/tfnqeSjMSHDNfdJ32OI0qF5M9NsVmeYcNZwldwZvh35Pbq+kaaAgVoEcB= larry@gentoo

The public key must be added to \~/.ssh/authorized\_keys on the destination server, in the format:

**`~/.ssh/authorized_keys`**

## Usage

Once the keys are created and deployed on the destination, they are ready to be used.

### ssh-agent U2F configuration

The ssh-agent is not required, but is helpful as it can store keys. It can be started with:

`user $``eval "$(ssh-agent -s)"`
Successful execution should return the PID for the created agent.

With an active ssh-agent keys can be added using ssh-add {keyfile}:

`user $````
ssh-add example_key_file_ed25519_sk
Identity added: example_key_file_ed25519_sk (larry@gentoo)
```
The loaded keys can be viewed with:

`user $``ssh-add -L`
sk-ssh-ed25519@openssh.com AAAAW2XXFA0S2f2tHUFyEb6ktQmtjcpfO2McZKg2r/tfnqeSjMSHDNfdJ32OI0qF5M9NsVmeYcNZwldwZvh35Pbq+kaaAgVoEcB= larry@gentoo

Once the keys are installed, ssh can be used without entering a password if the private key is already loaded into the ssh-agent.

`user $``ssh remoteserver`
Confirm user presence for key ED25519-SK SHA256:pdjHnsEQIzUfAyofW/Ff9KMVrqEahJPxrumDHzSr4vuN8h
user@remoteserver \~ $

### U2F Agentless

If an agent is not being used, the key can be manually specified, requiring the key password on every usage:

`user $``ssh -i .ssh/id_ed25519_sh remoteserver`
Enter passphrase for key '.ssh/id\_ed25519\_sk': 
Confirm user presence for key ED25519-SK SHA256:pdjHnsEQIzUfAyofW/Ff9KMVrqEahJPxrumDHzSr4vuN8h
user@remoteserver \~ $

### GPG

If the OpenGPG Authentication key is being used, the GPG agent can be activated with:

`user $``export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)`
Once activated, the **gpg-agent** can be used like a typical **ssh-agent**

## See also

- [SSH](https://wiki.gentoo.org/wiki/SSH) — the ubiquitous tool for logging into and working on remote machines securely.
