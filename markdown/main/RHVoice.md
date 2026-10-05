<!-- source: https://wiki.gentoo.org/wiki/RHVoice | group: Gentoo Wiki (Main) | wiki-title: RHVoice -->
---
title: RHVoice
url: https://wiki.gentoo.org/wiki/RHVoice
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-02-06"
fingerprint: be1cc41f0ba6b2c2
license: CC BY-SA 4.0
---

# RHVoice

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**RHVoice** is a text-to-speech engine with extended language support.

## Installation

### Emerge

Enable the [GURU](https://wiki.gentoo.org/wiki/GURU) repository:

`root #````
eselect repository enable guru
```
`root #````
emaint sync -r guru
```
Install the RHVoice metapackage:

`root #``emerge --ask app-accessibility/rhvoice`
It will pull voices for all locales set by the [*L10N* USE\_EXPAND](https://wiki.gentoo.org/wiki/Localization#Package_manager), skipping nonfree by default.

## Configuration

### Files

- /etc/RHVoice/RHVoice.conf - Global (system wide) configuration file.

### Speech Dispatcher

Uncomment the following line in /etc/speech-dispatcher/speechd.conf:

Set RHVoice as the default module (optional):

Test the new configuration:

`user $````
killall speech-dispatcher
```
`user $````
spd-say "Hello!"
```
## Usage

Test if RHVoice works:

`user $``echo "Hello!" | RHVoice-test`
