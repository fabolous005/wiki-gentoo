<!-- source: https://wiki.gentoo.org/wiki/Sunrise_(project_archive)/Reviewing | group: Gentoo Wiki (Main) | wiki-title: Sunrise (project archive)/Reviewing -->
---
title: Sunrise (project archive)/Reviewing
url: https://wiki.gentoo.org/wiki/Sunrise_(project_archive)/Reviewing
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-09-20"
fingerprint: "2575f4c08cddefda"
license: CC BY-SA 4.0
---

# Sunrise (project archive)/Reviewing

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**

## Initial setup

This procedure only needs to be done once.

1. Clone the sunrise repo as described in [how to commit](https://wiki.gentoo.org/wiki/Sunrise/How_to_commit)
2. Add the reviewed repo as second remote repo

## Review procedure

1. Fetch changes from both remotes `user $``git pull --rebase origin``user $``git fetch reviewed`
2. Review changes `user $``git log reviewed/master..origin/master`
3. Run the review script `user $``scripts/review`
