<!-- source: https://wiki.gentoo.org/wiki/Toolkit_USE_Flags | group: Gentoo Wiki (Main) | wiki-title: Toolkit USE Flags -->
---
title: Toolkit USE Flags
url: https://wiki.gentoo.org/wiki/Toolkit_USE_Flags
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: fe3a06eff6826b90
license: CC BY-SA 4.0
---

# Toolkit USE Flags

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page summarizes main points from discussions about toolkit (GTK, Qt) USE flags (gtk2, gtk3, qt4, qt5, etc.). The centithreads go back to 2005. It seems sometimes the discussion is going in circles, and it's hard to read all of the previous replies. This is an attempt to summarize the main points, pros and cons, to facilitate a more productive and informed effort towards a usable solution.

## Use cases

List of use cases (i.e. scenarios people want to work, or things that happen with packages, etc):

- Enable just gtk2 for just gtk2 support if available [\[1\]](https://groups.google.com/d/msg/linux.gentoo.dev/rtlwoHjhA-4/Cl95_f35AtoJ)
- [app-i18n/uim](https://packages.gentoo.org/packages/app-i18n/uim) , [x11-themes/light-themes](https://packages.gentoo.org/packages/x11-themes/light-themes) --> flag provides support for gtk3 apps, in addition to gtk(2) [\[2\]](https://groups.google.com/d/msg/linux.gentoo.dev/Y1PF_2XlFKI/DwEM3Q45YvQJ)
- [gnome-base/librsvg](https://packages.gentoo.org/packages/gnome-base/librsvg) --> flag for gtk3 libraries \*and\* executables [\[3\]](https://groups.google.com/d/msg/linux.gentoo.dev/Y1PF_2XlFKI/DwEM3Q45YvQJ)
- [media-sound/audacious](https://packages.gentoo.org/packages/media-sound/audacious) --> REQUIRED\_USE="^^ ( gtk gtk3 )" with default switching from version to version (current stable is gtk, previous was gtk3) [\[4\]](https://groups.google.com/d/msg/linux.gentoo.dev/Y1PF_2XlFKI/DwEM3Q45YvQJ)
- [www-client/midori](https://packages.gentoo.org/packages/www-client/midori) --> USE=deprecated instead of USE=gtk3 in unstable [\[5\]](https://groups.google.com/d/msg/linux.gentoo.dev/Y1PF_2XlFKI/DwEM3Q45YvQJ)
- [media-libs/libcanberra](https://packages.gentoo.org/packages/media-libs/libcanberra) --> USE=gtk3 enables extra support, in addition to gtk: "Enables building of gtk+3 helper library, gtk+3 runtime sound effects and the canberra-gtk-play utility. To enable the gtk+3 sound effects add canberra-gtk-module to the colon separated list of modules in the GTK\_MODULES environment variable." — very unclear: is it needed? recommended? also, why doesn't the package handle the environment variable by itself? [\[6\]](https://groups.google.com/d/msg/linux.gentoo.dev/Y1PF_2XlFKI/DwEM3Q45YvQJ)
- we need to ship webkit with gtk2 and gtk3 support [\[7\]](https://groups.google.com/d/msg/linux.gentoo.dev/Y1PF_2XlFKI/jbd8VHFyucgJ)
- MATE desktop can be built against gtk+ 2 or gtk+ 3, and upstream supports doing both [\[8\]](https://groups.google.com/d/msg/linux.gentoo.dev/dLGhvTXUFfw/K26zZGrBk5UJ)
- a single process cannot load both gtk2 and gtk3 - you \*will\* get random crashes [\[9\]](https://groups.google.com/d/msg/linux.gentoo.dev/dLGhvTXUFfw/K3wlmjxzbo4J)
- only three packages (providing libs) needed exception to the rule to avoid unreasonable maintenance overhead: spice, gtk-vnc and avahi [\[10\]](https://groups.google.com/d/msg/linux.gentoo.dev/4stGjUnErGI/B9e8j4YkZIAJ)
- Let's take Emacs as an example. The upstream package supports Athena widgets (both in Xaw and Xaw3d variants), Motif, GTK2, and GTK3. [\[11\]](https://groups.google.com/d/msg/linux.gentoo.dev/4stGjUnErGI/xgWt2dPZekMJ)
- You're trying to compare gtk to qt directly. They are not the same. gtk regards only the graphic library, qt is a library of utility functions too. Qt can be considered like gtk+glib, and that make things more complex. [\[12\]](https://groups.google.com/d/msg/linux.gentoo.dev/DCUWsXZpI24/6CNqf5nN-p4J)
- users that currently have -qt are going to be confused when it no longer does what they expect [\[13\]](https://groups.google.com/d/msg/linux.gentoo.dev/DCUWsXZpI24/eC_m8R240LoJ)
- The gtk 'solution' forced some ugly things like masking gtk+:3, gconf:3, ... and then selecting packages based on specific -r200 / -r300 revisions. So much work to avoid regressing into gtk3! [\[14\]](https://groups.google.com/d/msg/linux.gentoo.dev/Xev-k6rMyQI/8_6kG6GNBwAJ)
- QA has spoken out pretty clearly against unversioned gtk or qt useflags, and in favour of explicit versioned useflags. [\[15\]](https://groups.google.com/d/msg/linux.gentoo.dev/Xev-k6rMyQI/2z2vAdGVBwAJ)
- [x11-misc/spacefm](https://packages.gentoo.org/packages/x11-misc/spacefm) supports multiple toolkits as well [\[16\]](https://groups.google.com/d/msg/linux.gentoo.dev/CSUlym9nvoI/XqE3IUxkAQAJ)
- I would really like a way to toggle gtk3 for testing [\[17\]](https://groups.google.com/d/msg/linux.gentoo.dev/CSUlym9nvoI/iRJgF2Z9AQAJ)
- Suppose you want to run on an embedded system with limited RAM and the ability to choose means you can use one of the two libraries  exclusively, thus eliminating the need to load the other library? [\[18\]](https://groups.google.com/d/msg/linux.gentoo.dev/CSUlym9nvoI/jT_kcMq9AQAJ)

| Approach | Open questions | Pros | Cons | Supporters | Opponents | 
|---|---|---|---|---|---|
| [\[19\]](https://archives.gentoo.org/gentoo-project/message/dcb55e3f8f62c141912da382429dce20) |  |  |  |  | (opponents placeholder) | 
| gtk = any version, gtkN = gtkN support [\[22\]](https://groups.google.com/d/msg/linux.gentoo.dev/rtlwoHjhA-4/BSFJka3fSHUJ) |  |  |  |  |  | 
| [Project:GNOME/Gnome\_Team\_Ebuild\_Policies#gtk3](https://wiki.gentoo.org/wiki/Project:GNOME/Gnome_Team_Ebuild_Policies#gtk3) |  |  |  |  |  | 
| gtkN = gtkN support, **no** non-versioned gtk [\[56\]](https://groups.google.com/d/msg/linux.gentoo.dev/4stGjUnErGI/Kb-PvvU_ikcJ) | (open questions placeholder) |  |  |  | (opponents placeholder) | 
| gtk=newest GTK version, gtkN=only for older versions [\[67\]](https://groups.google.com/d/msg/linux.gentoo.dev/DCUWsXZpI24/sfibgymj8IMJ) | (open questions placeholder) | (pros placeholder) |  |  | (opponents placeholder) | 
| remove gtkN, make latest version default, possibly add USE flags or separate packages for deprecated versions [\[70\]](https://groups.google.com/d/msg/linux.gentoo.dev/rtlwoHjhA-4/FDpSKj6L2zIJ) |  | (pros placeholder) |  |  |  | 
| USE\_QT\_VERSIONS in make.conf, and single qt USE flag [\[84\]](https://groups.google.com/d/msg/linux.gentoo.dev/DCUWsXZpI24/P6OKWytwD5AJ) | (open questions placeholder) | (pros placeholder) |  |  |  | 
| eselect module [\[88\]](https://groups.google.com/d/msg/linux.gentoo.dev/Xev-k6rMyQI/-jYnH6VfAwAJ) | (open questions placeholder) | (pros placeholder) |  | (supporters placeholder) | (opponents placeholder) | 
| fork Gentoo for legacy toolkit support [\[92\]](https://groups.google.com/d/msg/linux.gentoo.dev/ClO7FIWUf0A/dnKyZAOOcM4J) | (open questions placeholder) | (pros placeholder) | (cons placeholder) |  |  |
