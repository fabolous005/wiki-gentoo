<!-- source: https://wiki.gentoo.org/wiki/Localization | group: Gentoo Wiki (Main) | wiki-title: Localization -->
---
title: Localization
url: https://wiki.gentoo.org/wiki/Localization
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-24"
fingerprint: "5bbe5be99a99184"
license: CC BY-SA 4.0
---

# Localization

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

When dealing with GNU/Linux systems in an international context or for a specific country or region, both localization (abbreviated to *[l10n](https://en.wikipedia.org/wiki/Language_localisation)*) and internationalization (abbreviated to *[i18n](https://en.wikipedia.org/wiki/Internationalization_and_localization)*) play an important part. It allows administrators and users to select the language of choice on the platform, timezone selection, character ordering and more.

In Gentoo Linux, localization is supported in various levels ranging from kernel support up to end user application support.

The handbook explains [locale setup](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Configure_locales) during installation.

## Localization in GNU/Linux

Localization plays a part in many layers of a GNU/Linux system.

### Linux kernel

In the Linux kernel, localization is enabled through the *Native Language Support* setting as exemplified by the [UTF-8 article](https://wiki.gentoo.org/wiki/UTF-8#Application_support).

### Core system

On the core system level (C libraries and affiliated tools), most localization is handled through the [locale system](https://wiki.gentoo.org/wiki/Localization/Guide#Locale_system) and [console keyboard layout](https://wiki.gentoo.org/wiki/Localization/Guide#Keyboard_layout_for_the_console) which are described well in the [Gentoo Localization Guide](https://wiki.gentoo.org/wiki/Localization/Guide) article.

### Package manager

#### Portage

Portage respects the `USE_EXPAND` variable called `L10N` in /etc/portage/:

**`/etc/portage/package.use/localization`**

**L10N assignment**

```
 L10N: en en-US
```
### Graphical environments

For the graphical environments, Xorg honors the locale settings, but has its own method for selecting the [keyboard layout for the X server](https://wiki.gentoo.org/wiki/Localization/Guide#Keyboard_layout_for_the_X_server). The desktop environments on top, such as [KDE](https://wiki.gentoo.org/wiki/KDE#Localization) and [GNOME](https://wiki.gentoo.org/wiki/GNOME), might have additional steps you have to go through in order to enable the localization and internationalization settings correctly.
