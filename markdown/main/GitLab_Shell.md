<!-- source: https://wiki.gentoo.org/wiki/GitLab/Shell | group: Gentoo Wiki (Main) | wiki-title: GitLab/Shell -->
---
title: GitLab/Shell
url: https://wiki.gentoo.org/wiki/GitLab/Shell
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-02-24"
fingerprint: "72d155cddb7bfcdf"
license: CC BY-SA 4.0
---

# GitLab/Shell

From Gentoo Wiki

\< [GitLab](https://wiki.gentoo.org/wiki/GitLab)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gitlab-Shell provides a limited interface for git supporting push/pull operations.

## Installation

To install this package one must enable the Gitlab overlay, described under [GitLab](https://wiki.gentoo.org/wiki/GitLab), and installed as follows :

`root #``emerge --ask dev-vcs/gitlab-shell`
## Configuration

The configuration file, /opt/gitlab-shell/config.yaml, handles the applications settings.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose dev-vcs/gitlab-shell`
