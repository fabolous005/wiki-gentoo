<!-- source: https://wiki.gentoo.org/wiki/Nano/Guide | group: Gentoo Wiki (Main) | wiki-title: Nano/Guide -->
---
title: Nano/Guide
url: https://wiki.gentoo.org/wiki/Nano/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-09-04"
fingerprint: "1654cf40e7bdafd6"
license: CC BY-SA 4.0
---

# Nano/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide covers basic operations in nano, and is meant to be very concise. For more information about nano check out: [https://www.nano-editor.org](https://www.nano-editor.org), and the [main article on nano](https://wiki.gentoo.org/wiki/Nano).

## Opening and creating files

To open or create files, type:

`user $``nano filename`
Nano is a [modeless](https://en.wikipedia.org/wiki/Vi#Interface) editor: start typing immediately to insert text. To edit a configuration file, such as [/etc/fstab](https://wiki.gentoo.org/wiki//etc/fstab), for example:

`root #``nano /etc/fstab`
## Saving and exiting

To save the changes, press `Ctrl` + `O` (letter "o", not number zero). To exit nano, type `Ctrl` + `X`.

Trying to exit with unsaved changes, it will ask to save. Press `N` to discard changes. Press `Y` to save - it will then ask for a filename, type it in and press `Enter`. If a save is started accidentally, cancel by pressing `Ctrl` + `C` when prompted for a filename.

## Cutting and pasting

To cut a single line, use `Ctrl` + `K` (hold down `Ctrl` and then press `K`). The line disappears. To paste it, move the cursor to where to paste, and punch `Ctrl` + `U`. The line reappears.

To move multiple lines, simply cut them with several `Ctrl` + `K` in a row, then paste with a single `Ctrl` + `U`.

For more fine-grained control, mark the text: move the cursor to the beginning of what should be cut, hit `Ctrl` + `6` (or `Alt` + `A`). Now move the cursor to the end of the text to be cut: the marked text gets highlighted (to cancel at this point, simply hit `Ctrl` + `6` again). Press `Ctrl` + `K` to cut the marked text. Use `Ctrl` + `U` to paste.

## Searching for text

Searching for a string is easy, but think *"WhereIs"* instead of *"Search"*. Simply hit `Ctrl` + `W`, type in the search string, and press `Enter`. To search for the same string again, hit `Alt` + `W`.

## More options

Read /etc/nanorc for options.
