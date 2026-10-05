<!-- source: https://wiki.gentoo.org/wiki/Jira | group: Gentoo Wiki (Main) | wiki-title: Jira -->
---
title: Jira
url: https://wiki.gentoo.org/wiki/Jira
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-11-22"
fingerprint: "924b094a25df2983"
license: CC BY-SA 4.0
---

# Jira

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Atlassian Jira is a ticket based tracking application. This document links to the relevant information at the software suppliers webpage and explains the integration into OpenRC.

## Dependencies

a supported database (see [https://confluence.atlassian.com/adminjiraserver/supported-platforms-938846830.html](https://confluence.atlassian.com/adminjiraserver/supported-platforms-938846830.html))

Hint: 
[MariaDB](https://wiki.gentoo.org/wiki/MariaDB) works fine, though it is not officially supported.
Use mysql-connector-java-5.1.47.jar and mysql-connector-java-5.1.47-bin.jar for the connectors.

## Installation

Download the official tarball from atlassian and configure accordingly.
[https://confluence.atlassian.com/adminjiraserver/installing-jira-applications-on-linux-938846841.html](https://confluence.atlassian.com/adminjiraserver/installing-jira-applications-on-linux-938846841.html)

### Files

- /opt/atlassian/jira - Application
- /var/atlassian/application-data/jira/ - Configuration

## OpenRC

The automatically installed script will fail to start at boot, at it does not wait for the database or apache to start. Therefore change it accordingly:

Replace the content of the file to match the following

**`/etc/init.d/jira`**

**Modify startup script**
