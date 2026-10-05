<!-- source: https://wiki.gentoo.org/wiki/List_of_software_for_Wayland | group: Gentoo Wiki (Main) | wiki-title: List of software for Wayland -->
---
title: List of software for Wayland
url: https://wiki.gentoo.org/wiki/List_of_software_for_Wayland
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-07"
categories: ['gui-wm']
fingerprint: "387755c94ab07eed"
license: CC BY-SA 4.0
---

# List of software for Wayland

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page gives brief lists of various desktop related packages for Wayland, such as compositors, application launchers, and so on.

For the basics of Wayland, see [Wayland](https://wiki.gentoo.org/wiki/Wayland).

## Compositors

In Gentoo, many Wayland compositors are found in the category [gui-wm](https://packages.gentoo.org/categories/gui-wm).

Window managers can be classified mostly as three kinds below. (Notice Wayland compositors have the role of window managers.)

- **Stacking** (aka **floating**): The traditional mode of how window managers are expected to behave, similar to that of Windows or OS X. Windows act like pieces of paper on a desk, and can be stacked on top of each other.
- **Tiling**: Windows are *tiled* so that none of them overlap. These usually make very extensive use of key-bindings and traditionally have little to no reliance on the mouse. Tiling window managers may be manual, offer predefined layouts, or both.
- **Dynamic**: These window managers can *dynamically* switch between stacking and tiling configurations.
- **Kiosk**: A single full-screen application is allowed, and attempts are made to prevent switching to any other application by normally-privileged users.

In the following table, the "wlroots?" column indicates whether a compositor is based on the [wlroots](https://wiki.gentoo.org/wiki/Wlroots) library. Certain Wayland applications, such as [Wofi](https://wiki.gentoo.org/wiki/Wofi) and [gui-apps/wayvnc](https://packages.gentoo.org/packages/gui-apps/wayvnc), are designed for use with wlroots-based compositors; wlroots implements various [Wayland protocols](https://wayland.app/protocols/) not necessarily supported by all compositors.

| Name | Package | Type | [wlroots](https://wiki.gentoo.org/wiki/Wlroots)? | Description | 
|---|---|---|---|---|
| [Cage](https://github.com/cage-kiosk/cage) | [gui-wm/cage::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-wm/cage) | Kiosk | Yes | \[Beta\] Kiosk based compositor for displaying a single fullscreen application (in GURU overlay) | 
| [Cagebreak](https://github.com/project-repo/cagebreak) | [gui-wm/cagebreak::wayland-desktop](https://github.com/bsd-ac/wayland-desktop/tree/master/gui-wm/cagebreak) | Tiling | Yes | \[Beta\] Tiling compositor inspired by RatPoison (in wayland-desktop overlay) | 
| [dwl](https://codeberg.org/dwl/dwl) | [gui-wm/dwl](https://packages.gentoo.org/packages/gui-wm/dwl) | Tiling | Yes | \[Unstable\] DWM clone | 
| [Enlightenment](https://wiki.gentoo.org/wiki/Enlightenment) | [x11-wm/enlightenment](https://packages.gentoo.org/packages/x11-wm/enlightenment) | Stacking | No? | Eye candy compositor part of the Enlightenment desktop environment | 
| [Hyprland](https://wiki.gentoo.org/wiki/Hyprland) | [gui-wm/hyprland::hyproverlay](https://repos.gentoo.org/#hyproverlay) | Tiling | No <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> | Dynamic tiling compositor that doesn't sacrifice on its looks | 
| [Kiwmi](https://github.com/buffet/kiwmi) | [gui-wm/kiwmi::wayland-desktop](https://github.com/bsd-ac/wayland-desktop/tree/master/gui-wm/kiwmi) | Stacking | Yes | \[Unstable\] Fully programmable compositor configurable with Lua (in wayland-desktop overlay) | 
| [KWin](https://wiki.gentoo.org/wiki/KWin) | [kde-plasma/kwin](https://packages.gentoo.org/packages/kde-plasma/kwin) | Dynamic | No | [KDE](https://wiki.gentoo.org/wiki/KDE)'s compositing window manager/Wayland compositor. | 
| [Labwc](https://wiki.gentoo.org/wiki/Labwc) | [gui-wm/labwc](https://packages.gentoo.org/packages/gui-wm/labwc) | Stacking | Yes | Openbox inspired stacking compositor | 
| [Liri](https://github.com/lirios/shell) | [gui-liri/liri-shell::wayland-desktop](https://github.com/bsd-ac/wayland-desktop/tree/master/gui-liri/liri-shell) | Stacking | No? | \[Unstable\] QT shell from LiriOS (in wayland-desktop overlay) | 
| [Mango](https://wiki.gentoo.org/wiki/MangoWM) | [gui-wm/mangowm::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-wm/mangowm) | Variable layout | Yes | Lightweight compositor based on [wlroots](https://wiki.gentoo.org/wiki/Wlroots) and scenefx | 
| [Miracle](https://miracle-wm.org/) | [gui-wm/miracle-wm::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-wm/miracle-wm) | Tiling | No | Built on top of [Mir](https://github.com/canonical/mir) | 
| [Mutter](https://wiki.gentoo.org/wiki/Mutter) | [x11-wm/mutter](https://packages.gentoo.org/packages/x11-wm/mutter) | Stacking | No | [GNOME](https://wiki.gentoo.org/wiki/GNOME)'s compositing window manager/Wayland compositor. | 
| [Newm](https://github.com/jbuchermn/newm) | [gui-wm/newm::wayland-desktop](https://github.com/bsd-ac/wayland-desktop/tree/master/gui-wm/newm) | Tiling | ? | \[Unstable\] Wayland compositor written with laptops and touchpads in mind (in wayland-desktop overlay). Note that newm has now become unmaintained <sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>, though a fork of it has been created<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>. | 
| [Niri](https://wiki.gentoo.org/wiki/Niri) | [gui-wm/niri::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-wm/niri) | Tiling | No | A scrollable-tiling Wayland compositor. | 
| [River](https://wiki.gentoo.org/wiki/River) | [gui-wm/river::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-wm/river) | Variable layout | Yes | \[Unstable\] Non-monolithic compositor separating the window manager from the compositor (in GURU overlay). | 
| [Sway](https://wiki.gentoo.org/wiki/Sway) | [gui-wm/sway](https://packages.gentoo.org/packages/gui-wm/sway) | Tiling | Yes | [x11-wm/i3](https://packages.gentoo.org/packages/x11-wm/i3) clone | 
| [Waybox](https://github.com/wizbright/waybox) | [gui-wm/waybox::wayland-desktop](https://github.com/bsd-ac/wayland-desktop/tree/master/gui-wm/waybox) | Stacking | Yes | \[Unstable\] OpenBox clone (in wayland-desktop overlay) | 
| [Wayfire](https://wiki.gentoo.org/wiki/Wayfire) | [gui-wm/wayfire](https://packages.gentoo.org/packages/gui-wm/wayfire) | Stacking | Yes | Beautiful, eye candy compositor inspired by [Compiz](https://wiki.gentoo.org/wiki/Compiz) | 
| [Weston](https://wiki.gentoo.org/wiki/Weston) | [dev-libs/weston](https://packages.gentoo.org/packages/dev-libs/weston) | Stacking | No | \[Not for general use\] Reference compositor implementation for developers | 

### Functionality by compositor

The following tables are based on the information provided at [Wayland Explorer](https://wayland.app); last updated 3 May 2026. A number indicates which version of an extension is supported by a compositor.

#### Wayland extensions marked 'core'

| Extension | Latest version | [KWin](https://wiki.gentoo.org/wiki/KWin) | [Mir](https://wiki.gentoo.org/index.php?title=Mir&action=edit&redlink=1) | [Mutter](https://wiki.gentoo.org/wiki/Mutter) | [Niri](https://wiki.gentoo.org/wiki/Niri) | [Sway](https://wiki.gentoo.org/wiki/Sway) | [Weston](https://wiki.gentoo.org/wiki/Weston) | 
|---|---|---|---|---|---|---|---|
| [wl\_compositor](https://wayland.app/protocols/wayland#wl_compositor) | 7 | 6 | 6 | 6 | 6 | 6 | 5 | 
| [wl\_shm](https://wayland.app/protocols/wayland#wl_shm) | 2 | 2 | 1 | 2 | 2 | 2 | 2 | 
| [wl\_data\_device\_manager](https://wayland.app/protocols/wayland#wl_data_device_manager) | 4 | 3 | 3 | 3 | 3 | 3 | 3 | 
| [wl\_shell](https://wayland.app/protocols/wayland#wl_shell) | 1 (deprecated) | ✗ | 1 | ✗ | ✗ | ✗ | ✗ | 
| [wl\_seat](https://wayland.app/protocols/wayland#wl_seat) | 10 | 10 | 9 | 10 | 9 | 9 | 7 | 
| [wl\_output](https://wayland.app/protocols/wayland#wl_output) | 4 | 4 | 4 | 4 | 4 | 4 | 4 | 
| [wl\_subcompositor](https://wayland.app/protocols/wayland#wl_subcompositor) | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 
| [wl\_fixes](https://wayland.app/protocols/wayland#wl_fixes) | 1 | 1 | ✗ | 1 | ✗ | ✗ | ✗ | 

#### Wayland extensions marked 'stable'

| Extension | Latest version | [KWin](https://wiki.gentoo.org/wiki/KWin) | [Mir](https://wiki.gentoo.org/index.php?title=Mir&action=edit&redlink=1) | [Mutter](https://wiki.gentoo.org/wiki/Mutter) | [Niri](https://wiki.gentoo.org/wiki/Niri) | [Sway](https://wiki.gentoo.org/wiki/Sway) | [Weston](https://wiki.gentoo.org/wiki/Weston) | 
|---|---|---|---|---|---|---|---|
| [Presentation time](https://wayland.app/protocols/presentation-time) | 2 | 2 | ✗ | 2 | 2 | 2 | 1 | 
| [Viewporter](https://wayland.app/protocols/viewporter) | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 
| [XDG shell](https://wayland.app/protocols/xdg-shell) | 7 | 6 | 5 | 7 | 6 | 5 | 5 | 

#### Wayland extensions marked 'staging'

| Extension | Latest version | [KWin](https://wiki.gentoo.org/wiki/KWin) | [Mir](https://wiki.gentoo.org/index.php?title=Mir&action=edit&redlink=1) | [Mutter](https://wiki.gentoo.org/wiki/Mutter) | [Niri](https://wiki.gentoo.org/wiki/Niri) | [Sway](https://wiki.gentoo.org/wiki/Sway) | [Weston](https://wiki.gentoo.org/wiki/Weston) | 
|---|---|---|---|---|---|---|---|
| [Idle notify](https://wayland.app/protocols/ext-idle-notify-v1) | 2 | 2 | ✗ | ✗ | 2 | 2 | ✗ | 
| [Session lock](https://wayland.app/protocols/ext-session-lock-v1) | 1 | ✗ | 1 | ✗ | 1 | 1 | ✗ | 

#### Wayland extensions marked 'unstable'

| Extension | Latest version | [KWin](https://wiki.gentoo.org/wiki/KWin) | [Mir](https://wiki.gentoo.org/index.php?title=Mir&action=edit&redlink=1) | [Mutter](https://wiki.gentoo.org/wiki/Mutter) | [Niri](https://wiki.gentoo.org/wiki/Niri) | [Sway](https://wiki.gentoo.org/wiki/Sway) | [Weston](https://wiki.gentoo.org/wiki/Weston) | 
|---|---|---|---|---|---|---|---|
| [XDG decoration](https://wayland.app/protocols/xdg-decoration-unstable-v1) | 2 | 1 | 1 | ✗ | 1 | 1 | ✗ | 
| [Idle inhibit](https://wayland.app/protocols/idle-inhibit-unstable-v1) | 1 | 1 | 1 | 1 | 1 | 1 | ✗ | 
| [Primary selection](https://wayland.app/protocols/primary-selection-unstable-v1) | 1 | 1 | 1 | 1 | 1 | 1 | ✗ | 

## Display managers

A [display manager](https://wiki.gentoo.org/wiki/Display_manager) (DM), sometimes known as **login manager**, presents the user with a graphical login screen to start a GUI session.

For the full article and available DMs, see [display manager](https://wiki.gentoo.org/wiki/Display_manager).

## Application launchers

| Name | Package | Description | 
|---|---|---|
| [bemenu](https://github.com/Cloudef/bemenu) | [dev-libs/bemenu](https://packages.gentoo.org/packages/dev-libs/bemenu) | dmenu clone | 
| [Fuzzel](https://codeberg.org/dnkl/fuzzel) | [gui-apps/fuzzel::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/fuzzel) | Application launcher similar to rofi's 'drun' mode (in GURU overlay) | 
| [j4-dmenu-desktop](https://github.com/enkore/j4-dmenu-desktop) | [x11-misc/j4-dmenu-desktop](https://packages.gentoo.org/packages/x11-misc/j4-dmenu-desktop) | i3-desktop-menu replacement | 
| [lavaLauncher](https://github.com/patchedsoul/lavalauncher) | [gui-apps/lavalauncher](https://packages.gentoo.org/packages/gui-apps/lavalauncher) | Simple, static, launcher | 
| [nwg-launchers](https://github.com/nwg-piotr/nwg-launchers) | [gui-apps/nwg-launchers::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/nwg-launchers) | GTK-based static-bar + logout + grid (in GURU overlay) | 
| rofi | [x11-misc/rofi](https://packages.gentoo.org/packages/x11-misc/rofi) | A window switcher, run dialog and dmenu replacement | 
| [wmenu](https://codeberg.org/adnano/wmenu/) | [gui-apps/wmenu](https://packages.gentoo.org/packages/gui-apps/wmenu) | dynamic menu for wlroots compositors, maintains the look and feel of dmenu | 
| [Wofi](https://wiki.gentoo.org/wiki/Wofi) | [gui-apps/wofi](https://packages.gentoo.org/packages/gui-apps/wofi) | rofi clone | 

## Clipboard managers

| Name | Package | Description | 
|---|---|---|
| [cliphist](https://github.com/sentriz/cliphist) | [app-misc/cliphist::guru](https://github.com/gentoo-mirror/guru/tree/master/app-misc/cliphist) | Wayland clipboard manager with support for multimedia | 
| [clipman](https://github.com/chmouel/clipman/) | [gui-apps/clipman::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/clipman) | Simple clipboard manager for Wayland | 
| [wl-clipboard](https://github.com/bugaevc/wl-clipboard) | [gui-apps/wl-clipboard](https://packages.gentoo.org/packages/gui-apps/wl-clipboard) | Simple command-line programs, [wl-copy(1)](https://man.archlinux.org/man/wl-copy.1.en) [and](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [wl-paste(1)](https://man.archlinux.org/man/wl-paste.1.en)  | 

## Desktop notifications

| Name | Package | Description | 
|---|---|---|
| [Dunst](https://wiki.gentoo.org/wiki/Dunst) | [x11-misc/dunst](https://packages.gentoo.org/packages/x11-misc/dunst) | Customizable and lightweight notification-daemon | 
| [fnott](https://codeberg.org/dnkl/fnott) | [gui-apps/fnott::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/fnott) | Keyboard driven and lightweight Wayland notification daemon for wlroots-based compositors | 
| [Mako](https://wiki.gentoo.org/wiki/Mako) | [gui-apps/mako](https://packages.gentoo.org/packages/gui-apps/mako) | Lightweight Wayland notification daemon | 
| [Notification Daemon](https://gitlab.gnome.org/Archive/notification-daemon) | [x11-misc/notification-daemon](https://packages.gentoo.org/packages/x11-misc/notification-daemon) | \[Archived\] Notification daemon from GNOME project | 

## Status bars

| Name | Package | Description | 
|---|---|---|
| [i3status-rust](https://github.com/greshake/i3status-rust) | [x11-misc/i3status-rust::guru](https://github.com/gentoo-mirror/guru/tree/master/x11-misc/i3status-rust) | Very resource friendly and feature-rich replacement for i3status (in GURU overlay) | 
| [SFWBar](https://github.com/LBCrion/sfwbar) | [gui-apps/sfwbar::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/sfwbar) | Sway Floating Window Bar (in GURU overlay) | 
| [Swayrbar](https://sr.ht/~tsdh/swayr/#swayrbar) | [gui-apps/swayrbar::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/swayrbar) | Implementation of swaybar-protocol for sway/swaybar | 
| [Waybar](https://wiki.gentoo.org/wiki/Waybar) | [gui-apps/waybar](https://packages.gentoo.org/packages/gui-apps/waybar) | Highly customizable Wayland bar for Sway and wlroots-based compositors | 
| [Yambar](https://codeberg.org/dnkl/yambar) | [gui-apps/yambar::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/yambar) | \[Archived\] Modular status panel for X11 and Wayland, inspired by Polybar (in GURU overlay) | 

## Terminal emulators

| Name | Package | Description | 
|---|---|---|
| [Alacritty](https://wiki.gentoo.org/wiki/Alacritty) | [x11-terms/alacritty](https://packages.gentoo.org/packages/x11-terms/alacritty) | GPU-accelerated terminal emulator | 
| [Kitty](https://wiki.gentoo.org/wiki/Kitty) | [x11-terms/kitty](https://packages.gentoo.org/packages/x11-terms/kitty) | A modern, hackable, featureful, OpenGL-based terminal emulator | 
| [Mlterm](https://github.com/arakiken/mlterm) | [x11-terms/mlterm](https://packages.gentoo.org/packages/x11-terms/mlterm) | A multi-lingual terminal emulator | 
| [foot](https://wiki.gentoo.org/wiki/Foot) | [gui-apps/foot](https://packages.gentoo.org/packages/gui-apps/foot) | A fast and lightweight terminal emulator for Wayland | 
| [ghostty](https://wiki.gentoo.org/wiki/Ghostty) | [x11-terms/ghostty](https://packages.gentoo.org/packages/x11-terms/ghostty) | A fast, feature-rich, and cross-platform terminal emulator with platform-native UI and GPU acceleration | 

## Wallpaper managers

| Name | Package | Description | 
|---|---|---|
| [Azote](https://github.com/nwg-piotr/azote) | [gui-apps/azote::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/azote) | Wallpaper and color manager for X and wlroots-based compositors (in GURU overlay) | 
| [MPVPaper](https://github.com/GhostNaN/mpvpaper) | [gui-apps/mpvpaper::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/mpvpaper) | Video wallpaper program for wlroots-based compositors (in GURU overlay) | 
| [Oguri](https://github.com/vilhalmer/oguri) | [gui-apps/oguri::wayland-desktop](https://gpo.zugaina.org/Overlays/wayland-desktop/gui-apps/oguri) | \[Archived\] Wallpaper daemon supporting animated wallpapers for Wayland compositors (in wayland-desktop overlay) | 
| [awww](https://wiki.gentoo.org/wiki/Awww) | [gui-apps/awww::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/awww) | Formerly known as swww. A direct replacement of Oguri. Supports animated gifs and transition effects. | 
| [SwayBG](https://github.com/swaywm/swaybg) | [gui-apps/swaybg](https://packages.gentoo.org/packages/gui-apps/swaybg) | Wallpaper utility for all Wayland compositors | 
| [Hyprpaper](https://github.com/hyprwm/hyprpaper) | [gui-apps/hyprpaper::hyproverlay](https://repos.gentoo.org/#hyproverlay) | Fast wallpaper utility with support for all wlroots-based compositors (in GURU overlay) | 

## Screenlock and idle management

| Name | Package | Description | 
|---|---|---|
| [Swayidle](https://github.com/swaywm/swayidle) | [gui-apps/swayidle](https://packages.gentoo.org/packages/gui-apps/swayidle) | Idle management daemon for all Wayland compositors | 
| [Swaylock](https://github.com/swaywm/swaylock) | [gui-apps/swaylock](https://packages.gentoo.org/packages/gui-apps/swaylock) | Screen locker for all Wayland compositors | 
| [Swaylock-effects](https://github.com/mortie/swaylock-effects) | [gui-apps/swaylock-effects::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/swaylock-effects) | Swaylock fork with fancy effects (in GURU overlay) | 
| [Hypridle](https://github.com/hyprwm/hypridle) | [gui-apps/hypridle::hyproverlay](https://repos.gentoo.org/#hyproverlay) | Hyprland's idle daemon | 

## Power management

| Name | Package | Description | 
|---|---|---|
| [wlopm](https://www.mankier.com/1/wlopm) | [gui-apps/wlopm::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/wlopm) | Wayland output power management | 

## Multiple display configuration

| Name | Package | Description | 
|---|---|---|
| [Kanshi](https://gitlab.freedesktop.org/emersion/kanshi) | [gui-apps/kanshi](https://packages.gentoo.org/packages/gui-apps/kanshi) | Dynamic display configuration, like autorandr, for all Wayland compositors | 
| [wdisplays](https://github.com/artizirk/wdisplays) | [gui-apps/wdisplays::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/wdisplays) | GUI-based display configuration for all Wayland compositors (in GURU overlay) | 
| [wl-mirror](https://github.com/Ferdi265/wl-mirror) | [gui-apps/wl-mirror::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/wl-mirror) | Mirror an output onto a client surface (in GURU overlay) | 
| [wlr-randr](https://github.com/emersion/wlr-randr) | [gui-apps/wlr-randr::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/wlr-randr) | \[Archived\] Xrandr clone for all Wayland compositors (in GURU overlay) | 

## Screenshots and recording

| Name | Package | Description | 
|---|---|---|
| [Grim](https://gitlab.freedesktop.org/emersion/grim) | [gui-apps/grim](https://packages.gentoo.org/packages/gui-apps/grim) | Screen image grabber for all Wayland compositors | 
| [Slurp](https://github.com/emersion/slurp) | [gui-apps/slurp](https://packages.gentoo.org/packages/gui-apps/slurp) | Screen region selector for all Wayland compositors | 
| [Swappy](https://github.com/jtheoof/swappy) | [gui-apps/swappy](https://packages.gentoo.org/packages/gui-apps/swappy) | Screenshotting and editing tool for all Wayland compositors, inspired by OS X snappy | 
| [wf-recorder](https://github.com/ammen99/wf-recorder) | [gui-apps/wf-recorder](https://packages.gentoo.org/packages/gui-apps/wf-recorder) | Screen recorder for all Wayland compositors | 

## Remote access

| Name | Package | Description | 
|---|---|---|
| [Wayvnc](https://github.com/any1/wayvnc) | [gui-apps/wayvnc](https://packages.gentoo.org/packages/gui-apps/wayvnc) | \[Beta\] VNC server for wlroots-based compositors | 
| [Waypipe](https://wiki.gentoo.org/wiki/Waypipe) | [gui-apps/waypipe](https://packages.gentoo.org/packages/gui-apps/waypipe) | \[Unstable\] Transparent proxy for all Wayland compositors | 

## Miscellaneous

| Name | Package | Description | 
|---|---|---|
| [imv](https://wiki.gentoo.org/wiki/Imv) | [media-gfx/imv](https://packages.gentoo.org/packages/media-gfx/imv) | Image viewer for Wayland and X | 
| [lswt](https://git.sr.ht/~leon_plickat/lswt/) | [gui-apps/lswt::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/lswt) | List Wayland toplevels/windows | 
| [Waycheck](https://gitlab.freedesktop.org/serebit/waycheck) | [gui-apps/waycheck::parona-overlay](https://repos.gentoo.org/#parona-overlay) | Qt6 app to display Wayland protocols supported/unsupported by the running compositor | 
| [wev(1)](https://man.archlinux.org/man/wev.1.en) | [gui-apps/wev::guru](https://github.com/gentoo-mirror/guru/tree/master/gui-apps/wev) | Show Wayland events | 

## See also

- [Recommended applications](https://wiki.gentoo.org/wiki/Recommended_applications) — applications recommended for use in a graphical environment ([X11](https://wiki.gentoo.org/wiki/Xorg), [Wayland](https://wiki.gentoo.org/wiki/Wayland))
- [Recommended tools](https://wiki.gentoo.org/wiki/Recommended_tools) — lists system-administration related tools recommended for use in a **[shell](https://wiki.gentoo.org/wiki/Shell) environment** ([terminal/console](https://wiki.gentoo.org/wiki/Terminal_emulator))
- [Qt Desktop applications](https://wiki.gentoo.org/wiki/Qt_Desktop_applications) — a list of recommendations for a light-weight, non-[KDE](https://wiki.gentoo.org/wiki/KDE), [Qt](https://wiki.gentoo.org/wiki/Qt)-only desktop environment.

## External resources

- [awesome-wayland, a curated list of Wayland resources](https://github.com/rcalixte/awesome-wayland).
- [GitHub repository for the wayland-desktop overlay](https://github.com/bsd-ac/wayland-desktop).
- [Arch Linux wiki article](https://wiki.archlinux.org/index.php/Wayland).

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) "Hyprland is no longer a wlroots-based Wayland compositor, and instead, a fully independent implementation of the protocol." ["Hyprland is now fully independent!"](https://hyprland.org/news/independentHyprland/). Retrieved on 2025-02-06.
2. [↑](https://wiki.gentoo.org#cite_ref-2) [https://github.com/jbuchermn/newm#current-state](https://github.com/jbuchermn/newm#current-state)
3. [↑](https://wiki.gentoo.org#cite_ref-3) [https://sr.ht/\~atha/newm-atha/](https://sr.ht/~atha/newm-atha/)
