<!-- source: https://wiki.gentoo.org/wiki/Logcheck | group: Gentoo Wiki (Main) | wiki-title: Logcheck -->
---
title: Logcheck
url: https://wiki.gentoo.org/wiki/Logcheck
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-11"
fingerprint: d852bf21705dbc8d
license: CC BY-SA 4.0
---

# Logcheck

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**logcheck** is a tool to analyze the system logs.

## Getting Started With logcheck

### Background

[app-admin/logcheck](https://packages.gentoo.org/packages/app-admin/logcheck) is an updated version of [app-admin/logsentry](https://packages.gentoo.org/packages/app-admin/logsentry), which is a tool to analyze the system logs. Additionally, logcheck comes with a built-in database of common, not-interesting log messages to filter out the noise. The general idea of the tool is that all messages are interesting, except the ones explicitly marked as noise. logcheck periodically sends an email with a summary of interesting messages.

### Installing logcheck

`root #``emerge -c logsentry``root #``rm -rf /etc/logcheck`
Next, the installation of logcheck can be proceeded.

`root #``emerge --ask app-admin/logcheck`
### Basic configuration

[app-admin/logcheck](https://packages.gentoo.org/packages/app-admin/logcheck) creates a separate user "logcheck" to avoid running as root. Actually, it will refuse to run as root. To allow it to analyze the logs, it need to be sure they are readable by logcheck. Here is an example for [app-admin/syslog-ng](https://packages.gentoo.org/packages/app-admin/syslog-ng):

**`/etc/syslog-ng/syslog-ng.conf`**

**syslog-ng configuration snippet**

```
options {
    owner(root);
 
    ## (Make log files group-readable by logcheck)
    group(logcheck);
    perm(0640);
};
```
Next, reload the configuration and make sure the changes work as expected.

`root #````
/etc/init.d/syslog-ng reload
```
`root #``ls -l /var/log/messages`
-rw-r----- 1 root logcheck 1694438 Feb 12 12:18 /var/log/messages

Some basic logcheck settings in /etc/logcheck/logcheck.conf need to be adjusted:

**`/etc/logcheck/logcheck.conf`**

**Basic /etc/logcheck/logcheck.conf setup**

```
# Controls the level of filtering:
# Can be set to "workstation", "server" or "paranoid" for different
# levels of filtering. Defaults to server if not set.
## (The workstation level includes server, and server includes paranoid.
The paranoid level filters almost no messages)
REPORTLEVEL="server"
 
# Controls the address mail goes to:
# *NOTE* the script does not set a default value for this variable!
# Should be set to an offsite "emailaddress@some.domain.tld"
## (Make sure you can receive the logcheck e-mails. Testing is strongly
recommended)
SENDMAILTO="root"
 
# Controls if syslog-summary is run over each section.
# Alternatively, set to "1" to enable extra summary.
# HINT: syslog-summary needs to be installed.
## (If you get a lot of similar messages in the logs, you
may want to install app-admin/syslog-summary and enable
this setting)
SYSLOGSUMMARY=0
```
It must be specified logcheck which log files to scan (/etc/logcheck/logcheck.logfiles.d). More files can be added into /etc/logcheck/logcheck.logfiles.d each of one containing a list of log files to be checked. The installation script will generate 2 files: journal.logfiles and syslog.logfiles.

**`/etc/logcheck/logcheck.logfiles.d/syslog.logfiles`**

**Basic setup**

```
## (This is an example for syslog-ng)
## Log entries in the logs listed below will be checked by logcheck
 
# The default is to check standard syslog files
# created by rsyslog or other syslog daemons
 
# (If the system does not use a syslog daemon, these
# lines can be commented out)
#/var/log/syslog
#/var/log/auth.log
/var/log/messages
```
### Enable periodical log check

Finally, enable a periodical check of the log files.

#### Cron users

If logcheck is emerged with the [cron](https://packages.gentoo.org/useflags/cron) [USE flag enabled, it can read /etc/cron.hourly/logcheck.cron](https://wiki.gentoo.org/wiki/USE_flag)

**`/etc/cron.hourly/logcheck.cron`**

**Basic /etc/cron.hourly/logcheck.cron**

```
#!/bin/sh
#
# To enable sync via cron, execute "sudo -u logcheck touch /etc/logcheck/cron-logcheck-enabled"
if [[ ! -f /etc/logcheck/cron-logcheck-enabled ]]; then
    exit
fi
if [ ! -d /var/lock/logcheck ]; then
    mkdir -p /var/lock/logcheck
    chown logcheck:logcheck /var/lock/logcheck
fi
sudo -u logcheck nice -n10 /usr/sbin/logcheck
```
To enable an hourly cron job, run:

`root #``sudo -u logcheck touch /etc/logcheck/cron-logcheck-enabled`
#### Systemd users

If logcheck is emerged with [systemd](https://packages.gentoo.org/useflags/systemd) [USE flag enabled, a logcheck.timer can be activated running:](https://wiki.gentoo.org/wiki/USE_flag)

`root #``systemctl enable --now logcheck.timer`
Now the user will be regularly getting important log messages by email. An example message looks like this:

## Troubleshooting

### General tips

To display more debugging information the logcheck's `-d` switch can be used. Example:

`root #``su -s /bin/bash -c '/usr/sbin/logcheck -d' logcheck`
D: \[1281318818\] Turning debug mode on
D: \[1281318818\] Sourcing - /etc/logcheck/logcheck.conf
D: \[1281318818\] Finished getopts c:dhH:l:L:m:opr:RsS:tTuvw
D: \[1281318818\] Trying to get lockfile: /var/lock/logcheck/logcheck.lock
D: \[1281318818\] Running lockfile-touch /var/lock/logcheck/logcheck.lock
D: \[1281318818\] cleanrules: /etc/logcheck/cracking.d/kernel
...
D: \[1281318818\] cleanrules: /etc/logcheck/violations.d/su
D: \[1281318818\] cleanrules: /etc/logcheck/violations.d/sudo
...
D: \[1281318825\] logoutput called with file: /var/log/messages
D: \[1281318825\] Running /usr/sbin/logtail2 on /var/log/messages
D: \[1281318825\] Sorting logs
D: \[1281318825\] Setting the Intro
D: \[1281318825\] Checking for security alerts
D: \[1281318825\] greplogoutput: kernel
...
D: \[1281318825\] greplogoutput: returning 1
D: \[1281318825\] Checking for security events
...
D: \[1281318825\] greplogoutput: su
D: \[1281318825\] greplogoutput: Entries in checked
D: \[1281318825\] cleanchecked - file: /tmp/logcheck.uIFLqU/violations-ignore/logcheck-su
D: \[1281318825\] report: cat'ing - Security Events for su
...
D: \[1281318835\] report: cat'ing - System Events
D: \[1281318835\] Setting the footer text
D: \[1281318835\] Sending report: 'localhost 2010-08-09 03:53 Security Events' to root
D: \[1281318835\] cleanup: Killing lockfile-touch - 17979
D: \[1281318835\] cleanup: Removing lockfile: /var/lock/logcheck/logcheck.lock
D: \[1281318835\] cleanup: Removing - /tmp/logcheck.uIFLqU
