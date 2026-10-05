<!-- source: https://wiki.gentoo.org/wiki/Halloy | group: Gentoo Wiki (Main) | wiki-title: Halloy -->
---
title: halloy
url: https://wiki.gentoo.org/wiki/Halloy
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-15"
fingerprint: a7fd9d4a8e25999c
license: CC BY-SA 4.0
---

# halloy

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Halloy** is an open-source IRC client written in Rust, with the iced GUI library. It aims to provide a simple and fast client for Mac, Windows, and Linux platforms.

## Installation

### USE flags

Halloy supports the following USE flags:

opengl vulkan wayland X debug

### Emerge

Halloy is available on the [Project:GURU](https://wiki.gentoo.org/wiki/Project:GURU) repository.

Instructions for enabling this repository can be found here:
[Project:GURU/Information\_for\_End\_Users](https://wiki.gentoo.org/wiki/Project:GURU/Information_for_End_Users)

After enabling the repository, emerge Halloy.

`root #``emerge --ask net-irc/halloy`
## Usage

### Config

Halloy uses toml configuration files to manage connections and settings. An example configuration file:

**`~/.config/halloy/config.toml`**
