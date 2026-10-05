<!-- source: https://wiki.gentoo.org/wiki/Fonts | group: Gentoo Wiki (Main) | wiki-title: Fonts -->
---
title: Fonts
url: https://wiki.gentoo.org/wiki/Fonts
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-16"
fingerprint: "8fad13d76a8761d1"
license: CC BY-SA 4.0
---

# Fonts

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a home page for information about using fonts on Gentoo.

In general usage, a *font* is typically a file containing one or more variations of a [typeface](https://en.wikipedia.org/wiki/typeface). In typographical usage, a *font* is a particular size, weight and style of a typeface.

The best starting point for general information about configuration, use and management of fonts on Gentoo, particularly for software running under X or a Wayland compositor (including [terminal emulators](https://wiki.gentoo.org/wiki/Terminal_emulator)), is the [Fontconfig](https://wiki.gentoo.org/wiki/Fontconfig) page.

Additionally:

- For information about configuring fonts for the [Linux console](https://en.wikipedia.org/wiki/Linux_console) specifically (rather than for GUI-based terminal emulators), refer to the [Fonts/Console](https://wiki.gentoo.org/wiki/Fonts/Console) page.

- For information about software for working with fonts, refer to the [Fonts/Software](https://wiki.gentoo.org/wiki/Fonts/Software) page.

- For background on font-related concepts, terminology, and systems (e.g. Unicode), refer to the [Fonts/Background](https://wiki.gentoo.org/wiki/Fonts/Background) page.

- For a high-level introduction to the systems involved in displaying text on Linux, refer to the external article "[Modern text rendering with Linux: Overview](https://mrandri19.github.io/2019/07/24/modern-text-rendering-linux-overview.html)".

## Font installation

A variety of fonts are provided by the `media-fonts` package category. Fonts provided by the `gentoo` repository can be listed by viewing the contents of the /var/db/repos/gentoo/media-fonts/ directory, or by using [eix](https://wiki.gentoo.org/wiki/Eix):

`user $``eix -C media-fonts`
[media-fonts/fonts-meta](https://packages.gentoo.org/packages/media-fonts/fonts-meta) is a meta package providing fonts to cover most needs.

[media-fonts/corefonts](https://packages.gentoo.org/packages/media-fonts/corefonts) provides Microsoft's TrueType core fonts.

### Manual font installation

#### Global

System administrators (those with root privileges) can copy fonts into /usr/local/share/fonts. This will make fonts available to any user on the system.

`root #``cp /home/larry/Downloads/Inconsolata.otf /usr/local/share/fonts`
#### Per-user

Users can create a .local/share/fonts directory in their home directory. Font files can then be added to that directory, or to a subdirectory of that directory:

`user $````
mkdir -p ~/.local/share/fonts
```
`user $````
cp ~/Downloads/Inconsolata.otf ~/.local/share/fonts
```
To make a newly-installed font available to applications, refresh the Fontconfig cache via [fc-cache(1)](https://man.archlinux.org/man/fc-cache.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``fc-cache -fv`
Use an application such as a [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) or an office program to confirm that the font is now available.

### Font support for specific characters

Gentoo doesn't install many fonts by default. If an application needs to display a particular character, but the current font (or, perhaps, any font known to the application) doesn't have a glyph for it, the character will be rendered using the *.notdef* character, informally known as *tofu*. Tofu is typically displayed as either:

- an empty square, ☐;
- a box with an X in it, ☒;
- a box with a question mark in it, ⍰;
- a box containing the Unicode code point in hexadecimal.

To check if any installed font provides a glyph for a particular Unicode code point, use [fc-list(1)](https://man.archlinux.org/man/fc-list.1.en) [- provided by the](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [media-libs/fontconfig](https://packages.gentoo.org/packages/media-libs/fontconfig) package - to list all installed fonts with such a glyph, specifying the code point in hexadecimal:

`user $``fc-list ':charset=3088'`
#### Noto

Google's "Noto" typeface family is an attempt to provide a single typeface with glyphs for all assigned Unicode code points, which cover most of the world's scripts; the name "Noto" is an abbreviation of "No Tofu".

On Gentoo, the Noto family is available via the [media-fonts/noto](https://packages.gentoo.org/packages/media-fonts/noto) (including support for Arabic, Bengali, Persian, Tamil, and Thai), [media-fonts/noto-cjk](https://packages.gentoo.org/packages/media-fonts/noto-cjk) (including support for Chinese, Japanese and Korean), and [media-fonts/noto-emoji](https://packages.gentoo.org/packages/media-fonts/noto-emoji) packages.

Once installed, Noto Emoji can be configured for use as a fallback font (used when a glyph does not exist in the selected font) for Emoji by running:

`root #``eselect fontconfig enable 75-noto-emoji-fallback.conf`
Note that Web browsers tend to use their own font selection logic; often simply installing the package is sufficient.

#### Symbols

One option for a symbol font is Symbola, provided by the [media-fonts/ttf-ancient-fonts::guru](https://gpo.zugaina.org/Overlays/guru/media-fonts/ttf-ancient-fonts) package in the [GURU overlay](https://wiki.gentoo.org/wiki/Project:GURU). This font has previously been provided by the [media-fonts/symbola::guru](https://gpo.zugaina.org/Overlays/guru/media-fonts/symbola) package, [which is now obsolete](https://gitweb.gentoo.org/repo/proj/guru.git/commit/?id=79239b5207c500a5cd0defd9012345fbe97db3ea).


## Picking fonts

The following are some recommendations regarding well-known fonts:


### Liberation

Provided by [media-fonts/liberation-fonts](https://packages.gentoo.org/packages/media-fonts/liberation-fonts).

This is the [Gentoo Fonts team](https://wiki.gentoo.org/wiki/Project:Fonts)'s recommendation for default Latin fonts.

Pros:

- [Red Hat's](https://en.wikipedia.org/wiki/Red_Hat) fonts.
- Metric-compatible with MS TrueType [corefonts](https://en.wikipedia.org/wiki/Corefonts).
- Have a decent, modern look.
- Covers about 2,600 code points.

Cons:

- Latin, Greek, Cyrillic, and Hebrew only.
- A few glyphs may have hinting trouble.


### ChromeOS fonts

ChromeOS core fonts, provided by [media-fonts/croscorefonts](https://packages.gentoo.org/packages/media-fonts/croscorefonts), and ChromeOS extra fonts, provided by [media-fonts/crosextrafonts-caladea](https://packages.gentoo.org/packages/media-fonts/crosextrafonts-caladea) and [media-fonts/crosextrafonts-carlito](https://packages.gentoo.org/packages/media-fonts/crosextrafonts-carlito).

Pros:

- Metric compatible with Microsoft corefonts and two of the ClearType fonts
- very good hinting
- includes more glyphs and languages over Liberation

Cons:

- Some packages have a [hard dependency](https://bugs.gentoo.org/show_bug.cgi?id=627842) on [media-fonts/corefonts](https://packages.gentoo.org/packages/media-fonts/corefonts) or [media-fonts/liberation-fonts](https://packages.gentoo.org/packages/media-fonts/liberation-fonts), so in many cases users can't *only* install the ChromeOS fonts, unless performing some hackery via [package.provided](https://wiki.gentoo.org/wiki//etc/portage/profile/package.provided).


### Linux Libertine

Provided by [media-fonts/libertine](https://packages.gentoo.org/packages/media-fonts/libertine).

Pros:

- Very similar to Liberation, covering about 2,700 code points.
- Linux Libertine itself is proportional serif only, but the package contains less extensive sans and mono fonts, as well.
- Can be used as a fallback for some glyphs not in Liberation.

Cons:

- Latin, Greek, Cyrillic, and Hebrew only.
- Sans and mono fonts are limited.


### Noto

Provided by [media-fonts/noto](https://packages.gentoo.org/packages/media-fonts/noto).

Recommended as a fallback for many glyphs not covered by Liberation.

Pros:

- Google's font family that aims to support all the world's languages (well over 60,000 code points).
- Goes well with Liberation or Droid.
- Adobe's Source Han Sans fonts are included for [CJK](https://en.wikipedia.org/wiki/CJK).

Cons:

- Big download.


### DejaVu

Provided by [media-fonts/dejavu](https://packages.gentoo.org/packages/media-fonts/dejavu).

Pros:

- Many styles and covers a lot of code points (about 6,100 for sans).

Cons:

- Exceptionally wide — even condensed is wider than same-height monospace. Overall second to [Verdana](https://en.wikipedia.org/wiki/Verdana) (an MS font) in width. Sans-serif font is only average.


### Droid

Provided by [media-fonts/droid](https://packages.gentoo.org/packages/media-fonts/droid).

Pros:

- Covers a lot of code points and scripts.

Cons:

- Very dry, wide yet thin glyphs. Clearly designed with handheld devices and their small screens in mind.


### Gentium Plus

Provided by [media-fonts/sil-gentium](https://packages.gentoo.org/packages/media-fonts/sil-gentium).

Pros:

- Fairly distinctive; might appeal to people who like narrow fonts.

Cons:

- Serif only. As with other [SIL](https://en.wikipedia.org/wiki/SIL_International) fonts, the hinting is questionable.


### Ubuntu

Provided by [media-fonts/ubuntu-font-family](https://packages.gentoo.org/packages/media-fonts/ubuntu-font-family).

Pros:

- Used in [Ubuntu](<https://en.wikipedia.org/wiki/Ubuntu_(operating_system)>) (obviously).
- A distinctive font family with a style which might not appeal to everyone.
- Overall looks good and covers a fair number of code points.

Cons:

- Only the sans-serif font is truly polished; narrow and monospaced versions are unfinished.
- No known serif font that would accompany it well.


### URW

Provided by [media-fonts/urw-fonts](https://packages.gentoo.org/packages/media-fonts/urw-fonts).

Pros:

- Metric compatible with popular Adobe fonts (among others?).

Cons:

- Seem to require slight hinting.


### MS TrueType corefonts

Provided by [media-fonts/corefonts](https://packages.gentoo.org/packages/media-fonts/corefonts).

Pros:

- Includes most fonts used in documents and on the web.

Cons:

- MS does not distribute them nowadays, so the available fonts are from many years ago and do not reflect their current state (not to mention the state of the art). Obviously, lacks fonts introduced more recently.
- Require full hinting.


### Unifont

Provided by [media-fonts/unifont](https://packages.gentoo.org/packages/media-fonts/unifont).

Pros:

- Covers a lot of code points.

Cons:

- In addition to being *ugly as sin*, it also fails some basic requirements to be considered a typeface. Is it sans-serif? Is it serif? *Please never use this.*

## See also

- [Fontconfig](https://wiki.gentoo.org/wiki/Fontconfig) — intended to provide uniform font selection and configuration amongst all GUI applications.
- [Fonts/Background](https://wiki.gentoo.org/wiki/Fonts/Background) — a quick and informal introduction to font-related concepts, terminology, and systems, with the aim of facilitating understanding and solving font-related issues
- [Fonts/Console](https://wiki.gentoo.org/wiki/Fonts/Console)
- [Fonts/Software](https://wiki.gentoo.org/wiki/Fonts/Software) — list of end-user software for working with fonts
- [Localization/Guide/The\_Euro\_symbol](https://wiki.gentoo.org/wiki/Localization/Guide/The_Euro_symbol) — how to display the Euro symbol (€) for the console and in X.

## External resources

- [Font Library](https://fontlibrary.org/) - Font distribution website that beautifully displays fonts.
