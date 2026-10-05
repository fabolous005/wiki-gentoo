<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/Logging | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/Logging -->
---
title: Security Handbook/Logging
url: https://wiki.gentoo.org/wiki/Security_Handbook/Logging
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-26"
fingerprint: c572c00a65e158e1
license: CC BY-SA 4.0
---

# Security Handbook/Logging

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

System administrators should choose between different system loggers in order to gain better viability into attacks.

Logging verbosity can be increased to catch warnings or errors that might indicate an ongoing attack or successful compromise. Attackers often scan or probe before directly attacking a targeted system, and these probes can be detected with proper logging.

It is also vital that log files are readable, well managed, and stored safely. Loggers should be chosen with consideration to security as well as the use case. There is no best logging utility for every job, although some are more versatile than others.

The following log utilities may be unnecessary on systemd systems, since [it includes logging as a core functionality (journald)](https://wiki.gentoo.org/wiki/Systemd#Handling_of_log_files).

[Sysklogd](https://wiki.gentoo.org/wiki/Sysklogd) is very commonly used with Linux and Unix systems in general. It has some log rotation facilities, but using [logrotate](https://wiki.gentoo.org/wiki/Logrotate) in a [cron](https://wiki.gentoo.org/wiki/Cron) job or [systemd](https://wiki.gentoo.org/wiki/Systemd) timer offers more features and control over how log files are rotated. The frequency of log rotation depends on many factors, such as load and capacity.

Below is the standard sysklogd configuration, located at, syslog.conf with some added features. The following *cron* and *tty* lines have been uncommented, and a remote logging server had been added.

**`/etc/syslog.conf`**

**Syslog example**

~~Metalog by Frank Dennis is not able to log to a remote server, but it does have advantages when it comes to performance and logging flexibility~~. It can log by program name, urgency, facility (like sysklogd), and comes with regular expression matching with which you can launch external scripts when specific patterns are found. It is very good at taking action when needed.

The standard configuration is usually enough. To be notified by email whenever a password failure occurs use one of the following scripts.

For postfix:

**`/usr/local/sbin/mail_pwd_failures.sh`**

**Postfix**

```
#!/bin/sh
echo "$3" | mail -s "Warning (program : $2)" root
```
For netqmail:

**`/usr/local/sbin/mail_pwd_failures.sh`**

**Netqmail**

```
#!/bin/sh
echo "To: root
Subject:Failure (Warning: $2)
$3
" | /var/qmail/bin/qmail-inject -f root
```
Remember to make the script executable by issuing chmod +x /usr/local/sbin/mail\_pwd\_failures.sh

Then uncomment the command line under "Password failures" in /etc/metalog/metalog.conf like:

**`/etc/metalog/metalog.conf`**

**Metalog**

```
command  = "/usr/local/sbin/mail_pwd_failures.sh"
```
[Syslog-ng](https://wiki.gentoo.org/wiki/Syslog-ng) provides many of the same features as sysklogd and metalog in a single package that does not run as root. It can filter messages based on level and content (like metalog), provide remote logging (like sysklogd), handle logs from *syslog* (or even Solaris).  In addition to standard log handling features, syslog-ng can write to a TTY, execute programs, and act as a logging server.

Below is a copy of the gentoo-hardened configuration from /usr/share/doc/syslog-ng-4.6.0/syslog-ng.conf.gentoo.hardened.bz2, which can be deployed at /etc/syslog-ng/syslog-ng.conf:

**`/etc/syslog-ng/syslog-ng.conf`**

**Syslog-ng**

```
@version: 4.6
# Copyright 1999-2019 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2
# https://bugs.gentoo.org/426814
@include "scl.conf"
#
# Syslog-ng configuration file, compatible with default hardened installations.
#
options {
	threaded(yes);
	chain_hostnames(no);
	stats(freq(43200));
};
source src {
    system();
    internal();
};
source kernsrc {
    file("/proc/kmsg");
};
#source net { udp(); };
#log { source(net); destination(net_logs); };
#destination net_logs { file("/var/log/HOSTS/$HOST/$YEAR$MONTH$DAY.log"); };
destination authlog { file("/var/log/auth.log"); };
destination _syslog { file("/var/log/syslog"); };
destination cron { file("/var/log/cron.log"); };
destination daemon { file("/var/log/daemon.log"); };
destination kern { file("/var/log/kern.log"); };
destination lpr { file("/var/log/lpr.log"); };
destination user { file("/var/log/user.log"); };
destination uucp { file("/var/log/uucp.log"); };
#destination ppp { file("/var/log/ppp.log"); };
destination mail { file("/var/log/mail.log"); };
destination avc { file("/var/log/avc.log"); };
destination audit { file("/var/log/audit.log"); };
destination pax { file("/var/log/pax.log"); };
destination grsec { file("/var/log/grsec.log"); };
destination mailinfo { file("/var/log/mail.info"); };
destination mailwarn { file("/var/log/mail.warn"); };
destination mailerr { file("/var/log/mail.err"); };
destination newscrit { file("/var/log/news/news.crit"); };
destination newserr { file("/var/log/news/news.err"); };
destination newsnotice { file("/var/log/news/news.notice"); };
destination debug { file("/var/log/debug"); };
destination messages { file("/var/log/messages"); };
destination console { usertty("root"); };
destination console_all { file("/dev/tty12"); };
#destination loghost { udp("loghost" port(999)); };
destination xconsole { pipe("/dev/xconsole"); };
filter f_auth { facility(auth); };
filter f_authpriv { facility(auth, authpriv); };
filter f_syslog { not facility(authpriv, mail); };
filter f_cron { facility(cron); };
filter f_daemon { facility(daemon); };
filter f_kern { facility(kern); };
filter f_lpr { facility(lpr); };
filter f_mail { facility(mail); };
filter f_user { facility(user); };
filter f_uucp { facility(uucp); };
#filter f_ppp { facility(ppp); };
filter f_news { facility(news); };
filter f_debug { not facility(auth, authpriv, news, mail); };
filter f_messages { level(info..warn)
	and not facility(auth, authpriv, mail, news); };
filter f_emergency { level(emerg); };
filter f_info { level(info); };
filter f_notice { level(notice); };
filter f_warn { level(warn); };
filter f_crit { level(crit); };
filter f_err { level(err); };
filter f_avc { message(".*avc: .*"); };
filter f_audit { message("^(\\[.*\..*\] |)audit.*") and not message(".*avc: .*"); };
filter f_pax { message("^(\\[.*\..*\] |)PAX:.*"); };
filter f_grsec { message("^(\\[.*\..*\] |)grsec:.*"); };
log { source(src); filter(f_authpriv); destination(authlog); };
log { source(src); filter(f_syslog); destination(_syslog); };
log { source(src); filter(f_cron); destination(cron); };
log { source(src); filter(f_daemon); destination(daemon); };
log { source(kernsrc); filter(f_kern); destination(kern); destination(console_all); };
log { source(src); filter(f_lpr); destination(lpr); };
log { source(src); filter(f_mail); destination(mail); };
log { source(src); filter(f_user); destination(user); };
log { source(src); filter(f_uucp); destination(uucp); };
log { source(kernsrc); filter(f_pax); destination(pax); };
log { source(kernsrc); filter(f_grsec); destination(grsec); };
log { source(kernsrc); filter(f_audit); destination(audit); };
log { source(kernsrc); filter(f_avc); destination(avc); };
log { source(src); filter(f_mail); filter(f_info); destination(mailinfo); };
log { source(src); filter(f_mail); filter(f_warn); destination(mailwarn); };
log { source(src); filter(f_mail); filter(f_err); destination(mailerr); };
log { source(src); filter(f_news); filter(f_crit); destination(newscrit); };
log { source(src); filter(f_news); filter(f_err); destination(newserr); };
log { source(src); filter(f_news); filter(f_notice); destination(newsnotice); };
log { source(src); filter(f_debug); destination(debug); };
log { source(src); filter(f_messages); destination(messages); };
log { source(src); filter(f_emergency); destination(console); };
#log { source(src); filter(f_ppp); destination(ppp); };
log { source(src); destination(console_all); };
```
Of course, keeping logs alone is only half the battle. An application such as Logcheck can make regular log analysis much easier. logcheck is a script, accompanied by a binary called logtail, that runs from the cron daemon and checks the system logs against a set of rules for suspicious activity. It then mails the output to root's mailbox.

logcheck and logtail are part of the [app-admin/logcheck](https://packages.gentoo.org/packages/app-admin/logcheck) package.

Logcheck uses four files to filter important log entries from the unimportant:

- logcheck.hacking - Contains known hacking attack messages.
- logcheck.violations - Contains patterns indicating security violations.
- logcheck.violations.ignore - Contains keywords likely to be matched by the violations file, allowing normal entries to be ignored.
- logcheck.ignore - matches those entries to be ignored.
