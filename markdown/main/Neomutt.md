<!-- source: https://wiki.gentoo.org/wiki/Neomutt | group: Gentoo Wiki (Main) | wiki-title: Neomutt -->
---
title: Neomutt
url: https://wiki.gentoo.org/wiki/Neomutt
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-10-10"
fingerprint: eb05485c5cb439c4
license: CC BY-SA 4.0
---

# Neomutt

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**neomutt** is a command-line mail client forked from [mutt](https://wiki.gentoo.org/wiki/Mutt).

## Installation

### Emerge

`root #``emerge --ask mail-client/neomutt`
### USE Flags


| [autocrypt](https://packages.gentoo.org/useflags/autocrypt) | Enable autocrypt.org support | 
| [berkdb](https://packages.gentoo.org/useflags/berkdb) | Enable BDB (Berkley DB) backend for header caching | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [gdbm](https://packages.gentoo.org/useflags/gdbm) | Enable GDBM (GNU dbm) backend for header caching | 
| [gnutls](https://packages.gentoo.org/useflags/gnutls) | Prefer net-libs/gnutls as SSL/TLS provider (ineffective with USE=-ssl) | 
| [gpgme](https://packages.gentoo.org/useflags/gpgme) | Build gpgme backend to support S/MIME, PGP/MIME and traditional/inline PGP | 
| [idn](https://packages.gentoo.org/useflags/idn) | Enable support for Internationalized Domain Names | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [kyotocabinet](https://packages.gentoo.org/useflags/kyotocabinet) | Enable Kyoto Cabinet database backend for header caching | 
| [lmdb](https://packages.gentoo.org/useflags/lmdb) | Enable LMDB (Lightning Memory-Mapped Database) backend for header caching | 
| [lz4](https://packages.gentoo.org/useflags/lz4) | Add lz4 support for header cache compression | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [notmuch](https://packages.gentoo.org/useflags/notmuch) | Enable support for net-mail/notmuch | 
| [pgp-classic](https://packages.gentoo.org/useflags/pgp-classic) | Build classic-pgp backend to support PGP/MIME and traditional/inline PGP | 
| [qdbm](https://packages.gentoo.org/useflags/qdbm) | Enable QDBM (Quicker Database Manager) database backend for header caching | 
| [sasl](https://packages.gentoo.org/useflags/sasl) | Add support for the Simple Authentication and Security Layer | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [smime-classic](https://packages.gentoo.org/useflags/smime-classic) | Build classic-smime backend to support S/MIME | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [tokyocabinet](https://packages.gentoo.org/useflags/tokyocabinet) | Enable Tokyo Cabinet database backend for header caching | 
| [zlib](https://packages.gentoo.org/useflags/zlib) | Add zlib support for header cache compression | 
| [zstd](https://packages.gentoo.org/useflags/zstd) | Add zstd support for header cache compression | 

## Configuration

Configure Neomutt in the configuration file located in \~/.config/neomutt/neomuttrc.

Here is an example from the official github:

**`~/.config/neomutt/neomuttrc`**

### Configuration Scripts

#### Mutt Wizard

Initial setup of Neomutt can be intimidating to those who have not done it before. To make the process easier, [mail-client/mutt-wizard](https://packages.gentoo.org/packages/mail-client/mutt-wizard) was created. As the name suggests, it walks the user step-by-step through the process of setting up a proper muttrc file.

There might be a few errors which can be solved with the USE Flag sasl.

Set the use flag in /etc/portage/package.use, then re-emerge neomutt:

`root #``emerge --ask neomutt`
