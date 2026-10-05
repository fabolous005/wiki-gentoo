<!-- source: https://wiki.gentoo.org/wiki/Calcurse | group: Gentoo Wiki (Main) | wiki-title: Calcurse -->
---
title: calcurse
url: https://wiki.gentoo.org/wiki/Calcurse
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-13"
fingerprint: "67f07f99bb3bf8f2"
license: CC BY-SA 4.0
---

# calcurse

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


calcurse is a TUI calendar and scheduling application. It supports hooks, CalDAV, TODO items, and more.

## Installation

### USE flags


### Emerge

`root #``emerge --ask app-office/calcurse`
## Configuration

### CalDAV syncing

To sync from a CalDAV server, first ensure calcurse is built with the `caldav` USE flag enabled.

Next, is a basic configuration file for syncing with CalDav:

**`~/.config/calcurse/caldav/config`**
