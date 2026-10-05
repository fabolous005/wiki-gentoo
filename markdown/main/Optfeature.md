<!-- source: https://wiki.gentoo.org/wiki/Optfeature | group: Gentoo Wiki (Main) | wiki-title: Optfeature -->
---
title: Optfeature
url: https://wiki.gentoo.org/wiki/Optfeature
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: "24f68a5a5d8710f9"
license: CC BY-SA 4.0
---

# Optfeature

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

"Optfeature" in short is a concept for optional features. Some programs are able to dynamically load a library and add functionality, while not breaking the main program if the optional library is missing. Some python scripts etc are essentially capable of doing the same. Optfeature can also be used to advertise additional programs to complete a full software suite, e.g. with [gui-wm/sway](https://packages.gentoo.org/packages/gui-wm/sway) one may have a better experience after also installing [gui-apps/swaybg](https://packages.gentoo.org/packages/gui-apps/swaybg), [gui-apps/swaylock](https://packages.gentoo.org/packages/gui-apps/swaylock), and so on. These additional programs aren't linked to sway and sway can work without them.

Optfeature is not to be mixed with ["automagic dependencies"](https://wiki.gentoo.org/wiki/Project:Quality_Assurance/Automagic_dependencies).

## Usage

### In ebuilds

```
inherit optfeature
...
...
pkg_postinst() {
        optfeature "foo support" app-misc/foo
        optfeature "bar support" app-misc/bar
}
```
See the [eclass documentation](https://devmanual.gentoo.org/eclass-reference/optfeature.eclass/index.html).

### In your system, as a user

Optional features can simply be **emerge**d and they will take effect. However bloating /var/lib/portage/world file may later make it harder to identify and remember why some entries are there. Another way is to use [portage sets](https://wiki.gentoo.org/wiki//etc/portage/sets) dedicated to optfeature. These set files allow commenting, to identify why an entry has been added.

**`/etc/portage/sets/optfeature`**

Add **@optfeature** to your /var/lib/portage/world\_sets file, or issue

`root #``emerge -av @optfeature`
## See also

- [https://www.gentoo.org/glep/glep-0062.html](https://www.gentoo.org/glep/glep-0062.html) - Optional runtime dependencies via runtime-switchable USE flags (deferred due to inactivity)
