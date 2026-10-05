<!-- source: https://wiki.gentoo.org/wiki/Anki | group: Gentoo Wiki (Main) | wiki-title: Anki -->
---
title: Anki
url: https://wiki.gentoo.org/wiki/Anki
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-11"
fingerprint: b053277d40a7b8f0
license: CC BY-SA 4.0
---

# Anki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Anki** is a flashcard memory training program that uses the science of spaced repetition to expedite the learning process and enhance recall<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

This article covers [Anki Desktop](https://github.com/ankitects/anki), the official desktop application. There's also an optional web synchronization service called [AnkiWeb](https://ankiweb.net/about).

## Installation

### Emerge

Install [app-misc/anki](https://packages.gentoo.org/packages/app-misc/anki) using emerge:

`root #``emerge --ask app-misc/anki`
#### Optional features

Anki offers additional features that can be enabled by installing additional packages. These will be shown to the user after installing Anki. Some notable features are:

\- [dev-python/orjson](https://packages.gentoo.org/packages/dev-python/orjson): Will (in theory) result in faster database operations.

### Flatpak

Anki is also packaged as a Flatpak, which may be desirable since [app-misc/anki](https://packages.gentoo.org/packages/app-misc/anki) depends on [dev-python/PyQtWebEngine](https://packages.gentoo.org/packages/dev-python/PyQtWebEngine) to build.

The Anki flatpak is available from [Flathub](https://flathub.org/apps/details/net.ankiweb.Anki):

`user $````
flatpak install flathub net.ankiweb.Anki
```
`user $````
flatpak run net.ankiweb.Anki
```
### Python Wheel

Anki's beta version can be installed on both x86\_64 and ARM systems using [pip](https://wiki.gentoo.org/wiki/Pip) rather easily. Refer to [Anki's documentation](https://betas.ankiweb.net/#via-pypipip) as for how do this.

## Configuration

### Environment variables

- `ANKIDEV` - If set, additional logging messages will be printed to standard output and automatic backups will be disabled.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-env-2)</sup>
- `TRACESQL` - If set, SQL statements will be printed at execution time.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-env-2)</sup>
- `LOGTERM` - If set, messages bound for *collection2.log* will also be printed to standard output.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-env-2)</sup>
- `ANKI_PROFILE_CODE` - If set, Python profiling data will be sent to standard output upon exit.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-env-2)</sup>

#### Files

- $XDG\_DATA\_HOME/Anki2 - the main configuration directory<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>.

## Usage

### Invocation

`user $``anki --help````
usage: anki [OPTIONS] [file to import]
Anki 2.1.15
optional arguments:
  -h, --help            show this help message and exit
  -b BASE, --base BASE  path to base folder
  -p PROFILE, --profile PROFILE
                        profile name to load
  -l LANG, --lang LANG  interface language (en, de, etc)
```
## Troubleshooting

See the [official Linux Anki documentation](https://docs.ankiweb.net/platform/linux/installing.html) for Linux-specific issues.

## External resources

- [AnkiWeb](https://ankiweb.net/about) – The companion web service to the desktop Anki application.
