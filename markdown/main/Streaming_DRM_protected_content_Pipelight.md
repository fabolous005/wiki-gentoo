<!-- source: https://wiki.gentoo.org/wiki/Streaming_DRM_protected_content/Pipelight | group: Gentoo Wiki (Main) | wiki-title: Streaming DRM protected content/Pipelight -->
---
title: Streaming DRM protected content/Pipelight
url: https://wiki.gentoo.org/wiki/Streaming_DRM_protected_content/Pipelight
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-10-30"
fingerprint: fe101857949eb5e5
license: CC BY-SA 4.0
---

# Streaming DRM protected content/Pipelight

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

Recently it has become possible to watch Netflix on Linux and FreeBSD via [Wine](https://wiki.gentoo.org/wiki/Wine) with good performance.

## Prequisites

### xattr

File system extended attributes are used to keep the precious DRM working, so you’ll potentially need to add support in your kernel as well as having your file system mounted explicitly with the option.

#### Checking for support

`user $``touch ~/.xattr_test && setfattr -n 'user.testAttr' -v 'attribute value' ~/.xattr_test &> /dev/null; getfattr ~/.xattr_test 2>&1 | grep -q user.testAttr && echo 'It works!' || echo 'No workie!'; rm ~/.xattr_test &> /dev/null`
#### Adding support

In recent kernel versions there doesn’t appear to be an option for ext4 extended attributes, presumably meaning they’re always on already.

See [Kernel/Upgrade](https://wiki.gentoo.org/wiki/Kernel/Upgrade) for in-depth kernel rebuild instructions.

**`/etc/fstab`**

If you were only missing the option in /etc/fstab:

`root #``mount / -o remount`
## Installation

Initially, pipelight is only keyworded on **\~amd64**. Stable users will need to keyword it. Otherwise, it is a simple emerge command:

`root #``emerge --ask www-plugins/pipelight`
## Initialization

After the installation just initialize the plugin(s) you want to use. To use Netflix you have to enable Silverlight.

`user $``pipelight-plugin --enable silverlight`
Press `Y` to accept the plugin license. Then run Firefox (or your chosen web browser) from the Terminal to check if the plugin will run, i.e. for firefox-bin:

`user $``firefox-bin`
Watch the startup in the terminal for any errors. The plugin installation either starts immediately, or when visiting about:plugins in your webbrowser. If there are some errors please check again that you really installed Wine and Pipelight according to this page, if not then proceed with the following steps.

### Microsoft core fonts (optional)

For Wine-Compholio \< 1.7.14 this step was required such that Silverlight works properly (see this [FAQ entry](https://answers.launchpad.net/pipelight/+faq/2441)). With Wine-Compholio 1.7.14 or greater the patchset already includes a [Arial replacement font](https://github.com/compholio/wine-compholio-daily/tree/master/patches/10-Missing_Fonts), which seems to be sufficient for the Silverlight plugin.

To install the Microsoft core fonts for Wine, you can use winetricks; just emerge it:home directory):

`root #``emerge --ask winetricks`
Then run it, specifying the Wine prefix used for Pipelight:

`user $``WINEPREFIX=$HOME/.wine-pipelight winetricks`
Then select "Select the default wineprefix" -> "Install a font" -> "corefonts" and click "Okay".

### Netflix

Install an extension listed at [http://answers.launchpad.net/pipelight/+faq/2351](http://answers.launchpad.net/pipelight/+faq/2351) such as [https://addons.mozilla.org/en-US/firefox/addon/user-agent-switcher/](https://addons.mozilla.org/en-US/firefox/addon/user-agent-switcher/) and configure a Windows UA string, such as this one:[\*](http://answers.launchpad.net/pipelight/+faq/2351)

Mozilla/5.0 (Windows NT 6.1; WOW64; rv:15.0) Gecko/20120427 Firefox/15.0a1

Enjoy improved Netflix.

## Troubleshooting

- Try different UA strings
- (Re)Move \~/.wine-pipelight
- (Re)Move pluginreg.dat from \~/.mozilla. Find it by running:

- find \~/.mozilla/ -name 'pluginreg.dat'

- Check paths and other variables in \~/.config/pipelight-silverlight5.1 (or if nonexistent, /usr/share/pipelight/configs/pipelight-silverlight5.1)

`root #``eselect opengl list``root #``eselect opengl set #`
### Audio

If you use Pulseaudio but find that wine32 is trying to load 64-bit pulseaudio drivers, consider the solution found in this [here](https://forums.gentoo.org/viewtopic-p-7060060.html)
