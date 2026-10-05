<!-- source: https://wiki.gentoo.org/wiki/Rxvt-unicode | group: Gentoo Wiki (Main) | wiki-title: Rxvt-unicode -->
---
title: rxvt-unicode
url: https://wiki.gentoo.org/wiki/Rxvt-unicode
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-25"
fingerprint: ee80e2594a9773b4
license: CC BY-SA 4.0
---

# rxvt-unicode

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**rxvt-unicode**, also known simply as urxvt, is a fast and lightweight [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) with [Xft](https://en.wikipedia.org/wiki/Xft) and [Unicode](https://en.wikipedia.org/wiki/Unicode) support.

Many Gentoo users enjoy using urxvt inside the [i3](https://wiki.gentoo.org/wiki/I3) and [Sway](https://wiki.gentoo.org/wiki/Sway) window managers.


| [+font-styles](https://packages.gentoo.org/useflags/+font-styles) | Enable support for bold and italic fonts | 
| [+mousewheel](https://packages.gentoo.org/useflags/+mousewheel) | Enable scrolling via mouse wheel or buttons 4 and 5 | 
| [24-bit-color](https://packages.gentoo.org/useflags/24-bit-color) | Enable 24-bit color support. Note that this feature is unofficial, may cause visual glitches due to the fact there is no termcap/terminfo definition for rxvt-unicode-24bit yet so it is necessary to use the one for 256 colours, visibly increases memory usage, and might slow urxvt down dramatically when more than six fonts are in use in a terminal instance. | 
| [256-color](https://packages.gentoo.org/useflags/256-color) | Enable 256 color support | 
| [blink](https://packages.gentoo.org/useflags/blink) | Enable blinking text | 
| [fading-colors](https://packages.gentoo.org/useflags/fading-colors) | Enable colors fading when off focus | 
| [gdk-pixbuf](https://packages.gentoo.org/useflags/gdk-pixbuf) | Enable transparency support using x11-libs/gdk-pixbuf | 
| [iso14755](https://packages.gentoo.org/useflags/iso14755) | Enable ISO-14755 support | 
| [perl](https://packages.gentoo.org/useflags/perl) | Enable perl script support. You can still disable this at runtime with -pe "" | 
| [startup-notification](https://packages.gentoo.org/useflags/startup-notification) | Enable application startup event feedback mechanism | 
| [unicode3](https://packages.gentoo.org/useflags/unicode3) | Use 21 instead of 16 bits to represent unicode characters | 
| [wide-glyphs](https://packages.gentoo.org/useflags/wide-glyphs) | Enable support for wide glyphs, required for certain symbol/icon fonts to display correctly. Note that this feature is \*unofficial\* and has been observed to cause stability issues for some users. | 
| [xft](https://packages.gentoo.org/useflags/xft) | Build with support for XFT font renderer (x11-libs/libXft) | 

Install [x11-terms/rxvt-unicode](https://packages.gentoo.org/packages/x11-terms/rxvt-unicode):

`root #``emerge --ask rxvt-unicode`
It is possible to operate urxvt as a daemon, which will lead to lower resource usage and quicker startup for new terminals. It is a good idea to start the daemon at the beginning of the X session.

The following command will start the daemon and fork it into the background.

`user $``urxvtd --quiet --opendisplay --fork`
After this, new clients can be opened on the single daemon process, rather than spawning new processes for each terminal. To do this, simply run urxvtc in place of the usual urxvt command. Keep in mind that if for any reason the daemon is terminated, any subsequent urxvtc calls as well all client instances will be closed.

Environment variable can be used to specify different location for the daemon \~/.urxvt/urxvtd-hostname listening socket.

Configuration for urxvt is done mainly through the [X resources](https://wiki.gentoo.org/wiki/X_resources) system, though command line equivalents are also available in most cases. A full list of these options can be found in the urxvt [manpage](https://linux.die.net/man/1/urxvt). To configure all urxvt options in a different file and including this file in .Xresources my be advisable. For example:

**`~/.Xresources`**

Some common configuration options are listed below.

urxvt's [font](https://wiki.gentoo.org/wiki/Fonts) can be configured using either [XLFD](https://en.wikipedia.org/wiki/X_logical_font_description) notation or, provided the package was compiled with the [xft](https://packages.gentoo.org/useflags/xft) [USE flag](https://wiki.gentoo.org/wiki/USE_flag), [Xft](https://en.wikipedia.org/wiki/Xft) fonts.

**`~/.Xresources`**

Fonts can be modified while urxvt is running by assigning actions to keys:

**`~/.Xresources`**

Rendering settings can be tweaked for Xft fonts as well. Note that this is not specific to urxvt.

**`~/.Xresources`**

The look of the scrollbar can be changed, or it can be removed entirely.

**`~/.Xresources`**

The size of the scrollback buffer can be increased with:

**`~/.Xresources`**

By default, urxvt will print out a screen dump, via lpr, when `PrntScrn` is pressed. Using `Ctrl`+`PrintScrn` or `Shift`-`PrintScrn` will include the terminal's scroll back in the printout as well. This behavior can be changed, or disabled entirely, based on personal preference and need.

**`~/.Xresources`**

By default, urxvt will copy selected text to the `PRIMARY` clipboard, and paste it when middle click is pressed.

To use the `CLIPBOARD` clipboard, use `Ctrl`+`Alt`+`C` to copy, and `Ctrl`+`Alt`+`V` to paste.

The default urxvt [Perl](https://wiki.gentoo.org/wiki/Perl) extensions can be used for copy and paste actions as well for URL handling capabilities. In order to use Perl extensions in urxvt, the package must have been compiled with [perl](https://packages.gentoo.org/useflags/perl)[. The](https://wiki.gentoo.org/wiki/USE_flag) [x11-misc/urxvt-perls](https://packages.gentoo.org/packages/x11-misc/urxvt-perls) package provides the **keyboard-select** extension not included by default. The package sources code can be found in muennich's [GitHub repository](https://github.com/muennich/urxvt-perls) or an [ebuild](https://github.com/tokiclover/bar-overlay/tree/master/dev-perl/perl-URxvt) for example. It is possible to get other Perl [extensions](https://github.com/search?l=Perl&q=urxvt&type=Repositories&utf8=%E2%9C%93).

Here is an example of a \~/.Xresources. The following lines could also be added to \~/.Xdefaults, though \~/.Xresources is preferred.

**`~/.Xresources`**

The default selection-to-clipboard extension will put the selected text into the clipboard automatically. To add pasting functionality we have to create a simple extension:

**`/usr/lib64/urxvt/perl/pasta`**

```
#! /usr/bin/env perl -w
# Author:   Aaron Caffrey
# Website:  https://github.com/wifiextender/urxvt-pasta
# License:  GPLv3
# Usage: put the following lines in your .Xdefaults/.Xresources:
# URxvt.perl-ext-common           : selection-to-clipboard,pasta
# URxvt.keysym.Control-Shift-V    : perl:pasta:paste
use strict;
sub on_user_command {
  my ($self, $cmd) = @_;
  if ($cmd eq "pasta:paste") {
    $self->selection_request (urxvt::CurrentTime, 3);
  }
  ()
}
```
This adds menu entry and menu icon for urxvt. If urxvt doesn't have a .desktop file, create one.

**`/usr/share/applications/urxvt.desktop`**

```
[Desktop Entry]
Name=Urxvt
Comment=Terminal emulator
TryExec=urxvt
Exec=urxvt
Icon=utilities-terminal
Type=Application
Categories=GTK;TerminalEmulator;System;
# This assumes the 'startup-notification' USE flag being enabled
StartupNotify=true
```
For setting application icon [x11-terms/rxvt-unicode](https://packages.gentoo.org/packages/x11-terms/rxvt-unicode) has to be compiled with [pixbuf](https://packages.gentoo.org/useflags/pixbuf)[.](https://wiki.gentoo.org/wiki/USE_flag)

**`~/.Xresources`**

The main urxvt's color palette is defined by `background`, `foreground` and `color` resources. It is also possible to set color of other elements (e.g. cursor or text underline). For more information consult the urxvt **n**[manpage](https://linux.die.net/man/1/urxvt).

**`~/.Xresources`**

**`~/.Xresources`**

**Tango colors**

**`~/.Xresources`**

**Linux colors**

As of v9.30 the default urxvt configuration does not support icon-oriented fonts, such as [Powerline](https://github.com/powerline/powerline) or [Nerd Fonts](https://www.nerdfonts.com/). Therefore the icon symbols fail to display correctly unless the `unicode3` USE flag is set.

Rendering of a common Powerline symbol (solid triangle separator) can be tested as:

`user $``echo -e '\xEE\x82\xB0'`
Using the standard fonts and enabling [media-fonts/powerline-symbols](https://packages.gentoo.org/packages/media-fonts/powerline-symbols) fallback via [Fontconfig](https://wiki.gentoo.org/wiki/Fontconfig) does not seem to work for urxvt<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>. This forces installing one of the Powerline [patched fonts](https://powerline.readthedocs.io/en/latest/installation/linux.html#patched-font-installation) (bearing the  `for Powerline` suffix).

See also [rxvt-unicode and the Powerline symbols](http://lists.schmorp.de/pipermail/rxvt-unicode/2016q1/002204.html) thread on the urxvt mailing list.

To be able to use font size changing within running terminal session, first verify urxvt has been build with [perl](https://packages.gentoo.org/useflags/perl)[.](https://wiki.gentoo.org/wiki/USE_flag) 

Install the [x11-misc/urxvt-font-size](https://packages.gentoo.org/packages/x11-misc/urxvt-font-size) package:

`root #``emerge --ask x11-misc/urxvt-font-size`
Open the \~/.Xresources file for editing and replace the entry:

**`~/.Xresources`**

With following entry, or just add the `font-size` to the end of the listed extensions:

**`~/.Xresources`**

Configure a keyboard key combination to increase or decrease the font size, add following entries:

**`~/.Xresources`**

To increase the font size press `Ctrl`+`+`, to decrease the font size press `Ctrl`+`-`. To reset to the default font size press `Ctrl`+`0`.

1\. Check for syntax errors in \~/.Xresources based on [X resources](https://wiki.gentoo.org/wiki/X_resources) article.

2\. If you're using \~/.xinitrc add `[[ -f ~/.Xresources ]] && xrdb -merge ~/.Xresources` to \~/.xinitrc and reboot.

3\. Invoking \`xrdb -merge -I$HOME \~/.Xresources\`might also resolve the issue.

URxvt's prompt may appear in the center of the window when first opened, rather than at the top (as is typical for terminal emulators).

To correct this, add the following to the final line of \~/.bashrc (or similar):

**`~/.bashrc`**

This section describes the instructions for reporting and fixing urxvt bugs.

1. Install Arch GNU/Linux.
  1. Test on Arch to see if the bug persists.
    1. If the bug does not persist, then contact the ebuild maintainer [Jeroen Roovers (jer)](https://wiki.gentoo.org/wiki/User:JeR)
2. Contact upstream developer. Be sure to note testing was performed on Gentoo Linux as Gentoo GNU/Linux or you will be ignored and possibly yelled at.
3. When all else fails, contact the maintainer [Jeroen Roovers (jer)](https://wiki.gentoo.org/wiki/User:JeR)

These instructions are based on official upstream [instructions from the developer](http://pod.tst.eu/http://cvs.schmorp.de/rxvt-unicode/doc/rxvt.7.pod#I_use_Gentoo_and_I_have_a_problem).

- [X resources](https://wiki.gentoo.org/wiki/X_resources) — configuration options for X applications
- [Fonts](https://wiki.gentoo.org/wiki/Fonts) — home page for information about using fonts on Gentoo
