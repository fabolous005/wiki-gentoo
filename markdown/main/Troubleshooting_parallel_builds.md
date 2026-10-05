<!-- source: https://wiki.gentoo.org/wiki/Troubleshooting_parallel_builds | group: Gentoo Wiki (Main) | wiki-title: Troubleshooting parallel builds -->
---
title: Troubleshooting parallel builds
url: https://wiki.gentoo.org/wiki/Troubleshooting_parallel_builds
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-02-09"
fingerprint: "3e8cbc4a4e55ed4f"
license: CC BY-SA 4.0
---

# Troubleshooting parallel builds

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Parallel build fails, because the build system needs to work sequentially

### Parallel build fails in src\_compile

- Please report the bug upstream, if upstream is still active, as it was done for example in [bug #482542](https://bugs.gentoo.org/show_bug.cgi?id=482542).
- Please add comment with a bug id, so that others know the reason for the workaround. For example:

- GNU make 4.4 and newer now supports --shuffle. It will also report the shuffle seed used which allows reproducing build failures.

### Workaround for the user

The broken package entitled "foo" can still be installed with the following workaround:

`root #``MAKEOPTS="-j1" emerge app-misc/foo`
### Resources

- Tracker [bug #351559](https://bugs.gentoo.org/show_bug.cgi?id=351559)
