<!-- source: https://wiki.gentoo.org/wiki/Gentodo | group: Gentoo Wiki (Main) | wiki-title: Gentodo -->
---
title: Gentodo
url: https://wiki.gentoo.org/wiki/Gentodo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-22"
fingerprint: b9c3787d0e97dbf8
license: CC BY-SA 4.0
---

# Gentodo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Gentodo** is a todo program designed for a Gentoo development workflow.

## Installation

### Emerge

Install [app-misc/gentodo::guru](https://github.com/gentoo-mirror/guru/tree/master/app-misc/gentodo) from the [GURU](https://wiki.gentoo.org/wiki/GURU) repository:

`root #``emerge --ask app-misc/gentodo`
## Usage

### Adding an item

To add an item to the todo list:

`user $``gentodo add -t Title -d Details`
It is also possible to not have a description:

`user $``gentodo add -t Title`
### Listing entries

To list entries:

`user $``gentodo`
To get item IDs (required for deletion):

`user $``gentodo -v`
### Deleting an item

First get the item ID and then:

`user $``gentodo del 123`
## Files

### Storage

To find your `todo.json` file, go to `~/.local/share/gentodo/todo.json`, an example of which looks like:

**`~/.local/share/gentodo/todo.json`**

### Configuration

**Gentodo** will eventually store its config in `~/.config/gentodo/config.toml`. For now, this is an example of a config file:

**`~/.config/gentodo/config.toml`**

## Tips

A handy use for this program is to add it to the user's \~/.bashrc so every time they open a new terminal they will be reminded of upcoming tasks that they need to look at.



**`~/.bashrc`**
