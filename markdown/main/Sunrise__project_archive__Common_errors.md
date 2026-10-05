<!-- source: https://wiki.gentoo.org/wiki/Sunrise_(project_archive)/Common_errors | group: Gentoo Wiki (Main) | wiki-title: Sunrise (project archive)/Common errors -->
---
title: Sunrise (project archive)/Common errors
url: https://wiki.gentoo.org/wiki/Sunrise_(project_archive)/Common_errors
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-09-20"
fingerprint: "1654d06ae0fdbf9f"
license: CC BY-SA 4.0
---

# Sunrise (project archive)/Common errors

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**

## Making executable file non-executable

Git treats mode changes just like a change to the content of the file.

`user $``chmod -x example.file``user $``git add example.file`
## Rolling back committed changes

If your commit has not been pushed yet, you can use git reset to remove the last commit.

`user $``git reset --hard HEAD^`
If it has already been pushed, use git revert.

`user $``git revert <commit>`
