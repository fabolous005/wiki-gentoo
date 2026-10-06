<!-- source: https://wiki.gentoo.org/wiki/Calcurse | group: Gentoo Wiki (Main) | wiki-title: Calcurse -->
---
title: calcurse
url: https://wiki.gentoo.org/wiki/Calcurse
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-13"
fingerprint: "61f04558bf2bddfa"
license: CC BY-SA 4.0
---

# calcurse

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


calcurse is a TUI calendar and scheduling application. It supports hooks, CalDAV, TODO items, and more.

## Installation

### USE flags


### USE flags for
            [app-office/calcurse](https://packages.gentoo.org/packages/app-office/calcurse)
            
            A text-based calendar and scheduling application

### Emerge

`root #``emerge --ask app-office/calcurse`
## Configuration

### CalDAV syncing

To sync from a CalDAV server, first ensure calcurse is built with the `caldav` USE flag enabled.

Next, is a basic configuration file for syncing with CalDav:

FILE **`~/.config/calcurse/caldav/config`**

```
[General]
Hostname = example.com:8443
Path = /calendars/larry/default/
AuthMethod = basic
InsecureSSL = No
HTTPS = Yes
SyncFilter = cal,todo
DryRun = no
[Auth]
Username = larry@example.com
Password = SuperSecretPassword
```
