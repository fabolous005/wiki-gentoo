<!-- source: https://wiki.gentoo.org/wiki/Plymouth/Theme_creation | group: Gentoo Wiki (Main) | wiki-title: Plymouth/Theme creation -->
---
title: Plymouth/Theme creation
url: https://wiki.gentoo.org/wiki/Plymouth/Theme_creation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-30"
fingerprint: "7a1097779d76008e"
license: CC BY-SA 4.0
---

# Plymouth/Theme creation

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

It is possible to create themes for Plymouth, however documentation for theme creation is extremely difficult to find on the internet. This page aims to provide a few helpful links so that one may understand the Plymouth theme creation process. Eventually there may be a tutorial (or 'guide' as we like to call them here on the wiki) on how to create a simple theme.

Of course, [Plymouth](https://wiki.gentoo.org/wiki/Plymouth) must be installed before any newly created themes can be viewed or tested. Head up a level and do that now if it is not done yet.

Any potential theme creator should have a bit of knowledge about programming in C, as C is the language Plymouth is written. A full depth of knowledge will not be required.

## How Plymouth works

Before starting a design process, it is important to know how the program(s) being used in project operate.

As Plymouth's [README states](https://cgit.freedesktop.org/plymouth/plain/README):

plymouth ships with two binaries: /sbin/plymouthd and /bin/plymouth

The first one, plymouthd, does all the heavy lifting. It logs the session and shows the splash screen. The second one, /bin/plymouth, is the control interface to plymouthd.

It supports things like plymouth show-splash, or plymouth ask-for-password, which trigger the associated action in plymouthd.[\[1\]](https://wiki.gentoo.org#cite_note-1)


## Creating a simple Plymouth theme

### Design

Before something can be well engineered, there must first be a plan. The plan is what happens during the design phase. For the purpose of keeping this article short and sweet, we will be modifying the default "Solar" Plymouth theme to include some text: "My First Theme".

### The files

The files for the Solar theme come with Plymouth. On Gentoo systems they can be found in the /usr/share/plymouth/themes/solar directory. Change to this directory now.

### Test

Testing and debugging can be performed from an X environment. Start plymouthd in non-daemon mode:

`root #``plymouthd --no-daemon --debug`
Ask to render the splash via the plymouth front end:

`root #``plymouth show-splash`
To stop the rendering, [ssh](https://wiki.gentoo.org/wiki/Ssh) to the unit and issue:

`root #``plymouth quit`
### Build

#### dracut

## External resources

- [Some helpful information on Plymouth theme creation](https://web.archive.org/web/20100609183746/http://blog.fpmurphy.com/2009/09/project-plymouth.html) by Finnbarr P. Murphy (archived)
- [Plymouth Theme Guide (part 1)](http://brej.org/blog/?p=158) by Charlie Brej
- [Plymouth Theme Guide (part 2)](http://brej.org/blog/?p=174) by Charlie Brej
- [Plymouth Theme Guide (part 3)](http://brej.org/blog/?p=197) by Charlie Brej
- [Plymouth Theme Guide (part 4)](http://brej.org/blog/?p=238) by Charlie Brej
- [Falling blocks game in Plymouth](http://brej.org/blog/?p=409) by Charlie Brej
- [Plymouth ⟶ X transition](http://blogs.gnome.org/halfline/2009/11/28/plymouth-%E2%9F%B6-x-transition/) by Ray Strode (Plymouth maintainer)
- [5 "Stunning" Ubuntu Themes](http://www.techdrivein.com/2011/05/5-stunning-plymouth-screen-themes-for.html)
