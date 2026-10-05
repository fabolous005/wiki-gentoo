<!-- source: https://wiki.gentoo.org/wiki/Xft_support_for_GNU_Emacs | group: Gentoo Wiki (Main) | wiki-title: Xft support for GNU Emacs -->
---
title: Xft support for GNU Emacs
url: https://wiki.gentoo.org/wiki/Xft_support_for_GNU_Emacs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-12-28"
fingerprint: a66411fde813f1a4
license: CC BY-SA 4.0
---

# Xft support for GNU Emacs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes how to enable font anti-aliasing in Emacs using the Xft library.

## Enabling anti-aliased fonts for Emacs

### Installation and setup

First, set the appropriate USE flags – you *must* have the `xft` flag.

`root #``echo "app-editors/emacs xft" >> /etc/portage/package.use`
Now it's time to install Emacs:

`root #``emerge --ask app-editors/emacs`
You can now install some XFT fonts such as [media-fonts/dejavu](https://packages.gentoo.org/packages/media-fonts/dejavu):

`root #``emerge --ask media-fonts/dejavu`
Try starting Emacs with the desired XFT fonts:

`user $``emacs --font 'DejaVu Sans Mono-12'`
If you're happy with this as your default font, set it in your [\~/.Xresources](https://wiki.gentoo.org/wiki/X_resources):

`user $````
echo "Emacs.font: DejaVu Sans Mono-12" >> ~/.Xresources
```
`user $````
xrdb -merge ~/.Xresources
```
#### Lucid toolkit

When Emacs was built with the Lucid toolkit (i.e. with the `athena` or `Xaw3d` USE flags), the font of the menubar can be set using the following in your \~/.Xresources:

#### Motif toolkit

For Emacs built with the Motif toolkit (i.e. with the `motif` USE flag enabled), anti-aliased fonts can be enabled using the XFT font renderer in Motif. Make sure that [x11-libs/motif](https://packages.gentoo.org/packages/x11-libs/motif) was built with the `xft` flag, and add for example the following to your \~/.Xresources, in order to set the font globally:

More specific control of resources is also possible, e.g., the following will set a bold font for the menubar:

Don't forget to load the resources:

`user $``xrdb -merge ~/.Xresources`
## External resources

For more details on Emacs with XFT pretty fonts, see:

- [XftGnuEmacs](https://www.emacswiki.org/emacs/XftGnuEmacs) on the Emacs wiki
