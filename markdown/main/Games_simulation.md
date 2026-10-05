<!-- source: https://wiki.gentoo.org/wiki/Games/simulation | group: Gentoo Wiki (Main) | wiki-title: Games/simulation -->
---
title: Games/simulation
url: https://wiki.gentoo.org/wiki/Games/simulation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-07-08"
categories: ['https://packages.gentoo.org/categories/games-simulation']
fingerprint: "3a8fc2788465a8da"
license: CC BY-SA 4.0
---

# Games/simulation

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article provides an overview of simulator games that are available in the ::gentoo ebuild repository.

## FlightGear

![](https://wiki.gentoo.org/images/thumb/9/97/FlightGear_screenshot1.png/200px-FlightGear_screenshot1.png)

FlightGear is an open-source flight simulator whose goal is to create a sophisticated and open flight simulator framework for use in research or academic environments, pilot training, as an industry engineering tool, for DIY-ers to pursue their favorite interesting flight simulation idea, and last but certainly not least as a fun, realistic, and challenging desktop flight simulator. View a YouTube demo [here](https://www.youtube.com/watch?v=yvWJWueZ6mI).

`root #``emerge --ask games-simulation/flightgear`
## OpenTTD

![](https://wiki.gentoo.org/images/thumb/a/af/OpenTTD_shot.png/200px-OpenTTD_shot.png)

OpenTTD is a free real-time strategy game. It was originally one of many extensions of the proprietary Transport Tycoon Deluxe. Then it was an effort to reverse engineer it to have a fully free Transport Tycoon clone, retaining the extensions. It has since not only achieved its goal of remaking Transport Tycoon Deluxe, but also vastly surpassed the original game with a huge amount of unique features. For those who haven't played Transport Tycoon, OpenTTD is a planning and simulation and strategy game in which the player founds a transport company. The player must make this company successful through transportation of passengers and freight. There are trains, trams, cars, busses, aeroplanes, boats and more vehicles to enable this. The player must construct train stations, bus stations, railways, roads, *etc.* as well as the towns and industries on the map. The game play is incredibly deep, and probably perplexing for newcomers — but leads to some truly unique experiences once you get to know it. The game is also highly configurable and customizable. It lets you not only tweak seemingly all game rules, but also change the graphics, sounds, AI scripts and more. OpenTTD features networked multiplayer (LAN or Internet). View a YouTube demo [here](https://www.youtube.com/watch?v=bc8WKLsR9Ds).

`root #``emerge --ask games-simulation/openttd`
Note: OpenTTD requires timidity or fluidsynth support to play background music. This means you will need to provide a soundfont for them:

`root #``emerge --ask media-sound/fluid-soundfont`
## OpenRCT2

OpenRCT2 is an open-source re-implementation of RollerCoaster Tycoon 2 (RCT2), expanding the game with new features, fixing bugs and raising game limits. The gameplay revolves around building and maintaining an amusement park containing attractions, shops and facilities. The player must try to make a profit and maintain a good park reputation whilst keeping the guests happy. OpenRCT2 allows for both scenario and sandbox play. Scenarios require the player to complete a certain objective in a set time limit whilst sandbox allows the player to build a more flexible park with optionally no restrictions or finance.

`root #``emerge --ask games-simulation/openrct2`
## CorsixTH

CorsixTH aims to re-implement the game engine of Theme Hospital, and be able to load the original game data files. After most of the original engine has been re-implemented in open source code, the project will serve as a base from which extensions and improvements to the original game can be made.

The player has to design and build a hospital which will attract patients with comical illnesses and is a must play for anyone that enjoys dark humor and sim games.

`root #``emerge --ask games-simulation/corsix-th`
