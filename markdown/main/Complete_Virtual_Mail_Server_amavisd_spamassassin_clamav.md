<!-- source: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/amavisd_spamassassin_clamav | group: Gentoo Wiki (Main) | wiki-title: Complete Virtual Mail Server/amavisd spamassassin clamav -->
---
title: Complete Virtual Mail Server/amavisd spamassassin clamav
url: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/amavisd_spamassassin_clamav
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-16"
fingerprint: ae4390f63d27a4b2
license: CC BY-SA 4.0
---

# Complete Virtual Mail Server/amavisd spamassassin clamav

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Spam is becoming more and more of an issue on the Internet and a robust and solid solution is required. There are paid services for even more spam protection but that should not be required and will not be discussed in this article.

## Postfix

The first line of defense, is Postfix itself. Postfix offers a few basic means to block spam, or rather spammers. Using `smtpd_client_restrictions` it is possible to use public DNS blacklists. There are 3 popular DNS blacklists of which one is incorporated into the other. These two lists are also the [most accurate](http://www.intra2net.com/en/support/antispam/) ones. The most important one is [zen.spamhaus.org](http://zen.spamhaus.org) and as a backup [bl.spamcop.net](http://bl.spamcop.net) can be used. Using them with Postfix is very simple. Add these domains as `reject_rbl_client` in main.cf:

**`/etc/postfix/main.cf`**

**Use DNS blacklists**

```
# Block spam using DNS blacklists
smtpd_client_restrictions = permit_mynetworks, permit_sasl_authenticated, reject_rbl_client zen.spamhaus.org, reject_rbl_client bl.spamcop.net
```
## Amavis

### Introduction

SpamAssassin and ClamAV are the tools to block spam and viruses, however amavis is required to tie this all together. Amavis will actually behave as a mail server in itself, accept mail, filter it, and send it onwards again. For this to work, Postfix will need to actually listen for mail twice. The default port 25 is where mail initially is received on. From there on it is sent to amavis, which will be listening on port 10024. When amavis is done with the message, it will be sent to Postfix on a different port, 10025. The reason for this should be obvious. If mail would be offered again on port 25, it would be passed to amavis again and thus in an endless loop. Obviously, Postfix on port 10025 would only be listening to known hosts, like *localhost* and not check for spam anymore.

### Installation

Amavis should have been installed already, if not, emerge it. This should pull in SpamAssassin and clamav as its dependencies:

`root #``emerge --ask mail-filter/amavisd-new`
### Basic Configuration

amavisd offers an enormous amount of options and going over all them will take some time. However, the configuration file (/etc/amavisd.conf) is well documented and divided into clear sections. Each section will be examined as needed. Only options that will be changed will be mentioned to cut down the text for readability.

For this example, amavisd will be running on host foo, but this could be any other host as well. amavisd does not need to run on the same host as Postfix. Also, the domain used is only used to identify the server itself, not the domains amavisd will be scanning.

The first step is to disable all actual checks and to enable logging. Also, some default values should be setup:

**`/etc/amavisd.conf`**

**Disable anti-spam, enable logging**

```
@bypass_virus_checks_maps = (1);  # controls running of anti-virus code
@bypass_spam_checks_maps  = (1);  # controls running of anti-spam code
# $bypass_decode_parts = 1;         # controls running of decoders&dearchivers
 
$mydomain = 'example.com';
$myhostname = 'foo.example.com';
$MYHOME = '/var/lib/amavishome';   # a convenient default for other settings, -H
$log_level = 5;              # verbosity 0..5, -d
```
`$MYHOME` is used as the prefix for several locations amavisd needs to work within, so setting it to /var/lib/amavishome should be safe. That path should have been created as the **amavis** user's home directory by [acct-user/amavis](https://packages.gentoo.org/packages/acct-user/amavis), which is pulled in by [mail-filter/amavisd-new](https://packages.gentoo.org/packages/mail-filter/amavisd-new). The location does not matter as long as the **amavis** has write permissions to it. Otherwise, the daemon will crash on start.

Normally, restarting Postfix should restart amavisd as well. For now, only amavisd should be started to see if there are any initial problems:

`root #``/etc/init.d/amavisd start`
### Linking amavisd to Postfix

With amavisd working in bare skeletal mode, it should theoretically just pass mail through. Perfect for testing the postfix -> amavisd -> postfix binding.

First, a second Postfix transport, where amavis will inject its mail, is added. A lot of options are defaulted to empty, since either they have been checked already, or interfere otherwise:

**`/etc/postfix/master.cf`**

**Filter mail through amavisd**

```
localhost:10025 inet n  -       n       -       2       smtpd
  -o smtp_dns_support_level=enabled
  -o content_filter=
  -o myhostname=foo.example.com
  -o local_recipient_maps=
  -o relay_recipient_maps=
  -o smtpd_restriction_classes=
  -o smtpd_client_restrictions=
  -o smtpd_helo_restrictions=
  -o smtpd_sender_restrictions=
  -o smtpd_recipient_restrictions=permit_mynetworks,reject
  -o mynetworks=127.0.0.0/8
  -o strict_rfc821_envelopes=yes
  -o smtpd_error_sleep_time=0
  -o smtpd_soft_error_limit=1001
  -o smtpd_hard_error_limit=1000
  -o smtpd_client_connection_count_limit=0
  -o smtpd_client_connection_rate_limit=0
  -o receive_override_options=no_unknown_recipient_checks,no_header_body_checks
  -o smtpd_authorized_xforward_hosts=127.0.0.0/8
```
With this transport in place, it should only listen on localhost and only accept mail from localhost. This should be extended if amavis is run elsewhere, but keep in mind anything is accepted.

Next another transport is added for, which could be considers 'being' amavis in a sense:

**`/etc/postfix/master.cf`**

**amavis transport**

```
amavis    unix  -       -       n       -       2       lmtp
  -o disable_dns_lookups=yes
  -o lmtp_send_xforward_command=yes
  -o lmtp_data_done_timeout=1200
```
After the transport for amavis has been added, smtp should be told to route all mail through amavis. For this two option need to be added to smtpd and change the maxproc to match amavis's:

**`/etc/postfix/master.cf`**

**relay to amavis**

```
smtp       inet  n       -       n       -       2       smtpd
  -o content_filter=amavis:[127.0.0.1]:10024
  -o receive_override_options=no_address_mappings
smtps     inet  n       -       n       -       2       smtpd
  -o smtpd_tls_wrappermode=yes
  -o content_filter=amavis:[127.0.0.1]:10024
  -o receive_override_options=no_address_mappings
```
Restarting both amavisd and postfix then should pass all mail through amavisd:

`root #``/etc/init.d/amavisd restart``root #``/etc/init.d/postfix restart`
### Testing

Sending a message to *testuser@example.com* remotely and locally should work fine. After the message has arrived, the headers should be checked.

Looking at the mail headers, it should be noticed that it was sent through amavisd and re-delivered to Postfix:

Examining the above it is clearly visible that the mail was received by Postfix via SMTPS even. It was then forwarded to amavisd on port 10024 via LMTP and finally redelivered to Postfix using SMTP again.

## ClamAV

### Introduction

ClamAV is the de facto open source virus scanner for linux. Amavis can be linked to many different free and commercial virus scanners, but here clamav will be used. ClamAV is specifically designed for scanning e-mail. It consists of two parts, clamav itself, and freshclam, the clamav updating service. By default it updates every two hours, which should be enough for anyone.

### Installation

ClamAV should have been installed already, if not it should be emerged:

`root #``emerge --ask app-antivirus/clamav`
### Configuration

ClamAV will be configured to run in daemonized mode, e.g. it will be listening for connections (from amavisd). The other option (and the default fallback in amavisd) is to have amavisd use the commandline scanner, which is much slower and much much more resource intensive.

ClamAV does not have to be run on the same host, however it is recommended for performance reasons to keep it on the same host, depending on resource usage.

To be able to communicate, clamav needs to be part of amavisd's group:

`root #``gpasswd -a clamav amavis`
It always helps to allow clamd to output some debug information:

**`/etc/clamav/clamd.conf`**

**Enable Debug options**

```
# Enable debug messages in libclamav.
# Default: no
Debug yes
```
Also clamd needs some settings setup in its configuration file so that amavis can talk to it:

**`/etc/clamav/clamd.conf`**

**Allow amavis to conenct to clamd**

```
# Path to a local socket file the daemon will listen on.
# Default: disabled (must be specified by a user)
LocalSocket /var/run/clamav/clamd.sock
 
# Sets the group ownership on the unix socket.
# Default: disabled (the primary group of the user running clamd)
LocalSocketGroup amavis
```
When running clamav on a hardened kernel, there will be warnings about certain operations not being permitted:

This is expected and okay. ClamAV can run fine without JIT.

Before starting clamav for the first time, the virus database needs to be downloaded. Freshclam is responsible for downloading and keeping the virus database up to date. Freshclam gets automatically started by the clamd startup script, but clamd will fail to start due to a missing database:

`root #``/etc/init.d/clamd start`
Now monitor the clamav log file to see freshclam download the initial virus database:

**`/var/log/clamav/freshclam.log`**

**Freshclam startup log**

```
freshclam daemon 0.98.6 (OS: linux-gnu, ARCH: x86_64, CPU: x86_64)
ClamAV update process started at Sat Mar 21 16:30:16 2015
Downloading main.cvd [100%]
main.cvd updated (version: 55, sigs: 2424225, f-level: 60, builder: neo)
Downloading daily.cvd [100%]
daily.cvd updated (version: 20219, sigs: 1354642, f-level: 63, builder: neo)
Downloading bytecode.cvd [100%]
[LibClamAV] Bytecode: disabling JIT because SELinux is preventing 'execmem' access.
Run  'setsebool -P clamd_use_jit on'.
ERROR: During database load : LibClamAV Warning: RWX mapping denied: Can't allocate RWX Memory: Operation not permitted
WARNING: Database successfully loaded, but there is stderr output
bytecode.cvd updated (version: 247, sigs: 41, f-level: 63, builder: dgoddard)
Database updated (3778908 signatures) from database.clamav.net (IP: 200.236.31.1)
WARNING: Clamd was NOT notified: Can't connect to clamd through /var/run/clamav/clamd.sock: No such file or directory
```
Now that the database has been updated, restart clamd:

`root #``/etc/init.d/clamd restart`
**`/var/log/clamav/clamd.log`**

**ClamAV startup log**

```
Sat Mar 21 16:35:18 2015 -> clamd daemon 0.98.6 (OS: linux-gnu, ARCH: x86_64, CPU: x86_64)
Sat Mar 21 16:35:18 2015 -> Running as user clamav (UID 105, GID 206)
Sat Mar 21 16:35:18 2015 -> Log file size limited to 1048576 bytes.
Sat Mar 21 16:35:18 2015 -> Reading databases from /var/lib/clamav
Sat Mar 21 16:35:18 2015 -> Not loading PUA signatures.
Sat Mar 21 16:35:18 2015 -> Bytecode: Security mode set to "TrustSigned".
Sat Mar 21 16:35:29 2015 -> Loaded 3773304 signatures.
Sat Mar 21 16:35:30 2015 -> LOCAL: Unix socket file /var/run/clamav/clamd.sock
Sat Mar 21 16:35:30 2015 -> LOCAL: Setting connection queue length to 200
Sat Mar 21 16:35:30 2015 -> Limits: Global size limit set to 104857600 bytes.
Sat Mar 21 16:35:30 2015 -> Limits: File size limit set to 26214400 bytes.
Sat Mar 21 16:35:30 2015 -> Limits: Recursion level limit set to 16.
Sat Mar 21 16:35:30 2015 -> Limits: Files limit set to 10000.
Sat Mar 21 16:35:30 2015 -> Limits: MaxEmbeddedPE limit set to 10485760 bytes.
Sat Mar 21 16:35:30 2015 -> Limits: MaxHTMLNormalize limit set to 10485760 bytes.
Sat Mar 21 16:35:30 2015 -> Limits: MaxHTMLNoTags limit set to 2097152 bytes.
Sat Mar 21 16:35:30 2015 -> Limits: MaxScriptNormalize limit set to 5242880 bytes.
Sat Mar 21 16:35:30 2015 -> Limits: MaxZipTypeRcg limit set to 1048576 bytes.
Sat Mar 21 16:35:30 2015 -> Limits: MaxPartitions limit set to 50.
Sat Mar 21 16:35:30 2015 -> Limits: MaxIconsPE limit set to 100.
Sat Mar 21 16:35:30 2015 -> Archive support enabled.
Sat Mar 21 16:35:30 2015 -> Algorithmic detection enabled.
Sat Mar 21 16:35:30 2015 -> Portable Executable support enabled.
Sat Mar 21 16:35:30 2015 -> ELF support enabled.
Sat Mar 21 16:35:30 2015 -> Mail files support enabled.
Sat Mar 21 16:35:30 2015 -> OLE2 support enabled.
Sat Mar 21 16:35:30 2015 -> PDF support enabled.
Sat Mar 21 16:35:30 2015 -> SWF support enabled.
Sat Mar 21 16:35:30 2015 -> HTML support enabled.
Sat Mar 21 16:35:30 2015 -> Self checking every 600 seconds.
```
### Linking amavisd to clamav

Amavisd should connect to the socket of clamd and thus clamav needs to be enabled as one of the main antivirus scanners. The fallback of invoking clamav from the commandline should not be changed. Also the virus check bypass needs to be disabled to be effective:

**`/etc/amavisd.conf`**

**Enable clamav**

```
# @bypass_virus_checks_maps = (1);  # controls running of anti-virus code
 
['ClamAV-clamd',
  \&ask_daemon, ["CONTSCAN {}\n", "/var/run/clamav/clamd.sock"],
  qr/\bOK$/m, qr/\bFOUND$/m,
  qr/^.*?: (?!Infected Archive)(.*) FOUND$/m ],
```
After restarting amavisd, viruses should be able to detected and blocked:

`root #``/etc/init.d/amavisd restart`
### Testing

To test whether the virus filter works, an [anti-malware testfile](http://www.eicar.org/86-0-Intended-use.html) exists; sending an e-mail using this string should trigger the virus scanner:

Looking at the mail.log file, the following should be revealed:

**`/var/log/mail.log`**

**Virus infection test**

```
Dec 28 14:05:33 7of9 amavis[7554]: (07554-01) Blocked INFECTED (Eicar-Test-Signature) {DiscardedInternal,Quarantined}, MYNETS LOCAL [10.0.0.2]:41144 [10.0.0.2] <root@test.example.net> -> <testuser@example.com>, quarantine: virus-WYHuLpwdzPVr, Queue-ID: 7FC7B22DF6, mail_id: WYHuLpwdzPVr, Hits: -, size: 407, 166 ms
Dec 28 14:05:33 7of9 postfix/lmtp[15452]: 7FC7B22DF6: to=<testuser@example.com>, relay=127.0.0.1[127.0.0.1]:10024, delay=0.41, delays=0.21/0.02/0.01/0.16, dsn=2.7.0, status=sent (250 2.7.0 Ok, discarded, id=07554-01 - INFECTED: Eicar-Test-Signature)
```
In theory, the virus scanner should be fully functional now.

## SpamAssassin

### Introduction

SpamAssassin is an excellent spam filter. It has become quite complex throughout the years and requires some effort to configure correctly.

### Installation

There really should be no need to install SpamAssassin separately. The `spamassassin` USE flag should have pulled it in as a dependency of amavisd-new.

`root #``emerge --ask mail-filter/spamassassin`
### Configuration

The default configuration suffices for standard use.

### Updating

A key feature of SpamAssassin is its ability to self-update. Updates are handled via a so-called *[update channel](http://updates.spamassasin.org)*.

SpamAssassin comes with the sa-update tool so updates can be fully automated. Adding the SpamAssassin GPG key is a simple 2 step process.

`root #``wget "`[http://spamassassin.apache.org/updates/GPG.KEY](http://spamassassin.apache.org/updates/GPG.KEY)"`root #``sa-update --import GPG.KEY``root #``rm GPG.KEY`
After adding the spamassassin update channel, it needs to be updated. After running this command, check for any errors:

`root #``sa-update -D`
Once these updates have completed, they need to be compiled for use with SpamAssassin. Also, any errors should be spotted here:

`root #``sa-compile`
The [cron](https://packages.gentoo.org/useflags/cron) [USE flag on](https://wiki.gentoo.org/wiki/USE_flag) [mail-filter/spamassassin](https://packages.gentoo.org/packages/mail-filter/spamassassin) arranges for a cronjob at /etc/cron.daily/update-spamassassin-rules to update and recompile the filters daily.

### Linking amavisd to SpamAssassin

Actually, SpamAssassin does not need to be linked to amavisd, it is an integral part of amavisd. Enabling the spamfilter in amavisd does spam filtering:

**`/etc/amavisd.conf`**

**Enable SpamAssassin**

```
# @bypass_spam_checks_maps  = (1);  # controls running of anti-spam code
```
With this change, amavisd needs to be restarted:

`root #``/etc/init.d/amavisd restart`
### Testing

Testing is done again via a client connecting to port 25 that does not have its own spamfilter. For content [GTUBE](http://spamassassin.apache.org/gtube/) can be used. On that site there is also a suitable mail message in RFC-822 format.

Checking in the testusers inbox, or junkbox more likly, the message can be found and its header examined:

## Finetuning Amavisd

Amavis has a few more settings that can be changed to do some fine-tuning.

### Recipient delimiter

When using Postfix with the `recipient_delimiter`, amavisd can be told to make use of this feature:

**`/etc/amavisd.conf`**

**recipient\_delimiters for amavis**

```
# Delimiter must match the equivalent (final) MTA delimiter setting.
$recipient_delimiter = '+';		# (default is undef, i.e. disabled)
```
It might be interesting to add the following to /etc/postfix/main.cf, otherwise the user+foo@domain might not be delivered:

**`/etc/postfix/main.cf`**

**recipient\_delimiters for postfix**

```
# ADDRESS EXTENSIONS (e.g., user+foo)
#
# The recipient_delimiter parameter specifies the separator between
# user names and address extensions (user+foo). See canonical(5),
# local(8), relocated(5) and virtual(5) for the effects this has on
# aliases, canonical, virtual, relocated and .forward file lookups.
# Basically, the software tries user+foo and .forward+foo before
# trying user and .forward.
#
recipient_delimiter = +
```
### Disperse quarantine

It is possible to disperse the quarantine over several sub-directories. For this directories need to be created first:

`root #````
mkdir /var/amavis/quarantine/virus
```
`root #````
mkdir /var/amavis/quarantine/spam
```
`root #````
mkdir /var/amavis/quarantine/banned
```
`root #````
mkdir /var/amavis/quarantine/badh
```
`root #````
mkdir /var/amavis/quarantine/clean
```
`root #````
chown -R amavis:amavis /var/amavis/quarantine/*
```
`root #````
chmod -R gu+rwX /var/amavis/quarantine/*
```
`root #````
chmod -R o-rwx /var/amavis/quarantine/*
```
**`/etc/amavisd.conf`**

**Disperse quarantine**

```
#$clean_quarantine_method          = 'local:clean/%m';
$virus_quarantine_method          = 'local:virus/%m';
$spam_quarantine_method           = 'local:spam/%m.gz';
$banned_files_quarantine_method   = 'local:banned/%m';
$bad_header_quarantine_method     = 'local:badh/%m';
```
Also, setting a spam cutoff level helps in reducing stored spam. The cutoff level makes it that no spam is stored above a certain spam-score:

**`/etc/amavisd.conf`**

**set a cutoff level for spam**

```
$sa_quarantine_cutoff_level = 25; # spam level beyond which quarantine is off
```
### Spam Delivery

`$final_spam_destiny` is by default set to `D_PASS`, meaning that even with a high score at which it gets marked as spam, it is still delivered to the users mailbox. Modern mail-clients, which trust SpamAssassin, can then automatically move it to their SPAM folder.

### Bayes database path

Set the `bayes_path` option in SpamAssassin's configuration file so tools such as sa-learn write to the correct database location:

**`/etc/spamassassin/local.cf`**

**set bayes\_path to be within Amavisd's home directory**

```
bayes_path /var/amavis/.spamassassin/bayes
```
### Virtual hosts

If this server handles more than one domain, telling amavis can help here:

**`/etc/amavisd.conf`**

**Add additional aliases to amavis**

```
@local_domains_maps = ( [".$mydomain", "mail.example.net", "mail.example.org", "mail2.example.com"] );
```
## Cleanup

With SpamAssassin and ClamAV working as expected, debugging information can be reduced to the normal minimal:

**`/etc/amavisd.conf`**

**Disable debugging in amavsd**

```
# Section III - Logging
# true (e.g. 1) => syslog;  false (e.g. 0) => logging to file
$do_syslog = 1;                   # (defaults to 0)
 
#NOTE: levels are not strictly observed and are somewhat arbitrary
$log_level = 0;		   # (defaults to 0), -d
 
# Turn on SpamAssassin debugging (output to STDERR, use with 'amavisd debug')
#$sa_debug = '1,all';  # defaults to false
```
**`/etc/clamd.conf`**

**Disable debugging in clamav**

```
# Enable debug messages in libclamav.
# Default: no
#Debug yes
```
