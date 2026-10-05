<!-- source: https://wiki.gentoo.org/wiki/Ibus-libpinyin | group: Gentoo Wiki (Main) | wiki-title: Ibus-libpinyin -->
---
title: Ibus-libpinyin
url: https://wiki.gentoo.org/wiki/Ibus-libpinyin
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-08-10"
fingerprint: fedc9e1a6c1739bc
license: CC BY-SA 4.0
---

# Ibus-libpinyin

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[app-i18n/ibus-libpinyin](https://packages.gentoo.org/packages/app-i18n/ibus-libpinyin) is a Chinese input engine on [IBus](https://wiki.gentoo.org/wiki/IBus). It is similar to, and a viable alternative for [Fcitx](https://wiki.gentoo.org/wiki/Fcitx) and [app-i18n/fcitx-libpinyin](https://packages.gentoo.org/packages/app-i18n/fcitx-libpinyin). It is a modern replacement for the original (outdated) [app-i18n/ibus-pinyin](https://packages.gentoo.org/packages/app-i18n/ibus-pinyin).

## Installation

### USE flags


### Emerge

`root #``emerge --ask app-i18n/ibus-libpinyin`
If a chinese compatible font is not already installed:

`root #``emerge --ask media-fonts/arphicfonts`
### Additional software

This will work alongside other [IBus](https://wiki.gentoo.org/wiki/IBus) input types (e.g. for Japanese input [app-i18n/ibus-anthy](https://packages.gentoo.org/packages/app-i18n/ibus-anthy)).

## Configuration

Open up the ibus setup dialogue:

`user $``ibus-setup`
Check the 'Next Input Method key' and change if desired. Then switch to 'Input Method' tab and add 'Chinese -> Intelligent Pinyin'. This adds 'Chinese - Intelligent Pinyin' to the list of input methods that ibus will cycle through when pressing the 'Input Method Key'. Select 'Chinese - Intelligent Pinyin' on the list and click 'Preferences' to set options as desired.

For [IBus](https://wiki.gentoo.org/wiki/IBus) (and the Chinese input) to run automatically on next login, configure as per configuration section for [IBus](https://wiki.gentoo.org/wiki/IBus).

## Usage

Press the ibus 'special key' (as configured above - by default it is super+Space) and select 'Intelligent Pinyin'. Then type in pinyin to get the Chinese character (type 'nihao' and if 你好 appears as 1. press space to accept).

### Invocation

To manually start ([IBus](https://wiki.gentoo.org/wiki/IBus)) for a user, for example:

`user $``ibus-daemon -d -x`
See [IBus](https://wiki.gentoo.org/wiki/IBus) wiki for more info.

## Troubleshooting

In xterm shows weird glyphs instead of Chinese characters, but it works OK in other software (e.g. browser), launch xterm with settings that can cope with double spaced characters, e.g.:

`user $``xterm -rv -en UTF-8 -fs 10 -fa 'AR PL UMing CN:style=Regular,Regular'`
