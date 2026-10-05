<!-- source: https://wiki.gentoo.org/wiki/Tracking_changes_to_%22/etc%22_with_git_for_backup | group: Gentoo Wiki (Main) | wiki-title: Tracking changes to "/etc" with git for backup -->
---
title: Tracking changes to "/etc" with git for backup
url: https://wiki.gentoo.org/wiki/Tracking_changes_to_%22/etc%22_with_git_for_backup
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-03"
fingerprint: b604f72d80e7bfd2
license: CC BY-SA 4.0
---

# Tracking changes to "/etc" with git for backup

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Tracking the /etc directory with git is a common procedure within the Gentoo community - although tools exist to solve configuration file change management (such as [dispatch-conf](https://wiki.gentoo.org/wiki/Dispatch-conf) or [etckeeper](https://wiki.gentoo.org/wiki/Etckeeper)).

## Quick and dirty

The gist of dirty configuration file management with git is as follows:

1. Change to the /etc directory:
  - `root #``cd /etc`
  - Initialize a new git repository:
  - `root #``git init`
  - Commit initial the initial state and any further known good revisions:
  - `root #``git add * # add all files not in git to the .gitignore file``root #``git commit -m 'required'`
2. To investigate revisions:
  - `root #``git rev-list #display all revisions`
3. To checkout/load a specific revision:
  - `root #``git checkout #<(insert a revision hash here>`
4. To revert unwanted changes:
  - `root #``git reset --hard # undo all changes from revision checked out`
  - Alternatively:
    - `root #``git reset``root #``git checkout .`

## See also

- [dispatch-conf](https://wiki.gentoo.org/wiki/Dispatch-conf) — a utility included with [Portage](https://wiki.gentoo.org/wiki/Portage), used to safely and conveniently manage configuration files after package updates.
- [etckeeper](https://wiki.gentoo.org/wiki/Etckeeper) — a collection of tools to store /etc in a VCS (version control) repository, to keep a backup of changes to system configuration files
