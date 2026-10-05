<!-- source: https://wiki.gentoo.org/wiki/Wofi | group: Gentoo Wiki (Main) | wiki-title: Wofi -->
---
title: Wofi
url: https://wiki.gentoo.org/wiki/Wofi
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-10-03"
fingerprint: b78d150f09873bc3
license: CC BY-SA 4.0
---

# Wofi

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Wofi** is a launcher/menu program for wlroots-based [Wayland](https://wiki.gentoo.org/wiki/Wayland) [compositors](https://wiki.gentoo.org/wiki/List_of_software_for_Wayland#Compositors), such as [Sway](https://wiki.gentoo.org/wiki/Sway).

## Installation

### Emerge

`root #``emerge --ask gui-apps/wofi`
### Configuration

Information about configuring Wofi can be found in the [wofi(5)](https://manpages.debian.org/bookworm/wofi/wofi.5.en.html) man page.

### Usage

One way to use Wofi is to create a simple shell script, e.g. wofi\_run.sh, which runs Wofi with a standard set of options and the list of items passed to it:

**`wofi_run.sh`**

Then, to open a dialog box with a list of programs to run:

`user $``wofi_run.sh run`
For further details about running wofi, refer to the [wofi(1)](https://manpages.debian.org/bookworm/wofi/wofi.1.en.html) man page. Information about modes available for the `--show` option can be found in the [wofi(7)](https://manpages.debian.org/bookworm/wofi/wofi.7.en.html) man page.

## See also

- [x11-misc/rofi](https://packages.gentoo.org/packages/x11-misc/rofi) - A window switcher, run dialog and dmenu replacement.
