<!-- source: https://wiki.gentoo.org/wiki/GitLab/Gitaly | group: Gentoo Wiki (Main) | wiki-title: GitLab/Gitaly -->
---
title: GitLab/Gitaly
url: https://wiki.gentoo.org/wiki/GitLab/Gitaly
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-02-24"
fingerprint: "5bc9b448fd70ec41"
license: CC BY-SA 4.0
---

# GitLab/Gitaly

From Gentoo Wiki

\< [GitLab](https://wiki.gentoo.org/wiki/GitLab)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gitaly provides the RPC interface for Gitlab.

## Installation

The package is installed as a art of the Gitlab installation.

USE flags Gitaly

```
U I
+ + gitaly_git    : Use the Git version provided by Gitaly
```
## Configuration

The configuration file, /etc/gitlab-gitaly/config.toml, handles the applications settings.

## See also

- [Gitlab](https://wiki.gentoo.org/wiki/Gitlab) — how to set up a self hosted **GitLab** instance.
