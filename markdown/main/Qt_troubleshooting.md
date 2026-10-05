<!-- source: https://wiki.gentoo.org/wiki/Qt/troubleshooting | group: Gentoo Wiki (Main) | wiki-title: Qt/troubleshooting -->
---
title: Qt/troubleshooting
url: https://wiki.gentoo.org/wiki/Qt/troubleshooting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-22"
fingerprint: fa10125795869fcf
license: CC BY-SA 4.0
---

# Qt/troubleshooting

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a list of frequently asked questions about the Qt framework.

## Why do I get blockers when trying to emerge Qt?

Gentoo uses split ebuilds of the various components of Qt to allow finer-grained control of dependencies from other packages and reduced compilation time for revision bumps or USE changes. However, despite there being separate ebuilds all those components must be of the same version, which means they must all be upgraded together.

Common causes:

- If some of the updated version packages are keyworded but others are not, those blockers appear.
- Another source of Qt blocks is incompatible USE flag combinations; the portage output say which those are.

### Solving the block

1. Check /etc/portage for stale `dev-qt/*` entries (e.g. tightly versioned =dev-qt/foo-1.2.3 entries). Especially on stable systems. Either no Qt should be in package.accept\_keywords or **all** of Qt needs to be in there. Optionally try [**checkworldfile**](https://wiki.gentoo.org/wiki/User:Vaukai/checkworldfile#Keep_it_clean) ([alternative version](https://wiki.gentoo.org/wiki/User:Luttztfz/checkworldfile)).
2. Check /var/lib/portage/world for any packages not personally needed. Libraries, dependencies, etc should **not** be in the world file.
3. Run emerge -p -uvDU @world --backtrack=9999. If this isn't sufficient, continue on.
4. Run emerge -pev @world --backtrack=0
  1. For any lines that say "there are no ebuilds to satisfy ...", there are a few options:
    1. emerge --deselect ... for any packages which are no longer desired
    2. Fix the issue preventing them being installed (e.g. add them to package.accept\_keywords)
  2. Run until there are no fixable issues
5. Try emerge -p -uvDU @world --backtrack=9999 again.
6. Failing that, try emerge -p -uvDU @world --ignore-built-slot-operator-deps=y. This should reveal some more output and allow further diagnosis of the problem. It's best not to use this option to do the upgrade (without pretend) unless truly needed. If it is used for the full upgrade, one **must** run emerge -a -uvDU @world afterwards.
