<!-- source: https://wiki.gentoo.org/wiki/Logrotate | group: Gentoo Wiki (Main) | wiki-title: Logrotate -->
---
title: Logrotate
url: https://wiki.gentoo.org/wiki/Logrotate
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-20"
fingerprint: f404fb7e97858083
license: CC BY-SA 4.0
---

# Logrotate

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Logrotate** is a tool to periodically rotate (archive), delete, and optionally compress and/or mail historic log files. Logrotate ships with, and is typically invoked by a /etc/cron.daily cron job.

### USE flags


| [+cron](https://packages.gentoo.org/useflags/+cron) | Installs cron file | 
| [acl](https://packages.gentoo.org/useflags/acl) | Add support for Access Control Lists | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

## Installation

### Emerge

`root #``emerge --ask app-admin/logrotate`
## Introduction

Logrotate can be used to ensure logs are retained based on a defined policy. This policy can be based on file age, size, and number of total similar files.

Proper log storage is important, for a variety of reasons:

- Readability - If logs are disorganized, they become harder to use.
- Security - Logs are essential for incident responses, poorly organized and incomplete logs can make this more difficult or impossible.
- Integrity - If logs are managed poorly, data could be lost or overwritten.

## Configuration

### Files

- /etc/logrotate.conf - The daemon's configuration file.
- /etc/logrotate.d - The directory containing configuration files installed by other services.

#### Configure daily rotation

By default, logrotate is configured to rotate logs weekly, this can be changed to daily rotation with:

**`/etc/logrotate.conf`**

**Switch to daily log rotation**

#### Portage logrotate module

To rotate log files created by portage:

**`/etc/logrotate.d/portage`**

## Usage

Logrotate is typically called by a cron job, but can be manually used with:

`root #``logrotate --verbose /etc/logrotate.conf`
