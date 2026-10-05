<!-- source: https://wiki.gentoo.org/wiki/How_to_read_and_write_in_Chinese | group: Gentoo Wiki (Main) | wiki-title: How to read and write in Chinese -->
---
title: How to read and write in Chinese
url: https://wiki.gentoo.org/wiki/How_to_read_and_write_in_Chinese
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-25"
fingerprint: ee588a1c849f3898
license: CC BY-SA 4.0
---

# How to read and write in Chinese

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This guide aims at explaining how to read and write in Chinese on a non-Chinese system. Please feel free to amend it based on personal knowledge or experience.

## Requirements

In order to support Chinese language and characters, a number of required tools, libraries and capabilities need to be installed on the system.

### Chinese fonts

Most non-Chinese systems have no Chinese fonts installed. Whenever a user tries to enter Chinese characters from the keyboard, they will only see small rectangle boxes in place of the characters on the screen.

### Input method

To read and write in Chinese, the first thing that is needed is a way to enter Chinese characters with the keyboard. This is done via a piece of software usually called an *input method*. At the moment, for the Chinese language,  there are 3 kinds of common methods; phonetic, shape-based and a hybrid from both, see [Chinese input methods for computers (Wikipedia)](https://en.wikipedia.org/wiki/Chinese_input_methods_for_computers). The most common ones are *wubi* and *pinyin*.



### IME

On top of this users also need a way to switch from the input method normally used for the primary language to the one needed for the Chinese language. This functionality is provided by another piece of software called an IME (Input Method Editor) such as [app-i18n/ibus](https://packages.gentoo.org/packages/app-i18n/ibus), [app-i18n/scim](https://packages.gentoo.org/packages/app-i18n/scim) or [app-i18n/fcitx](https://packages.gentoo.org/packages/app-i18n/fcitx). 
fcitx sunpinyin working, table not yet
ibus not confirmed yet
scim not confirmed yet

Once installed, this allows users to switch from one language's input method to the Chinese input method using a key combination or using the mouse to select a relevant icon in the icon tray.

## Installation

### Chinese fonts

Install the [noto-cjk](https://packages.gentoo.org/packages/media-fonts/noto-cjk) package.

`root #``emerge --ask media-fonts/noto-cjk`
Then, list the available font configs with

`root #````
eselect fontconfig list
```
Enable the noto-cjk font configuration with

`root #````
eselect fontconfig enable 70-noto-cjk.conf
```

Additionally, the following packages are also available:

### Input tools

It is recommended to use *fcitx* instead of *scim* or *ibus*.

To install fcitx, install [app-i18n/fcitx](https://packages.gentoo.org/packages/app-i18n/fcitx):

`root #``emerge --ask fcitx`
#### fcitx-rime

fcitx-rime is provided through the [app-i18n/fcitx-rime](https://packages.gentoo.org/packages/app-i18n/fcitx-rime)package. Note: the fcitx-rime package is mainly a traditional Chinese input engine.



#### Launching the fcitx daemon at login time

Add these lines to the \~/.xprofile file and log out/log in again.

**`~/.xprofile`**

```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export SDL_IM_MODULE=fcitx
export GLFW_IM_MODULE=fcitx
export INPUT_MODULE=fcitx
```
This will allow the fcitx daemon to start at login time.

#### Configuring

To configure the fcitx Input Method Editor, use the following [app-i18n/fcitx-configtool](https://packages.gentoo.org/packages/app-i18n/fcitx-configtool) package for graphical configuration.

### Latex

Here are some additional requirements to write Latex files in Chinese.

#### CJK and xetex support

In order to write Chinese chunks in Latex files, add support for CJK languages and for [xetex](https://en.wikipedia.org/wiki/XeTeX) in Texlive.

This can be accomplished by adding or modifying the following lines in /etc/portage/package.use:

**`/etc/portage/package.use/latex`**

**Enabling cjk and xetex support**

Then reinstall the packages:

`root #``emerge --ask --newuse app-text/texlive app-text/texlive-core`
Here is a working short LaTeX sample:

**`chinese.tex`**

```
\documentclass{article}
\usepackage{CJKutf8}
\usepackage{color}
 
\begin{document}
 
\begin{CJK}{UTF8}{gbsn}
\section{One simple example}
\textcolor{red}{你好世界}
\\
你好世界。
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

## See also

- [Fcitx](https://wiki.gentoo.org/wiki/Fcitx) — an input method framework with support for many languages and scripts.
- [TeX Live](https://wiki.gentoo.org/wiki/TeX_Live) — a complete [TeX](https://en.wikipedia.org/wiki/TeX) distribution with several programs to create professional documents.
- [How to read and write in Japanese](https://wiki.gentoo.org/wiki/How_to_read_and_write_in_Japanese) — how to read and write in Japanese on a non-Japanese system.
- [IBus](https://wiki.gentoo.org/wiki/IBus) — an open source input framework for Linux and Unix.
