<!-- source: https://wiki.gentoo.org/wiki/GitLab/Runner | group: Gentoo Wiki (Main) | wiki-title: GitLab/Runner -->
---
title: GitLab/Runner
url: https://wiki.gentoo.org/wiki/GitLab/Runner
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-02-24"
fingerprint: f25785ea1b8388fe
license: CC BY-SA 4.0
---

# GitLab/Runner

From Gentoo Wiki

\< [GitLab](https://wiki.gentoo.org/wiki/GitLab)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

GitLab Runner is an application that works with GitLab CI/CD to run jobs in a pipeline.

## Installation

`root #``emerge --ask dev-util/gitlab-runner`
## Configuration

`root #``gitlab-runner register`
Depending on the executor selected during registration, GitLab Runner may need to run as root or can run as an ordinary user.

To run as an ordinary user update:

FILE **`/etc/conf.d/gitlab-runner`**

```
runner_user="gitlab-runner"
```
## Services

### OpenRC

`root #````
rc-update add gitlab-runner default
```
`root #``rc-service gitlab-runner start`
### Systemd

`root #``systemctl enable --now gitlab-runner.service`
## Removal

`root #``emerge --ask --depclean --verbose dev-util/gitlab-runner`
