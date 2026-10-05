<!-- source: https://wiki.gentoo.org/wiki/Elsw | group: Gentoo Wiki (Main) | wiki-title: Elsw -->
---
title: elsw
url: https://wiki.gentoo.org/wiki/Elsw
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-27"
fingerprint: "4811f6feaed7970f"
license: CC BY-SA 4.0
---

# elsw

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**elsw** is a command line tool that provides a nice way to view the [World file (Portage)](<https://wiki.gentoo.org/wiki/World_file_(Portage)>).

## Installation

### Emerge

`root #``emerge --ask app-portage/elsw`
## Usage

elsw can be run with no arguments to display the world file:

`user $``elsw`
### Invocation

`user $``elsw --help````
usage: elsw [-h] [-V] [-a] [-i] [-w] [-s] [-e EXCLUDE]
elsw - View the Portage world file
options:
  -h, --help            show this help message and exit
  -V, --version         show program's version number and exit
  -a, --all             List all available packages
  -i, --installed       List @installed packages
  -w, --world           List @world set(s) packages
  -s, --sets            List non-@world packages
  -e, --exclude EXCLUDE
                        Exclude package categories from being listed (pass in
                        a delimited list string)
```
## See also

- [World file (Portage)](<https://wiki.gentoo.org/wiki/World_file_(Portage)>) — contains the user-selected "world" packages that are listed in the /var/lib/portage/world file.
- [World set (Portage)](<https://wiki.gentoo.org/wiki/World_set_(Portage)>) — the combination of the [*system set*](<https://wiki.gentoo.org/wiki/System_set_(Portage)>), the [*selected set*](<https://wiki.gentoo.org/wiki/Selected_set_(Portage)>), and the *@profile set*.
