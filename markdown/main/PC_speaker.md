<!-- source: https://wiki.gentoo.org/wiki/PC_speaker | group: Gentoo Wiki (Main) | wiki-title: PC speaker -->
---
title: PC speaker
url: https://wiki.gentoo.org/wiki/PC_speaker
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-04"
fingerprint: "9cd2ac89093b398c"
license: CC BY-SA 4.0
---

# PC speaker

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **PC speaker** is more commonly referred to the speaker located on the motherboard. Nowadays this speaker is usually a simple piezoelectric speaker providing just enough speaker hardware to distinguish simple frequency and tones of beeps. Beeps usually occur on error, but can be customized as this article will document.

## Installation

Make sure the motherboard has two pins for the PC speaker, as well as being connected to a small speaker or piezoelectric speaker.

### Kernel

Build the Linux kernel module, named `INPUT_PCSPKR`:

**Enabling`INPUT_PCSPKR`**

If the kernel has been built with *pcspkr* as a module, it can be disabled using the modprobe *blacklist*, by adding a line with `blacklist pcspkr` to /etc/modprobe.d/blacklist.conf, preventing this module from being used. The module can be immediately disabled with:

`root #``modprobe -r pcspkr`
## PC speaker configuration files and settings

### Baselayout file

The [sys-apps/baselayout](https://packages.gentoo.org/packages/sys-apps/baselayout) package provides a template /etc/inputrc file for users. This /etc/inputrc and respective \~/.inputrc file(s) are referenced by Bash's readline program, for further configuring or optimizing readline.

**`/etc/inputrc`**

### Util-Linux utility

While within the Linux console or terminal, use the following:

`user $````
setterm -blength <0-2000>
```
`user $````
setterm -bfreq freqnumber
```
### Xorg xset utility

While within [Xorg](https://wiki.gentoo.org/wiki/Xorg), use the following.

Query current xset options:

`user $``xset q``user $``xset b 100`
For users of the old PC keyboards having a speaker, users can easily activate a typewriting sound effect after each key press by doing:

`user $``xset c on``user $``xset c 100``user $``xset q`
Should show "key click percent: 100", for key clicks being activated.

### Screen

By default, [screen](https://wiki.gentoo.org/wiki/Screen) enables audible bell style, unless visual style is activated. So disable visual style:

**`${HOME}/.screenrc`**

From within GNU Screen, pressing `Ctrl`+`a`+`Ctrl`+`g` can switch visual or audible bell styles.

### Beep

There's a program which simplifies playing and customizing frequencies of beeps through the PC Speaker:

`root #``emerge --ask app-misc/beep`
### xkbutils

For historical purposes, providing only a simple bell or monotone beep, xkbbell:

`root #``emerge --ask x11-apps/xkbutils`
## Using the PC speaker

### On demand via console or script

On demand use of the PC speaker can be accomplished by using a simple echo statement using an escape sequence indicating the command line terminal to process a beep.

`user $``echo -e '\a'`
This echo statement is commonly placed within scripts and programs, to either indicate the task has finished, or has exited on error.

### Johnath's beep

Johnath's beep utility, mentioned above and provided by the Gentoo [app-misc/beep](https://packages.gentoo.org/packages/app-misc/beep) package, provides frequency, length, delay, along with a few other much desired features.  However it seems to lack volume control.  Perhaps the volume might be muddled in with the frequency of the beep.

`user $``beep -f 700 -l 30`
See the bottom of the beep's manual page for further examples.

### Typewriter effect

If you're unfortunate like the many of us nowadays and do not own a keyboard having a speaker for using "xset c", then you'll need to either script or program something, taking the keyboard events and playing a beep or sound.

The following options can either use [app-misc/beep](https://packages.gentoo.org/packages/app-misc/beep) for simplicity and using fewer system resources, or use alsa-utils aplay for playing more dramatic effects.  (Or insert your other favorite command line media player such as afplay for playing AIFF files.)

#### Bash

A simple Bash script utilizing [app-misc/beep](https://packages.gentoo.org/packages/app-misc/beep).

Where xinput test argument is your AT Keyboard, extracted from xinput --list, pipe the output to an infinite while variable (or until `Ctrl`+`c` is pressed).  Read each line via read and perform a REGEX search for lines containing "press" and if found use the beep command.

`user $``$ xinput --test 10 | while IFS= read -r LINE; do if [[ ${LINE} == *press* ]] && [[ $LINE != *50* ]] ; then beep -f 700 -l 30; fi; done`
FIXME: Trying to alias the previous incantation within $HOME/.bashrc within double fails, likely due to \*press\* not being within quotes. Specifying the incantation within single quotes as an alias works.

FIXME: Trying to pipe the 'xinput' output to 'cut -b 5-' then to the infinite read while loop fails. This would be for using an if/then to omit modifier keys.

FIXME: If further omitting modifier keys (for which would be nice and more realistic), should condense the script to reduce the number of time consuming System or CPU processes.

#### Vim

[Vim](https://wiki.gentoo.org/wiki/Vim) can be made to sound like a typewriter as you're typing by a simple vimrc script.

**`$HOME/.vimrc`**

#### Python

Simple script for test bell:

**`/path/to/the/script.py`**

```
#!/usr/bin/env python
print ("\a")
```
or:

`user $````
python -c "print(\"\a\")"
```
## Troubleshooting

### No PC speaker beeps or sounds

Make sure your motherboard has a speaker pin outputs along with a speaker attached, check your BIOS PC Speaker or BIOS Keyboard settings for older hardware, and make sure the pcspkr module is not blacklisted. Use the setterm and xset utilities above for increasing the beep length, and ensure GNU Screen is not using the visual bell.

### Playing musical notes on the PC speaker

See the package [app-misc/beep](https://packages.gentoo.org/packages/app-misc/beep) manual page. Toward the end of the beep manual page, is documentation corresponding approximate frequency correlations for each of the musical scale notes.

### How do I mute the PC speaker?

Some methods for disabling the PC speaker:

- The easy method, and once and for all solution for muting the PC speaker, add the pcspkr module to your /etc/modprobe.d/blacklist.conf file or omit compiling of the driver within the kernel.
- For temporary or per user disabling, add "set bell-style none" or substitute "visual" for "none" within the /etc/inputrc file or the users $HOME/.inputrc file.
- Other temporary or per user methods include setting the beep length to zero or disabling the bell using setterm and xset utilities.
- If using GNU Screen, `Ctrl`+`a Ctrl`+`g`; or adding `vbell off` or `vbell on` within your $HOME/.screenrc; but you'll still have the bell everywhere else unless any of the above are performed.
