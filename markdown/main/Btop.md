<!-- source: https://wiki.gentoo.org/wiki/Btop | group: Gentoo Wiki (Main) | wiki-title: Btop -->
---
title: btop
url: https://wiki.gentoo.org/wiki/Btop
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-13"
fingerprint: bea757620c39d9f1
license: CC BY-SA 4.0
---

# btop

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**btop** is a resource monitor that shows usage and stats for processor, memory, disks, network and processes. It is the third iteration of [bpytop](https://wiki.gentoo.org/wiki/Bpytop) and [bashtop](https://wiki.gentoo.org/wiki/Bashtop).

## Installation

### Emerge

`root #``emerge --ask sys-process/btop`
## Usage

To start btop, run:

`user $``btop`
When using btop via [ssh](https://wiki.gentoo.org/wiki/Ssh), and possibly other software such as [tmux](https://wiki.gentoo.org/wiki/Tmux), the `-t` option may be needed to get items to render correctly:

`user $``btop -t`
### Invocation

`user $``btop --help````
usage: btop [-h] [-v] [-/+t] [-p <id>] [--utf-force] [--debug]
optional arguments:
  -h, --help            show this help message and exit
  -v, --version         show version info and exit
  -lc, --low-color      disable truecolor, converts 24-bit colors to 256-color
  -t, --tty_on          force (ON) tty mode, max 16 colors and tty friendly graph symbols
  +t, --tty_off         force (OFF) tty mode
  -p, --preset <id>     start with preset, integer value between 0-9
  --utf-force           force start even if no UTF-8 locale was detected
  --debug               start in DEBUG mode: shows microsecond timer for information collect
                        and screen draw functions and sets loglevel to DEBUG
```
## GPU monitoring

### Intel GPU

To monitor an Intel GPU without requiring elevated privileges, grant btop the required CAP\_PERFMON capability using setcap.

`root #``` setcap cap_perfmon=+ep `readlink -f "$(command -v btop)"` `` ### AMD GPU

For monitoring an AMD GPU using btop, install the ROCm SMI library.

`user $````
emerge --ask dev-util/rocm-smi
```
Once ROCm SMI is installed, btop can monitor GPU usage, VRAM usage, clocks, and temperature.

### NVIDIA GPU

NVIDIA GPU monitoring should work out of the box with btop as long as the official NVIDIA drivers are installed (both closed and open kernel modules). No extra configuration is required.

## See also

- [htop](https://wiki.gentoo.org/wiki/Htop) — a cross-platform interactive process viewer. It is a text-mode application (for console or X terminals) and requires ncurses.
- [Recommended tools](https://wiki.gentoo.org/wiki/Recommended_tools) — lists system-administration related tools recommended for use in a **[shell](https://wiki.gentoo.org/wiki/Shell) environment** ([terminal/console](https://wiki.gentoo.org/wiki/Terminal_emulator))
