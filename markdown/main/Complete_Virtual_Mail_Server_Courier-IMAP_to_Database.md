<!-- source: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Courier-IMAP_to_Database | group: Gentoo Wiki (Main) | wiki-title: Complete Virtual Mail Server/Courier-IMAP to Database -->
---
title: Complete Virtual Mail Server/Courier-IMAP to Database
url: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Courier-IMAP_to_Database
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-06"
fingerprint: a601291a778431f7
license: CC BY-SA 4.0
---

# Complete Virtual Mail Server/Courier-IMAP to Database

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Courier-IMAP will be used to provide [IMAP](https://en.wikipedia.org/wiki/Internet_Message_Access_Protocol) services.

To use POP3, which is explicitly discouraged, see [Complete Virtual Mail Server/POP3](https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/POP3).

## Installing Courier-IMAP

### USE flags

[net-mail/courier-imap](https://packages.gentoo.org/packages/net-mail/courier-imap) has a few USE flags that need to be examined. [net-libs/courier-authlib](https://packages.gentoo.org/packages/net-libs/courier-authlib) is an important dependency as it is responsible for how users authenticate, so it's USE flags are examined as well.


| [berkdb](https://packages.gentoo.org/useflags/berkdb) | Add support for sys-libs/db (Berkeley DB) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [gdbm](https://packages.gentoo.org/useflags/gdbm) | Add support for sys-libs/gdbm (GNU database libraries) | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [mysql](https://packages.gentoo.org/useflags/mysql) | Add mySQL Database support | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Add support for the postgresql database | 
| [sqlite](https://packages.gentoo.org/useflags/sqlite) | Add support for sqlite - embedded sql database | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 

Only choose the desired database backends.

### Emerge

If the proper USE flags are set, emerging [net-mail/courier-imap](https://packages.gentoo.org/packages/net-mail/courier-imap) should pull in [net-libs/courier-authlib](https://packages.gentoo.org/packages/net-libs/courier-authlib):

`root #``emerge --ask net-mail/courier-imap`
### Configuring courier-authlib

#### Postgres

Courier-authlib runs as root, so access to the socket will be permitted by default.

**`/etc/courier/authlib/authpgsqlrc`**

**Obtain access to the database**

If logins are used in the syntax of `user@domain.com` instead of `username`, the value of `PGSQL_LOGIN_FIELD` needs to be changed from `local_part` to `username`.

To use more advanced authentication, SQL statements the `PGSQL_SELECT_CLAUSE` can be used. Courier-authlib will ignore any of the previous set parameters for SELECT statements, but will use them for counting the number of accounts or for changing the password and thus when changing the password the username field is still used. If authentication is done against username remember that usernames need to be unique:

**`/etc/courier/authlib/authpgsqlrc`**

**Obtain custom access to the database**

#### MySQL

Next, reconfigure the authentication to use the mailsql database in courier-imap and postfix. In all of the following examples, replace `$password` with the password set for the mailsql mysql user:

**`/etc/courier/authlib/authmysqlrc`**

**Configuring authentication**

Reload the necessary services:

`root #````
rc-service courier-authlib restart
```
`root #``rc-service saslauthd restart`
### Configuring authdaemon

Authdaemon is actually responsible for doing the authentication. Here authdaemon is configured to use the right database backend. Portage actually does set the `authmodulelist` variable properly. Verify this in the configuration file. Also enable debugging here as this will help with any potential issues later.

#### PostgreSQL

**`/etc/courier/authlib/authdaemonrc`**

**Verify authentication modules**

#### MySQL

**`/etc/courier/authlib/authdaemonrc`**

**Verify authentication modules**

### Access permissions

Permissions must be set correctly, as the files can contain sensitive password information:

`root #``chmod 660 /etc/courier/authlib/auth*rc``root #``chown mail:mail /etc/courier/authlib/auth*rc`
### Testing courier-authlib

Courier-authlib includes a simple testing utility. It requires a valid username as parameter. Don't hesitate to check out the authtest manual page for more information. It is short but concise.

To perform some basic tests start the authlib daemon:

`root #``/etc/init.d/courier-authlib start`
Run authtest with the testuser:

`root #``authtest testuser````
Authentication succeeded.
 
     Authenticated: testuser  (uid 5000, gid 5000)
    Home Directory: /var/vmail
           Maildir: example.com/testuser/
             Quota: (none)
Encrypted Password: $1$16117118$ajxN3QRilmP5zLVHjTkE31
Cleartext Password: (none)
           Options: (none)
```
## Configuring IMAP

IMAP has a few *capabilities* that can be enabled:

- `SORT` to allow server side searching through messages.
- `IDLE` to allow 'push' like connections.

**`/etc/courier-imap/imapd`**

**Enable imapd and some options**

Some clients need a minimum DH parameter length of 2048 bits (such as those using >=dev-libs/nss-3.19.1). Generate one with:

`root #``DH_BITS=2048 mkdhparams`
Next ensure Courier is using it:

**`/etc/courier-imap/imapd-ssl`**

**Ensure Courier is using our dhparams.pem**

## Testing IMAP

Courier-imapd should be started:

`root #``/etc/init.d/courier-imapd start`
Once started, telnet could be used to identify initial problems. Once logging in with telnet works, a mail client can be used:

`user $``telnet foo.example.com 143`
Trying 127.0.0.1...
Connected to foo.example.com.
Escape character is '^\]'.
\* OK \[CAPABILITY IMAP4rev1 UIDPLUS CHILDREN NAMESPACE THREAD=ORDEREDSUBJECT THREAD=REFERENCES SORT IDLE ACL ACL2=UNION STARTTLS\] Courier-IMAP ready. Copyright 1998-2008 Double Precision, Inc.  See COPYING for distribution information.
1 LOGIN testuser secret 
1 OK LOGIN Ok.
1 LOGOUT
\* BYE Courier-IMAP server shutting down
1 OK LOGOUT completed
Connection closed by foreign host.

If testing works properly, add courier-imap to the default runlevel:

`root #``rc-update add courier-imapd default`
## Wrapping things up

Turn off debugging if this stage works properly.

**`/etc/courier/authlib/authdaemonrc`**

**Disable debugging**
