<!-- source: https://wiki.gentoo.org/wiki//etc/portage/modules | group: Gentoo Wiki (Main) | wiki-title: /etc/portage/modules -->
---
title: "/etc/portage/modules"
url: https://wiki.gentoo.org/wiki//etc/portage/modules
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-12"
fingerprint: "6857d76677b5f9d8"
license: CC BY-SA 4.0
---

# /etc/portage/modules

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **/etc/portage/modules** file can be used to override the metadata cache implementation. In practice, `portdbapi.auxdbmodule` is the only variable that the user will want to override.

**`/etc/portage/modules`**

**Modules example**

After changing the `portdbapi.auxdbmodule` setting, it may be necessary to transfer or regenerate metadata cache. Users of the rsync tree need to run emerge --metadata if they have enabled `FEATURES="metadata-transfer"` in [make.conf](https://wiki.gentoo.org/wiki/Make.conf).

In order to regenerate metadata for repositories not distributing pre-generated metadata cache, run emerge --regen (see [emerge](https://wiki.gentoo.org/wiki/Emerge)).

When using something like the sqlite module and want to keep all metadata in that format alone (useful for querying), enable `FEATURES="metadata-transfer"` in make.conf.

## Troubleshooting

### equery

Problem: equery u portage stopped working (`UnicodeDecodeError: 'ascii'...`), but equery h qt5 still works.

Solution: cleanup Portage profile's deps:

`root #````
rm -rf /var/db/repos/gentoo/profiles/desc
```
`root #````
emerge --metadata
```
