<!-- source: https://wiki.gentoo.org/wiki/Overgrowth | group: Gentoo Wiki (Main) | wiki-title: Overgrowth -->
---
title: Overgrowth
url: https://wiki.gentoo.org/wiki/Overgrowth
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-12-19"
fingerprint: "8d51971b9ad7b69b"
license: CC BY-SA 4.0
---

# Overgrowth

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Overgrowth is a third person, 3D, fast paced, cross-platform action computer game developed by Wolfire Games. It has been in development since September 17, 2008<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

## Installation

At this point, since the game is currently in development, the install requirements might change. Hopefully this wiki page will reflect package requirements accordingly. At minimum the following packages are required for Overgrowth to run properly:

`root #``emerge --ask --verbose gnome-base/gconf media-libs/freeimage media-libs/freealut media-libs/libpng`
## Configuration

### User groups

Make sure the users that will be running the game have the proper group permissions. Substitute `<username>` in the command below for each user that will require permission to play Overgrowth:

`root #````
gpasswd -a <username> audio
```
`root #````
gpasswd -a <username> video
```
## Troubleshooting

### S3TC support

If the game display a white box during the loading screen it might be because S3TC support is missing. This is fixable by installing the [media-libs/libtxc\_dxtn](https://packages.gentoo.org/packages/media-libs/libtxc_dxtn) package:

`root #``emerge --ask --verbose media-libs/libtxc_dxtn`
## See also

- [Steam](https://wiki.gentoo.org/wiki/Steam) — a video game digital distribution service by Valve.

## External resources

- [Wolfire Games](https://www.wolfire.com/) - The parent company of Overgrowth.
- [Wolfire blog](http://blog.wolfire.com/) - Used to post status updates for *Overgrowth*
- [What is *Overgrowth* Video](https://www.youtube.com/watch?v=Vb9NK2t2JuQ) - A video explaining Overgrowth.
- [Official Wolfire Wiki](https://wiki.wolfire.com/index.php/Main_Page)
- [Overgrowth on Linux (Official Wolfire Wiki)](https://wiki.wolfire.com/index.php/Overgrowth_Linux)
- [Wolfires Overgrowth Comics](https://www.wolfire.com/comic) - The Lore of Overgrowth.
