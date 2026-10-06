<!-- source: https://wiki.gentoo.org/wiki/Asterisk | group: Gentoo Wiki (Main) | wiki-title: Asterisk -->
---
title: Asterisk
url: https://wiki.gentoo.org/wiki/Asterisk
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-01"
fingerprint: ee40595c598639cc
license: CC BY-SA 4.0
---

# Asterisk

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Asterisk** is a PBX server program to manage phones.

## Installation

### USE flags


### USE flags for
            [net-misc/asterisk](https://packages.gentoo.org/packages/net-misc/asterisk)
            
            Asterisk: A Modular Open Source PBX System

| [+caps](https://packages.gentoo.org/useflags/+caps) | Use Linux capabilities library to control privilege | 
| [+ssl](https://packages.gentoo.org/useflags/+ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [alsa](https://packages.gentoo.org/useflags/alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [blocks](https://packages.gentoo.org/useflags/blocks) | Utlize -fblocks (only supported by, and required when using, clang/LLVM) | 
| [bluetooth](https://packages.gentoo.org/useflags/bluetooth) | Enable Bluetooth Support | 
| [calendar](https://packages.gentoo.org/useflags/calendar) | Add support for calendars (not using mcal!) | 
| [cluster](https://packages.gentoo.org/useflags/cluster) | Enable high-availability support through the Corosync Cluster Engine | 
| [codec2](https://packages.gentoo.org/useflags/codec2) | Enable Codec2 support in asterisk | 
| [curl](https://packages.gentoo.org/useflags/curl) | Add support for client-side URL transfer library | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [deprecated](https://packages.gentoo.org/useflags/deprecated) | Enable deprecated features (eg, app\_macro) | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [freetds](https://packages.gentoo.org/useflags/freetds) | Add support for the TDS protocol to connect to MSSQL/Sybase databases | 
| [gtalk](https://packages.gentoo.org/useflags/gtalk) | Enable support for Google Talk services | 
| [http](https://packages.gentoo.org/useflags/http) | Enable embedded web server | 
| [iconv](https://packages.gentoo.org/useflags/iconv) | Enable support for the iconv character set conversion library | 
| [ilbc](https://packages.gentoo.org/useflags/ilbc) | Enable the Internet Low Bitrate Codec (iLBC) | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [lua](https://packages.gentoo.org/useflags/lua) | Enable Lua scripting support | 
| [mysql](https://packages.gentoo.org/useflags/mysql) | Add mySQL Database support | 
| [newt](https://packages.gentoo.org/useflags/newt) | Include additional tools that require redhats windowing toolkit | 
| [odbc](https://packages.gentoo.org/useflags/odbc) | Add ODBC Support (Open DataBase Connectivity) | 
| [pjproject](https://packages.gentoo.org/useflags/pjproject) | Enable support for pjproject (pjsip) | 
| [portaudio](https://packages.gentoo.org/useflags/portaudio) | Add support for the crossplatform portaudio audio API | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Add support for the postgresql database | 
| [radius](https://packages.gentoo.org/useflags/radius) | Add support for RADIUS authentication | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [snmp](https://packages.gentoo.org/useflags/snmp) | Add support for the Simple Network Management Protocol if available | 
| [span](https://packages.gentoo.org/useflags/span) | Enable support for the spandsp codec | 
| [speex](https://packages.gentoo.org/useflags/speex) | Add support for the speex audio codec (used for speech) | 
| [srtp](https://packages.gentoo.org/useflags/srtp) | Enable support for encrypted voice transmission (secure RTP) | 
| [static](https://packages.gentoo.org/useflags/static) | !!do not set this during bootstrap!! Causes binaries to be statically linked instead of dynamically | 
| [statsd](https://packages.gentoo.org/useflags/statsd) | Enable statsd integration | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [unbound](https://packages.gentoo.org/useflags/unbound) | Enable improved DNS core (SRV, NAPTR) by use of libunbound | 
| [vorbis](https://packages.gentoo.org/useflags/vorbis) | Add support for the OggVorbis audio codec | 
| [xmpp](https://packages.gentoo.org/useflags/xmpp) | Enable support for Extensible Messaging and Presence Protocol (XMPP) formerly known as Jabber | 

### Extended USE flags

The [net-misc/asterisk](https://packages.gentoo.org/packages/net-misc/asterisk) supports the [VOICEMAIL\_STORAGE](https://packages.gentoo.org/useflags/search?q=voicemail_storage) [USE\_EXPAND](https://devmanual.gentoo.org/general-concepts/use-flags/#use_expand-and-arch-use-flags) variable that is definable in [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf):

FILE **`/etc/portage/package.use/astertrisk`****Extended USE flags for Asterisk**

```
net-misc/asterisk VOICEMAIL_STORAGE: file imap odbc
```
### Emerge

Install [net-misc/asterisk](https://packages.gentoo.org/packages/net-misc/asterisk):

`root #``emerge --ask net-misc/asterisk`
## Configuration

## Usage

## Testing

The information in this section is probably **outdated**. You can help the Gentoo community by verifying and [updating this section](https://wiki.gentoo.org/index.php?title=Asterisk&action=edit).

If no PBX hardware phones are available, [linphone](https://wiki.gentoo.org/wiki/Linphone) can be used to test the setup. This is a much less expensive solution than purchasing PBX hardware.
