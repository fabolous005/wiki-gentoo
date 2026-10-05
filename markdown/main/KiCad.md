<!-- source: https://wiki.gentoo.org/wiki/KiCad | group: Gentoo Wiki (Main) | wiki-title: KiCad -->
---
title: KiCad
url: https://wiki.gentoo.org/wiki/KiCad
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-11-06"
fingerprint: "86437348e836b9f2"
license: CC BY-SA 4.0
---

# KiCad

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

KiCad is a cross-platform and open-source electronics design automation (EDA) suite for creation of electronic schematic diagrams and printed circuit board (PCB) artwork. Jean-Pierre Charras began development of the program in 1992<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> in order to have "a tool to teach electronics to his students, and also to learn how to code in C++".<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> CERN has contributed to its development since 2013<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> and KiCad joined the Linux Foundation in 2019.<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup> The "Ki" part of the program name was based on a friend's company's name<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup> because other options were taken already.

## Installation

### USE flags


| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [examples](https://packages.gentoo.org/useflags/examples) | Install examples, usually source code | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [openmp](https://packages.gentoo.org/useflags/openmp) | Build support for the OpenMP (support parallel computing), requires >=sys-devel/gcc-4.2 built with USE="openmp" | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 

### Emerge

Emerging [sci-electronics/kicad](https://packages.gentoo.org/packages/sci-electronics/kicad) will install the basic program.

`root #``emerge --ask sci-electronics/kicad`
### Additional software

To install all of:

- [sci-electronics/kicad-footprints](https://packages.gentoo.org/packages/sci-electronics/kicad-footprints)
- [sci-electronics/kicad-packages3d](https://packages.gentoo.org/packages/sci-electronics/kicad-packages3d)
- [sci-electronics/kicad-symbols](https://packages.gentoo.org/packages/sci-electronics/kicad-symbols)
- [sci-electronics/kicad-templates](https://packages.gentoo.org/packages/sci-electronics/kicad-templates)

at once,

`root #``emerge --ask sci-electronics/kicad-meta`
The KiCad footprint libraries can be individually downloaded [here](https://kicad.github.io/footprints/) if desired.

### Flatpak

A KiCad [Flatpak](https://wiki.gentoo.org/wiki/Flatpak) is available.[\[6\]](https://wiki.gentoo.org#cite_note-6)

`user $``flatpak install flathub org.kicad.KiCad`
## Configuration

### Environment variables

The following paths can be viewed and edited under 'Preferernces' > 'Configure Paths...'.

- KICAD7\_3DMODEL\_DIR: Base path of 3D models used in footprints.
- KICAD7\_3RD\_PARTY: Location for plugins, libraries, and color themes installed by the Plugin and Content Manager.
- KICAD7\_FOOTPRINT\_DIR: Base path of footprint library files.
- KICAD7\_SYMBOL\_DIR: Base path of symbol library files.
- KICAD7\_TEMPLATE\_DIR: Location of project templates installed with KiCad.
- KICAD\_USER\_DIR: Location of local user content, such as libraries, plugins and themes.
- KICAD\_USER\_TEMPLATE\_DIR: Location of personal project templates.

### Files

User configuration files are stored in \~/.config/kicad.

The default path for user content (KICAD\_USER\_DIR) is \~/.local/share/kicad/\<version>.

### Vendor libraries

There is a currently unmaintained<sup>[\[7\]](https://wiki.gentoo.org#cite_note-7)</sup> DigiKey Symbol and Footprint Library for KiCad 5,<sup>[\[8\]](https://wiki.gentoo.org#cite_note-8)</sup> as well as a partner library from suppliers.[\[9\]](https://wiki.gentoo.org#cite_note-9)

## Usage

Additional libraries, plugins and themes can be installed via the Plugin and Content Manager (PCM). Some libraries not distributed via the official KiCad respository, such as [Espressif KiCad Library](https://github.com/espressif/kicad-libraries), are installed by downloading the release and adding via PCM.

### KiCad-Push-to-DigiKey

DigiKey recently released an add-on to assist with collection of parts in a schematic file and pushing them to their myLists service.[\[10\]](https://wiki.gentoo.org#cite_note-10)

## Troubleshooting

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose sci-electronics/kicad`
## See also

## External resources

- [https://klc.kicad.org/](https://klc.kicad.org/) — KiCad Library Convention: a set of requirements for contributing to the official KiCad libraries
- [https://ohwr.org/project/cern-kicad/wikis/home](https://ohwr.org/project/cern-kicad/wikis/home) — CERN BE-CO-HT Contributions to KiCad
- [https://ohwr.org/welcome](https://ohwr.org/welcome) — the Open Hardware Repository
- [https://www.amazon.com/Complete-Reference-Manual-Jean-Pierre-Charras/dp/1680921274](https://www.amazon.com/Complete-Reference-Manual-Jean-Pierre-Charras/dp/1680921274) — KiCad Complete Reference Manual, 2018 edition
- [https://www.kicad.org/made-with-kicad/](https://www.kicad.org/made-with-kicad/) — A list of projects made by users, along with links to their respective git repositories.
- [https://dev-docs.kicad.org/en/contribute/](https://dev-docs.kicad.org/en/contribute/) — Guidance on contributing to KiCad
