<!-- source: https://wiki.gentoo.org/wiki/Pkgcheck | group: Gentoo Wiki (Main) | wiki-title: Pkgcheck -->
---
title: pkgcheck
url: https://wiki.gentoo.org/wiki/Pkgcheck
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-17"
fingerprint: fa99ec8f0481b3c8
license: CC BY-SA 4.0
---

# pkgcheck

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**pkgcheck** is a [pkgcore](https://wiki.gentoo.org/wiki/Pkgcore)-based QA utility for ebuild repos.

## Installation

### Emerge

`root #``emerge --ask dev-util/pkgcheck`
## Configuration

Upstream's documentation [describes](https://pkgcore.github.io/pkgcheck/man/pkgcheck.html#config-file-support) the config file format and locations.

For example, a configuration file for the current user:

**`~/.config/pkgcheck/pkgcheck.conf`**

```
[DEFAULT]
# Enable PerlCheck to the default list of checks (it's not enabled by default upstream)
checks = +PerlCheck
# Disable two noisy keywords (checks)
keywords = -UnstableOnly,-PotentialStable
[gentoo]
# Enable network checks when scanning in the 'gentoo' repository with a timeout of 15 seocnds
# Examples: check for HOMEPAGE which 404s, or for a HTTP -> HTTPS redirect
net =
timeout = 15
```
## Usage

The upstream repository [covers usage](https://github.com/pkgcore/pkgcheck#usage) briefly.

pkgcheck has the following subcommands:

- pkgcheck scan - this is the most common command needed: scans the current directory (can take arguments instead) for QA issues
  - pkgcheck scan --net - enables checks that require network access (can fail if scanning lots of packages).
  - pkgcheck scan --commits - same as pkgcheck scan but restricted to changes in local unpushed commits. Enables additional checks which require more context (like commit message format/length, pushing straight-to-stable, etc).
- pkgcheck show - this lists all possible results (try pkgcheck show --checks etc to find things to disable or change severity of locally if desired)

## Examples

The most common pattern is to enter a repository, navigate to a package (using [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) as an example), and run pkgcheck scan, like so:

`user $````
cd ~/git/gentoo/sys-kernel/gentoo-sources
```
`user $````
pkgcheck scan
```
sys-kernel/gentoo-sources
  DroppedKeywords: version 5.18.11: m68k

To scan packages per maintainer, one could try:

`user $````
cd ~/git/gentoo
```
`user $````
git grep -l "python@gentoo.org" */*/metadata.xml | cut -d/ -f1-2 | pkgcheck scan -
```
app-arch/brotli
  PythonCompatUpdate: version 1.0.9-r3: PYTHON\_COMPAT update available: python3\_11
  StableRequest: version 1.0.9-r4: slot(0) no change in 45 days for unstable keywords: \[ \~amd64, \~arm, \~arm64, \~hppa, \~ppc, \~ppc64, \~sparc, \~x86 \]
app-text/cssmin
  PythonCompatUpdate: version 0.2.0: PYTHON\_COMPAT update available: python3\_11

Or to specifically only check for packages which may be due a [stabilization request](https://wiki.gentoo.org/wiki/Stable_request):

`user $````
cd ~/git/gentoo
```
`user $````
git grep -l "python@gentoo.org" */*/metadata.xml | cut -d/ -f1-2 | pkgcheck scan -k StableRequest -
```
app-arch/brotli
  StableRequest: version 1.0.9-r4: slot(0) no change in 45 days for unstable keywords: \[ \~amd64, \~arm, \~arm64, \~hppa, \~ppc, \~ppc64, \~sparc, \~x86 \]

To get only specific errors:

`user $````
pkgcheck scan --keywords DoubleEmptyLine
```
app-admin/agru
  DoubleEmptyLine: version 0.1.10: ebuild has unneeded empty line on line: 13
app-admin/himitsu
  DoubleEmptyLine: version 0.7: ebuild has unneeded empty line on line: 20
  DoubleEmptyLine: version 0.8: ebuild has unneeded empty line on line: 20
  DoubleEmptyLine: version 9999: ebuild has unneeded empty line on line: 20

## Troubleshooting

### error: repos.conf: default repo 'gentoo' is undefined or invalid

See [bug #730502](https://bugs.gentoo.org/show_bug.cgi?id=730502). This is caused by having a /etc/portage/repos.conf defined without mentioning the gentoo repository.

If /etc/portage/repos.conf is a directory:

`root #``cp /usr/share/portage/config/repos.conf /etc/portage/repos.conf/gentoo.conf`
Alternatively, if it's a file:

`root #``cat /usr/share/portage/config/repos.conf >> /etc/portage/repos.conf`
### failed retrieving origin/HEAD commit hash for git repo

See the upstream [documentation](https://pkgcore.github.io/pkgcheck/man/pkgcheck.html#git) on git support.

Run git remote set-head origin master.

## See also

- [Standard git workflow](https://wiki.gentoo.org/wiki/Standard_git_workflow) — describing a **modern git workflow** for contributing to Gentoo, with [pkgcheck] and [pkgdev](https://wiki.gentoo.org/wiki/Pkgdev)
- [Gentoo git workflow](https://wiki.gentoo.org/wiki/Gentoo_git_workflow) — outlines some rules and best-practices regarding the typical workflow for ebuild developers using git.
- [pkgdev](https://wiki.gentoo.org/wiki/Pkgdev) — a collection of tools for Gentoo development.
