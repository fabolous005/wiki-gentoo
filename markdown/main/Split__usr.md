<!-- source: https://wiki.gentoo.org/wiki/Split_/usr | group: Gentoo Wiki (Main) | wiki-title: Split /usr -->
---
title: Split /usr
url: https://wiki.gentoo.org/wiki/Split_/usr
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-29"
fingerprint: "1354d4fbb0704dca"
license: CC BY-SA 4.0
---

# Split /usr

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

The split-usr or split /usr layout refers to the legacy layout, where /bin, /sbin, /lib, and /lib64 are independent directories, separate from /usr/bin, /usr/sbin, /usr/lib and /usr/lib64. The opposite (now typical) configuration is the merged-usr layout, where the contents of the directories under / are migrated to /usr and the directories are replaced with symlinks to their /usr counterparts.

Splitting binaries and libraries between root and /usr was historically common to enable spreading disk usage across multiple drives. With modern disk sizes and file systems, this is not typically an issue.

In modern systems, split-usr can cause incompatibility issues, e.g. when a script calls on /bin/bash when bash is located under /usr/bin/bash or the inverse. Starting from systemd-255, merged-usr layout is **mandatory** for systemd [profiles](<https://wiki.gentoo.org/wiki/Profile_(Portage)>)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

## Quirks

### elogind with Dracut

The path to fully support [elogind on Dracut](https://wiki.gentoo.org/wiki/Dracut#Elogind) with split-usr layout starting from sys-auth/elogind-255.5 is:

**`/etc/dracut.conf.d/elogind.conf`**

```
install_items+=" /usr/lib/elogind/elogind-uaccess-command "
```
## See also

- [Merge-usr](https://wiki.gentoo.org/wiki/Merge-usr) — a script which may be used to migrate a system from the legacy "[split-usr]" layout to the newer "merged-usr" layout as well as the "sbin merge".
