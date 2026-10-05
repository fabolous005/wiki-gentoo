<!-- source: https://wiki.gentoo.org/wiki/Recommended_tools | group: Gentoo Wiki (Main) | wiki-title: Recommended tools -->
---
title: Recommended tools
url: https://wiki.gentoo.org/wiki/Recommended_tools
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-07"
fingerprint: "15df5e7a48852bd5"
license: CC BY-SA 4.0
---

# Recommended tools

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page lists system-administration related tools recommended for use in a **[shell](https://wiki.gentoo.org/wiki/Shell) environment** ([terminal/console](https://wiki.gentoo.org/wiki/Terminal_emulator)), with suggestions for reliable and [easy to install](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Portage#Installing_software) software for common Gentoo Linux needs.

Most of these packages are in the [stable branch](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Branches#Stable), but some useful and otherwise high quality software is still in the [testing branch](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Branches#Testing). Testing branch packages may be made available for installation by [accepting a keyword for a single package](https://wiki.gentoo.org/wiki/Knowledge_Base:Accepting_a_keyword_for_a_single_package), however packages from the testing branch should only be used after [taking note of any risks](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Branches#Testing)}.

To reference a new piece of software here, please read the [adding to this page](https://wiki.gentoo.org/wiki/Recommended_tools#Adding_to_this_page) section.

This is a "best of kind" list, much more software is available on Gentoo. Use [eix](https://wiki.gentoo.org/wiki/Eix) or browse [packages.gentoo.org](https://packages.gentoo.org/categories) to find **all** applications available on Gentoo.

The applications listed here should be widely useful, and of sufficient quality, to merit inclusion.

If you regularly use a desktop software package from the Gentoo repository and can confirm it is of *excellent quality*, *stable*, and of ***broad appeal for common tasks***, please [add it](https://wiki.gentoo.org/wiki/Help:Editing_pages) to the list ! The software should at least be *maintained* (i.e. relatively recent commits to the source; have periodic releases; not have too many reported bugs; most bugs should be getting fixed rather than accumulating, etc.), and preferably be *well documented* and from the stable branch. Please don't use this page just to promote a package because you like it, are an author or have other interest etc.

It is good practice to create a [stub article](https://wiki.gentoo.org/wiki/Help:Starting_a_new_page#Stub_articles) for any package added here that does not have a page already, as anything notable enough to be listed here will also be notable enough to have a dedicated page.

| Name | Package | Description | 
|---|---|---|
| [fdupes](https://wiki.gentoo.org/wiki/Fdupes) | [app-misc/fdupes](https://packages.gentoo.org/packages/app-misc/fdupes) | Identify duplicate files residing in specified directories. | 
| fzf | [app-shells/fzf](https://packages.gentoo.org/packages/app-shells/fzf) | Super-fast replacement for find that enables fuzzy searching of files (and, also, searches command history, processes, bookmarks, git commits, etc). | 
| Midnight Commander | [app-misc/mc](https://packages.gentoo.org/packages/app-misc/mc) | GNU Midnight Commander is a text based file manager. | 
| n³ | [app-misc/nnn](https://packages.gentoo.org/packages/app-misc/nnn) | Missing terminal file manager for X. | 
| [ranger](https://wiki.gentoo.org/wiki/Ranger) | [app-misc/ranger](https://packages.gentoo.org/packages/app-misc/ranger) | Console file manager with VI key bindings providing a minimalistic curses interface with a view on the directory hierarchy. | 
| [Vifm](https://wiki.gentoo.org/wiki/Vifm) | [app-misc/vifm](https://packages.gentoo.org/packages/app-misc/vifm) | Console file manager with vi(m)-like keybindings. Offers familiar navigation for vim junkies. | 

See also the article on [file managers](https://wiki.gentoo.org/wiki/File_managers#Command_line).

| Name | Package | Description | 
|---|---|---|
| acpiclient | [sys-power/acpi](https://packages.gentoo.org/packages/sys-power/acpi) | Attempts to replicate the functionality of the 'old' apm command on ACPI systems. | 
| AcpiTool | [sys-power/acpitool](https://packages.gentoo.org/packages/sys-power/acpitool) | Linux ACPI client, allowing you to query or set ACPI values. It provides informations on battery status, AC adapter presence, thermal reading, etc. | 

## Hardware information

| Name | Package | Description | 
|---|---|---|
| cpuid | [sys-apps/cpuid](https://packages.gentoo.org/packages/sys-apps/cpuid) | Linux tool to dump x86 CPUID information about the CPUs. | 
| [fastfetch](https://wiki.gentoo.org/wiki/Fastfetch) | [app-misc/fastfetch](https://packages.gentoo.org/packages/app-misc/fastfetch) | neofetch-like tool mainly written in C, updated often, with performance and customizability in mind. | 
| [hwinfo](https://wiki.gentoo.org/wiki/Hwinfo) | [sys-apps/hwinfo](https://packages.gentoo.org/packages/sys-apps/hwinfo) | Small utility created by OpenSUSE to gather information on system hardware. | 
| [Neofetch](https://wiki.gentoo.org/wiki/Neofetch) | [app-misc/neofetch](https://packages.gentoo.org/packages/app-misc/neofetch) | Simple information system script, presents pretty system info in terminal. | 
| [pciutils](https://wiki.gentoo.org/wiki/Pciutils) | [sys-apps/pciutils](https://packages.gentoo.org/packages/sys-apps/pciutils) | Utilities dealing with the PCI bus. Run lspci to list PCI devices. | 
| resolve-march-native | [app-misc/resolve-march-native](https://packages.gentoo.org/packages/app-misc/resolve-march-native) | Resolve GCC flag -march=native, read the CPU id product family codename. | 
| [usbutils](https://wiki.gentoo.org/wiki/Usbutils) | [sys-apps/usbutils](https://packages.gentoo.org/packages/sys-apps/usbutils) | Get information on USB devices. | 

See also [hardware detection](https://wiki.gentoo.org/wiki/Hardware_detection).

| Name | Package | Description | 
|---|---|---|
| iftop | [net-analyzer/iftop](https://packages.gentoo.org/packages/net-analyzer/iftop) | Display bandwidth usage on an interface. | 
| IPTraf-ng | [net-analyzer/iptraf-ng](https://packages.gentoo.org/packages/net-analyzer/iptraf-ng) | Console-based network monitoring utility. | 
| Layer Four Traceroute (LFT) | [net-analyzer/lft](https://packages.gentoo.org/packages/net-analyzer/lft) | An advanced traceroute implementation. | 
| [nload](https://wiki.gentoo.org/wiki/Nload) | [net-analyzer/nload](https://packages.gentoo.org/packages/net-analyzer/nload) | Console application which monitors network traffic and bandwidth usage in real time. | 
| vnStat | [net-analyzer/vnstat](https://packages.gentoo.org/packages/net-analyzer/vnstat) | Network traffic monitoring. | 
| wavemon | [net-wireless/wavemon](https://packages.gentoo.org/packages/net-wireless/wavemon) | Curses based monitor for IEEE 802.11 wireless LAN cards. | 
| Xprobe | [net-analyzer/xprobe](https://packages.gentoo.org/packages/net-analyzer/xprobe) | Active OS fingerprinting tool. This is Xprobe2. | 
| Yersinia | [net-analyzer/yersinia](https://packages.gentoo.org/packages/net-analyzer/yersinia) | FrameWork for layer 2 protocol attacks. Working on DHCP, STP, IEEE 802.1q and also some other Cisco proprietary network protocols. | 

See the [pager](https://wiki.gentoo.org/wiki/Pager) article.

| Name | Package | Description | 
|---|---|---|
| [Atuin](https://wiki.gentoo.org/wiki/Atuin) | [app-shells/atuin](https://packages.gentoo.org/packages/app-shells/atuin) | History manager supporting encrypted synchronization. | 
| autojump | [app-shells/autojump](https://packages.gentoo.org/packages/app-shells/autojump) | Change directory command that learns. | 
| Generic Colouriser | [app-misc/grc](https://packages.gentoo.org/packages/app-misc/grc) | Generic colouriser that beautifies system log files or command output. | 
| rlwrap | [app-misc/rlwrap](https://packages.gentoo.org/packages/app-misc/rlwrap) | Readline wrapper. | 
| [wgetpaste](https://wiki.gentoo.org/wiki/Wgetpaste) | [app-text/wgetpaste](https://packages.gentoo.org/packages/app-text/wgetpaste) | Command-line interface to various pastebin-like websites. | 
| Xclip | [x11-misc/xclip](https://packages.gentoo.org/packages/x11-misc/xclip) | Command-line interface to X selections (clipboard). | 

See also [the shell article](https://wiki.gentoo.org/wiki/Shell) for available command-line interpreters.

| Name | Package | Description | 
|---|---|---|
| atop | [sys-process/atop](https://packages.gentoo.org/packages/sys-process/atop) | Resource-specific view of processes. | 
| [btop](https://wiki.gentoo.org/wiki/Btop) | [sys-process/btop](https://packages.gentoo.org/packages/sys-process/btop) | A monitor of resources | 
| [htop](https://wiki.gentoo.org/wiki/Htop) | [sys-process/htop](https://packages.gentoo.org/packages/sys-process/htop) | Interactive process viewer (improved alternative for top), with easy function-keys for process management. | 
| Iotop | [sys-process/iotop](https://packages.gentoo.org/packages/sys-process/iotop) | Simple top like I/O monitor. | 
| lsof | [sys-process/lsof](https://packages.gentoo.org/packages/sys-process/lsof) | Lists open files for running Unix processes. | 
| NCurses Disk Usage (ncdu and ncdu-bin) | [sys-fs/ncdu](https://packages.gentoo.org/packages/sys-fs/ncdu) [sys-fs/ncdu-bin](https://packages.gentoo.org/packages/sys-fs/ncdu-bin) | Curses-based disk usage tool, with easy navigation through the filesystem tree to see du results. | 
| nmon | [sys-process/nmon](https://packages.gentoo.org/packages/sys-process/nmon) | Nigel's performance MONitor for CPU, memory, network, disks, etc. nmon uses ascii graphics for some informations and is able to log data to csv files. | 
| pydf | [app-admin/pydf](https://packages.gentoo.org/packages/app-admin/pydf) | Enhanced df (disk free) tool, which uses colors and a semi-graphical representation of disk usage. | 

[Terminal](https://wiki.gentoo.org/wiki/Terminal_emulator) multiplexers manage several applications simultaneously on the command line. Often they manage sessions in the background and allow reattaching if a terminal is closed or a connection lost. Some also permit some form of session saving, even across reboots.

| Name | Package | Description | 
|---|---|---|
| abduco | [app-misc/abduco](https://packages.gentoo.org/packages/app-misc/abduco) | lightweight session manager with {de,at}tach support | 
| byobu | [app-misc/byobu](https://packages.gentoo.org/packages/app-misc/byobu) | GPLv3 text-based window manager and terminal multiplexer. | 
| dtach | [app-misc/dtach](https://packages.gentoo.org/packages/app-misc/dtach) | Run a program detached from the terminal and reattach to it later. Useful for long emerges for example. May be unmaintained. | 
| **d**ynamic **v**irtual **t**erminal **m**anager | [app-misc/dvtm](https://packages.gentoo.org/packages/app-misc/dvtm) | Console window manager for working with multiple console based programs. Works with [app-misc/abduco](https://packages.gentoo.org/packages/app-misc/abduco) to provide session management. | 
| [screen](https://wiki.gentoo.org/wiki/Screen) | [app-misc/screen](https://packages.gentoo.org/packages/app-misc/screen) | Screen manager with VT100/ANSI terminal emulation. | 
| [tmux](https://wiki.gentoo.org/wiki/Tmux) | [app-misc/tmux](https://packages.gentoo.org/packages/app-misc/tmux) | Terminal multiplexer. Used with the *continuum* and *resurrect* extensions, can restore environment after reboot. | 
| [zellij](https://wiki.gentoo.org/wiki/Zellij) | [app-misc/zellij](https://packages.gentoo.org/packages/app-misc/zellij) | The latest one among alternatives, written in Rust. | 

See the [Version control systems](https://wiki.gentoo.org/wiki/Version_control_systems) article.

| Name | Package | Description | 
|---|---|---|
| [etckeeper](https://wiki.gentoo.org/wiki/Etckeeper) | [sys-apps/etckeeper](https://packages.gentoo.org/packages/sys-apps/etckeeper) | Log /etc changes to a version control system to keep a backup of modifications to configuration files. | 
| Mytop | [dev-db/mytop](https://packages.gentoo.org/packages/dev-db/mytop) | top clone for mysql. N.B. a customized mytop is included in >=dev-db/mariadb-5.3. | 
| [Pass](https://wiki.gentoo.org/wiki/Pass) | [app-admin/pass](https://packages.gentoo.org/packages/app-admin/pass) | Stores, retrieves, generates, and synchronizes passwords securely using gpg, pwgen, and git. | 
| Visual Binary Diff (VBinDiff) | [dev-util/vbindiff](https://packages.gentoo.org/packages/dev-util/vbindiff) | Visual diffing tool for binary files. | 
| Whowatch | [app-admin/whowatch](https://packages.gentoo.org/packages/app-admin/whowatch) | Interactive who-like program that displays information about users currently logged on in real time. | 

- [Gentoo for Network Admins](https://wiki.gentoo.org/wiki/Gentoo_for_Network_Admins) — a guide for forging Gentoo into a fully-fledged, network-debugging Swiss army knife.
- [Qt Desktop applications](https://wiki.gentoo.org/wiki/Qt_Desktop_applications) — a list of recommendations for a light-weight, non-[KDE](https://wiki.gentoo.org/wiki/KDE), [Qt](https://wiki.gentoo.org/wiki/Qt)-only desktop environment.
- [Recommended applications](https://wiki.gentoo.org/wiki/Recommended_applications) — applications recommended for use in a graphical environment ([X11](https://wiki.gentoo.org/wiki/Xorg), [Wayland](https://wiki.gentoo.org/wiki/Wayland))
- [Shell](https://wiki.gentoo.org/wiki/Shell) — command-line interpreter that provides a text-based interface to users
- [Terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) — emulates a video terminal within another display architecture (e.g. in [X](https://wiki.gentoo.org/wiki/X_server)).
- [Terminal productivity software](https://wiki.gentoo.org/wiki/Terminal_productivity_software) — applications designed to run within the constraints of a text-based terminal window that are typically associated with GUI-based office productivity software
- [Wayland Desktop Landscape](https://wiki.gentoo.org/wiki/Wayland_Desktop_Landscape) — various desktop related packages for Wayland

- [Backup](https://wiki.gentoo.org/wiki/Backup) — prevent loss of data by ensuring it can be recovered.
- [Text editor](https://wiki.gentoo.org/wiki/Text_editor) — a program to create and edit text files.
- [Useful Portage tools](https://wiki.gentoo.org/wiki/Useful_Portage_tools) — provides a list of Gentoo-specific system management tools, notably for [Portage](https://wiki.gentoo.org/wiki/Portage), available in the [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository).
