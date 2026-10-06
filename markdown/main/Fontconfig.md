<!-- source: https://wiki.gentoo.org/wiki/Fontconfig | group: Gentoo Wiki (Main) | wiki-title: Fontconfig -->
---
title: Fontconfig
url: https://wiki.gentoo.org/wiki/Fontconfig
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-18"
fingerprint: f0e7fff4069b4fc
license: CC BY-SA 4.0
---

# Fontconfig

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Fontconfig ([media-libs/fontconfig](https://packages.gentoo.org/packages/media-libs/fontconfig)) is intended to provide uniform font selection and configuration amongst all GUI applications. Although it is common for a [desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment) to provide its own font overrides and configuration utilities, Fontconfig is still the underlying system.

Refer to the [Fonts](https://wiki.gentoo.org/wiki/Fonts) page for more general information about using fonts on Gentoo, and to the [Fonts/Background](https://wiki.gentoo.org/wiki/Fonts/Background) page for background information about font-related concepts, terminology and systems.


## Installing

When Fontconfig is needed, it will almost certainly be installed already. If not, make sure to set up the graphical subsystem (i.e., [X](https://wiki.gentoo.org/wiki/X) or [Wayland](https://wiki.gentoo.org/wiki/Wayland)) and [desktop environment](https://wiki.gentoo.org/wiki/Desktop_environment) properly.


### USE flags


### USE flags for
            [media-libs/fontconfig](https://packages.gentoo.org/packages/media-libs/fontconfig)
            
            A library for configuring and customizing font access

| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 


## Configuration


### Generic

Fontconfig uses [XML](https://en.wikipedia.org/wiki/XML) files in /etc/fonts/ for its internal configuration.

By default, Fontconfig parses /etc/fonts/fonts.conf to set some basic defaults and parse files in /etc/fonts/conf.d/. Users should **not** edit this file.

The /etc/fonts/conf.avail/ directory contains configuration files that can optionally be used to configure Fontconfig. To 'enable' a file, a symlink needs to be created in /etc/fonts/conf.d/. These files are executed in order they are named; thus, their names start with a two-digit number, with the first digit indicating which configuration *class* the file affects.


### Gentoo-specific

Gentoo ships an [eselect](https://wiki.gentoo.org/wiki/Eselect) module, fontconfig, to manage symlinks in the /etc/fonts/conf.d/ directory. Since doing so has system-wide effects, this requires superuser permissions.


#### Listing available files

The list command shows the available Fontconfig files, giving each file a temporary index number in square brackets, and marking enabled files with an asterisk (`*`):

`root #``eselect fontconfig list`
Available fontconfig .conf files (\* is enabled):
  \[1\]   10-autohint.conf \*
  \[2\]   10-no-sub-pixel.conf
  \[3\]   10-sub-pixel-bgr.conf
  \[4\]   10-sub-pixel-rgb.conf
  \[5\]   10-sub-pixel-vbgr.conf
  \[6\]   10-sub-pixel-vrgb.conf
  \[7\]   10-unhinted.conf
  \[8\]   11-lcdfilter-default.conf
  \[9\]   11-lcdfilter-legacy.conf
  ...


#### Enabling a file

Files can be enabled either by their filename or their index number. Given the above output from the list command, these two commands do the same thing:

`root #````
eselect fontconfig enable 10-sub-pixel-rgb.conf
```
`root #````
eselect fontconfig enable 4
```

#### Disabling a file

Files can be disabled likewise:

`root #````
eselect fontconfig disable 10-sub-pixel-rgb.conf
```
`root #````
eselect fontconfig disable 4
```

### Custom system-wide configuration

To create a custom system-wide Fontconfig file, enable 51-local.conf and create the XML file /etc/fonts/local.conf. For example:

**`/etc/fonts/local.conf`**

**Set preferred font fallback order for sans-serif font**

```
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <alias>
    <family>sans-serif</family>
    <prefer>
      <family>Ubuntu</family>
      <family>TakaoPGothic</family>
      <family>Droid Sans</family>
    </prefer>
    <default><family>DejaVu Sans</family></default>
  </alias>
</fontconfig>
```
This file indicates that, if a sans-serif font is required, the Ubuntu, TakaoPGothic, and Droid Sans fonts are preferred over DejaVu Sans, which will be used as a fallback choice only when necessary.


### Per-user configuration

To create per-user Fontconfig files, enable 50-user.conf (which might be enabled by default).

Once 50-user.conf is enabled, users can specify custom configurations in the XML file ${XDG\_CONFIG\_HOME}/fontconfig/fonts.conf.


### Checking configuration

To check the default font replacement, for example for Arial, use the [fc-match(1)](https://man.archlinux.org/man/fc-match.1.en) [command:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``fc-match Arial`

### Anti-aliasing, hinting, and sub-pixel rendering

*Anti-aliasing* is enabled by default. It makes fonts less 'blocky'.

*Hinting* is an attempt to address the low pixel count per unit of area of displays. Correct hinting makes characters more 'crisp'; however, since font metrics aren't changed (and arguably should not change), it also affects how rendered text looks overall.

*Sub-pixel rendering* uses the fact that an [LCD](https://en.wikipedia.org/wiki/Liquid-crystal_display) matrix has three primary colors. It effectively triples the resolution of text, but can make characters appear not entirely black. To combat that, lcdfilter can be used with sub-pixel rendering, but this can blur the characters too much.


#### Forcing hinting

Default Fontconfig behavior regarding hinting is rather undocumented, but its behavior can be made more deterministic via a system-wide default.

Firstly, [enable /etc/fonts/local.conf](https://wiki.gentoo.org/wiki/Fontconfig#Custom_system-wide_configuration).

Then, add the following snippet to local.conf to enable full hinting:

**`/etc/fonts/local.conf`**

**Enable full hinting**

```
<match target="font">
  <edit mode="assign" name="hintstyle">
    <const>hintfull</const>
  </edit>
</match>
```

#### Using sub-pixel rendering

To use sub-pixel rendering, determine the sub-pixel layout of the LCD matrix. It's usually RGB (10-sub-pixel-rgb.conf), but the only way to be sure is to either consult the display specification, or to use [this sub-pixel layout test](http://www.lagom.nl/lcd-test/subpixel.php).

Once the layout is determined, enable the appropriate 10-sub-pixel-\<layout>.conf file, e.g.:

`root #``eselect fontconfig enable 10-sub-pixel-rgb.conf`
When using sub-pixel rendering, lcdfilter should also be used if available. It comes in different varieties, but the default (11-lcdfilter-default.conf) should be appropriate for all common fonts.

`root #``eselect fontconfig enable 11-lcdfilter-default.conf`
Some graphical toolkits ignore the Fontconfig sub-pixel rendering settings and might require additional setup. For example, [GTK](https://wiki.gentoo.org/wiki/GTK) requires setting the `gtk-xft-rgba` option:

**`~/.config/gtk-3.0/settings.ini`**

**Enable sub-pixel rendering with RGB layout**

```
[Settings]
gtk-xft-rgba = rgb
```

#### Autohint

Autohinting attempts to do hinting automatically. Two snippets are available via eselect fontconfig: 09-autohint-if-no-hinting.conf, and 10-autohint.conf, with the latter disregarding any preexisting hinting information.

Autohinting was previously the default, due to potential patent issues related to TrueType; however, the relevant patents have now expired, so there's very little reason to use it. From a technical point of view, autohinting is better than broken or no hinting information, but it will be strongly sub-optimal for fonts with good hinting information. As system fonts are generally of the second kind, autohinting should not be used.


## Querying installed fonts

Fontconfig provides the fc-\* suite of programs to provide information about the fonts it's aware of.

To list all fonts known to Fontconfig, use [fc-list(1)](https://man.archlinux.org/man/fc-list.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``fc-list`
/usr/share/fonts/noto/NotoSansArabic-SemiCondensedExtraBold.ttf: Noto Sans Arabic,Noto Sans Arabic SemCond ExtBd:style=SemiCondensed ExtraBold,Regular
/usr/share/fonts/noto/NotoSerifDevanagari-CondensedBold.ttf: Noto Serif Devanagari Condensed:style=Bold
/usr/share/fonts/urw-fonts/URWGothic-DemiOblique.ttf: URW Gothic:style=Demi Oblique
\[...\]

If a particular font is known to be installed in one of the paths configured for use by Fontconfig, but doesn't appear in the output of [fc-list(1)](https://man.archlinux.org/man/fc-list.1.en)[, it might be that the Fontconfig cache needs to be refreshed, which can be done by running](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [fc-cache(1)](https://man.archlinux.org/man/fc-cache.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``fc-cache -fv`
To retrieve information about a specific font, use [fc-query(1)](https://man.archlinux.org/man/fc-query.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``fc-query /usr/share/fonts/urw-fonts/URWGothic-DemiOblique.ttf`
Pattern has 25 elts (size 32)
	family: "URW Gothic"(s)
	familylang: "en"(s)
	style: "Demi Oblique"(s)
	stylelang: "en"(s)
	fullname: "URW Gothic Demi Oblique"(s)
	fullnamelang: "en"(s)
	slant: 110(i)(s)
	weight: 200(f)(s)
	width: 100(f)(s)
	foundry: "URW "(s)
	file: "/usr/share/fonts/urw-fonts/URWGothic-DemiOblique.ttf"(s)
	index: 0(i)(s)
	outline: True(s)
	scalable: True(s)
	charset: 
	0000: 00000000 ffffffff ffffffff 7fffffff 00000000 ffffffff ffffffff ffffffff
	0001: ffffffff ffffffff ffffffff ffffffff 00040000 00000000 00000000 fc000000
	0002: 0f000000 00000000 00000000 00000000 00000000 00000000 3f0002c0 00000000
	0003: 00000000 00000000 00000000 00000000 ffffd7f0 fffffffb 00627fff 00000000
	0004: ffffffff ffffffff ffffffff 003c000c 3fcf0000 0fcfcc0f 03009801 0000c30c
	001e: 00000000 00000000 00000000 00000000 0000003f 00000000 00000000 000c0000
	0020: 7fb80004 560d0047 00000010 83f10000 00000000 00009098 20000000 00000000
	0021: 514e8020 00e0e145 78000000 00000000 03ff0000 00200100 003f0050 00000000
	0022: e6aeabed 00b04fa9 00000120 00000c37 03e000fc 0800003c 00000000 00000000
	0023: 00010004 00000603 00000000 00000000 00000000 00000000 00000000 00000000
	0025: 11111005 10101010 ffff0000 0001ffff 000f1111 96241c03 03008cd8 00000040
	0026: 00000000 1c000000 00000005 00000c69 00000000 00000000 00000000 00000000
	0030: 0c000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000
	00ef: 00000000 00000000 00000000 00000000 00000000 80000000 ffffffff fc001fff
	00fb: 0000001f 00000000 00000000 00000000 00000000 00000000 00000000 00000000
(s)
	lang: aa|af|av|ay|ba|be|bg|bi|br|bs|bua|ca|ce|ch|co|cs|cy|da|de|el|en|eo|es|et|eu|fi|fj|fo|fr|fur|fy|gd|gl|gv|ho|hr|hu|ia|id|ie|ik|io|is|it|kaa|ki|kk|kl|kum|ky|la|lb|lez|lt|lv|mg|mh|mk|mo|mt|nb|nds|nl|nn|no|nr|nso|ny|oc|om|os|pl|pt|rm|ro|ru|se|sel|sh|sk|sl|sma|smj|smn|so|sq|sr|ss|st|sv|sw|tg|tk|tl|tn|tr|ts|tt|tyv|uk|uz|vo|vot|wa|wen|wo|xh|yap|zu|an|crh|csb|fil|hsb|ht|jv|kj|ku-tr|kwm|lg|li|mn-mn|ms|na|ng|pap-an|pap-aw|rn|rw|sc|sg|sn|su|za(s)
	fontversion: 65536(i)(s)
	fontformat: "TrueType"(s)
	decorative: False(s)
	postscriptname: "URWGothic-DemiOblique"(s)
	color: False(s)
	symbol: False(s)
	variable: False(s)
	fonthashint: True(s)
	order: 0(i)(s)

Each of the listed properties, such as `lang` and `fontformat`, is called an element.

To search for fonts that best match specified elements, use [fc-match(1)](https://man.archlinux.org/man/fc-match.1.en)[:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``fc-match -s 'sans:lang=de' | head -n5`
LiberationSans-Regular.ttf: "Liberation Sans" "Regular"
NotoSans-Regular.ttf: "Noto Sans" "Regular"
NotoSans-Italic.ttf: "Noto Sans" "Italic"
DejaVuSans.ttf: "DejaVu Sans" "Book"
DejaVuSans-Bold.ttf: "DejaVu Sans" "Bold"

The `-s` option is used to return all possible matches, ranked by quality of match; the pipe to [head(1)](https://man.archlinux.org/man/head.1.en) [returns the top five results. Without the](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) `-s` option, only the best match is returned.

To list all serif OTF fonts known to Fontconfig, pass `CFF` ([Compact Font Format](https://en.wikipedia.org/wiki/Compact_Font_Format)) as the argument to the `fontformat` element:

`user $``fc-match -s 'serif:fontformat=CFF'`
AccanthisADFStd-Regular.otf: "Accanthis ADF Std" "Regular"
AurelisADFNo2Std-Regular.otf: "Aurelis ADF No2 Std" "Regular"
latinmodern-math.otf: "Latin Modern Math" "Regular"
\[...\]

To determine which font will be chosen by Fontconfig to display certain text in a given style, set the `FC_DEBUG` environment variable and use [pango-view(1)](https://man.archlinux.org/man/pango-view.1.en)[, from the](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [x11-libs/pango](https://packages.gentoo.org/packages/x11-libs/pango) package:

`user $``FC_DEBUG=4 pango-view -q --font=monospace --text=gentoo | grep 'family:' | tail -n1`
family: "Liberation Mono"(s)


## External resources

- In-depth articles from Arch Linux wiki on:
- [Official Fontconfig documentation for users](http://freedesktop.org/software/fontconfig/fontconfig-user.html)
- [Wikipedia article on font hinting](https://en.wikipedia.org/wiki/Hinting)
