<!-- source: https://wiki.gentoo.org/wiki/Fonts/Background | group: Gentoo Wiki (Main) | wiki-title: Fonts/Background -->
---
title: Fonts/Background
url: https://wiki.gentoo.org/wiki/Fonts/Background
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-12"
fingerprint: "84b4a0932ac302dd"
license: CC BY-SA 4.0
---

# Fonts/Background

[Fonts](https://wiki.gentoo.org/wiki/Fonts)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page intends to provide a quick and informal introduction to font-related concepts, terminology, and systems, with the aim of facilitating understanding and solving font-related issues. As such, this page occasionally sacrifices accuracy in order to convey the essentials.

## Writing systems

The term character typically refers to what is better described as a grapheme - the smallest 'unit' within a writing system. An alphabet is a collection of graphemes which represent both [consonants](https://en.wikipedia.org/wiki/consonant) and [vowels](https://en.wikipedia.org/wiki/vowel). The core of the English language uses an alphabet of 26 letters, with each letter having a lower-case and an upper-case variant.

However, not all writing systems have an 'alphabet'. There are also, for example:

- [Abjads](https://en.wikipedia.org/wiki/abjad), such as the Arabic script, in which only consonants are represented.

- [Abugidas](https://en.wikipedia.org/wiki/abugida), or 'alphasyllabaries', such as Devanagari - used for writing Hindu - in which each grapheme represents a consonsant together with a vowel.

- [Logographic](https://en.wikipedia.org/wiki/logographic) scripts, such as Chinese, where a single grapheme represents an entire word or concept.

Further, not all writing systems make a distinction between upper-case and lower-case. Indeed, usage of upper-case and lower-case can even vary between languages using basically the same script, as in the case of English and German.

A glyph is a particular stylistic representation of a grapheme - for example, the glyph for a serif version of the 'a' grapheme can be different from a sans-serif version of that same grapheme. A font within a typeface family provides a collection of similarly-styled glyphs, each representing a particular grapheme.

## Character sets

A character set, character map or code page is a collection of graphemes for one or more writing systems, with each grapheme occupying a specific code point, designated by a number. An overview of various character sets is available in the [charsets(7)](https://man.archlinux.org/man/charsets.7.en) [man page.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

### Encodings

An encoding is a specific representation of a collection of graphemes at the hardware level. ASCII, the "American Standard Code for Information Interchange", is an encoding, using 7 bits to assign a number to various letters, numbers, and punctuation used in the core of English (cf. the [ascii(7)](https://man.archlinux.org/man/ascii.7.en) [man page). For example, 'A' is represented by the number 65, 'a' by the number 97. To represent these numbers in hardware, we need to represent them in binary, and additionally, choose an](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [endianness](https://en.wikipedia.org/wiki/endianness), i.e. big-endian ('BE') or little-endian ('LE'), describing the position of the most significant bit. A big-endian binary representation of the number 65 is 1000001: the first digit represents the 64s place, the last digit represents the 1s place, and a '1' in both these positions, together with '0' in all other places, thus represents 64 + 1 = 65.

Importantly, if a given piece of computer software is expecting data in one encoding, but that data is actually in the form of a *different encoding*, various issues can result, ranging in severity from 'slightly inconvenient' to 'critical'. The visual result of encoding issues is known as [mojibake](https://en.wikipedia.org/wiki/mojibake); in the context of Unicode in particular, it can result in one or more instances of the generic Unicode [replacement character](https://en.wikipedia.org/wiki/replacement_character), '�', representing an invalid encoding at that position.

One example of encoding issues was the "[Bush hid the facts](https://en.wikipedia.org/wiki/Bush_hid_the_facts)" bug in Windows, which caused:

text encoded in ASCII to be interpreted as if it were UTF-16LE, resulting in garbled text. When the string "Bush hid the facts", without quotes, was put in a new Notepad document and saved, closed, and reopened, the nonsensical sequence of the Chinese characters "畂桳栠摩琠敨映捡獴" would appear instead.


Since encodings such as ASCII and Latin-1 can be entirely represented by one byte, people who have never encountered other encodings have often assumed that a 'character' is identical to a 'byte', and this assumption has often been reflected in code. However, this is not the case, as demonstrated below.

### Unicode

Seven bits only allows for 128 code points / graphemes. If an entire byte is used, there's space for 2^8 = 256 code points / graphemes. For example, [Latin-1](https://en.wikipedia.org/wiki/Latin-1) - more formally, ISO-8859-1 - is an 8-bit encoding of graphemes used in various European languages. Another example is [ISCII](https://en.wikipedia.org/wiki/ISCII), the "Indian Standard Code for Information Interchange", which encodes several writing systems used in India, such as Devanagari and Gujarati.

Still, a byte doesn't provide anywhere near enough space to encode, for example the writing systems used in China and Japan. As a result, encodings were developed for wide characters - graphemes whose encoding required more than one byte. [Shift JIS](https://en.wikipedia.org/wiki/Shift_JIS) is a variable-width encoding for the Japanese language; it uses one or two bytes.  [EUC-CN](https://en.wikipedia.org/wiki/EUC-CN) uses two bytes to represent the GB/T 2312 character set for Chinese.

The issues created by needing different character sets and encodings for different writing systems and languages led to the development of Unicode.

[Unicode](https://en.wikipedia.org/wiki/unicode), more formally the "The Unicode Standard", ISO-10646, is an attempt to gather most current and historical writing systems into a single character set. Quoting Wikipedia:

Version 15.1 of the standard defines 149813 characters and 161 scripts used in various ordinary, literary, academic, and technical contexts.


The codespace for Unicode consists of 1 112 064 valid code points, within an interval represented in hexadecimal notation as 'U+0000' to 'U+10FFFF. Each assigned code point has a name: for example, 'A' is "LATIN CAPITAL LETTER A", 'ç' is "LATIN SMALL LETTER C WITH CEDILLA", 'ı' is "LATIN SMALL LETTER DOTLESS I", 'ग' is "DEVANAGARI LETTER GA", '人' is "CJK IDEOGRAPH-4EBA", 'よ' is "HIRAGANA LETTER YO", and so on.

There are various encodings of Unicode. One such encoding is [UTF-16](https://en.wikipedia.org/wiki/UTF-16), where 'UTF' stands for 'Unicode Transformation Format' and '16' indicates the 16 bits it uses to encode. UTF-16 is a fixed-width encoding: every code point is represented by two bytes. However, its design requires distinct 'BE' (big-endian) and 'LE' (little-endian) variants; the [byte order mark](https://en.wikipedia.org/wiki/Byte_order_mark) may be used at the start of a stream or file to indicate the variant. UTF-16 is used by, for example, the Windows API and the Java programming language.

Another encoding of Unicode is [UTF-8](https://wiki.gentoo.org/wiki/UTF-8). UTF-8 is a variable-width encoding: a particular code point might be encoded by one, two or three bytes. Additionally, UTF-8 is superset of ASCII: the first 128 codepoints are used the same way by both encodings, such that codepoint 65 in both ASCII and UTF-8 is occupied by the English letter 'A'. However, other code points can require more than one byte: for example, 'ζ' requires two bytes, and 'அ' requires three bytes.

In 2024, use of UTF-8 is pervasive. To again quote [Wikipedia](https://en.wikipedia.org/wiki/UTF-8):

UTF-8 is the dominant encoding for the World Wide Web (and internet technologies), accounting for 98.2% of all web pages, 99.0% of the top 10,000 pages, and up to 100% for many languages, as of 2024.


Note that neither UTF-16, nor UTF-8, nor any other encoding, is the same as 'Unicode'. Unicode is an abstract character set; things like UTF-16 and UTF-8 are specific encodings of Unicode for concrete software and hardware purposes.

Various emoji are [available in Unicode](https://en.wikipedia.org/wiki/Emoji#In_Unicode), but not all emoji are represented by a single code point. For example, an emoji with a specific skin tone is created by combining the code point of a particular emoji with a code point used as a 'variation selector'.

National flag emoji are not defined by a single code point per flag, but by combining *two* code points, each representing a [regional indicator symbol](https://en.wikipedia.org/wiki/regional_indicator_symbol), which together refer to a [two-letter country code as defined by the ISO-3166 standard](https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes). In the context of UTF-8, each regional indicator symbol is encoded by four bytes, meaning that a single flag emoji actually uses eight bytes.

The Unicode Consortium provides a [glossary of Unicode terms](https://www.unicode.org/glossary/).

## Fonts

A font, properly speaking, is a specific collection of glyphs within an overall typeface family. A particular typeface family might consist of several fonts: an upright and medium-weight version; an upright and bold version, an italic and medium-weight version; an italic and bold version; and so on.

Note that using Unicode to create 'bold', 'italic', etc. effects doesn't actually involve the use of different fonts; instead, it involves the use of particular codepoints with specific stylistic representations within a single font. For example, 'bolding' the word 'Gentoo' by using the Unicode MATHEMATICAL BOLD codepoints to write '𝐆𝐞𝐧𝐭𝐨𝐨' results in people using screenreaders not hearing the word 'Gentoo' read out, but instead "MATHEMATICAL BOLD CAPITAL G MATHEMATICAL BOLD SMALL E MATHEMATICAL BOLD SMALL N ..."

There's no requirement for a font to provide a glyph for *every* currently assigned Unicode code point. Thus, a given font might contain glyphs for all scripts used by European languages, but provide none for any other scripts; another font might provide glyphs for a wide variety of writing systems, but not provide any glyphs for emoji. When a font doesn't provide such a glyph, the result is often a rectangle containing the hexadecimal representation of the codepoint lacking a glyph, informally referred to as '[tofu](https://en.wikipedia.org/wiki/Noto_fonts#Etymology)'.

Google's "Noto" typeface family is an attempt to provide a single typeface with glyphs for all assigned Unicode code points; the name itself is an abbreviation of "No Tofu". Noto is available via the [media-fonts/noto](https://packages.gentoo.org/packages/media-fonts/noto), [media-fonts/noto-cjk](https://packages.gentoo.org/packages/media-fonts/noto-cjk), and [media-fonts/noto-emoji](https://packages.gentoo.org/packages/media-fonts/noto-emoji) packages.

### Font formats

There are two broad categories of font formats: bitmap and outline.

#### Bitmap fonts

A bitmap font, as its name implies, provides a specific bitmap for a specific grapheme at a particular size. Different sizes require distinct bitmaps.

The Linux console uses bitmap fonts; its current standard font format is [PC Screen Font](https://en.wikipedia.org/wiki/PC_Screen_Font) (PSF).

Originally X used Adobe's [Bitmap Distribution Format](https://en.wikipedia.org/wiki/Bitmap_Distribution_Format) (BDF) fonts, but subsequently moved to [Portable Compiled Format](https://en.wikipedia.org/wiki/Portable_Compiled_Format) (PCF) fonts. Nowadays X usually makes use of scalable outline fonts.

#### Outline fonts

An outline font uses mathematical representations of curves, such as [Bézier curves](https://en.wikipedia.org/wiki/B%C3%A9zier_curve), to define a specific glyph. This means that different sizes can be represented via mathematical transformations. However, manual optical corrections can be required at certain sizes.

Type 1 ('T1') fonts were introduced in 1984 by Adobe as part of the [PostScript](https://en.wikipedia.org/wiki/PostScript) page description language. Adobe [ended support for Type 1 fonts in January 2023](https://helpx.adobe.com/au/fonts/kb/postscript-type-1-fonts-end-of-support.html). Font formats associated with Type 1 fonts include AFM (Adobe Font Metric), PFA (Printer Font ASCII), and PFB (Printer Font Binary).

PostScript level 1 defined 13 fonts (the original '[PostScript fonts](https://en.wikipedia.org/wiki/PostScript_fonts)'), representing four type families: Courier (Regular, Oblique, Bold, Bold Oblique), Helvetica (Regular, Oblique, Bold, Bold Oblique), Times (Roman, Italic, Bold, Bold Italic), and Symbol. These fonts, together with a second symbol font, ITC Zapf Dingbats, form the base set of [PDF](https://en.wikipedia.org/wiki/PDF) fonts (which are nevertheless not *guaranteed* to be available when using a given PDF reader).

PostScript level 2, introduced in 1991, defined 35 fonts, representing ten type families: ITC Avant Garde Gothic (Book, Book Oblique, Demi, Demi Oblique) ITC Bookman (Light, Light Italic, Demi, Demi Italic), Helvetica (Narrow, Narrow Oblique, Narrow Bold, Narrow Bold Oblique, in addition to the 4 font styles in PostScript Level 1), New Century Schoolbook (Roman, Italic, Bold, Bold Italic), Palatino (Roman, Italic, Bold, Bold Italic), ITC Zapf Chancery (Medium Italic), and ITC Zapf Dingbats.

The URW Type Foundry provides free implementations of the 35 PostScript level 2 fonts, available on Gentoo via the [media-fonts/urw-fonts](https://packages.gentoo.org/packages/media-fonts/urw-fonts) package.

In the late 80s, Apple created the [TrueType](https://en.wikipedia.org/wiki/TrueType) (TTF) font format to compete with Adobe's T1 fonts, and licensed TrueType to Microsoft for free. As a result, TrueType support was part of Windows 3.1. Subsequent licensing arrangements led Microsoft to begin development of a variant of TrueType, with Adobe joining the effort in 1996; that variant, utilising Compact Font Format (CFF), itself a compressed variant of Type 1, is now known as [OpenType](https://en.wikipedia.org/wiki/OpenType), ISO 14496-22, the OpenType Format (OTF).

The Web Open Font Format (WOFF) is a Web-oriented container format for [SFNT](https://en.wikipedia.org/wiki/SFNT) font formats, such as OTF, TTF and PostScript.

[Graphite](https://graphite.sil.org/) is a font format developed by SIL, intended to significantly lower the barriers to creating fonts for smaller language communities. A font can support both OpenType/TrueType and Graphite simultaneously, as in the case of [Annapurna SIL](https://software.sil.org/annapurna/):

`user $``fc-query ./AnnapurnaSIL-2.000/AnnapurnaSIL-Regular.ttf`
\[...\]
capability: "ttable:Silf  otlayout:DFLT otlayout:dev2 otlayout:deva otlayout:latn"(s)
fontformat: "TrueType"(s)
\[...\]


The `ttable:Silf` entry for the `capability` element indicates that the font has Graphite support.

### Specifying fonts

For many years, the standard way of specifying a font when using the X Window System (e.g. in an .Xdefaults file) was an [X Logical Font Description](https://en.wikipedia.org/wiki/X_logical_font_description) (XFLD), introduced in 1988. An XFLD consists of 14 fields, each specifying a particular characteristic of the font: for example, `-misc-fixed-medium-r-semicondensed--13-120-75-75-c-60-iso10646-1`. Nowadays, however, both X and Wayland compositors use the [fontconfig](https://wiki.gentoo.org/wiki/Fontconfig) system, and [Xft](https://en.wikipedia.org/wiki/Xft) (the X [FreeType](https://wiki.gentoo.org/wiki/FreeType) Interface Library) allows the specification of fonts via a string such as `xft:Gentium:pixelsize=14` The general format for Fontconfig font specifications is described in the [fonts-conf(5)](https://man.archlinux.org/man/fonts-conf.5.en) [man page:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

The representation is in three parts, first a list of family names, second a list of point sizes and finally a list of additional properties:

`<families>-<point sizes>:<name1>=<values1>:<name2>=<values2>...`

Values in a list are separated with commas. The name needn't include either families or point sizes; they can be elided. In addition, there are symbolic constants that simultaneously indicate both a name and a value.


Additionally, software that utilizes the [Pango](https://pango.gnome.org/) library ([x11-libs/pango](https://packages.gentoo.org/packages/x11-libs/pango)) for text rendering can potentially allow fonts to be specified by a string of the form:

`[FAMILY-LIST] [STYLE-OPTIONS] [SIZE] [VARIATIONS]`

e.g.:

`Cantarell Italic Light 15`

Details are provided in [the relevant section of the Pango documentation](https://docs.gtk.org/Pango/type_func.FontDescription.from_string.html).

### Libraries

In addition to Xft ([x11-libs/libXft](https://packages.gentoo.org/packages/x11-libs/libXft), mentioned above, some other libraries relevant to fonts on Gentoo include:

- [FreeType](https://wiki.gentoo.org/wiki/FreeType) ([project page](https://freetype.org/)), a font rasterization engine, including support for the OpenType, TrueType and Type 1 font formats.
- [dev-libs/fribidi](https://packages.gentoo.org/packages/dev-libs/fribidi) ([project page](https://github.com/fribidi/fribidi)), an implementation of the Unicode bidirectional algorithm.
- [media-gfx/graphite2](https://packages.gentoo.org/packages/media-gfx/graphite2) ([project page](https://graphite.sil.org/)), a library providing rendering capabilities for complex non-Roman writing systems.
- [media-libs/harfbuzz](https://packages.gentoo.org/packages/media-libs/harfbuzz) ([project page](https://github.com/harfbuzz/harfbuzz), which provides a 'shaping engine'; necessary for scripts such as Arabic where glyphs can need to be rendered differently depending on context.
- [x11-libs/libXfont2](https://packages.gentoo.org/packages/x11-libs/libXfont2) ([project page](https://gitlab.freedesktop.org/xorg/lib/libxfont)), the legacy X11 font system.
- [x11-libs/pango](https://packages.gentoo.org/packages/x11-libs/pango) ([project page](https://pango.gnome.org/)), an internationalized library for text layout and rendering, incorporating HarfBuzz.

## See also

## External resources

- [Glossary of Unicode terms](https://www.unicode.org/glossary/)
- [Text rendering hates you](https://faultlore.com/blah/text-hates-you/) — "Rendering text, how hard could it be? As it turns out, incredibly hard!"
- [State of Text Rendering 2024](https://behdad.org/text2024/) — "In this survey paper, I cover advances in the OpenType standard and the Open Source text stack and applications since 2009."
