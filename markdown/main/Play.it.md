<!-- source: https://wiki.gentoo.org/wiki/Play.it | group: Gentoo Wiki (Main) | wiki-title: Play.it -->
---
title: Play.it
url: https://wiki.gentoo.org/wiki/Play.it
hostname: gentoo.org
sitename: Play.it
date: "2025-07-06"
fingerprint: "85b7ba0ac8c721f5"
license: CC BY-SA 4.0
---

# Play.it

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

./play.it is [libre software](https://en.wikipedia.org/wiki/Free_software) that automates the build of native packages for multiple distributions, including Gentoo, from [DRM-free](https://en.wikipedia.org/wiki/Digital_rights_management) installers for commercial games. The generated packages are then installed using the standard tools provided by the distribution.

Native Linux games are supported, as well as games developed for other systems thanks to tools like [Wine](https://wiki.gentoo.org/wiki/Wine), [DOSBox](https://wiki.gentoo.org/wiki/Games/emulation#DOSBox) and [ScummVM](https://wiki.gentoo.org/wiki/Games/engines#ScummVM).

## Installation

An [overlay](https://wiki.gentoo.org/wiki/Ebuild_repository) is provided allowing for an easy installation of ./play.it on Gentoo: [BetaRayʼs Gentoo overlay](https://framagit.org/BetaRays/gentoo-overlay)

## Usage

Assuming the game installer is called setup.exe, using ./play.it to install a game is a two-steps process:

1. Run ./play.it by giving it the path to the game installer:
  - `user $``play.it ~/Downloads/setup.exe`
2. Follow the [emerge](https://wiki.gentoo.org/wiki/Portage) instructions provided at the end of the process, or use the provided [quickunpkg script](https://downloads.dotslashplay.it/resources/gentoo/).

## Contact

Contact information can be found [here](https://doc.dotslashplay.it/contact.en.xhtml).

## See also

- [Games/engines](https://wiki.gentoo.org/wiki/Games/engines) — provides an overview of game engines available in the ::gentoo ebuild repository.
- [Games/emulation](https://wiki.gentoo.org/wiki/Games/emulation) — provides an overview of game emulators available in the ::gentoo ebuild repository.
- [Wine](https://wiki.gentoo.org/wiki/Wine) — an application compatibility layer that allows [Microsoft Windows](https://en.wikipedia.org/wiki/Microsoft_Windows) software to run on Linux and other [POSIX](https://en.wikipedia.org/wiki/POSIX)-compliant operating systems.
