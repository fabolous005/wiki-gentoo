<!-- source: https://wiki.gentoo.org/wiki/Notes_on_ebuilds_with_GUI | group: Gentoo Wiki (Main) | wiki-title: Notes on ebuilds with GUI -->
---
title: Notes on ebuilds with GUI
url: https://wiki.gentoo.org/wiki/Notes_on_ebuilds_with_GUI
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-03-21"
fingerprint: d8029a4e88aca2fa
license: CC BY-SA 4.0
---

# Notes on ebuilds with GUI

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

When this wiki page was started (2017-11-13), there was a lot confusion about how to write clean ebuilds for desktop applications.
As result, there are many ebuilds which use obsolete eclasses, or even worse do not install, update, or remove the icons and menus.
The [eclasses](https://wiki.gentoo.org/wiki/Eclass) are hardly documented and the documentation has mistakes. Many for loops for icons miss the definition of the local variable.

## Eclasses

### xdg.eclass

eclass reference: [xdg.eclass](https://devmanual.gentoo.org/eclass-reference/xdg.eclass)

- xdg inherits xdg-utils

### xdg-utils.eclass

eclass reference: [xdg-utils.eclass](https://devmanual.gentoo.org/eclass-reference/xdg-utils.eclass)

### gnome2.eclass

eclass reference: [gnome2.eclass](https://devmanual.gentoo.org/eclass-reference/gnome2.eclass)

### gnome2-utils.eclass

eclass reference: [gnome2-utils.eclass](https://devmanual.gentoo.org/eclass-reference/gnome2-utils.eclass)

## What repoman QA can detect

## Code examples to create icons

## .desktop files

## brainstorming section (delete later, when merged in the text)

This page provides a summary of various files whose installation should be accompanied by appropriate postinst/postrm trigger calls.

| Path | Conditions | preinst | prerm | postrm | postinst | 
|---|---|---|---|---|---|
| xdg-utils.eclass (or automatic in xdg.eclass) |  |  |  |  |  | 
| /usr/share/applications | if MimeType= is specified | - | - | xdg\_desktop\_database\_update | xdg\_desktop\_database\_update | 
| /usr/share/mime |  | - | - | xdg\_mimeinfo\_database\_update | xdg\_mimeinfo\_database\_update | 
| /usr/share/icons |  | - | - | xdg\_icon\_cache\_update | xdg\_icon\_cache\_update | 
| gnome2-utils.eclass (or automatic in gnome2.eclass) |  |  |  |  |  | 
| /etc/gconf/schemas/ |  | gnome2\_gconf\_savelist | - | - | gnome2\_gconf\_install | 
| /usr/share/glib-2.0/schemas |  | gnome2\_schemas\_savelist | - | gnome2\_schemas\_update | gnome2\_schemas\_update | 
| /usr/share/omf |  | gnome2\_scrollkeeper\_savelist | - | gnome2\_scrollkeeper\_update | gnome2\_scrollkeeper\_update | 
| /usr/lib\*/gdk-pixbuf-2.0 |  | gnome2\_gdk\_pixbuf\_savelist | - | - | gnome2\_gdk\_pixbuf\_update | 
| /usr/lib\*/gio/modules |  | - | - | gnome2\_giomodule\_cache\_update | gnome2\_giomodule\_cache\_update | 

- \*1) Not needed in general, but still useful to use in phase defining eclasses, like gnome2.eclass.

- Needs to be updated since some savelists are not needed
- what about fonts?

### Working principle of a trigger in bash



- portage generates the QA warnings here: [https://github.com/gentoo/portage/tree/master/bin/postinst-qa-check.d](https://github.com/gentoo/portage/tree/master/bin/postinst-qa-check.d)
