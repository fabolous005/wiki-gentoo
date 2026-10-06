<!-- source: https://wiki.gentoo.org/wiki/Games/emulation | group: Gentoo Wiki (Main) | wiki-title: Games/emulation -->
---
title: Games/emulation
url: https://wiki.gentoo.org/wiki/Games/emulation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-19"
categories: ['https://packages.gentoo.org/categories/games-emulation']
fingerprint: a6561d760293ab5d
license: CC BY-SA 4.0
---

# Games/emulation

[Games](https://wiki.gentoo.org/wiki/Games)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article provides an overview of game emulators available in the ::gentoo ebuild repository.

## DeSmuME

DeSmuME is a Nintendo DS emulator running homebrew demos and commercial games.
View a YouTube demo [here](https://www.youtube.com/watch?v=oauosUpDvRI).

`root #``emerge --ask games-emulation/desmume`
## Dolphin

Dolphin is an emulator for two recent Nintendo video game consoles: the GameCube and the Wii. It allows PC gamers to enjoy games for these two consoles in full HD (1080p) with several enhancements: compatibility with all PC controllers, turbo speed, networked multiplayer, and even more! View a YouTube demo [here](https://www.youtube.com/watch?v=g3-XUbfnEnU).

`root #``emerge --ask games-emulation/dolphin`
## DOSBox

![](https://wiki.gentoo.org/images/thumb/8/82/Dosbox_screenshot1.png/150px-Dosbox_screenshot1.png)

DOSBox: the famous DOS-emulator that uses the SDL-library. It emulates CPU:286/386 realmode/protected mode, Directory FileSystem/XMS/EMS, Tandy/Hercules/CGA/EGA/VGA/VESA graphics and a SoundBlaster/Gravis Ultra Sound card for excellent sound compatibility with older games. No matter which oldschool DOS game you want to play... *Dune II*, *Monkey Island* or *Syndicate*, DOSBox runs them all. Also see the [compatibility list](https://www.dosbox.com/comp_list.php?letter=a) and the [wiki](https://www.dosbox.com/wiki/Main_Page) for help. View a YouTube demo [here](https://www.youtube.com/watch?v=HpLsaoTXvZc).

`root #``emerge --ask games-emulation/dosbox`
## Higan

Higan (formerly known as bsnes) is a Nintendo multi-system emulator that focuses on accuracy and clean code above all else. It never uses speed or compatibility hacks. As a result, the minimum system requirements are higher than with other emulators. View a YouTube demo [here](https://www.youtube.com/watch?v=eFfa7VXdcxc).

It currently supports the following systems:

- Nintendo Entertainment System/Famicom
- Super Nintendo Entertainment System/Super Famicom
- Game Boy
- Game Boy Color
- Game Boy Advance

Higan also supports the following subsystems:

- Super Game Boy
- BS-X Satellaview
- Sufami Turbo



`root #``emerge --ask games-emulation/higan`
## Mednafen

Mednafen is a command-line driven emulator for many different systems. It has full support for OpenGL and SDL graphics, network play, remappable input configuration, joystick and keyboard support, save states, game rewinding, GSF playback, and screenshots. The systems supported by Mednafen are:

- Atari Lynx
- Neo Geo Pocket
- Neo Geo Pocket Color
- WonderSwan
- Game Boy
- Game Boy Color
- Game Boy Advance
- Nintendo Entertainment System/Famicom
- Super Nintendo Entertainment System/Super Famicom
- Virtual Boy
- PC Engine/TurboGrafx 16 (CD)
- SuperGrafx
- PC-FX
- Sega Game Gear
- Sega Genesis/Megadrive
- Sega Master System
- Sega Saturn
- Sony PlayStation

View a YouTube demo [here](https://www.youtube.com/watch?v=yJTvxi3pJVQ)

`root #``emerge --ask games-emulation/mednafen`
## mGBA

mGBA is a Game Boy Advance emulator which also runs original Game Boy and Game Boy Color software. It is designed to be faster, allowing it to run on lower end systems, and to more accurately emulate the original hardware than other emulators.

`root #``emerge --ask games-emulation/mgba`
## Mupen64Plus

![](https://wiki.gentoo.org/images/thumb/d/da/Mupen64plus_screenshot1.png/150px-Mupen64plus_screenshot1.png)

Mupen64Plus is a cross-platform plugin-based fork of the famous Mupen64 Nintendo 64 emulator. Included are four MIPS R4300 CPU emulators, with dynamic recompilers for 32-bit **x86** and 64-bit **amd64** systems, and necessary plugins for audio, graphical rendering (RDP), signal co-processor (RSP), and input. An OpenGL video plugin is included (called RiceVideo). You can use the gui (`ui-m64py` USE flag) or use the command line interface (run: mupen64plus --nogui). View a YouTube demo [here](https://www.youtube.com/watch?v=SAq6EwHmuVE).

`root #``emerge --ask games-emulation/mupen64plus`
## PCSX2

PCSX2 is a PlayStation 2 emulator for x86 computers. It supports most PlayStation 2 games with high compatibility rates and can apply enhancements to gameplay, including features like widescreen patches, improved resolutions or use of custom textures. Check [PCSX2](https://wiki.gentoo.org/wiki/PCSX2) for more information.

`root #``emerge --ask games-emulation/pcsx2`
## VBA-M

![](https://wiki.gentoo.org/images/thumb/8/89/Vbam_screenshot1.png/150px-Vbam_screenshot1.png)

VBA-M is a fork of VisualBoyAdvance and emulates Game Boy, Game Boy Color, Game Boy Advance, Super Game Boy and Super Game Boy 2. It makes an effort to bring some cohesion to the multiplicity of releases floating around the community and is actively maintained. Examples of games running on VBA-M include *Zelda - Link's Awakening*, *Dragon Ball Advanced* and [other highlights](https://en.wikipedia.org/wiki/List_of_best-selling_Game_Boy_Advance_video_games).

`root #``emerge --ask games-emulation/vbam`
## Yabause

Yabause is a Sega Saturn emulator with the following features:

- Booting games from CD-ROM and ISO files.
- Booting games with either an emulated or original BIOS.
- Screenshot support.
- Save game backups.
- Cheat system.
- Fullscreen playing.
- Multiple debugging options.
- Joystick support.
- Region select.

View a YouTube demo [here](https://www.youtube.com/watch?v=l5JFTbVK2wI).

`root #``emerge --ask games-emulation/yabause`
## ZSNES

![](https://wiki.gentoo.org/images/thumb/1/1c/Zsnes_screenshot1.png/150px-Zsnes_screenshot1.png)

[more...](http://sourceforge.net/p/zsnes/wiki/Home/)

ZSNES is a Super Nintendo emulator and is known for high compatibility with commercial games. Because it uses x86 assembly it does not use much resources and has decent performance even on old hardware. It's very configurable and runs known games such as *Secret of Mana* or *Final Fantasy 4*. You might also want to check out the [Top 100 SNES Games list](http://www.retro-sanctuary.com/Top-100-SNES-Games-Page-1.html).

`root #``emerge --ask games-emulation/zsnes`
