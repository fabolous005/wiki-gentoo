<!-- source: https://wiki.gentoo.org/wiki/Tkinter | group: Gentoo Wiki (Main) | wiki-title: Tkinter -->
---
title: Tkinter
url: https://wiki.gentoo.org/wiki/Tkinter
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-04"
fingerprint: "241127f4f4e4f8fe"
license: CC BY-SA 4.0
---

# Tkinter

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Tkinter (tkinter in Python 3.x) claims to be "most portable GUI toolkit for Python"[\[1\]](http://tkinter.unpythonic.net/wiki/) and is Python's "de-facto standard GUI (Graphical User Interface) package"[\[2\]](https://wiki.python.org/moin/TkInter). Although there are several other graphical toolkits for Python, Tkinter is the one most often used in GUI development with Python.

tkinter uses [dev-lang/tk](https://packages.gentoo.org/packages/dev-lang/tk) internally.

## Installation

### make.conf

Getting Tkinter can be accomplished by enabling the `tk` USE flag. This can be set for specifically for Python, or for all packages system-wide in the make.conf file.

**`/etc/portage/make.conf`**

**Add tk to the system's USE flags**

```
USE="tk"
```
### package.use

For an approach more limited in scope, modify a package.use file for Python only.

**`/etc/portage/package.use/python`**

**Add the tk USE flag for dev-lang/python**

```
 tk
```
### Emerge

Finally, re-emerge the [@world set](<https://wiki.gentoo.org/wiki/World_set_(Portage)>) using this command:

`root #``emerge --ask -uvDU @world`
That is it! Tkinter should now be installed.

## Usage

When attempting to use Tkinter in Python code, depending on the version(s) of Python are currently installed on the system, import Tkinter might need performed in different ways:

- When using Python 2.x.: use `import Tkinter`
- When using Python 3.x: use `import tkinter` (note the lower case **T**).

## See also

- [Python](https://wiki.gentoo.org/wiki/Python) — an extremely popular cross-platform object oriented programming language.
