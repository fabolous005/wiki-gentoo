<!-- source: https://wiki.gentoo.org/wiki/Zero-day_bump_requests | group: Gentoo Wiki (Main) | wiki-title: Zero-day bump requests -->
---
title: Zero-day bump requests
url: https://wiki.gentoo.org/wiki/Zero-day_bump_requests
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-09-30"
fingerprint: "973dbd855ce102ac"
license: CC BY-SA 4.0
---

# Zero-day bump requests

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Suppose that you've eagerly been following an upstream project's schedule, and when you check their homepage, guess what? They just released a new version a few minutes ago! Most users would immediately rush over to Gentoo's Bugzilla to report the new version is available; please bump the existing version and add it to the Gentoo ebuild repository. However, this is exactly what you should *not* do. These requests are called zero-day (or 0-day) bump requests, as they are made the same day that a new version is released.

## Why wait at least 48 hours?

1. It is quite rude to demand that Gentoo developers drop everything they are doing just to add a new release that came out 15 minutes ago. Your zero-day bump request could (and probably *should*) be marked as INVALID, as developers have plenty of pressing issues to keep them busy.
2. **Developers are usually aware of pending new releases well in advance of users**, as they must follow upstream quite closely. Many times they are aware a new version is on its way. In many cases, they will have already opened a bug, or might even already added it to the Gentoo ebuild repository as a masked package. Some software is released periodically (the Linux kernel, Thunderbird, Firefox, and GCC) and the maintainers know very well about new releases. However, it requires considerable effort to prepare and test these version bumps. The only chance to accelerate the process is trying to help them.
3. **48 hours is a short time**: Assume the Gentoo developer knows about release from the first second, downloads the latest version right after work, makes some tests, has to fix something on the next day before uploading to the ebuild repository.
4. The **Gentoo mirrors take up to *24 hours* to synchronize**: this is half the amount of time that should be passed anyway! Take this into consideration. Watching the [commit log](https://gitweb.gentoo.org/repo/gentoo.git/log/) and syncing the ebuild repository via git is better method of staying on the bleeding edge.

## Exceptions

At least one exception is for security vulnerabilities. Please file bugs for those immediately.
