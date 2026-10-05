<!-- source: https://wiki.gentoo.org/wiki//etc/portage/sets | group: Gentoo Wiki (Main) | wiki-title: /etc/portage/sets -->
---
title: "/etc/portage/sets"
url: https://wiki.gentoo.org/wiki//etc/portage/sets
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-29"
fingerprint: a7a93b6e8ae7892d
license: CC BY-SA 4.0
---

# /etc/portage/sets

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

/etc/portage/sets is an optional directory that is used to create user defined package sets. For each file in /etc/portage/sets, a package set is created, defining a one-to-many relationship between a set name and an arbitrary group of package atoms. User-defined set names correspond to the name of the file. Each file should contain a list of package atoms and nested package sets, one per line. You can see the full list of sets that exist at any time with

`user $``emerge --list-sets`
## Example

For the sake of example, say [Larry](https://wiki.gentoo.org/wiki/Larry) wanted to run live ebuilds for GPU drivers, and desired an easy shorthand for rebuilding them on demand. He could define the following set:

**`/etc/portage/sets/gpu-stack`**

```
x11-libs/libdrm
media-libs/mesa
x11-drivers/xf86-video-ati
x11-drivers/xf86-video-intel
```
Now, all that is needed to trigger a rebuild of all of them at once is:

`root #``emerge --ask @gpu-stack`
These are the packages that would be merged, in order:
Calculating dependencies \
...done!
\[ebuild   R   #\] x11-libs/libdrm-9999
\[ebuild   R   #\] x11-drivers/xf86-video-intel-9999
\[ebuild   R   #\] x11-drivers/xf86-video-ati-9999
\[ebuild   R   \~\] media-libs/mesa-9999
Would you like to merge these packages? \[Yes/No\]

To remove a user defined set from world\_sets, use the `--deselect` option to emerge, then clean up orphaned dependencies:

`root #````
emerge --deselect @gpu-stack
```
`root #````
emerge --ask --depclean
```
Comments can be added into set files and can be used for organization, making multi-purpose set files a possibility. System administrators can add desires packages into one big file, or create multiple set files per category or concept for more granularity. Set files can easily integrate into configuration management systems.

**`/etc/portage/sets/custom_package_set`**

```
# file compression utilities
app-arch/p7zip
app-arch/unrar
# top style utilities
sys-process/iotop
sys-process/lsof
# mail reader
mail-client/neomutt
```
To include the list of packages within the custom **@custom\_package\_set** set, add the name of the set to the [/var/lib/portage](https://wiki.gentoo.org/wiki//var/lib/portage)/world\_sets file:

`root #``echo "@custom_package_set" >> /var/lib/portage/world_sets`
Or simply emerge the set using the set name:

`root #``emerge -av @custom_package_set`
## See also

- [Package sets](https://wiki.gentoo.org/wiki/Package_sets) — describes package sets in high detail and includes a list of all typically available sets on a Gentoo system.
- [System set (Portage)](<https://wiki.gentoo.org/wiki/System_set_(Portage)>) — the software packages required for a standard Gentoo Linux installation to run properly.
- [World set (Portage)](<https://wiki.gentoo.org/wiki/World_set_(Portage)>) — the combination of the [*system set*](<https://wiki.gentoo.org/wiki/System_set_(Portage)>), the [*selected set*](<https://wiki.gentoo.org/wiki/Selected_set_(Portage)>), and the *@profile set*.

## External resources

For more detailed information about package sets, read the emerge manpage and search for "set" (`/^\s*set`).
