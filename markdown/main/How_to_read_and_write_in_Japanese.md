<!-- source: https://wiki.gentoo.org/wiki/How_to_read_and_write_in_Japanese | group: Gentoo Wiki (Main) | wiki-title: How to read and write in Japanese -->
---
title: How to read and write in Japanese
url: https://wiki.gentoo.org/wiki/How_to_read_and_write_in_Japanese
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-22"
fingerprint: cc1c8f56008f7f9d
license: CC BY-SA 4.0
---

# How to read and write in Japanese

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide aims at explaining how to read and write in Japanese on a non-Japanese system. Please feel free to amend it based on personal knowledge or experience.

## Requirements

In order to support Japanese language and characters, a number of required tools, libraries and capabilities need to be installed on the system.

### Japanese fonts

Most non-Japanese systems have no Japanese fonts installed. Whenever a user tries to enter Japanese characters from the keyboard, they will only see small rectangle boxes in place of the characters on the screen.

### Japanese Menus and Environment

For those interested (perhaps for immersion based learning) in having a Japanese language based environment, in order to change menus and other materials into the Japanese language, change the user's profile into LANG=ja\_JP.UTF-8 (in the user's locale as well as .bash\_profile<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>)

### Input method

To read and write in Japanese, the first thing that is needed is a way to enter Japanese characters with the keyboard. This is done via a piece of software usually called an *input method*. At the moment, for the Japanese language,  there are 2 such common methods: *anthy* and *mozc*.

With such a software component typing "ta" on the keyboard will input the kana **た** into the word processor. Some simple manipulation that is relevant to the way the input method works, will permit to easily switch from the hiragana **た** to the katakana  **タ**.

In a similar way typing "nihon" will input **にほん** and an other simple manipulation will permit to turn this to the kanji version of this word, **日本**.

### IME

On top of this users also need a way to switch from the input method normally used for the primary language to the one needed for the Japanese language. This functionality is provided by another piece of software called an IME (Input Method Editor) such as [app-i18n/ibus](https://packages.gentoo.org/packages/app-i18n/ibus), [app-i18n/scim](https://packages.gentoo.org/packages/app-i18n/scim) or [app-i18n/fcitx](https://packages.gentoo.org/packages/app-i18n/fcitx).

Once installed, this allows users to switch from one language's input method to the Japanese input method using a key combination or using the mouse to select a relevant icon in the icon tray.

## Installation

### Japanese fonts

As a minimum, install the [media-fonts/kochi-substitute](https://packages.gentoo.org/packages/media-fonts/kochi-substitute) package.

`root #``emerge --ask kochi-substitute`
Additionally, the following packages are also available:

### Input tools

It is recommended to use [IBus](https://wiki.gentoo.org/wiki/IBus) instead of [SCIM](https://en.wikipedia.org/wiki/Smart_Common_Input_Method).

#### anthy

`root #``emerge --ask ibus-anthy`
#### mozc

`root #``USE=ibus emerge --ask app-i18n/mozc`
#### Configuring

See [IBus](https://wiki.gentoo.org/wiki/IBus#Configuration) article on running IBus on login.

`user $``ibus-setup`
In the dialog box that appears, click on the "Input method" tab and add the "japanese-anthy" or "Japanese - Mozc" method. Then return to the "General" tab and define a key combination as a keyboard shortcut for switching the input method.

The following useful keybindings could be set up for the "Japanese - Mozc" method:

Preferences → General → Keymap → Keymap style → Customize…

| Mode | Key | Command | 
|---|---|---|
| Direct input | `` Ctrl ` `` | Set input mode to Hiragana | 
| Precomposition | `` Ctrl ` `` | Deactivate IME | 

#### Common USE Flags

The following use flags are commonly employed<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>:

**cjk** - Support for Hanzi-inspired characters (containing two bytes, hence the cause of accented a's *cum* sans cjk environment)

**nls** - 'native language support' - enables other languages in interface,

**immqt-bc** - For Qt to manage other language inputs

**immqt** - conflicts with immqt-bc as of Qt3. 

**unicode** - Standard except for cursive hebrew


### Latex

Here are some additional requirements to write Latex files in Japanese.

#### CJK and xetex support

In order to write Japanese chunks in Latex files, add support for CJK languages and for [xetex](https://en.wikipedia.org/wiki/XeTeX) in Texlive.

This can be accomplished by adding or modifying the following lines in /etc/portage/package.use:

**`/etc/portage/package.use/latex`**

**Enabling cjk and xetex support**

Then reinstall the packages:

`root #``emerge --ask --newuse app-text/texlive app-text/texlive-core`
Here is a working short LaTeX sample:

**`japanese.tex`**

```
\documentclass{article}
\usepackage{CJKutf8}
\usepackage{color}
 
\begin{document}
 
\begin{CJK}{UTF8}{min}
\section{One simple example}
\textcolor{red}{これは赤いです。}
\\
私は日本語で書けます。
\\
But I can also write with latin characters
\end{CJK}
 
\end{document}
```
#### Editor configuration

To compile and visualize the output of the sample above Texmaker or Texstudio editor needs to be configured properly.

Open Texmaker, and go to Options -> Configure Texmaker. Under the Commands tab change the following:

- At the LaTeX line, change "latex" with "platex".
- At the Dvipdfm line, change "divipdfm" with "dvipdfmx".

Through the Fast compile tab, choose "Latex + Dvipdfm + View PDF".

Finally go to the Editor tab, choose UTF8 encoding and deselect On the fly on the dictionary line.
