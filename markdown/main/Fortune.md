<!-- source: https://wiki.gentoo.org/wiki/Fortune | group: Gentoo Wiki (Main) | wiki-title: Fortune -->
---
title: fortune
url: https://wiki.gentoo.org/wiki/Fortune
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-25"
fingerprint: "32ca0cc297678bbb"
license: CC BY-SA 4.0
---

# fortune

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**fortune** is a command-line utility which displays a random quotation from a collection of quotes.

## Installation

### USE flags


### USE flags for
            [games-misc/fortune-mod](https://packages.gentoo.org/packages/games-misc/fortune-mod)
            
            The notorious fortune program

### Emerge

`root #``emerge --ask games-misc/fortune-mod`
## Usage

### Invocation

`user $``fortune -h`
fortune-mod version 3.18.0
fortune [-afilsw] [-m pattern] [-n number] [ [#%] file/directory/all

### Configuration

The fortune quote database is stored in separate files for different categories of quotes in the /usr/share/fortune directory. To add fortunes to the database, edit any of the text files in the directory, with a % before and after the quote, as an example,

FILE **`/usr/share/fortune/fortunes`**

```
%
Larry the Cow beckons you to explore the Gentoo Wiki!
%
```
### Cowsay

Fortune supports the cowsay package.

First, emerge cowsay:

`root #``emerge --ask games-misc/cowsay`
Then, use the following command to have cowsay write a fortune.

`user $``fortune | cowsay````
 _________________________________________ 
/ Bounders get bound when they are caught \
| bounding.                               |
|                                         |
\ -- Ralph Lewin                          /
 ----------------------------------------- 
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```
