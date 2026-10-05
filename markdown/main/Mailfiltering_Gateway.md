<!-- source: https://wiki.gentoo.org/wiki/Mailfiltering_Gateway | group: Gentoo Wiki (Main) | wiki-title: Mailfiltering Gateway -->
---
title: Mailfiltering Gateway
url: https://wiki.gentoo.org/wiki/Mailfiltering_Gateway
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-15"
fingerprint: a6b9003a3da21eb2
license: CC BY-SA 4.0
---

# Mailfiltering Gateway

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide provides step-by-step instructions for installing spam fighting technologies for Postfix. Among them Amavisd-new using SpamAssassin and ClamAV, greylisting, and SPF.

See the [Complete Virtual Mail Server](https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server) article for a full guide on setting up a well featured mail server.

## Introduction

This guide describes step by step how to install a spam and virus filtering mail gateway. It is quite simple to adopt this to a single server solution.

### The big picture

This document describe how to setup a spam filtering mail gateway with multiple domains. This server is meant to run in front of the mail servers actually keeping the mail accounts, e.g. Microsoft Exchange or Lotus Notes.

In this setup applications with good security records and readable configuration files have been chosen. The email MTA is postfix which has a good security record and is fairly easy to setup right. Postfix will listen normally on port 25 for incoming mail. Upon reception, it will forward it to Amavisd-new on port 10024. Amavisd-new will then filter the mail through different filters before passing the mail back to Postfix on port 10025, which in turn will forward the mail to the next mail server.

Amavisd-new is a content filtering framework which uses helper applications for virus filtering and spam filtering. In this setup, two helper applications will be used: ClamAV for filtering virus mails, and SpamAssassin for filtering spam. SpamAssassin itself can function as yet another layer of content filtering framework and utilize the helper applications Vipul's Razor2 and DCC.

Unlike many other spam fighting technologies like RBLs, SpamAssassin does not simply accept or reject a given email based on one single test. It uses a lot of internal tests and external helper applications to calculate a spam score for every mail passed through. This score is based on the following tests:

- Bayesian filtering
- Static rules based on regular expressions
- Distributed and collaborative networks:
  - RBLs
  - Razor2
  - Pyzor
  - DCC

The first part (chapters 1 to 4) of this guide describes the basic setup of a mailfiltering gateway. The next chapters can be implemented individually with no dependence between each chapter. These chapters describe how to:

- setup special IMAP folders for learning of the Bayesian filter and for delivery of false positives
- setup greylisting with Postfix
- setup Amavisd-new to use a MySQL backend for user preferences
- setup SpamAssassin to use a MySQL backend for AWL and Bayes data

A planned fifth part will contain various tips regarding performance and things you may want to know (running chrooted, postfix restrictions, etc.).

### Preparations

Before starting, ensure the system has a working Postfix installation where mails can be sent and received, and a backend mailserver.

For those inexperienced with setting up Postfix, it might quickly become too complicated if setting everything up at once. The [Complete Virtual Mail Server](https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server) article can be helpful in this case.

## Installing the programs needed

Start by installing the most important programs: Amavisd-new, SpamAssassin, and ClamAV.

`root #``echo "mail-filter/amavisd-new spamassassin clamav" >> /etc/portage/package.use``root #``emerge amavisd-new``root #``freshclam`
### Setting up DNS

While the programs are emerging fire up another shell and create the needed DNS records.

Start out by creating a `MX` record for the mail gateway and an `A` record for the next destination.





### Opening the firewall

In addition to allowing normal mail traffic you have to allow a few services through your firewall to allow the network checks to communicate with the servers.

| Application | Protocol | Port | 
|---|---|---|
| DCC | UDP | 6277 | 
| Razor (outgoing ping) | TCP | 7 | 
| Razor | TCP | 2703 | 

Razor uses pings to discover what servers are closest to it.

### Configuring Postfix

First we have to tell `postfix` to listen on port 10025 and we remove most of the restrictions as they have already been applied by the `postfix` instance listening on port 25. Also we ensure that it will only listen for local connections on port 10025. To accomplish this we have to add the following at the end of /etc/postfix/master.cf

The file master.cf tells the postfix master program how to run each individual postfix process. More info with `man 8 master` .

Next we need the main `postfix` instance listening on port 25 to filter the mail through `amavisd-new` listening on port 10024.

We also need to set the next hop destination for mail. Tell Postfix to filter all mail through an external content filter and enable explicit routing to let Postfix know where to forward the mail to.

**`/etc/postfix/main.cf`**

```
biff = no
empty_address_recipient = MAILER-DAEMON
queue_minfree = 120000000
  
content_filter = smtp-amavis:[127.0.0.1]:10024
#Equivalently when using lmtp:
#content_filter = lmtp-amavis:[127.0.0.1]:10024
  
# TRANSPORT MAP
#
# Insert text from sample-transport.cf if you need explicit routing.
transport_maps = hash:/etc/postfix/transport
  
relay_domains = $transport_maps
```
Postfix has a lot of options set in main.cf . For further explanation of the file please consult `man 5 postconf` or the same online [Postfix Configuration Parameters](http://www.postfix.org/postconf.5.html) .

The format of the transport file is the normal Postfix hash file. Mail to the domain on the left hand side is forwarded to the destination on the right hand side.

**`/etc/postfix/transport`**

After we have edited the file we need to run the `postmap` command. Postfix does not actually read this file so we have to convert it to the proper format with `postmap /etc/postfix/transport` . This creates the file /etc/postfix/transport.db . There is no need to reload Postfix as it will automatically pick up the changes.

If your first attempts to send mail result in messages bouncing, you've likely made a configuration error somewhere. Try temporarily enabling `soft_bounce` while you work out your configuration issues. This prevents postfix from bouncing mails on delivery errors by treating them as temporary errors. It keeps mails in the mail queue until `soft_bounce` is disabled or removed.

`root #````
postconf -e "soft_bounce = yes"
```
`root #``/etc/init.d/postfix reload`
Once you've finished creating a working configuration, be sure to disable or remove `soft_bounce` and reload postfix.

### Configuring Amavisd-new

Amavisd-new is used to handle all the filtering and allows you to easily glue together severel different technologies. Upon reception of a mail message it will extract the mail, filter it through some custom filters, handle white and black listing, filter the mail through various virus scanners and finally it will filter the mail using SpamAssassin.

Amavisd-new itself has a number of extra features:

- it identifies dangerous file attachments and has policies to handle them
- per-user, per-domain and system-wide policies for:
  - whitelists
  - blacklists
  - spam score thresholds
  - virus and spam policies

Apart from `postfix` and `freshclam` we will run all applications as the user `amavis` .

Edit the following lines in /etc/amavisd.conf

**`/etc/amavisd.conf`**

```
# (Insert the domains to be scanned)
$mydomain = 'example.com';
# (Bind only to loopback interface)
$inet_socket_bind = '127.0.0.1';
# (Forward to Postfix on port 10025)
$forward_method = 'smtp:127.0.0.1:10025';
$notify_method = $forward_method;
# (Define the account to send virus alert emails)
$virus_admin = "virusalert\@$mydomain";
# (Always add spam headers)
$sa_tag_level_deflt  = -100;
# (Add spam detected header aka X-Spam-Status: Yes)
$sa_tag2_level_deflt = 5;
# (Trigger evasive action at this spam level)
$sa_kill_level_deflt = $sa_tag2_level_deflt;
# (Do not send delivery status notification to sender.  It does not affect
# delivery of spam to recipient. To do that, use the kill_level)
$sa_dsn_cutoff_level = 10;
# Don't bounce messages left and right, quarantine
# instead
$final_virus_destiny      = D_DISCARD;  # (defaults to D_DISCARD)
$final_banned_destiny     = D_DISCARD;  # (defaults to D_BOUNCE)
$final_spam_destiny       = D_DISCARD;  # (defaults to D_BOUNCE)
```
Create a quarantine directory for the virus mails as we don't want these delivered to our users.

`root #````
mkdir /var/amavis/virusmails
```
`root #````
chown amavis:amavis /var/amavis/virusmails
```
`root #``chmod 750 /var/amavis/virusmails`
### Configuring ClamAV

As virus scanner we use ClamAV as it has a fine detection rate comparable with commercial offerings, it is very fast and it is Open Source Software. We love log files, so make `clamd` log using `syslog` and turn on verbose logging. Also do not run `clamd` as `root` . Now edit /etc/clamd.conf

**`/etc/clamd.conf`**

ClamAV comes with the `freshclam` deamon dedicated to periodical checks of virus signature updates. Instead of updating virus signatures twice a day we will make `freshclam` update virus signatures every two hours.

**`/etc/freshclam.conf`**

Start `clamd` with `freshclam` using the init scripts by modifying /etc/conf.d/clamd .

**`/etc/conf.d/clamd`**

```
START_CLAMD=yes
START_FRESHCLAM=yes
CLAMD_NICELEVEL=3
FRESHCLAM_NICELEVEL=19
IONICE_LEVEL=2
```
At last modify amavisd.conf with the new location of the socket.

**`/etc/amavisd.conf`**

```
# (Uncomment the clamav scanner and modify socket location)
['ClamAV-clamd',
\&ask_daemon, ["CONTSCAN {}\n", "/var/amavis/clamd"],
  qr/\bOK$/, qr/\bFOUND$/,
  qr/^.*?: (?!Infected Archive)(.*) FOUND$/ ],
```
### Configuring Vipul's Razor

Razor2 is a collaborative and distributed spam checksum network. Install it with `emerge razor` and create the needed configuration files. Do this as user `amavis` by running `su - amavis` followed `razor-admin -create` .

`root #``emerge razor``root #``su - amavis -s /bin/bash``user $````
razor-admin -create
```
`user $``exit`
### Configuring Distributed Checksum Clearinghouse (dcc)

Like Razor2, dcc is a collaborative and distributed spam checksum network. Its philosopy is to count the number of recipients of a given mail identifying each mail with a fuzzy checksum.

`root #``emerge dcc`
### Configuring SpamAssassin

Amavis is using the SpamaAsassin Perl libraries directly so there is no need to start the service. Also this creates some confusion about the configuration as some SpamAssassin settings are configured in /etc/mail/spamassassin/local.cf and overridden by options in /etc/amavisd.conf .

**`/etc/mail/spamassassin/local.cf`**



## Every good rule has good exceptions as well

Once mail really starts passing through this mail gateway you will probably discover that the above setup is not perfect. Maybe some of your customers like to receive mails that others wouldn't. You can whitelist/blacklist envelope senders quite easily. Uncomment the following line in amavisd.conf .

**`amavisd.conf`**

**do sitewide scoring**

```
("/var/amavis/sender_scores_sitewide"),
```
In the sender\_scores\_sitewide file you put complete email addresses or just the domian parts and then note a positive/negative score to add to the spam score.

**`whitelist_sender`**

**example**



While waiting for a better method you can add the following to amavisd.conf to bypass spam checks for `postmaster` and `abuse` mailboxes.

## Adding more rules

If you want to use more rules provided by the SARE Ninjas over at the [SpamAssassin Rules Emporium](http://www.rulesemporium.com/) you can easily add and update them using the `sa-update` mechanism included in SpamAssassin.

A brief guide to using SARE rulesets with `sa-update` can be found [here](http://daryl.dostech.ca/sa-update/sare/sare-sa-update-howto.txt) .

## Testing and finishing up

### Testing the setup

Now before you start `freshclam` you can manually verify that it works.

`root #``freshclam`
ClamAV update process started at Sun May  2 09:13:41 2004
Reading CVD header (main.cvd): OK
Downloading main.cvd \[\*\]
main.cvd updated (version: 22, sigs: 20229, f-level: 1, builder: tkojm)
Reading CVD header (daily.cvd): OK
Downloading daily.cvd \[\*\]
daily.cvd updated (version: 298, sigs: 1141, f-level: 2, builder: diego)
Database updated (21370 signatures) from database.clamav.net (193.1.219.100).

Now you have updated virus definitions and you know that freshclam.conf is working properly.

Test freshclam and amavisd from the cli and amavisd testmails. Start `clamd` and `amavis` with the following commands:

`root #````
/etc/init.d/clamd start
```
`root #````
/etc/init.d/amavisd start
```
`root #``/etc/init.d/postfix reload`
If everything went well `postfix` should now be listening for mails on port 25 and for reinjected mails on port 10024. To verify this check your log file.

`root #``tail -f /var/log/mail.log`
Now if no strange messages appear in the log file it is time for a new test.

Use `netcat` to manually connect to `amavisd` on port 10024 and `postfix` on port 10025.

`root #``nc localhost 10024`
220 \[127.0.0.1\] ESMTP amavisd-new service ready

`root #``nc localhost 10025`
220 example.com ESMTP Postfix



Add `amavisd` and `clamd` to the `default` runlevel.

`root #````
rc-update add clamd default
```
`root #``rc-update add amavisd default`
## Autolearning and sidelining emails

### Creating the spamtrap user

Create the spamtrap account and directories.

`root #````
useradd -m spamtrap
```
`root #````
maildirmake /home/spamtrap/.maildir
```
`root #``chown -R spamtrap:spamtrap /home/spamtrap/.maildir`
Give the spamtrap user a sensible password.

`root #``passwd spamtrap`
If you manually want to check some of the mails to ensure that you have no false positives you can use the following `procmail` recipe to sideline spam found into different mail folders.

### Creating .procmailrc

**`/home/spamtrap/.procmailrc`**

```
#Set some default variables
MAILDIR=$HOME/.maildir
  
SPAM_FOLDER=$MAILDIR/.spam-found/
  
LIKELY_SPAM_FOLDER=$MAILDIR/.likely-spam-found/
  
#Sort mails with a spamscore of 7+ to the spamfolder
:0:
* ^X-Spam-Status: Yes
* ^X-Spam-Level: \*\*\*\*\*\*\*
$SPAM_FOLDER
  
#Sort mail with a spamscore between 5-7 to the likely spam folder
:0:
* ^X-Spam-Status: Yes
$LIKELY_SPAM_FOLDER
  
#Sort all other mails to the inbox
:0
*
./
```
Now make sure that Postfix uses `procmail` to deliver mail.

**`/etc/postfix/main.cf`**

```
mailbox_command = /usr/bin/procmail -a "DOMAIN"
```
### Create mailfolders

Now we will create shared folders for ham and spam.

`root #````
maildirmake /var/amavis/.maildir
```
`root #````
maildirmake -S /var/amavis/.maildir/Bayes
```
`root #````
maildirmake -s write -f spam /var/amavis/.maildir/Bayes
```
`root #````
maildirmake -s write -f ham /var/amavis/.maildir/Bayes
```
`root #``maildirmake -s write -f redeliver /var/amavis/.maildir/Bayes`
Amavisd-new needs to be able to read these files as well as all mailusers. Therefore we add all the relevant users to the mailuser group along with amavis.

`root #````
groupadd mailusers
```
`root #````
usermod -G mailusers spamtrap
```
`root #````
chown -R amavis:mailusers /var/amavis/.maildir/
```
`root #````
chown amavis:mailusers /var/amavis/
```
`root #````
chmod -R 1733 /var/amavis/.maildir/Bayes/
```
`root #````
chmod g+rx /var/amavis/.maildir/
```
`root #``chmod g+rx /var/amavis/.maildir/Bayes/`
This makes the spam and ham folders writable but not readable. This way users can safely submit their ham without anyone else being able to read it.

Then run the following command as the `spamtrap` user:

`user $``maildirmake --add Bayes=/var/amavis/.maildir/Bayes $HOME/.maildir`
### Adding cron jobs

Now run `crontab -u amavis -e` to edit the amavis crontab to enable automatic learning of the Bayes filter every hour.

**`crontab`**

**for amavis user**

### Modifying amavisd.conf

Now modify amavis to redirect spam emails to the `spamtrap` account and keep spamheaders.

**`/etc/amavisd.conf`**

```
# (Define the account to send virus spam emails)
$spam_quarantine_to = "spamtrap\@$myhostname";
```
### Cleaning up

We don't want to keep mail forever so we use `tmpwatch` to clean up regularily. Emerge it with `emerge tmpwatch` . Only `root` is able to run `tmpwatch` so we have to edit the `root` crontab.

**`crontab`**

**root user**

## Greylisting

### Introduction

Greylisting is one of the newer weapons in the spam fighting arsenal. As the name implies it is much like whitelisting and blacklisting. Each time an unknown mailserver tries to deliver mail the mail is rejected with a *try again later* message. This means that mail gets delayed but also that stupid spam bots that do not implement the RFC protocol will drop the attempt to deliver the spam and never retry. With time spam bots will probably adjust, however it will give other technologies more time to identify the spam.

Postfix 2.1 come with a simple Perl greylisting policy server that implements such a scheme. However it suffers from unpredictable results when the partition holding the greylisting database run out of space. There exists an improved version that do not suffer this problem. First I will show how to install the builtin greylisting support that come with Postfix and then I will show how to configure the more robust replacement.

### Simple greylisting

We need the file greylist.pl but unfortunately the ebuild does not install it as default.

`root #````
cp /var/cache/distfiles/postfix-your-version-here.tar.gz /root/
```
`root #````
tar xzf postfix-your-version-here.tar.gz
```
`root #``cp postfix-2.1.0/examples/smtpd-policy/greylist.pl /usr/bin/`
Now we have the file in place we need to create the directory to hold the greylisting database:

`root #````
mkdir /var/mta
```
`root #``chown nobody /var/mta`
### Configuring greylisting

Now that we have all this ready all that is left is to add it to the postfix configuration. First we add the necessary information to the master.cf :

The postfix spawn daemon normally kills its child processes after 1000 seconds but this is too short for the greylisting process so we have to increase the timelimit in main.cf :

**`main.cf`**

**use greylisting**

```
policy-greylist_time_limit = 3600
# (Under smtpd_recipient_restrictions add:)
check_sender_access hash:/etc/postfix/sender_access
# (Later on add:)
restriction_classes = greylist
greylist = check_policy_service unix:private/policy-greylist
```
We don't want to use greylisting for all domains but only for those frequently abused by spammers. After all it will delay mail delivery. A list of frequently forged MAIL FROM domains can be found [online](http://www.monkeys.com/anti-spam/filtering/sender-domain-validate.in) . Add the domains you receive a lot of spam from to /etc/postfix/sender\_access :

If you want a more extensive list:

`root #``cat sender-domain-validate.in | sort | awk {'print $1 "\t\t greylist"'} > /etc/postfix/sender_access`
Now we only have to initialize the sender\_access database:

`root #``postmap /etc/postfix/sender_access`
Now the setup of simple greylisting is complete.

### Configuring improved greylisting with postgrey

You can install the enhanced greylisting policy server with a simple `emerge` :

`root #``emerge postgrey`
After installing `postgrey` we have to edit main.cf . Changes are almost exactly like the built in greylisting.

**`main.cf`**

**use greylisting**

```
# (Under smtpd_recipient_restrictions add:)
check_sender_access hash:/etc/postfix/sender_access
# (Later on add:)
smtpd_restriction_classes = greylist
greylist = check_policy_service inet:127.0.0.1:10030
```
Finally, start the server and add it to the proper runlevel.

`root #````
/etc/init.d/postgrey start
```
`root #``rc-update add postgrey default`
## SPF (Sender Policy Framework)

### Introduction

SPF allows domain owners to state in their DNS records which IP addressess should be allowed to send mails from their domain. This will prevent spammers from spoofing the `Return-Path`.

First domain owners have to create a special `TXT` DNS record. Then an SPF-enabled MTA can read this and if the mail originates from a server that is not described in the SPF record the mail can be rejected. An example entry could look like this:

The `-all` means to reject all mail by default but allow mail from the `A`( `a` ), `MX`( `mx` ) and `PTR`( `ptr` ) DNS records. For more info consult further resources below.

SpamaAsassin 3.0 has support for SPF, however it is not enabled by default and the new policy daemon in Postfix supports SPF so let's install SPF support for Postfix.

### Preparations

First you have to install Postfix 2.1 as described above. When you have fetched the source grab the spf.pl with:

`root #``cp postfix-<version>/examples/smtpd-policy/spf.pl /usr/local/bin/`
This Perl script also needs some Perl libraries that are not in portage but it is still quite simple to install them:

`root #``emerge Mail-SPF-Query Net-CIDR-Lite Sys-Hostname-Long`
Now that we have everything in place all we need is to configure Postfix to use this new policy.

**`master.cf`**

**use SPF**

Now add the SPF check in main.cf . Properly configured SPF should do no harm so we could check SPF for all domains:

**`main.cf`**

**use SPF**

## Configuring amavisd-new to use MySQL

### Configuring MySQL

For large domains the default values you can set in amavisd.conf might not suit all users. If you configure amavisd-new with MySQL support you can have individual settings for users or groups of users.

`user $``mysql -u root -p mysql`
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 78 to server version: 4.0.18-log
  
Type 'help;' or '\h' for help. Type '\c' to clear the buffer.

`mysql>``create database maildb;``mysql>````
GRANT INSERT,UPDATE,DELETE,SELECT ON maildb.* TO 'mail'@'localhost' IDENTIFIED BY 'very_secret_password';
```
`mysql>``use maildb;`
Now that the database is created we'll need to create the necessary tables. You can cut and paste the following into the mysql prompt:

If you wish to use whitelisting and blacklisting you must add the sender and receiver to `mailadr` after which you create the relation between the two e-mail addresses in `wblist` and state if it is whitelisting ( `W` ) or blacklisting ( `B` ).

Now that we have created the tables let's insert a test user and a test policy:

This inserts a test user and a Test policy. Adjust these examples to fit your needs. Further explanation of the configuration names can be found in amavisd.conf .

### Configuring amavisd to use MySQL

Now that MySQL is ready we need to tell amavis to use it:

**`amavisd.conf`**

**Update to use MySQL**

```
@lookup_sql_dsn =
   ( ['DBI:mysql:maildb:host1', 'mail', 'very_secret_password']  );
  
# (For clarity uncomment the default)
$sql_select_policy = 'SELECT *,users.id FROM users,policy'.
   ' WHERE (users.policy_id=policy.id) AND (users.email IN (%k))'.
   ' ORDER BY users.priority DESC';
  
# (If you want sender white/blacklisting)
   $sql_select_white_black_list = 'SELECT wb FROM wblist,mailaddr'.
     ' WHERE (wblist.rid=?) AND (wblist.sid=mailaddr.id)'.
     '   AND (mailaddr.email IN (%k))'.
     ' ORDER BY mailaddr.priority DESC';
</pre>
```
## Configuring Spamassassin to use MySQL

As of SpamAssassin 3.0 it is possible to store the Bayes and AWL data in a MySQL database. We will use MySQL as the backend as it can generally outperform other databases. Also, using MySQL for both sets of data makes system management much easier. Here I will show how to easily accomplish this.

First start out by creating the new MySQL user and then create the needed tables.

`root #``mysql -u root -p mysql`
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 78 to server version: 4.0.18-log
  
Type 'help;' or '\h' for help. Type '\c' to clear the buffer.

`mysql>``create database dbname;``mysql>````
GRANT INSERT,UPDATE,DELETE,SELECT ON dbname.* TO 'dbuser'@'localhost' IDENTIFIED BY 'another_very_secret_password';
```
`mysql>``use dbname;`
Now that the database is created we'll create the necessary tables. You can cut and paste the following into the mysql prompt:

### Configuring Spamassassin to use the MySQL backend

If you have an old Bayes database in the DBM database and want to keep it follow these instructions:

`root #``su - amavis``user $````
sa-learn --sync
```
`user $````
sa-learn --backup > backup.txt
```
`user $``sa-learn --restore backup.txt`
Now give SpamAssassin the required info:

**`/etc/mail/spamassassin/secrets.cf`**

Next, change its permissions for proper security:

`root #``chmod 400 /etc/mail/spamassassin/secrets.cf`
Now all you have to do is `/etc/init.d/amavisd restart` .

## Troubleshooting

### Amavisd-new

To troubleshoot Amavisd-new start out by stopping it with `/etc/init.d/amavisd stop` and then start it manually in the foreground with `amavisd debug` and watch it for anomalies in the output.

### Spamassassin

To troubleshoot SpamaAsassin you can filter an email through it with `spamassassin -D < mail` . To ensure that the headers are intact you can move it from another machine with IMAP.

If you want you can make get the same information and more with Amavisd-new using `amavisd debug-sa` .

### Repeating tasks after installation

Some of the activities mentioned in this guide will need to be repeated after upgrades. For instance, the `chown -R amavis:mailusers` will need to be repeated after every update of amavisd-new.

Luckily, Gentoo provides you with the means to perform these steps automatically. In [Hooking in the Emerge Process](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Advanced#Hooking_in_the_emerge_process), the Gentoo Handbook explains how to execute tasks after installations of a particular package, like so:

### Getting help

If you need help a good place to go is the amavis-user mailing list. Before postting a question try searching the [Amavis User mailing list archives](http://marc.theaimsgroup.com/?l=amavis-user) . If you find no answer here you can subscribe to the [Amavis User mailing list](https://lists.sourceforge.net/lists/listinfo/amavis-user)

If your question is specific to SpamAssassin, DCC, Razor, or Postfix, please refer to their respective home pages listed below.

## Resources

### For further information

### General resources
