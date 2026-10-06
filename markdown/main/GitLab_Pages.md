<!-- source: https://wiki.gentoo.org/wiki/GitLab/Pages | group: Gentoo Wiki (Main) | wiki-title: GitLab/Pages -->
---
title: GitLab/Pages
url: https://wiki.gentoo.org/wiki/GitLab/Pages
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-03-23"
fingerprint: f378f56bd907fdce
license: CC BY-SA 4.0
---

# GitLab/Pages

[GitLab](https://wiki.gentoo.org/wiki/GitLab)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gitlab-Pages serves the static content generated through the Gitlab-CI machinery of a Gitlab-HQ server. It provides a convenient means of hosting statically generated websites; usually this including ones project documentation. It does this over both HTTP and HTTPS connections and may be exposed directly to the internet or proxied through load balancing server.

## Installation

To install this package one must enable the Gitlab overlay, described under [GitLab](https://wiki.gentoo.org/wiki/GitLab), and installed as follows :

`root #``emerge --ask dev-vcs/gitlab-pages`
## Configuration

The configuration file, /etc/conf.d/gitlab-pages, handles the applications settings.

At a minimum one must specify the following attributes for Gitlab Pages to operate :

gitlab-server

- The URL for ones Gitlab instance

api-secret-key

- An API key generated as described below

pages-domain

- The URL for ones Gitlab-Pages instance

listen-proxy or listen-http or listen-https

- The IP address and/or port Gitlab-Pages is to listen on

**`/etc/gitlab/gitlab.yml`**

**Gitlab-Pages**

```
pages:
  enabled: true
  ...
  secret_file: /opt/gitlab/gitlab/.gitlab-pages-secret
```
## Usage

Gitlb-Pages may be invoked directly, as follows, but this is best left to the OpenRC/SystemD service file.

`user $``gitlab-pages -listen-http=:8090 -pages-root=/opt/gitlab/gitlab/shared/pages api-secret-key=/opt/gitlab/gitlab/.gitlab_pages_secret -pages-domain=example.io -internal-gitlab-server=https://gitlab.example.com`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose dev-vcs/gitlab-pages`
