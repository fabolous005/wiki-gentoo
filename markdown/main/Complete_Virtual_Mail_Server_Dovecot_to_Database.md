<!-- source: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Dovecot_to_Database | group: Gentoo Wiki (Main) | wiki-title: Complete Virtual Mail Server/Dovecot to Database -->
---
title: Complete Virtual Mail Server/Dovecot to Database
url: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Dovecot_to_Database
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-01"
fingerprint: "3202099ed58439c8"
license: CC BY-SA 4.0
---

# Complete Virtual Mail Server/Dovecot to Database

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



Dovecot will be used to provide [IMAP](https://en.wikipedia.org/wiki/Internet_Message_Access_Protocol) services.

To use POP3, which is explicitly discouraged, see [Complete Virtual Mail Server/POP3](https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/POP3).

## Installing Dovecot

### USE flags

[net-mail/dovecot](https://packages.gentoo.org/packages/net-mail/dovecot) has a few USE flags that need to be examined.


### USE flags for
            [net-mail/dovecot](https://packages.gentoo.org/packages/net-mail/dovecot)
            
            An IMAP and POP3 server written with security primarily in mind

| [argon2](https://packages.gentoo.org/useflags/argon2) | Add support for ARGON2 password schemes | 
| [caps](https://packages.gentoo.org/useflags/caps) | Use Linux capabilities library to control privilege | 
| [cdb](https://packages.gentoo.org/useflags/cdb) | Add support for the CDB database engine from the author of qmail | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [lua](https://packages.gentoo.org/useflags/lua) | Enable Lua scripting support | 
| [lucene](https://packages.gentoo.org/useflags/lucene) | Add lucene full text search (FTS) support using dev-cpp/clucene | 
| [lz4](https://packages.gentoo.org/useflags/lz4) | Enable support for lz4 compression (as implemented in app-arch/lz4) | 
| [managesieve](https://packages.gentoo.org/useflags/managesieve) | Add managesieve protocol support | 
| [mysql](https://packages.gentoo.org/useflags/mysql) | Add mySQL Database support | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Add support for the postgresql database | 
| [rpc](https://packages.gentoo.org/useflags/rpc) | Add support for NFS quotas | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [sieve](https://packages.gentoo.org/useflags/sieve) | Add sieve support | 
| [solr](https://packages.gentoo.org/useflags/solr) | Add solr full text search (FTS) support | 
| [sqlite](https://packages.gentoo.org/useflags/sqlite) | Add support for sqlite - embedded sql database | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [stemmer](https://packages.gentoo.org/useflags/stemmer) | Add libstemmer support (for FTS) | 
| [suid](https://packages.gentoo.org/useflags/suid) | Enable setuid root program(s) | 
| [system-icu](https://packages.gentoo.org/useflags/system-icu) | Use system dev-libs/icu instead of the bundled library | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [tcpd](https://packages.gentoo.org/useflags/tcpd) | Add support for TCP wrappers | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [textcat](https://packages.gentoo.org/useflags/textcat) | Add libtextcat language guessing support for full text search (FTS) | 
| [unwind](https://packages.gentoo.org/useflags/unwind) | Add support for call stack unwinding and function name resolution | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [xapian](https://packages.gentoo.org/useflags/xapian) | Add xapian (flatcurve) full text search (FTS) support | 
| [zstd](https://packages.gentoo.org/useflags/zstd) | Enable support for ZSTD compression | 

Regarding the database flags, only choose the desired database backend. Other flags may be activated if their functionality is desired.

### Emerge

`root #``emerge --ask net-mail/dovecot`
### Configuring dovecot

**`/etc/dovecot/dovecot.conf`**

**enable imap**

```
protocols = imap
```
**`/etc/dovecot/conf.d/10-mail.conf`**

**mailbox setup**

```
mail_driver = maildir
mail_gid = 5000
mail_path = ~/%{maildir}
mail_uid = 5000
mailbox_idle_check_interval = 30 secs
mailbox_list_index = yes
maildir_copy_with_hardlinks = yes
namespace inbox {
  inbox = yes
}
```
**`/etc/dovecot/conf.d/10-ssl.conf`**

**TLS setup**

```
ssl_cipher_list = ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES256-GCM-SHA384
ssl_min_protocol = TLSv1.2
ssl_server {
  cert_file = /etc/letsencrypt/live/example.com/fullchain.pem
  key_file = /etc/letsencrypt/live/example.com/privkey.pem
}
```
### Configuring the authentication mechanism

#### PostgreSQL

**`/etc/dovecot/conf.d/10-auth.conf`**

**Authentication setup**

```
auth_allow_cleartext = no
auth_default_domain = example.com
auth_failure_delay = 2 secs
auth_mechanisms = plain login
auth_username_chars = abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ01234567890.-_@
```
**`/etc/dovecot/conf.d/20-sql.conf`**

**Connection with postgres**

```
dict_server {
  dict sql {
    sql_driver = pgsql
    pgsql localhost {
      parameters {
        dbname = postfix
        password = secret
        user = postfix
      }
    }
  }
}
passdb sql {
  query = SELECT local_part AS username, domain, password FROM mailbox WHERE local_part = '%n' AND domain = '%d'
}
userdb sql {
  query = SELECT local_part AS user, CONCAT('/var/vmail/',maildir) AS home FROM mailbox WHERE local_part = '%n' AND domain = '%d'
}
```
### Access permissions

Permissions must be set correctly, as the files can contain sensitive password information:

`root #``chmod 660 /etc/dovecot/conf.d/20-sql.conf`
### Testing authentication

Dovecot includes a simple testing utility. It requires a valid username as parameter.

To perform some basic tests, start dovecot:

`root #``rc-service dovecot start`
Run the auth utility with the testuser:

`root #``dovecot auth login testuser`
passdb: testuser auth succeeded
extra fields:
  user=testuser@example.com
  
  original\_user=testuser
userdb extra fields:
  testuser
  home=/var/vmail/example.com/testuser/
  auth\_mech=PLAIN

## Testing IMAP

Dovecot should be started:

`root #``rc-service dovecot start`
Once started, telnet could be used to identify initial problems. Once logging in with telnet works, a mail client can be used:

`user $``telnet example.com 143`
Trying 127.0.0.1...
Connected to example.com.
Escape character is '^\]'.
\* OK \[CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE LITERAL+ STARTTLS AUTH=PLAIN AUTH=LOGIN\] Dovecot ready.
1 LOGIN testuser secret 
1 OK \[CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE SORT SORT=DISPLAY THREAD=REFERENCES THREAD=REFS THREAD=ORDEREDSUBJECT MULTIAPPEND URL-PARTIAL CATENATE UNSELECT CHILDREN NAMESPACE UIDPLUS LIST-EXTENDED I18NLEVEL=1 CONDSTORE QRESYNC ESEARCH ESORT SEARCHRES WITHIN CONTEXT=SEARCH LIST-STATUS BINARY MOVE SNIPPET=FUZZY PREVIEW=FUZZY PREVIEW STATUS=SIZE SAVEDATE LITERAL+ NOTIFY SPECIAL-USE\] Logged in
1 LOGOUT
\* BYE Logging out
1 OK LOGOUT completed (0.001 + 0.000 secs).
Connection closed by foreign host.

If testing works properly, add dovecot to the default runlevel:

`root #``rc-update add dovecot default`
