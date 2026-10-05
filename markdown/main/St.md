<!-- source: https://wiki.gentoo.org/wiki/St | group: Gentoo Wiki (Main) | wiki-title: St -->
---
title: st
url: https://wiki.gentoo.org/wiki/St
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-07"
fingerprint: dc577659e9bfcaba
license: CC BY-SA 4.0
---

# st

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**st** is an extremely minimal [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) for the X environment made by the [suckless.org](https://suckless.org/) community. st is configured by editing [C](<https://en.wikipedia.org/wiki/C_(programming_language)>) source code header files, and recompiling. The suckless website states that the project ***focuses on advanced and experienced computer users***.

## Installation

### USE flags


Preferably, [savedconfig](https://packages.gentoo.org/useflags/savedconfig)

`root #``euse --enable savedconfig`
### Emerge

Install [x11-terms/st](https://packages.gentoo.org/packages/x11-terms/st):

`root #``emerge --ask x11-terms/st`
## Configuration

As stated previously, the main st configuration file is the [/etc/portage/savedconfig](https://wiki.gentoo.org/wiki//etc/portage/savedconfig)/x11-terms/st file and after each change, st needs to be recompiled for any changes to take effect.

### Patches and additional features

There are many user-created [patches](http://st.suckless.org/patches/) available from the official site that greatly extend the functionality of st. See [/etc/portage/patches](https://wiki.gentoo.org/wiki//etc/portage/patches) on how to apply these patches automatically.

## Troubleshooting

### Keyboard

#### Delete key is not working properly

Add following to the /etc/inputrc to make a system-wide change:

**`/etc/inputrc`**

Or to the relative user's \~/.inputrc file:

**`~/.inputrc`**
