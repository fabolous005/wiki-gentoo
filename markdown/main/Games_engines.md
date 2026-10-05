<!-- source: https://wiki.gentoo.org/wiki/Games/engines | group: Gentoo Wiki (Main) | wiki-title: Games/engines -->
---
title: Games/engines
url: https://wiki.gentoo.org/wiki/Games/engines
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-30"
categories: ['https://packages.gentoo.org/categories/games-engines']
fingerprint: "913a1953e0a48d1f"
license: CC BY-SA 4.0
---

# Games/engines

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article provides an overview of game engines available in the ::gentoo ebuild repository.

## GemRB

![](https://wiki.gentoo.org/images/thumb/7/7f/Gemrb_logo1.png/150px-Gemrb_logo1.png)

GemRB is a portable open-source implementation of Bioware's Infinity Engine that also works on Android. It runs the Baldur's Gate, Icewind Dale, and Planescape: Torment games, however it is not yet feature complete (see [todo list](https://github.com/gemrb/gemrb/issues?q=is%3Aopen+is%3Aissue+label%3Afeature)). It also includes an extensible plugin-based design that removes many limitations of the Infinity Engine and improves usability through a lot of [innovations](https://gemrb.org/Manpage.html). In order to play games you need to do some \[ configuration\]. View a YouTube demo [here](https://www.youtube.com/watch?v=rhWUOMIAetw).

## Luanti

![](https://wiki.gentoo.org/images/thumb/9/90/Minetest_screenshot1.png/150px-Minetest_screenshot1.png)

Luanti (formerly **Minetest**, see [bug #943292](https://bugs.gentoo.org/show_bug.cgi?id=943292)) is an infinite-world block sandbox game and engine, heavily inspired by Minecraft and InfiniMiner. The majority of ingame content is provided by the community in the form of [mods](https://content.luanti.org/packages/?type=mod), as well as [texture packs](https://content.luanti.org/packages/?type=txp). Mods are installed on the server-side and there are already quite a few [dedicated servers](https://www.luanti.org/servers/) running you can join. The engine is written in C/C++ and is meant to be portable and lightweight and should run even on fairly old hardware. Supported platforms are Linux, OS X, FreeBSD and others. Features include building, crafting, multiplayer, lightning graphics and a map generator. View a YouTube demo [here](https://www.youtube.com/watch?v=ss9kAQCAzVc).

`root #``emerge --ask games-engines/minetest`
See [Luanti](https://wiki.gentoo.org/wiki/Luanti).

## LÖVE

![](https://wiki.gentoo.org/images/thumb/8/8e/Love_screenshot1.png/150px-Love_screenshot1.png)

LÖVE is a framework you can use to make 2D games in Lua and also allows for commercial use. It features a [lot of games](https://www.love2d.org/wiki/Category:Games) including [Stabyourself games](http://stabyourself.net) such as [games-arcade/mari0](https://packages.gentoo.org/packages/games-arcade/mari0), [games-arcade/notpacman](https://packages.gentoo.org/packages/games-arcade/notpacman) or [games-arcade/nottetris2](https://packages.gentoo.org/packages/games-arcade/nottetris2). It is slotted in portage to avoid compatibility problems with old games, slot "0" being always the latest release. View a YouTube demo [here](https://www.youtube.com/watch?v=SaoHMjS04vU).

`root #``emerge --ask games-engines/love`
## OpenMW

![](https://wiki.gentoo.org/images/thumb/5/59/Openmw.png/150px-Openmw.png)

OpenMW is a free and opensource reimplementation of the engine that featured Morrowind. It's mainly written in C++, utilizing [SDL2](https://www.libsdl.org) and [openscenegraph](http://www.openscenegraph.org/) among other libraries. It's still in beta stage, but is already very playable and has a great developer community with lots of [contributors](https://github.com/OpenMW/openmw/graphs/contributors). It [supports mods](https://wiki.openmw.org/index.php?title=Mod_installation) and also comes with it's own [Editor (OpenCS)](http://downloads.openmw.org/opencs-manual/main.pdf) to create new games.
You need the original Morrowind Data files. If you haven't installed them yet, you can install them straight via the game launcher (launcher USE flag) which is the officially supported method or by using the [games-rpg/morrowind-data](https://packages.gentoo.org/packages/games-rpg/morrowind-data) package (this might not work for all morrowind releases out there). View a YouTube demo [here](https://www.youtube.com/watch?v=aST2MJd2Tis).

`root #``emerge --ask games-engines/openmw`
## ScummVM

![](https://wiki.gentoo.org/images/thumb/1/11/Scummvm_screenshot1.png/150px-Scummvm_screenshot1.png)

ScummVM is a reimplementation of the SCUMM game engine used in Lucasarts point-and-click adventures (such as Monkey Island 1-3, Day of the Tentacle, Sam & Max, ...), but also supports Sierra's AGI and SCI games (such as King's Quest 1-6, Space Quest 1-5, ...), [games-rpg/bass](https://packages.gentoo.org/packages/games-rpg/bass), [games-rpg/queen](https://packages.gentoo.org/packages/games-rpg/queen), [games-rpg/drascula](https://packages.gentoo.org/packages/games-rpg/drascula) and [many more](https://www.scummvm.org/compatibility/). Note that it always requires the original game files. Portage also provides [games-engines/scummvm-tools](https://packages.gentoo.org/packages/games-engines/scummvm-tools) which is a collection of utilities to extract data files from games, convert audio files and make video sequences usable under ScummVM. View a YouTube demo [here](https://www.youtube.com/watch?v=W0Y8IMLx-Bs).

`root #``emerge --ask games-engines/scummvm`
