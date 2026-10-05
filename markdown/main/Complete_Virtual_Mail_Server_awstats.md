<!-- source: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/awstats | group: Gentoo Wiki (Main) | wiki-title: Complete Virtual Mail Server/awstats -->
---
title: Complete Virtual Mail Server/awstats
url: https://wiki.gentoo.org/wiki/Complete_Virtual_Mail_Server/awstats
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-06"
fingerprint: e645a8eb195e9bb7
license: CC BY-SA 4.0
---

# Complete Virtual Mail Server/awstats

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Even though postfix provides logs, having them graphically displayed and analyzed can give much more insight. [www-misc/awstats](https://packages.gentoo.org/packages/www-misc/awstats) is a popular log analyzer that can parse logs and setup proper results.

## Installation

AWStats is a web-application but no longer relies on webapp-config. If it hasn't emerged already, it needs to be emerged:

`root #``emerge --ask www-misc/awstats`
Once installed, a config file needs to be created, either per domain, or one that handles all domains:

`root #``cp /etc/awstats/awstats.model.conf /etc/awstats/awstats.example.com.conf`
AWStats isn't installed via webapp-config, but rather a simple symlink is used:

`root #``ln -s /usr/share/awstats/wwwroot /var/www/mail.example.com/htdocs/awstats`
## Configuration

### AWStats

AWStats comes with reasonable defaults, but some need to be changed nevertheless.

For one, awstats assumes that vhosts aren't used. When using apache's default combined vhost logs for example, awstats will fail to run.

Assume apache's combined LogFormat is setup as follows:

The following changes then need to be made:

**`/etc/awstats/awstats.example.com.conf`**

**Match Logformat to apache's.**

Next, awstats needs to know about the domains and aliases to filter from the log file:

**`/etc/awstats/awstats.example.com.conf`**

**Make awstats listen to the domains**

Also, awstats needs to store its database somewhere. Gentoo has created /var/lib/awstats for this use, but it can be stored anywhere. Make sure the permissions are set so that the apache user can write to it:

**`/etc/awstats/awstats.example.com.conf`**

**AWStats database storage**

Any other changes to the configuration file are optional, but interesting to look into:

### Logging

awstats needs to process the apache log file to build its database. Once confirmed that it is working manually it can be automated.

#### Manually

First, awstats should be run from the console, to spot any initial errors:

`user $``awstats.pl -config=stats.example.com -update -showdropped`
This should list any issues and missing domain names from the config.

#### Cron

If everything is working perfectly, it can then be added to cron.hourly:

**`/etc/cron.hourly/awstats`**

**AWstats cronjob**

```
#!/bin/sh
awstats.pl -config=stats.example.com -update > /dev/null 2>&1
```
Remember to make the script executable, if needed.

#### Logrotate

Awstats will process the log file every hour, but when logrotate rotates apache's log, some entries may be missing. This is easily solved however:

**`/etc/logrotate.d/apache2`**

**Diff of pre-init script**

```
  missingok
  notifempty
  sharedscripts
+  prerotate
+  /etc/cron.hourly/awstats > /dev/null 2>&1
+  endscript
  postrotate
  /etc/init.d/apache2 reload > /dev/null 2>&1 || true
  endscript
}
```
### Apache

For awstats to be used from apache, the webhost needs to properly setup. In the alias section, the following needs to be added:

**`/etc/apache2/vhosts.d/stats.example.com`**

**Aliases for awstats**

Finally, awstats needs the correct permissions to be accessible:

**`/etc/apache2/vhosts.d/stats.example.com`**

**Aliases for awstats**

```
<Directory "/usr/share/awstats/wwwroot">
        Options None
        Require all granted
</Directory>
```
After a restart of apache, awstats should be available via [http://stats.example.com/awstats/awstats.pl?config=stats.example.com](http://stats.example.com/awstats/awstats.pl?config=stats.example.com). If no config option is passed to awstats, it uses the current hostname, which means in this case could have been omitted.

## Awstats for mail log

AWStats is known for being an apache log parser. However it can also be used to parse mail logs.

### Configuration

After copying awstats.stats.example.com.conf to awstats.mail.example.com.conf quite a few changes are required to turn awstats into a mail log parser:

**`/etc/awstats.mail.example.com.conf`**

**Log mail.log instead of access.log (diff -u, only additions and deletions)**

```
-LogFile="/var/log/apache2/access_log"
+LogFile="perl /usr/bin/awstats_maillogconvert.pl standard < /var/log/mail.log |"
 
-LogType=W
+LogType=M
 
-LogFormat=1
+LogFormat="%time2 %email %email_r %host %host_r %method %url %code %bytesd"
 
-SiteDomain="localhost"
+SiteDomain="example.com"
 
 
-HostAliases="localhost 127.0.0.1 REGEX[myserver\.com$]"
+HostAliases="localhost 127.0.0.1 REGEX[example\.(net|org)$]"
 
-DirData="."
+DirData="/var/lib/awstats"
 
-LevelForBrowsersDetection=2         # 0 disables Browsers detection.
+LevelForBrowsersDetection=0         # 0 disables Browsers detection.
                                     # 2 reduces AWStats speed by 2%
                                     # allphones reduces AWStats speed by 5%
-LevelForOSDetection=2               # 0 disables OS detection.
+LevelForOSDetection=0               # 0 disables OS detection.
                                     # 2 reduces AWStats speed by 3%
-LevelForRefererAnalyze=2            # 0 disables Origin detection.
+LevelForRefererAnalyze=0            # 0 disables Origin detection.
                                     # 2 reduces AWStats speed by 14%
-LevelForRobotsDetection=2           # 0 disables Robots detection.
+LevelForRobotsDetection=0           # 0 disables Robots detection.
                                     # 2 reduces AWStats speed by 2.5%
-LevelForSearchEnginesDetection=2    # 0 disables Search engines detection.
+LevelForSearchEnginesDetection=0    # 0 disables Search engines detection.
                                     # 2 reduces AWStats speed by 9%
-LevelForKeywordsDetection=2         # 0 disables Keyphrases/Keywords detection.
+LevelForKeywordsDetection=0         # 0 disables Keyphrases/Keywords detection.
                                     # 2 reduces AWStats speed by 1%
-LevelForFileTypesDetection=2        # 0 disables File types detection.
+LevelForFileTypesDetection=0        # 0 disables File types detection.
                                     # 2 reduces AWStats speed by 1%
 LevelForWormsDetection=0            # 0 disables Worms detection.
                                     # 2 reduces AWStats speed by 15%
 
-ShowRobotsStats=HBL
+ShowRobotsStats=0
 
-ShowEMailSenders=0
+ShowEMailSenders=HBML
 
-ShowEMailReceivers=0
+ShowEMailReceivers=HBML
 
-ShowSessionsStats=1
+ShowSessionsStats=0
 
-ShowPagesStats=PBEX
+ShowPagesStats=0
 
-ShowFileTypesStats=HB
+ShowFileTypesStats=0
 
-ShowFileSizesStats=0
+ShowFileSizesStats=1
 
-ShowDownloadsStats=HB
+ShowDownloadsStats=0
 
-ShowOSStats=1
+ShowOSStats=0
 
-ShowBrowsersStats=1
+ShowBrowsersStats=0
 
-ShowOriginStats=PH
+ShowOriginStats=0
 
-ShowKeyphrasesStats=1
+ShowKeyphrasesStats=0
 
-ShowKeywordsStats=1
+ShowKeywordsStats=0
 
-ShowMiscStats=a
+ShowMiscStats=0
 
-ShowHTTPErrorsStats=1
+ShowHTTPErrorsStats=0
 
-ShowSMTPErrorsStats=0
+ShowSMTPErrorsStats=1
```
With those changes in place, a manual run should work without any issues:

`user $``awstats.pl -config=mail.example.com -showcorrupted -showdropped`
### Logging

To scan the mail log every hour, the existing awstats script in cron.hourly can be appended with the following:

**`/etc/cron.hourly/awstats`**

**Add parsing of the mail log**

```
 -config=mail.example.com -update > /dev/null 2>&1
```
Also syslog is getting rotated and thus awstats needs to parse the mail log file before the mail log is being rotated:

**`/etc/logrotate.d/syslog-ng`**

**Modify mail log entry in syslog**

If logging of apache files is not desired, or webmail resides on a different server, the webserver log parsing can be removed from cron jobs.
