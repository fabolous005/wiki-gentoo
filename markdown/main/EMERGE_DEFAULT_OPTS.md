<!-- source: https://wiki.gentoo.org/wiki/EMERGE_DEFAULT_OPTS | group: Gentoo Wiki (Main) | wiki-title: EMERGE DEFAULT OPTS -->
---
title: EMERGE_DEFAULT_OPTS
url: https://wiki.gentoo.org/wiki/EMERGE_DEFAULT_OPTS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-25"
fingerprint: "288ff62a96af9d0c"
license: CC BY-SA 4.0
---

# EMERGE\_DEFAULT\_OPTS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**EMERGE\_DEFAULT\_OPTS** is a variable for [Portage](https://wiki.gentoo.org/wiki/Portage) that defines entries to be appended to the emerge command line.

`EMERGE_DEFAULT_OPTS` allows for parallel emerge operations through the `--jobs`  and `N``--load-average`  options. `X.YEMERGE_DEFAULT_OPTS` is used by Portage to reference system load, or load average, and limit how many packages are built at a time.

## Common use cases

### Parallel builds

The `--jobs`  argument (short form: `N``-j`) will emerge `NN` jobs at a time. Providing no arguments to `-j` floods the processor with as many jobs as possible. This is not recommended. Values for `N` should be no more than 2GB of ram per processor core. Packages can reach this constraint.

To run up to three build jobs simultaneously:

**`/etc/portage/make.conf`**

**Enabling 3 parallel package builds**

```
EMERGE_DEFAULT_OPTS="--jobs 3"
```
When used with `--load-average`  (short form: `X.Y``-l`), emerge tries to limit the load average of the system below the floating point number `X.YX.Y`. The running jobs are again limited by the `--jobs` parameter.

The load average value is the same as displayed by top or uptime, and for an `N`-core system, a load average of `N`.0`X.Y`=`N`\*0.9

`MAKEOPTS` and `EMERGE_DEFAULT_OPTS` are suited for long emerges including multiple source code files and make the most of the `--jobs` parameter. They should be used with caution and be commented out when they cause emerge errors.

## See also

- [MAKEOPTS](https://wiki.gentoo.org/wiki/MAKEOPTS) — a variable that defines and limits how many parallel make jobs can be launched from Portage.
- [Knowledge Base:Emerge out of memory](https://wiki.gentoo.org/wiki/Knowledge_Base:Emerge_out_of_memory)
- [Portage niceness](https://wiki.gentoo.org/wiki/Portage_niceness) — describes some configuration options available for system administrators to help manage [Portage](https://wiki.gentoo.org/wiki/Portage)'s resource usage.
