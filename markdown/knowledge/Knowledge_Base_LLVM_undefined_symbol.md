<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:LLVM_undefined_symbol | group: Gentoo Knowledge | wiki-title: Knowledge_Base:LLVM_undefined_symbol -->
---
title: Knowledge Base:LLVM undefined symbol
url: https://wiki.gentoo.org/wiki/Knowledge_Base:LLVM_undefined_symbol
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: fbc0957b5c0fdbd1
license: CC BY-SA 4.0
---

# Knowledge Base:LLVM undefined symbol

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This issue occurs with the LLVM 17+ toolchain enforcing stricter coding practices.

As described by [Oracle](https://docs.oracle.com/cd/E19957-01/806-0641/chapter2-9/index.html):

After all input files have been read and all symbol resolution is complete, the link-editor searches the internal symbol table for any symbol references that have not been bound to symbol definitions. These symbol references are referred to as undefined symbols. The effect of these undefined symbols on the link-edit process can vary according to the type of output file being generated, and possibly the type of symbol.

This issues should be checked if they have been reported on [bugs.gentoo.org](https://bugs.gentoo.org) and if not, must be reported so they can get fixed.

## Workaround

There is a workaround which can be used after checking and if needed, reporting the bug.

### package.env

First, create the following file in /etc/portage/env/undefined.conf

It may be required to create the `env` directory with:

`root #``mkdir /etc/portage/env`
**`/etc/portage/env/undefined.conf`**

**FileBox undefined.conf**

```
LDFLAGS="${LDFLAGS} -Wl,—undefined-version"
```
Next, create an /etc/portage/package.env/undefined and add the `app-category/package-version` to the file (This guide will use `media-libs/mesa-23.3.1` as an example.)

It may be required to create the `package.env` directory with:

`root #``mkdir /etc/portage/package.env`
**`/etc/portage/package.env/undefined`**

**FileBox undefined**

```
 undefined.conf
```
It is now possible to run the emerge command again for the program to install succesfully free from this issue.

`root #``emerge --ask --verbose --oneshot media-libs/mesa`
