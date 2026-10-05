<!-- source: https://wiki.gentoo.org/wiki/Portage/Help/Upgrading_Portage | group: Gentoo Wiki (Main) | wiki-title: Portage/Help/Upgrading Portage -->
---
title: Portage/Help/Upgrading Portage
url: https://wiki.gentoo.org/wiki/Portage/Help/Upgrading_Portage
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-21"
fingerprint: b629e77f8c8a9588
license: CC BY-SA 4.0
---

# Portage/Help/Upgrading Portage

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Background

Portage is Gentoo's main package manager.

For many years, Portage would always recommend updating itself before anything else. This ended up causing various problems when there was a "global Python target change" pending (e.g. `PYTHON_TARGETS` in the profiles changed from `PYTHON_TARGETS="python3_8"` to `PYTHON_TARGETS="python3_9"`). Its behavior was later updated in [bug #722748](https://bugs.gentoo.org/show_bug.cgi?id=722748) to silence the message when such a change is detected.

### Summary

Please do read the rest of this page in detail if you're struggling, but the gist is:

1. Try to do a world upgrade, obviously, but that's probably failed given you're here.
2. emerge --sync (see below for why this is so important, but make a note of what tree state you were in beforehand from emerge --info).
3. Try to upgrade Portage by itself: emerge --verbose --pretend --oneshot sys-apps/portage
4. If that fails due to Python related bits, try: PYTHON\_TARGETS="python3\_8" emerge --verbose --pretend --oneshot sys-apps/portage
5. If *that* fails due to Python related bits, try: PYTHON\_TARGETS="python3\_8" USE="-rsync-verify" emerge --ask -v1 sys-apps/portage
6. If *that* fails, use a [rescue Portage](https://wiki.gentoo.org/wiki/Project:Portage/Fixing_broken_portage) and try to upgrade world with it.

### The current version of portage supports EAPI '7'

Portage 3.0.20 and above supports EAPI 8, but earlier versions can't read ebuilds with this [newer](https://wiki.gentoo.org/wiki/Project:Package_Manager_Specification#EAPI_life_cycle) format specification.

#### Problem

The standard error looks like this when trying to upgrade `@world`:

`root #``emerge -p -uvDU @world`
\[...\]
!!! All ebuilds that could satisfy ">=dev-perl/HTTP-Message-6.0.0" have been masked.
!!! One of the following masked packages is required to complete your request:
- dev-perl/HTTP-Message-6.320.0::gentoo (masked by: EAPI 8)
The current version of portage supports EAPI '7'. You must upgrade to a
newer version of portage before EAPI masked packages can be installed.
(dependency required by "dev-perl/libwww-perl-6.530.0::gentoo" \[ebuild\])
(dependency required by "dev-perl/XML-Parser-2.460.0::gentoo" \[ebuild\])
(dependency required by "dev-util/intltool-0.51.0-r2::gentoo" \[installed\])
(dependency required by "xfce-base/thunar-4.16.8::gentoo" \[ebuild\])
(dependency required by "@selected" \[set\])
(dependency required by "@world" \[argument\])
For more information, see the MASKED PACKAGES section in the emerge
man page or refer to the Gentoo Handbook.

#### Solution

#### Bash

See [Project:Portage/Fixing\_broken\_portage#Possible\_failures\_with\_older\_Bash](https://wiki.gentoo.org/wiki/Project:Portage/Fixing_broken_portage#Possible_failures_with_older_Bash) if hitting a Bash-related error.

##### PYTHON\_TARGETS hack

First, run emerge --pretend -v1 sys-apps/portage. If you see output like the following:

`root #``emerge --pretend -v1 sys-apps/portage````
[...]
Calculating dependencies... done!
[ebuild   R    ] sys-apps/portage-3.0.18::gentoo  USE="(ipc) native-extensions rsync-verify xattr -apidoc -build -doc -gentoo-dev (-selinux) -test" PYTHON_TARGETS="-python3_10 -pypy3 -python3_8* python3_9*" 0 KiB
Total: 1 package (1 reinstall), Size of downloads: 0 KiB
!!! Multiple package instances within a single package slot have been pulled
!!! into the dependency graph, resulting in a slot conflict:
sys-apps/portage:0
  (sys-apps/portage-3.0.22:0/0::gentoo, ebuild scheduled for merge) USE="(ipc) native-extensions rsync-verify xattr -apidoc -build -doc -gentoo-dev (-selinux) -test" ABI_X86="(64)" PYTHON_TARGETS="-python3_10 -pypy3 -python3_8 python3_9" pulled in by
    sys-apps/portage (Argument)
  (sys-apps/portage-3.0.22:0/0::gentoo, installed) USE="(ipc) native-extensions rsync-verify xattr -apidoc -build -doc -gentoo-dev (-selinux) -test" ABI_X86="(64)" PYTHON_TARGETS="-python3_10 python3_8 -python3_9 -pypy3" pulled in by
    >=sys-apps/portage-3.0.18[python_targets_pypy3(-)?,python_targets_python3_8(-)?,python_targets_python3_9(-)?,python_targets_python3_10(-)?] required by (app-portage/repoman-3.0.3-r1:0/0::gentoo, installed) USE="" ABI_X86="(64)" PYTHON_TARGETS="-python3_10 python3_8 -python3_9 -pypy3"
    sys-apps/portage[python_targets_pypy3(-)?,python_targets_python3_8(-)?,python_targets_python3_9(-)?,python_targets_python3_10(-)?] required by (app-portage/gentoolkit-0.5.1-r1:0/0::gentoo, installed) USE="-test" ABI_X86="(64)" PYTHON_TARGETS="-python3_10 python3_8 -python3_9 -pypy3"
[...]
```
then it means that Python 3.8 is currently enabled, but will be disabled (say, due to a [change](https://www.gentoo.org/support/news-items/2021-05-05-python3-9.html) in the profile defaults).

As a hack, we can force Portage to just update itself and minimize its dependencies by choosing the *old* Python version which was in use and turning off an optional feature (rsync-verify -- but that's why the sync was performed first) so that we get a version which supports EAPI 8.

Try the following:

`root #``PYTHON_TARGETS="python3_8" USE="-rsync-verify" emerge --ask -v1 sys-apps/portage`
\[...\]
Calculating dependencies... done!
\[ebuild   U    \] sys-apps/portage-3.0.22::gentoo \[3.0.18::gentoo\] 0 KiB
 USE="(ipc) native-extensions -rsync-verify\* xattr -apidoc -build -doc -gentoo-dev (-selinux) -test" PYTHON\_TARGETS="-python3\_10 -pypy3 python3\_8 -python3\_9" 0 KiB
Total: 1 package (1 upgrade), Size of downloads: 0 KiB

Then proceed with trying to complete a world upgrade.

### AttributeError: 'NoneType' object has no attribute 'ebuild'

The Portage version being used is buggy and it has broken after syncing the tree to a newer state. See the [rescue Portage](https://wiki.gentoo.org/wiki/Project:Portage/Fixing_broken_portage) page.

The upstream fix is [here](https://gitweb.gentoo.org/proj/portage.git/commit/?id=0b78b4a90657ef582a68258f27eaf521dfb42038), which you can apply by hand to the /usr/lib\*/\*/site-packages/\_emerge/EbuildMetadataPhase.py file.
