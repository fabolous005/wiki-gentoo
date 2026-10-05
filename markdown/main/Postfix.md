<!-- source: https://wiki.gentoo.org/wiki/Postfix | group: Gentoo Wiki (Main) | wiki-title: Postfix -->
---
title: Postfix
url: https://wiki.gentoo.org/wiki/Postfix
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-16"
categories: ['mail-filter']
fingerprint: fe43c45a380b3a85
license: CC BY-SA 4.0
---

# Postfix

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Postfix** is the de facto standard [Mail Transfer Agent](https://wiki.gentoo.org/wiki/Category:Mail_Transfer_Agents) (MTA).

## Pre-installation

Since only one MTA can be installed at the same time on a system, the package manager will report a block when another MTA is still installed. This block may be resolved by manually removing the previously installed mail server package.

For example, to remove [mail-mta/ssmtp](https://packages.gentoo.org/packages/mail-mta/ssmtp), which might have been installed as the default when a certain program requested a mail server as a dependency), use `--depclean`:

`root #``emerge --ask --depclean mail-mta/ssmtp`
If `--depclean` does not uninstall the package, it may be a required dependency of another package. In that case, it may be required to remove the second package that is depending on the first package in order to uninstall the first package. It is technically possible to force the removal of a package that the package manager considers necessary, though this can be a risk to to the proper functioning of the system; see the [emerge section on removing packages](https://wiki.gentoo.org/wiki/Emerge#Remove_.28uninstall.29_packages) if needed.

## Installation

### USE flags

[mail-mta/postfix](https://packages.gentoo.org/packages/mail-mta/postfix) has several USE flags that may be desired for certain bigger setups. As this article aims at installing and configuring a *basic* Postfix setup, none of them will be required initially.


| [+berkdb](https://packages.gentoo.org/useflags/+berkdb) | Add support for sys-libs/db (Berkeley DB) | 
| [+eai](https://packages.gentoo.org/useflags/+eai) | Add support for SMTPUTF8 | 
| [+lmdb](https://packages.gentoo.org/useflags/+lmdb) | Add support for using dev-db/lmdb for lookup tables | 
| [cdb](https://packages.gentoo.org/useflags/cdb) | Add support for the CDB database engine from the author of qmail | 
| [dovecot-sasl](https://packages.gentoo.org/useflags/dovecot-sasl) | Enable net-mail/dovecot protocol version 1 (server only) SASL implementation | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [ldap-bind](https://packages.gentoo.org/useflags/ldap-bind) | Add support for binding to LDAP backend using dev-libs/cyrus-sasl | 
| [mbox](https://packages.gentoo.org/useflags/mbox) | Add support for mbox (/var/spool/mail) style mail spools | 
| [memcached](https://packages.gentoo.org/useflags/memcached) | Add support for using net-misc/memcached for lookup tables | 
| [mongodb](https://packages.gentoo.org/useflags/mongodb) | Add support for using dev-db/mongodb for lookup tables | 
| [mysql](https://packages.gentoo.org/useflags/mysql) | Add mySQL Database support | 
| [nis](https://packages.gentoo.org/useflags/nis) | Support for NIS/YP services | 
| [pam](https://packages.gentoo.org/useflags/pam) | Add support for PAM (Pluggable Authentication Modules) - DANGEROUS to arbitrarily flip | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Add support for the postgresql database | 
| [sasl](https://packages.gentoo.org/useflags/sasl) | Add support for the Simple Authentication and Security Layer | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [sqlite](https://packages.gentoo.org/useflags/sqlite) | Add support for sqlite - embedded sql database | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [tlsrpt](https://packages.gentoo.org/useflags/tlsrpt) | Add support for TLSRPT protocol | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

Install postfix:

`root #``emerge --ask mail-mta/postfix`
## Configuration

### Fully Qualified Domain Name (FQDN)

Though not entirely related, for a MTA to function properly, it is imperative that its hostname is set up correctly.

Under OpenRC, /etc/hostname and /etc/conf.d/net (if using [Netifrc](https://wiki.gentoo.org/wiki/Netifrc)) are the files responsible for this. In this example, the mail server is named `foo` on the domain `example.com`:

**`/etc/conf.d/net`**

**Setup domain name**

```
dns_domain_lo="example.com"
```
**`/etc/hostname`**

**Setup hostname**

Verifying that the FQDN is setup properly for the domain:

`user $``hostname --fqdn`
foo.example.com

If for any reason the FQDN cannot be set properly, Postfix needs to be told what its FQDN is, otherwise leave it at its commented out default:

**`/etc/postfix/main.cf`**

**Inform Postfix of its FQDN**

To have mail appear as coming from example.com instead of mail.example.com then:

**`/etc/postfix/main.cf`**

**specify your own FQDN**

### Trust and relay

By default, a Postfix installation is pretty tight, only allowing users [on the same subnet](https://www.postfix.org/postconf.5.html#mynetworks_style) as the mail server to relay email through Postfix.

This is critical to get right. If this gets messed around with, it can potentially open the door to all users from anywhere: this is called an *open relay* and is begging for abuse by spam merchants. Such abuse will result in the domain being quickly blacklisted - defeating the purpose of setting up a personal mail server, if nobody will talk or listen to it.

Depending on the trustworthiness of the users connected to the local network, it might be beneficial to restrict access even for machines within the own subnet. This also has the benefit of not letting potentially compromised machines in the local network abuse the mail system, but entails additional configuration.

To only allow the mail server itself to relay email without authentication, make a change to main.cf:

**`/etc/postfix/main.cf`**

**Trust no-one**

SMTP authentication can be used to further secure access to a mail server.

### Address extensions

Address extensions allow encoding additional information besides the recipient in a valid email address. The recipient is separated from the additional information usually by a `+` character, but it is configurable. To enable support for address extensions, edit main.cf:

**`/etc/postfix/main.cf`**

**Set a delimiter to enable address extensions**

The way this works is, when trying to deliver a message to `testuser+spam@example.com`, Postfix will try to deliver the message to `testuser+spam` first, if no such user is found, it will be delivered to `testuser`, the part of the email address before the delimiter.

This feature can be quite useful for sites that require email address registration: signing up with `testuser+somesite@example.com` allows to easily filter and trace where a message originated from. If for example some unsolicited mail was delivered to that address, it likely comes from *somesite*. Even better filtering of unsolicited email can be achieved by using SpamAssassin or amavisd-new.

### Miscellaneous configuration

#### maildir

When Postfix completes merging and the [mbox](https://packages.gentoo.org/useflags/mbox) [USE flag is set, a home\_mailbox directive is added at the bottom of the file. To make future updates easier, it's a good idea to move the](https://wiki.gentoo.org/wiki/USE_flag) `home_mailbox = .maildir/` to its appropriate location in the file.

For testing purposes, the following two features can be temporarily enabled:

#### Soft bounce

Soft bounce decreases the chances of endless bounce loops caused by an invalid email setup. To enable soft bouncing, add this to main.cf:

**`/etc/postfix/main.cf`**

**Soft Bounce**

#### Verbose SMTP

Before testing the basic mail server setup, the verbose flag of the smtp server can be enabled by adding a `-v` to the smtpd daemon invocation in master.cf:

**`/etc/postfix/master.cf`**

**Verbose smtpd**

That rounds up basic configuration. Postfix offers a vast amount of other features, including virtual domains and users, that exceed the current scope of this article.

### Storage for address rewriting / routing tables

Postfix supports using various databases to store address-related rewrite rules and routing information. The defaults are completely fine for simple setups.

To use a database (SQLite, MariaDB, etc.), consult the [Lookup Table documentation](https://www.postfix.org/DATABASE_README.html).

## Starting Postfix

Before starting Postfix for the first time, the local alias database has to be compiled. If this is not done, Postfix may appear to have started normally, but won't work and the log (usually found in /var/log/mail.log) will be peppered with errors:

Mar 16 11:40:32 foo postfix/smtpd\[18923\]: fatal: open database /etc/mail/aliases.db: No such file or directory

The `alias` database contains default local accounts required by various RFCs and common internet practice, as well as some pseudo accounts. Simply run the newaliases command to generate the database:

`root #``newaliases`
Now it is finally time to start Postfix:

`root #``/etc/init.d/postfix start`
It can be very useful to monitor the mail log file using tail -f. By default, Postfix logs to the syslog but this can be filtered, which will be covered later in this chapter. A successful start looks like this:

Nov 23 15:26:42 foo postfix/postfix-script\[13433\]: starting the Postfix mail system
Nov 23 15:26:42 foo postfix/master\[13434\]: daemon started -- version 2.8.4, configuration /etc/postfix

## Testing Postfix

Postfix should be running properly now and accept connections on port 25, the default SMTP port, and send email anywhere in the world.

If available, the *sendmail* compatible program (installed by Postfix) can be used to send a test e-mail:

`user $``echo "test" ¦ sendmail <(username)@(validdomain).(tld)>`
If not available, or a test from a remote machine (which is allowed to relay e-mail) is used, the following can be used also.

As SMTP is a simple plain-text protocol, email may be sent manually using a telnet client. Replace the example address `<(username)@(validdomain).(tld)>` with a real email address to see it work. This is an example SMTP conversation with the parts the client (in this case that is the user) sends are highlighted in yellow:

`user $``telnet foo.example.com 25````
Trying foo.example.com...
Escape character is '^]'.
220 foo.example.com ESMTP Postfix
HELO localhost
250 foo.example.com
MAIL FROM: <me@somewhere.com>
250 2.1.0 Ok
RCPT TO: <(username)@(validdomain).(tld)>
250 2.1.5 Ok
DATA
354 End data with <CR><LF>.<CR><LF>
Subject: Test email
Testmail to ensure Postfix is working.
.
250 2.0.0 Ok: queued as 6705C20E32
QUIT
221 2.0.0 Bye
```
Connected to foo.example.com.

Connection closed by foreign host.
Looking at the mail log, it can be verified that the message got properly relayed:

Nov 23 16:13:02 foo postfix/smtpd\[28494\]: connect from foo.example.com\[127.0.0.1\]
Nov 23 16:13:49 foo postfix/smtpd\[28494\]: 6705C20E32: client=foo.example.com\[127.0.0.1\]
Nov 23 16:13:51 foo postfix/cleanup\[28508\]: 6705C20E32: message-id=\<20111123151349.6705C20E32@foo.example.com>
Nov 23 16:13:51 foo postfix/qmgr\[28490\]: 6705C20E32: from=\<me@somewhere.com>, size=314, nrcpt=1 (queue active)
Nov 23 16:13:52 foo postfix/smtp\[28510\]: 6705C20E32: to=\<(username)@(validdomain).(tld)>, relay=mail.(validdomain)(.tld)\[5.6.7.8\]:25, ⏎
delay=19, delays=18/0.02/0.37/0.32, dsn=2.0.0, status=sent (250 2.0.0 Ok: queued as 469A684F8)
Nov 23 16:13:52 foo postfix/qmgr\[28490\]: 6705C20E32: removed
Nov 23 16:13:54 foo postfix/smtpd\[28494\]: disconnect from foo.example.com\[127.0.0.1\]

Performing the same test from a different host should fail, as it is untrusted:

`user $``telnet foo.example.com 25````
Trying foo.example.com...
Escape character is '^]'.
220 foo.example.com ESMTP Postfix
HELO localhost
250 foo.example.com
MAIL FROM: <me@somewhere.com>
250 2.1.0 Ok
RCPT TO: <(username)@(validdomain).(tld)>
454 4.7.1 <(username)@(validdomain).(tld)>: Relay access denied
QUIT
221 2.0.0 Bye
```
Connected to foo.example.com.

Connection closed by foreign host.
## Logging

By default, some sysloggers will include all mail-related logs in the same file as other, non-mail messages. Filtering mail messages to their own files can be very useful on a busy mail server. For [syslog-ng](https://wiki.gentoo.org/wiki/Syslog-ng), create file to define the filtering:

**`/etc/syslog-ng/mail.conf`**

**Mail log filter**

And then include it in the main syslog-ng configuration file:

**`/etc/syslog-ng/syslog-ng.conf`**

**Syslog-ng config**

Remember to reload syslog-ng to activate the filter.

## Safety net

By default, Postfix rejects mails to unknown mailboxes. While testing, it may be desirable to not have mail rejected just yet.

As a safety feature, it is possible to ask the sender to try again later using error code 450, from the default reject (550):

**`/etc/postfix/main.cf`**

**Postfix main**

## Related Articles

There are two full guides one may wish to consult:

- [Complete Virtual Mail Server](https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server)
- [Mailfiltering Gateway](https://wiki.gentoo.org/wiki/Mailfiltering_Gateway) for setting up a Postfix server with ClamAV, amavisd-new, Vipul's Razor, DCC, and SpamAssassin.

Alternatively, selectively add features from the articles below. The non-optional ones are highly recommended, and features are listed in roughly suggested order of implementation:

- **Inbound** (receiving mail to the server)
  - [rspamd](https://github.com/vstakhov/rspamd) ([github](https://github.com/vstakhov/rspamd))
  - [Blacklisting](https://wiki.gentoo.org/wiki/Postfix/Blacklisting), or banning the work of spammers already identified by third parties
  - Checksum-based systems
    - [Distributed Checksum Clearinghouse](https://wiki.gentoo.org/wiki/Postfix/DCC), a global bulk mail detection system
  - [Greylisting](https://wiki.gentoo.org/wiki/Postfix/Greylisting), or delaying accepting mail from new senders (long enough for blacklists to kick in)
  - [Miscellaneous anti-spam measures](https://wiki.gentoo.org/wiki/Postfix/Miscellaneous_anti-spam_measures)
  - Optional
    - [policyd-weight](https://wiki.gentoo.org/wiki/Postfix/policyd-weight), a weighted rejection policy daemon
    - Investigate the various packages in the [mail-filter](https://packages.gentoo.org/categories/mail-filter) category
- **Outbound** (sending mail to others from the server, and actually having it received)
  - [Sender Policy Framework (SPF)](https://wiki.gentoo.org/wiki/Postfix/SPF), to set IP address ranges allowed to send email from a domain
  - [DomainKeys Identified Mail (DKIM)](https://wiki.gentoo.org/wiki/Postfix/DKIM), to set cryptographic protection for outbound mail proving it is from a domain
  - [Domain-based Message Authentication, Reporting and Conformance (DMARC)](https://wiki.gentoo.org/wiki/Postfix/DMARC), to get feedback on issues third parties have processing mail sent by the server, and to inform them how such issues should be handled
- Optional
  - **Relaying** (sending mail on behalf of connecting clients to third-party servers)
    - **Simple Authentication and Security Layer (SASL)**, for identifying clients authorized to supply mail for relay
