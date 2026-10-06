<!-- source: https://wiki.gentoo.org/wiki/Gay | group: Gentoo Wiki (Main) | wiki-title: Gay -->
---
title: gay
url: https://wiki.gentoo.org/wiki/Gay
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-22"
fingerprint: fb97953c1e3339fc
license: CC BY-SA 4.0
---

# gay

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**gay** is a way to colour your text / terminal to be more gay.

## Installation

### Emerge

[games-misc/gay::guru](https://github.com/gentoo-mirror/guru/tree/master/games-misc/gay) is available in the [GURU](https://wiki.gentoo.org/wiki/GURU) repository.

First, enable the repository:

`root #``eselect repository enable guru`
Sync the repository:

`root #``emerge --sync guru`
Finally, emerge **gay**.

`root #``emerge --ask games-misc/gay`
## Usage

### Invocation

`user $``gay -h````
usage: gay [-h] [--encoding ENCODING] [-f] [-l] [-g] [-b] [-t] [-a] [-p] [-n]
           [--gq] [--mlm] [--aro] [--poly] [--db] [--dg] [--ag] [--bg] [--gf]
           [--abro] [--nt] [--tri] [-u] [-c {8,24}] [-i {1d,2d}] [--period PERIOD]
           [--tabs TABS]
options:
  -h, --help            show this help message and exit
  --encoding ENCODING
  -f, --flag
  -l, --les, --lesbian, --wlw
  -g, --gay
  -b, --bi, --bisexual
  -t, --trans, --transgender
  -a, --ace, --asexual
  -p, --pan, --pansexual
  -n, --nb, --non-binary
  --gq, --gender-queer
  --mlm
  --aro, --aromantic
  --poly, --polysexual
  --db, --demiboy
  --dg, --demigirl
  --ag, --agender
  --bg, --bigender
  --gf, --genderfluid
  --abro, --abrosexual
  --nt, --neut, --neutrois
  --tri, --trigender
  -u, --unbuffered
  -c {8,24}, --colour {8,24}
  -i {1d,2d}, --interpolation {1d,2d}
  --period PERIOD
  --tabs TABS, --tab-width TABS
```
### Piping

Gay is similar to **lolcat**. To use it:

`user $``echo Larry the Cow beckons you to explore the Gentoo Wiki! | gay`
