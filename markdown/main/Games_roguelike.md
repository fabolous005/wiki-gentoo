<!-- source: https://wiki.gentoo.org/wiki/Games/roguelike | group: Gentoo Wiki (Main) | wiki-title: Games/roguelike -->
---
title: Games/roguelike
url: https://wiki.gentoo.org/wiki/Games/roguelike
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-12-20"
categories: ['https://packages.gentoo.org/categories/games-roguelike']
fingerprint: "2a8d654e898bedcd"
license: CC BY-SA 4.0
---

# Games/roguelike

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article provides an overview of roguelike games that are available in the ::gentoo ebuild repository.

## Dungeon Crawl Stone Soup

![](https://wiki.gentoo.org/images/thumb/7/7e/Stonesoup_screenshot1.png/200px-Stonesoup_screenshot1.png)

Dungeon Crawl Stone Soup is a 1st class open-source rogue-like game of exploration and treasure-hunting in dungeons filled with dangerous and unfriendly monsters in a quest to rescue the mystifyingly fabulous Orb of Zot. It features a wide variety of classes, items and spells and can be played offline and [online](https://crawl.develz.org/wordpress/howto). You can play the game in gui-mode (tiles useflag) or ascii-mode (ncurses useflag) or even [in your browser](http://webtiles.akrasiac.org/). View a YouTube demo [here](https://www.youtube.com/watch?v=OEDGUPKm3Uc).

`root #``emerge --ask games-roguelike/stone-soup`
## TomeNET

![](https://wiki.gentoo.org/images/thumb/f/fd/TomeNET_Screenshot.png/200px-TomeNET_Screenshot.png)

TomeNET is a **multiplayer** rogue-like, based on and somewhat similar to MAngband and ToME, and also featuring some Zangband and Cthulu Angband monsters. It was created around 2001 (originally under the name of *PernMAngband* which had to be renamed due to a letter of Anne McCaffrey's attorney who prohibited the use of the name *Pern*) as a fork of MAngband which got some ToME design added to it. TomeNET is feared for being hard to master, but at the same time the more rewarding to the skillful player who has learned to make use of all the nifty possibilities open to him. The game has a full-fledged documentation called *The TomeNET Guide* - [HTML version](http://www.tomenet.eu/guide.php).

#### Main features

- Time passes slower on deeper levels to make up for speed gain of players and monsters, keeping the real-time experience at an optimum
- Day/night changes, four seasons, weather.
- Sound effects and dynamic background music.
- Various character modes, including: Traditional rogue-like (one life), MAngband-style (infinite resurrections), PvP-mode for killing each other off in the game world or in a special PvP arena.
- Automatically scheduled special events.
- Post-king game play: Characters that have beaten the main boss, Morgoth, may venture into another insanely dangerous dungeon to try and find the last remaining path to Valinor, to possibly retire on its shores.
- Over 250 static artifacts, infinite random artifacts, over 200 special item powers, over 1000 items, over 1100 monsters and many 'ego monster' types (IE variations of base monsters).
- Over 200 types of floor features/terrain.
- Multiple towns and a rich world map with lots of completely different types of dungeons spread out over it.
- Parties and guilds, which feature shared houses and internal chat.
- 17 distinct races and 13 classes from which to choose.
- A variety of shape-shifting classes and features.
- Powerful macro system, featuring a wizard that makes macro creation easily done in three steps even by beginners.
- Different types of monster AI regarding movement and combat that will result in challenging behavior.

`root #``emerge --ask games-roguelike/tomenet`
## Dwarf Fortress

[Dwarf Fortress](https://bay12games.com/dwarves) is a singleplayer open-ended rogue-like simulation about managing a colony of Dwarves. More info can be found on the game's wiki page [here](https://wiki.gentoo.org/wiki/Dwarf_Fortress).

`root #``emerge --ask games-roguelike/dwarf-fortress`
## ADOM

[Ancient Domains Of Mystery](https://www.adom.de/home/index.html), shortened to ADOM, is a rogue-like RPG with multiple game modes, including a story and multiplayer mode. The game has both ASCII and sprite graphics. A youtube demo can be found [here](https://www.youtube.com/watch?v=ChtBuBrFYc8)

`root #``emerge --ask games-roguelike/adom`
## Angband

[Angband](https://rephial.org) is an open-source singleplayer dungeon crawler. The game supports ASCII and sprite graphics. The game has has sounds which can be enabled with the sound USE flag. View a youtube demo [here](https://www.youtube.com/watch?v=BDidsq-HQP8)

`root #``emerge --ask games-roguelike/angband`
## Hengband

[Hengband](https://hengband.github.io/) is a variant of Angband with a Japanese Fantasy theme. View a youtube demo [here](https://www.youtube.com/watch?v=KqQsUweMCKU). The game has both an English and a Japanese version, the Japanese version can be installed with the l10n\_ja USE flag.

`root #``emerge --ask games-roguelike/hengband`
## ZAngband

ZAngband is an enhanced version of Angband. View a youtube demo [here](https://www.youtube.com/watch?v=LYCcpw8T_7E)

`root #``emerge --ask games-roguelike/zangband`
## Moria

[Moria](https://umoria.org/), also known as Umoria, also known as The Dungeons of Moria, is a modern port of the 1986 open-source game Moria. The game is a single-player dungeon crawler where you descend into the Dungeons of Moria in hopes of defeating the Balrog. View a youtube demo [here](https://www.youtube.com/watch?v=MnKyvlexxgM)

`root #``emerge --ask games-roguelike/moria`
## Nethack

[Nethack](https://Nethack.org) is an open-source singleplayer roguelike dungeon crawler. The game has ASCII, Curses, or tile graphics. View a youtube demo [here](https://www.youtube.com/watch?v=8L8LiQ-cIWA)

`root #``emerge --ask games-roguelike/nethack`
## Powder

[Powder](http://zincland.com/powder/) is an open-source singleplayer dungeon crawler roguelike originally released on the Gameboy Advance. The game has been ported to a wide variety of systems such as the OpenPandora Pandora or the PlayStation3's OtherOS. View a youtube demo [here](https://www.youtube.com/watch?v=bqr094lCaHo).

`root #``emerge --ask games-roguelike/powder`
## S.C.O.U.R.G.E.

[S.C.O.U.R.G.E.](https://sourceforge.net/projects/scourge/) is an open-source roguelike with a 3d user interface. You control 4 heroes working for a dungeon janitorial service on a quest for treasure, the game has an isometric view, similar to Diablo.

`root #``emerge --ask games-roguelike/scourge`
## ToME

[Tales of Middle Earth (ToME)](https://www.t-o-m-e.net/) is an open-source fantasy adventure roguelike inspired by J.R.R Tolkiens *Lord of The Rings* books as well as Anne McCafferey's *Pern* novels. It is forked from ZAngband. The game has ASCII graphics with support for graphical tilesets. View a youtube demo [here](https://www.youtube.com/watch?v=lz-Ni1dibbQ).

`root #``emerge --ask games-roguelike/tome`
## Warp Rogue

Warp Rogue is an open-source gothic science fantasy roguelike with some Warhammer 40k influences. The game was formerly known as "Tower of Doom". The game's website has gone down for an unkown reason, find third-party screenshots [here](https://www.abandonwaredos.com/abandonware-game.php?abandonware=Warp+Rogue&gid=3310).

`root #``emerge --ask games-roguelike/wrogue`
## Crossfire

[Crossfire](https://crossfire.real-time.com/) is an open-source cooperative RPG set in a medieval fantasy world. The game is still in active development. View a youtube tutorial [here](https://www.youtube.com/watch?v=gndu2v7Z46I). To play emerge the client program:

`root #``emerge --ask games-roguelike/crossfire-client`
For playing the game emerge the client

To host a server, emerge the server program:

`root #``emerge --ask games-server/crossfire-server`
## Faster Than Light

[Faster Than Light (FTL)](https://subsetgames.com/ftl.html)is a closed-source space-simulation Real Time Strategy roguelike-like game. In order to work you must purchase the game and include it in your ${DISTDIR}, see the [DISTDIR](https://wiki.gentoo.org/wiki/DISTDIR) page for more information. You can buy the game from Subset games' homepage or through Good Old Games. You must agree to a EULA before you can emerge either package, see more information about package licenses [here](https://wiki.gentoo.org/wiki/Package.license). View a youtube demo [here](https://www.youtube.com/watch?v=BbBXEuAxHQA). You can install the FTL package with:

`root #``emerge --ask games-roguelike/FTL`
or you can install the GOG version with:

`root #``emerge --ask games-roguelike/FTL-gog`
## Neon Chrome

[Neon Chrome](https://neonchromegame.com/) is a closed-source cyberpunk top-down action shooter with rogue-like elements. You must purchase the game from Humble Bundle and include it in your ${DISTDIR} to emerge it. The game has an all-rights-reserved license. See a youtube demo [here](https://www.youtube.com/watch?v=dDAkVKO6rqI)

`root #``emerge --ask games-roguelike/neon-chrome`
