<!-- source: https://wiki.gentoo.org/wiki/Dokuwiki | group: Gentoo Wiki (Main) | wiki-title: Dokuwiki -->
---
title: Dokuwiki
url: https://wiki.gentoo.org/wiki/Dokuwiki
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-21"
fingerprint: a7a59842b3fb4ff3
license: CC BY-SA 4.0
---

# Dokuwiki

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

DokuWiki is a wiki system based on files instead of a database. The file-based approach makes it easier to use with file hosting and sychronization services. Access controls are provided by default, more than 50 languages are supported, and a range of plugins and themes ('templates') are available.

## Installation

### USE flags


### USE flags for
            [www-apps/dokuwiki](https://packages.gentoo.org/packages/www-apps/dokuwiki)
            
            DokuWiki is a simple to use Wiki aimed at a small company's documentation needs

### Emerge

`root #``emerge --ask www-apps/dokuwiki`
Unmasking the package (if necessary):

`root #``echo "www-apps/dokuwiki" > /etc/portage/package.accept_keywords/dokuwiki`
And now the emerge command should install [www-apps/mediawiki](https://packages.gentoo.org/packages/www-apps/mediawiki).
