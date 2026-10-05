<!-- source: https://wiki.gentoo.org/wiki/Gnome_Cheat_Sheet | group: Gentoo Wiki (Main) | wiki-title: Gnome Cheat Sheet -->
---
title: Gnome Cheat Sheet
url: https://wiki.gentoo.org/wiki/Gnome_Cheat_Sheet
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2017-08-07"
fingerprint: "555b8419e8b323ba"
license: CC BY-SA 4.0
---

# Gnome Cheat Sheet

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Create a custom application launcher in GNOME Shell

Create a APP\_NAME.desktop (APP\_NAME application name) file under /usr/share/applications (or \~/.local/share/applications or directly in \~/Desktop) with the following content:

**`APP_NAME.desktop`**

**APP\_NAME.desktop file**

```
[Desktop Entry]
Encoding=UTF-8
Name=APP_NAME
Exec=/PATH/TO/APP/EXECUTABLE
Icon=/PATH/TO/APP/ICON
Type=Application
Categories=APPLICATION_CATEGORY_NAME;
```
For detailed `.desktop` specification (eg. list of [Registered Categories](https://specifications.freedesktop.org/menu-spec/latest/apa.html)) see: [specifications.freedesktop.org](https://specifications.freedesktop.org/desktop-entry-spec/desktop-entry-spec-latest.html).
