<!-- source: https://wiki.gentoo.org/wiki/Localization/Guide/The_Euro_symbol | group: Gentoo Wiki (Main) | wiki-title: Localization/Guide/The Euro symbol -->
---
title: Localization/Guide/The Euro symbol
url: https://wiki.gentoo.org/wiki/Localization/Guide/The_Euro_symbol
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-07"
fingerprint: f7c9f733440bc93d
license: CC BY-SA 4.0
---

# Localization/Guide/The Euro symbol

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

An article describing how to display the Euro symbol (€) for the console and in X.

## The Euro symbol for the console

### OpenRC

In order to get the console to display the [Euro symbol](https://unicode-table.com/en/20AC/), the `consolefont` variable will need to be set in [/etc/conf.d](https://wiki.gentoo.org/wiki//etc/conf.d)/consolefont to a file found in /usr/share/consolefonts/ (without the `.psfu.gz`). `lat9w-16` has the Euro symbol.

**`/etc/conf.d/consolefont`**

**Setting the console font**

```
consolefont="lat9w-16"
```
Verify that the consolefont service is in the boot runlevel:

`root #``rc-update -v show | grep consolefont`
If no runlevel is displayed for consolefont, then add it to the proper level:

`root #``rc-update add consolefont boot`
## The Euro symbol in X

### Most applications

Getting the Euro symbol to work properly in X is a little bit tougher. First change the `fixed` and `variable` definitions in /usr/share/fonts/misc/fonts.alias to end in `iso8859-15` instead of `iso8859-1`.

Some applications use their own font, they will need to be separately configured to use a font with the Euro symbol. This can be enacted on a user-specific level in .Xresources (copy this file to /etc/skel/ for use by new users), or at a global level for any application with a resource file in /usr/share/X11/app-defaults/ (like xterm). In these files, generally an existing line will need to be modified, rather than adding a new line to the file. To change the xterm font, for instance:

`user $````
echo 'XTerm*font: fixed' >> ~/.Xresources 
```
`user $````
xrdb -merge ~/.Xresources
```
### The Euro symbol in (X)Emacs

To use the Euro symbol in (X)Emacs, add the following to the .Xresources file:

**`~/.Xresources`**

**Setting the font for emacs**

For XEmacs (not plain Emacs), add the following line to \~/.xemacs/init.el:

**`~/.xemacs/init.el`**

**Setting the font for xemacs**
