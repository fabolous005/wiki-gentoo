<!-- source: https://wiki.gentoo.org/wiki/Qalculate | group: Gentoo Wiki (Main) | wiki-title: Qalculate -->
---
title: Qalculate
url: https://wiki.gentoo.org/wiki/Qalculate
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-04"
fingerprint: "5ee112b9f4cfdaa8"
license: CC BY-SA 4.0
---

# Qalculate

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Qalculate** is a multi-purpose cross-platform desktop calculator that features a large library of customizable functions, unit calculations and conversion, physical constants, symbolic calculations (including integrals and equations), arbitrary precision, uncertainty propagation, interval arithmetic, plotting, and a user-friendly interface.

## Installation

### USE flags


| [+curl](https://packages.gentoo.org/useflags/+curl) | Add support for client-side URL transfer library | 
| [+hardened](https://packages.gentoo.org/useflags/+hardened) | Disable unsafe functions like 'command' and variables like 'uptime'. | 
| [gnuplot](https://packages.gentoo.org/useflags/gnuplot) | Enable support for gnuplot (data and function plotting) | 
| [icu](https://packages.gentoo.org/useflags/icu) | Enable ICU (Internationalization Components for Unicode) support, using dev-libs/icu | 
| [readline](https://packages.gentoo.org/useflags/readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask sci-libs/libqalculate`
### GUI applications

#### Qalculate-gtk

`root #``emerge --ask sci-calculators/qalculate-gtk`
#### Qalculate-qt

`root #``emerge --ask sci-calculators/qalculate-qt`
## Usage

### Basic calculations

To perform basic calculations with qalculate, use it like a regular calculator:

`user $``qalc 1 + 1`
1 + 1 = 2

### Interactive mode

To enter qalculate's interactive mode, run qalc:

`user $``qalc`
\>

And start performing calculations:

`user $``qalc`
\> 1 + 1
  1 + 1 = 2
>

### Using the previous result

To use the previous result in a new calculation, use the `ans` keyword:

`user $``qalc`
\> 1 + 1
  1 + 1 = 2
> ans + 2
  ans + 2 = 4
>

### Performing currency conversions

Add `curl` to [sci-libs/libqalculate](https://packages.gentoo.org/packages/sci-libs/libqalculate)'s USE flags:

**`/etc/portage/package.use/libqalculate`**

To convert between currencies:

`user $``qalc`
\> 1 usd to aud
  1 USD ≈ AUD 1.519918094

## See also

\* [bc](https://wiki.gentoo.org/wiki/Bc) — arbitrary-precision fixed-point mathematical scripting language
