<!-- source: https://wiki.gentoo.org/wiki/Sawfish | group: Gentoo Wiki (Main) | wiki-title: Sawfish -->
---
title: Sawfish
url: https://wiki.gentoo.org/wiki/Sawfish
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-19"
fingerprint: "96464a64496f0bfc"
license: CC BY-SA 4.0
---

# Sawfish

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is

**deprecated (obsolete)**. Contents are

<u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

**Sawfish** is an extensible [window manager](https://wiki.gentoo.org/wiki/Window_manager) using a Lisp-based scripting language. Its policy is very minimal compared to most window managers. Its aim is simply to manage windows in the most flexible and attractive manner possible. All high-level WM functions are implemented in Lisp for future extensibility or redefinition. These are some of the features that set Sawfish apart from other window managers:

- Event hooking: For many events (moving windows etc.) you can customize the way Sawfish will respond.
- Window matching: When windows are created you can match them to a set of rules and automatically perform actions on them.
- Flexible theming: Sawfish allows for very different themes to be created and a variety of third-party themes are readily available.

## Installation

### Emerge

Install [x11-wm/sawfish](https://packages.gentoo.org/packages/x11-wm/sawfish):

`root #``emerge --ask x11-wm/sawfish`
## Configuration

There are two ways; using the configurator GUI, or preparing lisp code. The GUI can be run by middle-clicking background -> "Customize". Most customizations similar to other window managers can be done through GUI.

For customizations by lisp, first understand that in the startup, three files are read, in the order: sawfish-defaults, \~/.sawfish/custom, .sawfishrc.
