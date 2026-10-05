<!-- source: https://wiki.gentoo.org/wiki/Isync | group: Gentoo Wiki (Main) | wiki-title: Isync -->
---
title: isync
url: https://wiki.gentoo.org/wiki/Isync
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-26"
fingerprint: fe4785184ebff2ae
license: CC BY-SA 4.0
---

# isync

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


isync is a command line application which synchronizes mailboxes; currently Maildir and IMAP4 mailboxes are supported. New messages, message deletions and flag changes can be propagated both ways. isync is suitable for use in IMAP-disconnected mode.

## Installation

### USE flags


## Configuration

isync utilizes the `.mbsyncrc` file that lives in the home directory. An example of this looks like the following:

**`/home/larry/.mbsyncrc`**

## Usage

### Syncing mailboxes

To sync all mailboxes, run:

`user $``mbsync -a`
Channels: 2    Boxes: 44    Far: +0 \*0 #0 -0    Near: +0 \*0 #0 -0

### Emerge

`root #``emerge --ask net-mail/isync`
