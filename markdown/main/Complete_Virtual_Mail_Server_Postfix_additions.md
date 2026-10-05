<!-- source: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Postfix_additions | group: Gentoo Wiki (Main) | wiki-title: Complete Virtual Mail Server/Postfix additions -->
---
title: Complete Virtual Mail Server/Postfix additions
url: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/Postfix_additions
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-06"
fingerprint: fa1b6a3aa82b26ba
license: CC BY-SA 4.0
---

# Complete Virtual Mail Server/Postfix additions

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

At this point, Postfix should be running well and users through mailclients and webmail should have no problem sending and receiving e-mails securely. Tweaking the postfix installation can give a little more performance, make it a bit more secure and having a bit more redundancy never hurts either.

## Submission

### Introduction to submission

Mail submission is often incorrectly done on port 25 of the [Mail Transfer Agent (MTA)](https://en.wikipedia.org/wiki/Mail_transfer_agent). Mail should actually be submitted on port 587 to the [Mail Submission Agent (MSA)](https://en.wikipedia.org/wiki/Mail_submission_agent). Postfix is both an MTA and MSA. There are numerous advantages to having mail delivered to the MSA by the client as can be seen on Wikipedia. The most important however, is that nomadic users can send e-mail even when port 25 is being blocked by a firewall, as port 587 tends to be more open. Also when only letting authenticated users send e-mail, they could even bypass the spam filter for their outgoing messages.

### Configuration

The submission port is enabled in Postfix's master.cf. It is commented by default including its options:

**`/etc/postfix/master.cf`**

**Mail submission by postfix**

After restarting Postfix, this lets the client know that there is STARTTLS availability, but it is not required (set this to `smtpd_tls_security_level=encrypt` to enforce encryption) and rejects all mail, except for authenticated users:

`root #``/etc/init.d/postfix restart`
### Testing

Testing should be done by a mail client, and using port 587 for the smtp server port.

## Redundancy

### Introduction to backup-mx

For redundancy, postfix offers the feature of *backup mx*. Backup mx allows another mailhost to catch e-mail for a domain when the main mailserver is for whatever reason not responding. A backup mx server will try to deliver the mail to the original mailserver. This all sounds just perfect, however backup mx is dead. Spam killed it. The problem with backup mx nowadays is, due to spam, mail can end up bouncing the wrong way and the backup mx server may thus end up on a spam block list. This kind of mail is called [Backscatter mail](<https://en.wikipedia.org/wiki/Backscatter_(email)>). If a secondary mailserver is setup, it needs to know all of the users from the primary domain so that it can bounce *unknown users*. A simple way to do this, is having the entire database synchronized between primary and secondary mailserver. Caveat being however the `backupmx` flag in the domain table. This would need to be inverted on the secondary mailserver.

### Configuring backup-mx

The database already knows about backup domains. If a domain *has* a backup mail server, the `backupmx` field is set to "1" in the domain table. Connecting postfix to the database requires a file with connection information:

**`/etc/postfix/pgsql/relay_domains.cf`**

**Setup relay domains**

This then needs to be configured into postfix:

**`/etc/postfix/main.cf`**

**Modify postfix for relay domains**

To make proper use of `relay_domains`:

**`/etc/postfix/pgsql/relay_recipient_maps.cf`**

**Setup relay recipients**

Also this needs to be configured into postfix:

**`/etc/postfix/main.cf`**

**Modify postfix for relay recipients**

After restarting postfix, the mail server is now a less-open relay, relaying mail to only approved domains and approved users.

## Quotas

### Introduction

Quotas are a tool to help a user monitor their mailbox. They are very easily abused and should not be relied on. That said, they can work well enough most of the time and give the user feedback on the status of his usage pattern. IMAP supports quota reporting and thus the mail-client can even report this the user. Thunderbird does this via an [extension](http://addons.mozilla.org/en-US/thunderbird/addon/881), roundcube shows this per default.

### Configuration

The database supports mail quotas however postfix requires a patch to support these mail quotas. The `vda` USE flag enables this patch for postfix and allows the use of quotas.

First, the data needs to be obtained from the database:

**`/etc/postfix/pgsql/virtual_mailbox_limit_maps.cf`**

**Query the database for quotas**

There are a few things that need to be configured in postfix, next to the database query. The *Trash* folder for example should not be counted when automatically deleting Trash after a fixed amount of time. This can (and has to) be done in courier-imap.

**`/etc/postfix/main.cf`**

**Setup postfix to use quotas**

With VDA quota's in place, it's recommended to disable the postfix internal mailbox size limit.

**`/etc/postfix/main.cf`**

**Change allowed message size limit**

### Testing

Roundcube displays diskusage per default and hovering over it displays detailed information. Thunderbird has an extension, [Display quota](http://addons.mozilla.org/en-US/thunderbird/addon/881). For both and others to actually work a maildirsize file is required. This file will be created and updated whenever postfix delivers a message or when courier-imap makes changes. The file is located in each virtual users root mail dir, which would be /var/vmail/example.com/testuser/maildirsize in the case of testuser on example.com. Thus sending a message to testuser@example.com would create this file in the users maildir.

`user $``sendmail testuser@example.com`
Subject: Testmail to create quota file.
No content!
.

## HELO Restrictions

Hackers, spammers and everybody else can obtain information from the mail server by using the HELO and EHLO commands. Spammers usually put fake information in the HELO greeting and thus restricting and rejecting connections from servers that do not properly identify themselves can only be good. Postfix offers the `smtpd_helo_restrictions` variable to tune how to respond to connections. Restrictions here have to be thought over carefully however. Locking down the server too tight may make it so messages are rejected because the other server is just poorly configured. A quick overview of the available options.

| `smtpd_helo_restrictions` | Description | 
|---|---|
| `reject_invalid_hostname` | REJECT if HELO greeting is not a DN or an IP | 
| `reject_non_fqdn_hostname` | REJECT if HELO greeting is not a FQDN or an IP | 
| `reject_unknown_hostname` | REJECT if HELO host has no A or MX in DNS | 
| `permit_naked_ip_address` | PERMIT if HELO greeting is an IP | 

The following table should give an idea how mail will get rejected from the various restrictions. An **X** means the message will be rejected, an **?** means it depends on the proper DNS record setup for that domain and **O** means the message will be delivered normally.

|  | bogus | example | example.com | 192.0.23.58 | 
|---|---|---|---|---|
| `reject_invalid_hostname` | X | O | O | O | 
| `reject_non_fqdn_hostname` | X | X | O | O | 
| `reject_unknown_hostname` | X | X | ? | X | 

On a properly configured network the following will be tight and should work:

**`/etc/postfix/main.cf`**

**HELO Restrictions**

## Deny local username farming

Normally postfix, or general MTAs allow to verify whether a mailbox exists or not. This command may have been useful in the early days of mail, but is almost exclusively used by people who maintain bulk mailing lists and search if accounts are still valid. This command can be disabled by postfix:

**`/etc/postfix/main.cf`**

**Disable verify**

After a restart of postfix, telnet to port 25 will no longer show **250-VRFY**.

## SMTPD Banner

Another often abused feature is the SMTP header. Also some countries require senders to honor the "NO UCE" greeting message (No Unsolicited Commercial E-mail). Also it is wise not to tell any outsiders what MTA is being used or what version thereof. Postfix allows for changing the SMTP header quite easily. Find the `smtpd_banner` section in main.cf:

**`/etc/postfix/main.cf`**

**Change the SMTPD Banner**

## Message size

For years the default message size amongst MTA's has been 10MiB. Google raised the bar with their gmail service to 20MiB per message. If bandwidth isn't an issue this, can easily be accomplished with postfix:

**`/etc/postfix/main.cf`**

**Change allowed message size limit**

## Postfix Performance

### Biff

For compatibility reasons, postfix's local mail notification is enabled by default. With many users this can be a performance drain and since there are no local users to use the biff command anyway this can safely be disabled:

**`/etc/postfix/main.cf`**

**Disable biff**

### Processes

The amount of concurrent processes of any of postfix's applications is limited to 50. The first bump in high load environments can be very quickly be the amount of active daemons. Smaller setups should not need to worry about this setting:

**`/etc/postfix/main.cf`**

**Increase postfix processes**

### DNS Lookup

A common throughput limiter is the use of DNS lookups. These can be slow and can be an issue far before processes, CPU or memory are the issue. One lookup per message is required at the very least per message, a server MX record needs to be found. A local caching DNS server could help enormously here and packages such as [net-dns/dnsmasq](https://packages.gentoo.org/packages/net-dns/dnsmasq) or even a full [net-dns/bind](https://packages.gentoo.org/packages/net-dns/bind) setup should be used.

### Mail Storage

Postfix maintains a number of queue directories in /var/spool/postfix. Various postfix applications pass messages around each other using these queues. Placing these directories on a separate disk, raid array or an SSD can significantly improve overall throughput. Also making sure that the partition in question is marked with the `noatime` option helps a lot on disk access. Postfix does not use access timestamps.

### Multiple mail servers

If one server is not enough, multiple mail-servers can spread the load. Also having multiple mail-servers offers redundancy so it is something worth considering. spreading the load over several mail-servers can be done effectively using DNS round-robin. Two options are available here, either assign multiple A entries to the mail-server, or cleaner, have multiple MX entries which allow prioritization.
