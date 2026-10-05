<!-- source: https://wiki.gentoo.org/wiki/Khal | group: Gentoo Wiki (Main) | wiki-title: Khal -->
---
title: khal
url: https://wiki.gentoo.org/wiki/Khal
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-13"
fingerprint: ebc3b1ee97c3cbf1
license: CC BY-SA 4.0
---

# khal

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


khal is a CLI based calendar program that synchronizes with CalDAV utilizing [vdirsyncer](https://wiki.gentoo.org/wiki/Vdirsyncer).

## Installation

### USE flags


### Emerge

`root #``emerge --ask app-misc/khal`
## Configuration

A basic khal config file looks like the following:

**`~/.config/khal/config`**

Where the path is the [vdirsyncer](https://wiki.gentoo.org/wiki/Vdirsyncer) storage path for the specified calendar.

### Setting the date and time locale

To adjust the date and time locale, use the `[locale]` category like so:

**`~/.config/khal/config`**

Using this, dates will display as `1970-01-01`, times as `00:00`, and together as `00:00 1970-01-01`. With the UNIX epoch as the example date and time.

## Usage

### Displaying a list of events

To display a list of events, use the `list` argument:

`user $``khal list`
Today, 1970-01-01
07:00-15:00 Work
12:00-13:00 Lunch
