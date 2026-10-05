<!-- source: https://wiki.gentoo.org/wiki/Portage/Help/Upgrade_went_wrong | group: Gentoo Wiki (Main) | wiki-title: Portage/Help/Upgrade went wrong -->
---
title: Portage/Help/Upgrade went wrong
url: https://wiki.gentoo.org/wiki/Portage/Help/Upgrade_went_wrong
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-02"
fingerprint: ea01535fcbbe9dc6
license: CC BY-SA 4.0
---

# Portage/Help/Upgrade went wrong

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Sometimes, maybe an upgrade doesn't go well and you don't know what to do, despite following [these excellent tips](https://wiki.gentoo.org/wiki/User:Sam/Portage_help/Maintaining_a_Gentoo_system) to keep your system in good shape: this page is for when that happens.

General tips:

1. Read your news items first.
2. Do share *full* output of all commands in support channels: don't cut off bits.
3. Never unmerge anything unless told to by a news item or a support channel **as a last resort**. Unmerging things is not a normal operation to have to do.
4. Check your world file for [junk](https://wiki.gentoo.org/wiki/User:Sam/Portage_help/Maintaining_a_Gentoo_system#World_file_hygiene).
5. Use --keep-going=y on emerge invocations.
6. Start from the **bottom upwards**: the most fatal error is usually right down the bottom and causes noise above it. Resolving the bottommost issues usually sorts out the rest.

### Long list of dependency conflicts

As above, start from the bottom and work upwards. Resolve *one* issue at a time then run emerge again.

Rough steps:

1. Check your world file (and follow the *General tips* listed above **first**).
2. Check /etc/portage for stale e.g. `dev-qt/*` (same for dev-haskell, games) entries (e.g. tightly versioned =dev-qt/foo-1.2.3 entries).
  1. Either no Qt should be in package.accept\_keywords or **all** of Qt needs to be in there. Ditto Haskell.
  2. Make sure there's no too-specific entries like =games-misc/foo-1.2.3 which prevents it being upgraded.
3. If there's something obvious at the bottom (e.g. *autounmask* / package.use / package.accept\_keywords changes suggested), act on it.
4. If there's *nothing* obvious at the bottom changes suggested, try more backtracking first: emerge -p -uvDU @world --backtrack=9999.
5. If, at this point, the entire thing looks like a huge wall with nothing easy, try: emerge --pretend -ev --backtrack=0. This often leads to emerge shaking out missing/outdated package.accept\_keywords entries.
  1. For any lines that say "there are no ebuilds to satisfy ...", there are a few options:
    1. emerge --deselect ... for any packages which are no longer desired
    2. Fix the issue preventing them being installed (e.g. add them to package.accept\_keywords)
  2. Run until there are no fixable issues
6. Try emerge -p -uvDU @world --backtrack=9999 again.
7. Failing that, try emerge -p -uvDU @world --ignore-built-slot-operator-deps=y. This should reveal some more output and allow further diagnosis of the problem. It's best not to use this option to do the upgrade (without pretend) unless truly needed. If it is used for the full upgrade, one **must** run emerge -a -uvDU @world afterwards.

(A common issue here is overly restrictive package.accept\_keywords entries for e.g. games, Haskell, ...)
