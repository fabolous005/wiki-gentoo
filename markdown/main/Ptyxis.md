<!-- source: https://wiki.gentoo.org/wiki/Ptyxis | group: Gentoo Wiki (Main) | wiki-title: Ptyxis -->
---
title: Ptyxis
url: https://wiki.gentoo.org/wiki/Ptyxis
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-30"
fingerprint: "929ad47f4e81b36b"
license: CC BY-SA 4.0
---

# Ptyxis

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Ptyxis** is a terminal emulator for a container-oriented desktop.

## Installation

### USE flags


### Emerge

`root #``emerge --ask x11-terms/ptyxis`
## Troubleshooting

### Notifications Do not Work

See the troubleshooting section of the [GitHub repository](https://github.com/yonasBSD/ptyxis-terminal?tab=readme-ov-file#troubleshooting-issues).

### Terminal Transparency

See discussion on [Fedora forums](https://discussion.fedoraproject.org/t/use-dconf-to-set-transparency-for-ptyxis/135003).

### Open Terminal Here Not Working with Dolphin

Depending on the Ptyxis version, the "Open Terminal Here" option in Dolphin may not function properly when Ptyxis is the default terminal. It appears that more recent versions of Ptyxis may have a simple workaround as seen [here](https://discuss.kde.org/t/dolphins-open-terminal-here-doesnt-work-correctly-plasma-6/40670/7). However, this workaround does not work for older versions (possibly before 49.2) of Ptyxis as seen [here](https://github.com/ublue-os/bazzite/issues/3518).

Another workaround was found for Bazzite Linux that also works on Gentoo. Place this [bash wrapper](https://github.com/ublue-os/bazzite/blob/1545b4c1e952c1af533b57952241b673c4dde386/system_files/desktop/kinoite/usr/bin/kde-ptyxis) in /usr/local/bin and make it executable. This forces Pytxis to open a new window when called, which allows Dolphin to open the terminal in the correct directory. It is possible that this solution will no longer be necessary with future versions of Ptyxis.

## See also

- [Terminal\_emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) — emulates a video terminal within another display architecture (e.g. in [X](https://wiki.gentoo.org/wiki/X_server)).
