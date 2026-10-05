<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/Intrusion_detection | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/Intrusion detection -->
---
title: Security Handbook/Intrusion detection
url: https://wiki.gentoo.org/wiki/Security_Handbook/Intrusion_detection
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-26"
fingerprint: ad03f93a2b67af9b
license: CC BY-SA 4.0
---

# Security Handbook/Intrusion detection

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This section is on detecting changes made to your system indicating possible intrusions.



### qcheck

The [Q applets](https://wiki.gentoo.org/wiki/Q_applets) program [qcheck](https://wiki.gentoo.org/wiki/Q_applets#Verifying_package_integrity_.28qcheck.29) can be used to check the existence, modification times and MD5 sums of all files of packages installed by portage. It is a fast program that requires no manual configuration in order to check the integrity of your host's installed files. qcheck is provided through the [app-portage/portage-utils](https://packages.gentoo.org/packages/app-portage/portage-utils) package.

To use qcheck, type in a console:

`user $``qcheck package-name`
Replace `package-name` in the example above with the desired package.

To check the integrity of all packages installed, enter:

`user $``qcheck`
### AIDE

AIDE is a Host-Based Intrusion Detection System (**HIDS**), a free alternative to Tripwire. HIDS are used to detect changes to important system configuration files and binaries, generally by making a unique cryptographic hash for the files to be checked and storing it in a secure place. On a regular basis (such as once a day), the stored "known-good" hash is compared to the one generated from the current copy of each file, to determine if that file has changed. HIDS are a great way to detect disallowed changes to your system, but they take a little work to implement properly and make good use of.

The AIDE ebuild now comes with a working default configuration file, a helper script and a crontab script. The helper script does a number of tasks for you and provides an interface that is a little more script friendly. To see all available options, try aide --help. To get started, all that needs to be done is aide -i and the crontab script should detect the database and send mails as appropriate every day. We recommend that you review the /etc/aide/aide.conf file and ensure that the configuration accurately reflects what is in place on the machine.

Please see [AIDE](https://wiki.gentoo.org/wiki/AIDE) for more details on configuration and usage.

Let's watch a full blown example:

**`/etc/aide/aide.conf`**

```
 TOPDIR
@@define TOPDIR /
@@endif
@@ifndef AIDEDIR
@@define AIDEDIR /etc/aide
@@endif
@@ifhost smbserv
@@define smbactive
@@endif
# The location of the database to be read.
database=file:@@{AIDEDIR}/aide.db
# The location of the database to be written.
database_out=file:aide.db.new
verbose=20
report_url=stdout
# Rule definition
All=R+a+sha1+rmd160
Norm=s+n+b+md5+sha1+rmd160
@@{TOPDIR} Norm
!@@{TOPDIR}etc/aide
!@@{TOPDIR}dev
!@@{TOPDIR}media
!@@{TOPDIR}mnt
!@@{TOPDIR}proc
!@@{TOPDIR}root
!@@{TOPDIR}sys
!@@{TOPDIR}tmp
!@@{TOPDIR}var/log
!@@{TOPDIR}var/run
!@@{TOPDIR}usr/portage
!@@{TOPDIR}var/db/repos/gentoo
@@ifdef smbactive
!@@{TOPDIR}etc/smb/private/secrets.tdb
@@endif
=@@{TOPDIR}home Norm
```
In the above example we specify with some macros where the topdir starts and where the AIDE directory is. AIDE checks the /etc/aide/aide.db file when checking for file integrity. But when updating or creating a new file it stores the information in /etc/aide/aide.db.new. This is done so it won't automatically overwrite the old db file. The option `report_URL` is not yet implemented, but the author's intention was that it should be able to e-mail or maybe even execute scripts.

Now there is some risk inherent with storing the db files locally, since the attacker will (if they know that AIDE is installed) most certainly try to alter the db file, update the db file or modify /usr/bin/aide. So you should create a CD or other media and put on it a copy of the .db file and the AIDE binaries.

You can find information at the [AIDE project page](http://aide.sourceforge.net).

### Snort

Snort is a Network Intrusion Detection System (NIDS). To install and configure it use the following examples.

**`/etc/conf.d/snort`**

```
SNORT_IFACE="eth0"
PIDFILE=/run/snort_eth0.pid
MODE="full"
NETWORK="192.168.0.0/24"
LOGDIR="/var/log/snort"
SNORT_CONF=/etc/snort/snort.conf
SNORT_OPTS="-q -D -u snort -d -l $LOGDIR -h $NETWORK -c $SNORT_CONF"
```
Copy /etc/snort/snort.conf.distrib to /etc/snort/snort.conf.

`root #``cd /etc/snort && cp snort.conf.distrib snort.conf`
You might need to comment out the blacklist and whitelist entries if no lists are created.

More information is at the [Snort website](https://www.snort.org/).

### Detecting malware with chkrootkit

HIDS like AIDE are a great way to detect changes to your system, but it never hurts to have another line of defense. chkrootkit is a utility that scans common system files for the presence of rootkits-software designed to hide an intruder's actions and allow him to retain his access-and scans your system for likely traces of key loggers and other "malware". While chkrootkit (and alternatives like rkhunter) are useful tools, both for system maintenance and for tracking an intruder after an attack has occurred, they cannot guarantee your system is secure.

The best way to use chkrootkit to detect an intrusion is to run it routinely from cron. To start, emerge [app-forensics/chkrootkit](https://packages.gentoo.org/packages/app-forensics/chkrootkit):

`root #``emerge --ask app-forensics/chkrootkit`
chkrootkit can be run from the command line by the command of the same name, or from cron with an entry such as this:
