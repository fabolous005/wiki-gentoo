<!-- source: https://wiki.gentoo.org/wiki/Selected-packages_set_(Portage) | group: Gentoo Wiki (Main) | wiki-title: Selected-packages set (Portage) -->
---
title: selected-packages set (Portage)
url: https://wiki.gentoo.org/wiki/Selected-packages_set_(Portage)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-02"
fingerprint: "2c39e37f8dd7930e"
license: CC BY-SA 4.0
---

# selected-packages set (Portage)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The **selected-packages set** contains the user-selected "world" packages that are listed in the /var/lib/portage/world file. The selected-packages set is colloquially referred to as the **world file**.

## Managing the selected-packages set

### Listing the selected-packages set

[eix](https://wiki.gentoo.org/wiki/Eix) can be used to list the selected-packages set:

`user $``eix -c --selected-file`
### Emerge a package without adding it to the world file

In order to avoid problems in dependency resolution when updating the system, the /var/lib/portage/world file should contain as few dependencies as possible.  So use the `--oneshot` (`-1`) option for emerging dependencies.

`root #``emerge --ask --oneshot <category/atom>`
### Checking the world file

The emaint command can be used to see if any problems exist in the world file:

`user $``/usr/sbin/emaint --check world`
Emaint: check world        100% \[============================================>\]

If any problems are found then run the following:

`root #``/usr/sbin/emaint --fix world`
### Adding an atom without recompilation

To add a package to the selected-packages set without *recompiling* the package:

`root #``emerge --ask --noreplace <category/atom>`
It will add the atom to the /var/lib/portage/world file without compiling it again.

## Tips

### Editing world file by hand

Though the [emerge](https://wiki.gentoo.org/wiki/Emerge) man page says that the world file can "safely" be edited by hand, Portage will aggressively rewrite that file. Comments or changes in order of packages will be lost and there will be no checking for typos.

The `--deselect` (`-W`) or `--noreplace` (`-n`) options to the emerge command may be used to add or remove packages from the world file, without actually performing package installation or removal.

## See also

- [Package sets](https://wiki.gentoo.org/wiki/Package_sets) — describes package sets in high detail and includes a list of all typically available sets on a Gentoo system.
- [/etc/portage/sets](https://wiki.gentoo.org/wiki//etc/portage/sets) — an optional directory that is used to create user defined package sets
- [User:Sam/Portage help/Maintaining a Gentoo system#World file hygiene](https://wiki.gentoo.org/wiki/User:Sam/Portage_help/Maintaining_a_Gentoo_system#World_file_hygiene)
- [User:Vaukai/checkworldfile](https://wiki.gentoo.org/wiki/User:Vaukai/checkworldfile) ([alternative version](https://wiki.gentoo.org/wiki/User:Luttztfz/checkworldfile))

## External resources

- [https://forums.gentoo.org/viewtopic-t-1075276.html](https://forums.gentoo.org/viewtopic-t-1075276.html) - Cleaning the world file (wiki) - check the script.
