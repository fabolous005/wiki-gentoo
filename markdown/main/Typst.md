<!-- source: https://wiki.gentoo.org/wiki/Typst | group: Gentoo Wiki (Main) | wiki-title: Typst -->
---
title: Typst
url: https://wiki.gentoo.org/wiki/Typst
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-21"
fingerprint: "329ef2371b2f79de"
license: CC BY-SA 4.0
---

# Typst

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Typst** is a new markup-based typesetting system that is designed to be as powerful as LaTeX while being much easier to learn and use.

## Installation

### Emerge

[app-text/typst](https://github.com/gentoo/guru/tree/master/app-text/typst) is currently in [GURU](https://wiki.gentoo.org/wiki/GURU):

`root #``emerge --ask app-text/typst::guru`
## Usage

The example document used is:

**`hello.typ`**

### Compiling a document

To build a document, use typst compile:

`user $``typst compile hello.typ`
If the document was successfully compiled, nothing should be output.

## Troubleshooting

### Error compiling

If Typst encounters an issue in the document given, it will provide an error message detailing where the issue is:

`user $``typst compile hello.typ`
error: unclosed delimiter
   ┌─ hello.typ:13:0
   │
13 │ $lim\_(h arrow.r 0) (f(x + h) - f(x) / h)
   │ ^
