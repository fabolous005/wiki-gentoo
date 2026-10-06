<!-- source: https://wiki.gentoo.org/wiki/Gentoostats | group: Gentoo Wiki (Main) | wiki-title: Gentoostats -->
---
title: Gentoostats
url: https://wiki.gentoo.org/wiki/Gentoostats
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-19"
fingerprint: "8fcc188c81f18fc2"
license: CC BY-SA 4.0
---

# Gentoostats

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Deprecated article**

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

**Resources**

[**gentoostats**](https://soc.dev.gentoo.org/gentoostats/static/about.html) can collect several statistics from Gentoo machines. It was a [Google Summer of Code 2011 project](https://www.google-melange.com/gsoc/project/google/gsoc2011/vh4x0r/26001).

## Installation

Gentoostats is available through the *betagarden* [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository). The package is unstable, so add it to the /etc/portage/package.accept\_keywords file.

`root #``eselect repository enable betagarden``root #``emerge --ask gentoostats`
When first installed, the program created a unique identifier for your machine. Use this identifier later to view the statistics of your machine. The identifier is stored in /etc/gentoostats/auth.cfg.

## Configuration

### Sending statistics

Sending statistics from a machine is simple. You can control which statistics are sent by editing /etc/gentoostats/payload.cfg.

`root #``gentoostats-send`
## Usage

### Viewing statistics

The [website](https://soc.dev.gentoo.org/gentoostats/) can be used to view some aggregated statistics. To view the stats in JSON (instead of the regular HTML view), you have to add the "Accept:application/json" HTTP Header to your request. For instance:

`user $``curl -H "Accept:application/json" http://soc.dev.gentoo.org/gentoostats/arch`
You can also use the command-line interface to request JSON formatted statistics. For instance to view the statistics available on [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) or view what different architectures are used:

`root #``gentoostats-cli search -p gentoo-sources``root #``gentoostats-cli list arch`
The following options are available:

- search
  - *-c*, *--category*
  - *-p',* --package
  - *-v',* --version
  - *-r',* --repo
  - *--min\_hosts*
  - *--max\_hosts*
- list
  - *arch*
  - *feature*
  - *lang*
  - *mirror*
  - *repo*
  - *package*
  - *use*
