<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Overriding_environment_variables_per_package | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Overriding_environment_variables_per_package -->
---
title: Knowledge Base:Overriding environment variables per package
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Overriding_environment_variables_per_package
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-02-28"
fingerprint: bef83e39aea31152
license: CC BY-SA 4.0
---

# Knowledge Base:Overriding environment variables per package

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

The default environment variables during package builds are declared in /etc/portage/make.conf, but sometimes it is desirable to override a particular variable (like `CFLAGS` or `PORTAGE_TMPDIR`) for a single package.

## Environment

This article is applicable to all Gentoo Linux installations.

## Analysis

Portage supports per-package environment settings through /etc/portage/package.env and the /etc/portage/env subdirectories.

- Inside the /etc/portage/env directory create files that have the same syntax as make.conf and give it a sensible name. The filename will be used as the key to override settings.
- Inside the /etc/portage/package.env file define which of these files will be used on a per-package basis

## Resolution

First, create the /etc/portage/env directory structure. This one will host the files that contain the variable declarations to use:

`root #``mkdir -p /etc/portage/env`
Next, create the file(s) needed to override the settings. For instance, presume two specific overrides are needed: one is for a package where a slight tweak in the `[CFLAGS](https://wiki.gentoo.org/wiki/GCC_optimization)` and `[FEATURES](https://wiki.gentoo.org/wiki/FEATURES)` variables results in debug symbols to be built and the other is to update the `PORTAGE_TMPDIR` value for a package that has a huge build storage requirement:

`root #````
echo 'CFLAGS="${CFLAGS} -ggdb"' > /etc/portage/env/debug-cflags
```
`root #````
echo 'FEATURES="nostrip"' >> /etc/portage/env/debug-cflags
```
`root #``echo 'PORTAGE_TMPDIR="/home/build/tmp"' > /etc/portage/env/large-tmpdir`
Finally, create or edit /etc/portage/package.env and point towards these override files for the specific packages:

**`/etc/portage/package.env`**

**Overriding environment variables per package**
