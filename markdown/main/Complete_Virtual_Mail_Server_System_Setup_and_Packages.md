<!-- source: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/System_Setup_and_Packages | group: Gentoo Wiki (Main) | wiki-title: Complete Virtual Mail Server/System Setup and Packages -->
---
title: Complete Virtual Mail Server/System Setup and Packages
url: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/System_Setup_and_Packages
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-16"
fingerprint: db332356e4033d45
license: CC BY-SA 4.0
---

# Complete Virtual Mail Server/System Setup and Packages

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## System deployment

### Web Server

First, a web server needs to be chosen. This web server will handle the [roundcube](https://packages.gentoo.org/packages/roundcube) and [postfixadmin](https://packages.gentoo.org/packages/postfixadmin) web interfaces. Since the database will be generated using postfixadmin, a web server needs to be installed. After the initial setup, however, the web server may be uninstalled again.

There are a few popular choices for a web server:

Install the web server by following the information on the Gentoo Wiki.

### Imap service

Here, too, a choice needs to be made. The popular options include:

Dovecot is a reliable, secure and incredibly fast imap service supporting most known storage formats. It offers high configurability while still keeping the setup process manageable for beginners due to sane defaults. Offering many modern features, it reputes itself as one of the only standards-compliant mail servers.[\[1\]](https://wiki.gentoo.org#cite_note-1)

Courier is a solid, stable imap service. While not as fast as Dovecot, it instead claims higher reliability and has a longer track record. It is rather easy to set up, though it lacks quite a few features expected from modern imap services, like easy configuration of namespaces. Still, it is a reputable and highly reliable choice.

Cyrus is an imap service well known for it's sheer number of options and incredibly high configurability. It is not supported in this guide as of yet, but contributions are always welcome.

Install the chosen package with:

`root #``emerge --ask net-mail/dovecot`
Or:

`root #``emerge --ask net-libs/courier-authlib net-mail/courier-imap`
If the courier-imap route is chosen, cyrus-sasl should also be installed:

`root #``emerge --ask net-mail/cyrus-sasl`
### Database backend

Choose between [dev-db/postgresql](https://packages.gentoo.org/packages/dev-db/postgresql) and [dev-db/mysql](https://packages.gentoo.org/packages/dev-db/mysql). To select either one of them, set the `mysql` or the `postgres` [USE flags](https://wiki.gentoo.org/wiki/USE_flag) in /etc/portage/make.conf.

### Web apps

In order to configure the web server as recommended by this guide, both [www-apps/postfixadmin](https://packages.gentoo.org/packages/www-apps/postfixadmin) and [mail-client/roundcube](https://packages.gentoo.org/packages/mail-client/roundcube) should be installed.

Postfixadmin is strictly required due to being used to set up the database backend. Roundcube is not strictly required, though a necessary part for any webmail functionality. Installation should be done once the appropriate part of the guide is reached.

### Further packages

Postfix is a necessary component of this setup, thus it needs to be installed. For instructions, please read the bottom of this page.

In order to support spam and virus filtering, [mail-filter/amavisd-new](https://packages.gentoo.org/packages/mail-filter/amavisd-new), [mail-filter/spamassassin](https://packages.gentoo.org/packages/mail-filter/spamassassin) and [app-antivirus/clamav](https://packages.gentoo.org/packages/app-antivirus/clamav) need to be installed. Appropriate instructions are to be found here: [Anti-Spam measures](https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/amavisd_spamassassin_clamav).

## Prerequisites

At this point most users just want to get to the good stuff. Before digging into this, the following is strongly suggested to read through:

1. READ THIS DOCUMENT. This is a long document and there are many opportunities to go off the rails. Take the time to become familiar with all of the steps and understand the role that each of the packages play, and recovery from mistakes will be much quicker.
2. Not all of the packages included in this HOWTO are required to get the basic mail server working. SMTP Authorization, Web Administration Tools, SPAM filtering, etc. are not absolute necessities to offer mail services, just really good things to have. By reading through this first, figure out which ones are required and which ones aren't to save the work of installing them to begin with.
3. Like all HOWTOs, this one pretty well assumes that everything in the system is going to be where it should be and that everything will go right at each step. In reality, things will go wrong, at which time the web will prove to be an invaluable resource in resolving whatever issues may occur.
4. A key to testing all of this is having at least one domain (and two would be better to test virtual domains), with the appropriate MX records setup. There are many DNS options out there so a couple of domains and supporting DNS services shouldn't cost more then $10 for the year. This is not the time to go cheap. Invest the money and make life a whole lot simpler.
5. Pay close attention to the messages displayed after emerging any packages. Depending on the state of the machine when the installation was started, the installation of these packages may result in the need to update some of the configuration files. Use dispatch-conf to resolve this. There are no instructions in this HOWTO covering this as this is pretty system-specific.
6. Be sure that system is up to date. Complete an emerge --sync and emerge -uDNva @world before starting this. A number of problems may occur due to old applications not mixing with new utilities.

Finally, to avoid a lot of rework on this, check USE flags variables in the /etc/portage/make.conf file before starting to compiling anything. If the wrong flags are set, it is most likely that things need to be recompiled later to ensure the support needed to make this work is compiled in.

| USE flag | Description | 
|---|---|
| `authdaemond` | Needed by courier-imap if the courier route is taken. | 
| `bzip2` | Required by the virus scanner. | 
| `clamdtop` | ClamAV 'top' like tool. If heavy load on the mail server exists, due to clamav running a lot, clamdtop can help a lot debugging it. | 
| `crypt` | Crypt functions for PHP. Used for passwords in the web interface. | 
| `geoip` | IP to Location mapping for spam prevention. This only allows to resolve IPs to their locations, the location of the server will not be disclosed by this. | 
| `imap` | One of the main interfaces offered by this HOWTO to let clients access e-mail. | 
| `ipv6` | If the system has IPv6 connectivity, enable this. Tested extensively and works well in Postfix, Courier and Apache. | 
| `maildir` | Choose the maildir format supported by Postfix (one of the reasons for choosing Postfix). This is heavily recommended. | 
| `mbox` | Absolutly do not use mbox. | 
| `mysql` | If mysql support is desired enable this. | 
| `postgres` | If postgres support is desired enable this. | 
| `sasl` | Simple Authentication and Security Layer. Strongly recommended. | 
| `spamassassin` | Recommended if spam filtering is required. | 
| `spell` | Spell checking for PHP. Used by webmail. | 
| `ssl` | Secure Socket Layer connections. Absolutely necessary if any security is to be achieved. | 
| `urandom` | Used by cyrus-sasl. Strongly recommended. | 
| `vda` | Quota support for postfix. Adds support for virtual delivery agent quota enforcing. No support in Postfix 3.0, which is now stable in portage. | 
| `vhosts` | Support virtual hosts. Strongly recommended. | 

With the correct USE flags set, emerging just [mail-mta/postfix](https://packages.gentoo.org/packages/mail-mta/postfix) will pull in most, if not all the right packages.

`root #``emerge --ask mail-mta/postfix`
