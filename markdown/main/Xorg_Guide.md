<!-- source: https://wiki.gentoo.org/wiki/Xorg/Guide | group: Gentoo Wiki (Main) | wiki-title: Xorg/Guide -->
---
title: Xorg/Guide
url: https://wiki.gentoo.org/wiki/Xorg/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-29"
fingerprint: c7d0dc7e9897b99d
license: CC BY-SA 4.0
---

# Xorg/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Xorg is the [X Window server](https://wiki.gentoo.org/wiki/X_server) which allows users to have a graphical environment at their fingertips. This guide explains what Xorg is, how to install it, and the various configuration options.

## What is the X Window server?

### Graphical vs command-line

An average user may be frightened at the thought of having to type in commands at a command-line interface (CLI). Why wouldn't they be able to point-and-click their way through the freedom provided by Gentoo (and Linux in general)? Well, of course they can!

Gentoo offers a wide variety of flashy graphical interfaces such as [window managers](https://wiki.gentoo.org/wiki/Window_manager) and [desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment) which can be installed on top of an existing installation.

One of the biggest surprises users who are new to Linux come across: graphical user interfaces are nothing more than an application (or in some cases a suite of applications) which are run on a system. It is *not* part of the Linux kernel or any other internals of the system. That said, GUIs are powerful tools that unlock the graphical abilities of a workstation.

As standards are important, a standard for drawing and moving windows on a screen, interacting with the user through mouse, keyboard, and other basic, yet important aspects has been created and named the *X Window System*, commonly abbreviated as *X11* or just *X*. It is used on Unix, Linux, and Unix-like operating systems throughout the world.

The application that provides Linux users with the ability to run graphical user interfaces and that uses the X11 standard is Xorg-X11, a fork of the XFree86 project. XFree86 has decided to use a license that might not be compatible with the GPL license; the use of Xorg is therefore recommended. XFree86 packages are no longer provided through the Gentoo repository.

### The X.org project

The [X.org](http://www.x.org) project created and maintains a freely redistributable, open-source implementation of the X11 system. It is an open source X11-based desktop infrastructure.

Xorg provides an interface between hardware and the graphical software. Besides that, Xorg is also fully network-aware, allowing to run an application on one system while viewing it on a different one.

## Installation

Before installing Xorg, prepare the system for it. First, set up the kernel to support input devices and video cards. Then, prepare [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) so that the right drivers and Xorg packages are built and installed.

### Input driver support

Support for Event interface needs to be activated by making a change to the kernel configuration. Read the [Kernel Configuration Guide](https://wiki.gentoo.org/wiki/Kernel/Gentoo_Kernel_Configuration_Guide) for information on how to setup the kernel.

**Enable Event interface in the kernel (`CONFIG_INPUT_EVDEV`)**

### Kernel modesetting

Modern open source video drivers rely on kernel mode setting (KMS). KMS provides an improved graphical boot with less flickering, faster user switching, a built-in framebuffer console, seamless switching from the console to Xorg, and other features.

#### Verify legacy framebuffer drivers have been disabled

First prepare the kernel for KMS. This step regardless of which Xorg video driver will be used:

**Disable legacy framebuffer support and enable basic console FB support**

Next configure the kernel to use the proper KMS driver for the video card. Intel, NVIDIA, and AMD/ATI are the most common cards, so follow code listing for each card below.

#### Intel

For Intel cards see the [kernel section of the Intel article](https://wiki.gentoo.org/wiki/Intel#Kernel).

#### NVIDIA

For NVIDIA cards, two driver options are available. For a full open source system, an open source driver entitled [Nouveau](https://wiki.gentoo.org/wiki/Nouveau) is suggested. The second option is the closed source [NVIDIA driver](https://wiki.gentoo.org/wiki/NVIDIA), which is officially supported by NVIDIA. This article recommends the Nouveau driver, however be aware not all functionality for certain cards may be supported using the open source driver.

In addition to the kernel driver, certain cards require closed source firmware to be built-in to the Linux kernel. Depending on the selected driver, readers should visit each respective article to check to see if firmware (from the [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) is necessary for their specific card.

**Open source NVIDIA kernel support (`CONFIG_DRM_NOUVEAU`)**

#### AMD/ATI

For newer AMD/ATI cards ([RadeonHD 2000 and up](https://wiki.gentoo.org/wiki/ATI_FAQ)), emerge [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) (the package includes firmware for radeon and amdgpu drivers). Once one of these packages has been installed, make the Radeon driver a module in the kernel or, optionally, configure the kernel as detailed in the [firmware section](https://wiki.gentoo.org/wiki/Radeon#Firmware) of the Radeon article or, for newer AMD graphics cards (GCN1.1+), the [firmware section](https://wiki.gentoo.org/wiki/AMDGPU#Firmware) of the AMDGPU article.

Older cards:

**AMD/ATI Radeon settings**

Newer cards:

**AMDGPU settings**

Save any changes to the kernel configuration, [rebuild the kernel](https://wiki.gentoo.org/wiki/Kernel/Rebuild), and reboot.

### USE flags

Before installing Xorg, some adjustments might be needed to the configuration in [/etc/portage](https://wiki.gentoo.org/wiki//etc/portage).

Portage knows the [X](https://packages.gentoo.org/useflags/X) [USE flag](https://wiki.gentoo.org/wiki/USE_flag) for enabling support for X in other packages (default in all *desktop* [profiles](<https://wiki.gentoo.org/wiki/Profile_(Portage)>)). If using a non-desktop profile, make sure the list of USE flags in [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf)   contains `X` to enable optional X compatibility system wide:

**`/etc/portage/make.conf`**

#### USE\_EXPAND

There are two [USE\_EXPAND](https://wiki.gentoo.org/wiki/USE_EXPAND) variables that are particularly important to set before installing Xorg.

`[VIDEO_CARDS](https://wiki.gentoo.org/wiki/VIDEO_CARDS)` is used to enable support for various video cards. Most users will want to set this variable based on their active hardware configuration. Common values include `nouveau` for NVIDIA cards, `amdgpu radeonsi` for [AMD](https://wiki.gentoo.org/wiki/AMDGPU) cards, and `intel` for [Intel](https://wiki.gentoo.org/wiki/Intel#X_drivers) systems. These enable the well-supported, actively developed open-source drivers for their respective devices. To set `VIDEO_CARDS`, create a file like the one below in [/etc/portage/package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use). If /etc/portage/package.use does not exist, create it.

**`/etc/portage/package.use/00video_cards`**

**VIDEO\_CARDS examples**

```
## For a system with an Intel CPU and AMD GPU
*/* VIDEO_CARDS: -* amdgpu radeonsi intel
```
`[INPUT_DEVICES](https://wiki.gentoo.org/wiki//etc/portage/make.conf#INPUT_DEVICES)` is used to determine which drivers should be built to support various input devices.

make.defaults has [Libinput](https://wiki.gentoo.org/wiki/Libinput) as the default input device driver.

To check what is presently set, run:

`user $``portageq envvar INPUT_DEVICES`
The default value of `libinput` is sufficient in many cases. However, there are some input devices, such as Synaptics touchpads on laptops, that need other drivers. If this is the case, `INPUT_DEVICES` can be configured by creating a file like the one below in the [/etc/portage/package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use) directory.

**`/etc/portage/package.use/00input`**

**INPUT\_DEVICES example**

```
## (For generic mouse, generic keyboard, Synaptics touchpad, and Wacom drawing tablet support)
*/* INPUT_DEVICES: libinput synaptics wacom
```
If the suggested settings do not work, emerge the [x11-base/xorg-drivers](https://packages.gentoo.org/packages/x11-base/xorg-drivers) package (see the step below). Check all the options available and choose those which apply to the system. This example is for a system with a keyboard, mouse, Synaptics touchpad, and an AMD Radeon video card.

`root #``emerge --pretend --verbose x11-base/xorg-drivers`
These are the packages that would be merged, in order:
 
Calculating dependencies... done!
\[ebuild   R   \] x11-base/xorg-drivers-1.20-r1::gentoo  INPUT\_DEVICES="libinput synaptics -elographics -evdev -joystick -keyboard -mouse -vmmouse -void -wacom" VIDEO\_CARDS="amdgpu radeonsi -ast -dummy -fbdev (-freedreno) (-geode) -glint -i915 -i965 -intel -mga -nouveau -nv -nvidia (-omap) -qxl -r128 -radeon -siliconmotion (-tegra) (-vc4) -vesa -via -virtualbox -vmware" 0 KiB

The USE flags have the following meaning:


| [+elogind](https://packages.gentoo.org/useflags/+elogind) | Use elogind to get control over framebuffer when running as regular user | 
| [+udev](https://packages.gentoo.org/useflags/+udev) | Enable virtual/udev integration (device discovery, power and storage device support, etc) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [minimal](https://packages.gentoo.org/useflags/minimal) | Install a very minimal build (disables, for example, plugins, fonts, most drivers, non-critical features) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [suid](https://packages.gentoo.org/useflags/suid) | Enable setuid root program(s) | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [unwind](https://packages.gentoo.org/useflags/unwind) | Enable libunwind usage for backtraces | 
| [xcsecurity](https://packages.gentoo.org/useflags/xcsecurity) | Build Security extension | 
| [xephyr](https://packages.gentoo.org/useflags/xephyr) | Build the Xephyr server | 
| [xnest](https://packages.gentoo.org/useflags/xnest) | Build the Xnest server | 
| [xorg](https://packages.gentoo.org/useflags/xorg) | Build the Xorg X server (HIGHLY RECOMMENDED) | 
| [xvfb](https://packages.gentoo.org/useflags/xvfb) | Build the Xvfb server | 

### Locale

[x11-libs/libxcb](https://packages.gentoo.org/packages/x11-libs/libxcb) fails to emerge without a [UTF-8](https://wiki.gentoo.org/wiki/UTF-8) locale ([bug #913655](https://bugs.gentoo.org/show_bug.cgi?id=913655)). Use eselect locale to list available locales and set a locale with UTF-8 encoding:

`user $``eselect locale list`
Available targets for the LANG variable:
  \[1\]   C
  \[2\]   C.utf8
  \[3\]   en\_US
  \[4\]   en\_US.iso88591 \*
  \[5\]   en\_US.utf8
  \[6\]   POSIX
  \[ \]   (free form)

`root #``eselect locale set 5`
### Emerge

After setting all the necessary variables and USE flags Xorg can be installed:

`root #``emerge --ask x11-base/xorg-server`
When the installation is finished, some environment variables will need to re-initialized before continuing. Source the profile with this command:

`root #````
env-update
```
`root #````
source /etc/profile
```
## Configuration

The [X server](https://wiki.gentoo.org/wiki/X_server) is designed to work out-of-the-box, with no need to manually edit Xorg's configuration files. It *should* detect and configure devices such as displays, keyboards, and mice.

Try [using startx](https://wiki.gentoo.org/wiki/Xorg/Guide#Using_startx) without editing any configuration files. If Xorg will not start, or there is some other problem, then manual configuration of Xorg might be needed. This is explained in the following section.

To run Xorg with non-root users, as root, either enable a logind provider (see [Non root Xorg](https://wiki.gentoo.org/wiki/Non_root_Xorg)) or set the `suid` USE flag (see above note).

### The xorg.conf.d directory

Most of the configuration files for Xorg are stored in [/etc/X11/xorg.conf.d/](https://wiki.gentoo.org/wiki/Xorg.conf#xorg.conf.d.2C_xorg.conf). If that directory does not exist, then create it. Each file is given a unique name and ends in .conf. The file names in Xorg's configuration directory will be read in alpha numeric order. For example, 10-evdev.conf will be read before 20-synaptics.conf; a-evdev.conf will be read before b-synaptics.conf, and so on. The files in this directory are not required to be numbered, but doing so will help to keep them organized. Organization is helpful when debugging faulty configuration files.

Try startx to start up the [X server](https://wiki.gentoo.org/wiki/X_server). startx is a script (it's installed by [x11-apps/xinit](https://packages.gentoo.org/packages/x11-apps/xinit)) that executes an *X session*; that is, it starts the X server and some graphical applications on top of it. It decides which applications to run using the following logic:

- If a file named .xinitrc exists in the home directory, it will execute the commands listed there.

- Otherwise, it will read the value of the `XSESSION` variable from the /etc/env.d/90xsession file and execute the relevant session accordingly. Values for `XSESSION` are available in /etc/X11/Sessions/. To set a system wide default session run:

- `root #``echo XSESSION="Xfce4" > /etc/env.d/90xsession`

- This will create the 90xsession file and set the default X session to [Xfce](https://wiki.gentoo.org/wiki/Xfce/Guide). Remember to run env-update after making changes to 90xsession.

`user $``startx`
If no window manager has been installed a solid black screen will appear. Since this can also be a sign that something is wrong, the [x11-wm/twm](https://packages.gentoo.org/packages/x11-wm/twm) and [x11-terms/xterm](https://packages.gentoo.org/packages/x11-terms/xterm) packages can be installed only to test X.

Once the programs are installed, run startx again. A few xterm windows should appear, making it easy to verify the X server is working correctly. Once satisfied with the results, depclean [x11-wm/twm](https://packages.gentoo.org/packages/x11-wm/twm) and [x11-terms/xterm](https://packages.gentoo.org/packages/x11-terms/xterm) if installed in the step above to remove the testing packages. They will not be needed to setup a proper desktop environment.

The session (program to start) could also be given as an argument to startx:

`user $``startx /usr/bin/startfluxbox`
In addition, to pass X11 server options, by preceding them with a double dash:

`user $``startx -- vt7`
### Tweaking X settings

#### Setting the screen resolution

If the screen resolution looks to be wrong, check two sections in [xorg.conf.d](https://wiki.gentoo.org/wiki/Xorg.conf#xorg.conf.d.2C_xorg.conf) configuration. First of all, the *Screen* section lists the resolutions that the X server will run at. This section might not list any resolutions at all. If this is the case, Xorg will estimate the resolutions based on the information in the second section, *Monitor*.

Now change the resolution. In the next example from /etc/X11/xorg.conf.d/40-monitor.conf we add the `PreferredMode` line so that the X server starts at 1440x900 by default. The `Option` in the `Device` section must match the name of the monitor (`DVI-0`), which can be obtained by running xrandr. Install xrandr (emerge xrandr) just long enough to get this information. The argument after the monitor name (in the `Device` section) must match the `Identifier` in the `Monitor` section.

**`/etc/X11/xorg.conf.d/40-monitor.conf`**

```
Section "Device"
  Identifier  "RadeonHD 4550"
  Option      "Monitor-DVI-0" "DVI screen"
EndSection
Section "Monitor"
  Identifier  "DVI screen"
  Option      "PreferredMode" "1440x900"
EndSection
```
Run X (startx) to discover it uses the desired resolution.

#### Multiple monitors

More than one monitor in can be established in [/etc/X11/xorg.conf.d/](https://wiki.gentoo.org/wiki/Xorg.conf#xorg.conf.d.2C_xorg.conf). Give each monitor a unique identifier, then list its physical position, such as "RightOf" or "Above" another monitor. The following example shows how to configure a DVI and a VGA monitor, with the VGA monitor as the right-hand screen:

**`/etc/X11/xorg.conf.d/40-monitor.conf`**

```
Section "Device"
  Identifier "RadeonHD 4550"
  Option     "Monitor-DVI-0" "DVI screen"
  Option     "Monitor-VGA-0" "VGA screen"
EndSection
Section "Monitor"
  Identifier "DVI screen"
EndSection
Section "Monitor"
  Identifier "VGA screen"
  Option     "RightOf" "DVI screen"
EndSection
```
#### Configuring the keyboard

For methods of switching the keyboard layout see the [Keyboard layout switching](https://wiki.gentoo.org/wiki/Keyboard_layout_switching#X11) article.

To setup X to use an international keyboard create the appropriate config file in [/etc/X11/xorg.conf.d/](https://wiki.gentoo.org/wiki/Xorg.conf#xorg.conf.d.2C_xorg.conf). This example features a Czech keyboard layout:

**`/etc/X11/xorg.conf.d/30-keyboard.conf`**

```
Section "InputClass"
  Identifier "keyboard-all"
  Driver "evdev"
  MatchProduct "AT Translated Set 2 keyboard"    # apply to devices having this as a substring
  MatchIsKeyboard "true"                         # apply to "keyboard" devices only
  Option "XkbLayout" "us,cz"
  Option "XkbModel" "logitech_g15"
  Option "XkbRules" "xorg"
  Option "XkbOptions" "grp:alt_shift_toggle,grp:switch,grp_led:scroll,compose:rwin,terminate:ctrl_alt_bksp"
  Option "XkbVariant" ",qwerty"
  MatchIsKeyboard "on"
EndSection
```
The "terminate" command (`terminate:ctrl_alt_bksp`) lets users kill the X session by using the `Ctrl`+`Alt`+`Backspace` key combination. This will, however, make X exit disgracefully -- something that users might want to avoid. It can be useful when programs have frozen the display entirely, or when configuring and tweaking the Xorg environment. Be careful when killing the desktop with this key combination - most programs really do not like it when they are ended this way. Some, if not all, of the information that has not been written to the disk (information stored in "open documents") will be lost.

Because the "evdev" driver can handle multiple devices (even non-keyboards), limiting the section to only some devices might be needed for proper working of all the devices. Use the `MatchProduct` directive to specify the device name, consult man xorg.conf for more info.

For more information about `XkbModel` and `XkbOptions`, consult /usr/share/X11/xkb/rules/base.lst and man xkeyboard-config.

#### Finishing up

Run startx and be happy about the result. There should now be a (hopefully) working Xorg! The next step is to install a useful window manager or desktop environment such as [KDE](https://wiki.gentoo.org/wiki/KDE), [GNOME](https://wiki.gentoo.org/wiki/GNOME), or [Xfce](https://wiki.gentoo.org/wiki/Xfce). Information on installing these desktop environments can be found here on the wiki.

## See also

- [Non root Xorg](https://wiki.gentoo.org/wiki/Non_root_Xorg) — describes how an unprivileged user can run [Xorg](https://wiki.gentoo.org/wiki/Xorg) without using suid.
- [Wayland](https://wiki.gentoo.org/wiki/Wayland) — a [communication protocol](https://en.wikipedia.org/wiki/communication_protocol) between a [display server](https://en.wikipedia.org/wiki/display_server) and its clients
- [X (Security Handbook)](https://wiki.gentoo.org/wiki/Security_Handbook/Securing_services#X) - The Security Handbook's entry on securing the X server.
- [Xorg](https://wiki.gentoo.org/wiki/Xorg) — an open source implementation of the [X server](https://wiki.gentoo.org/wiki/X_server).
- [Xorg/Guide] — explains what Xorg is, how to install it, and the various configuration options.
- [Xrandr](https://wiki.gentoo.org/wiki/Xrandr) — [X](https://wiki.gentoo.org/wiki/X) protocol extension and its CLI tool xrandr are used to manage screen resolutions, rotation and screens with multiply displays in X
- [X server](https://wiki.gentoo.org/wiki/X_server) — the main component of the X Window system which abstracts the hardware and provides the foundation for most graphical user interfaces, like [desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment) or [window managers](https://wiki.gentoo.org/wiki/Window_manager), and their applications.

## External resources

### Creating and editing config files

man xorg.conf and man evdev provide quick yet complete references about the syntax used by these configuration files. Be sure to have them open on a terminal when editing Xorg configuration files!

Example configurations can be found at /usr/share/doc/xorg-server-\*/xorg.conf.example.bz2.

There are also many online resources on editing config files in /etc/X11/. Only a few are listed here; use a favorite search engine to find more.

More information about installing and configuring various graphical desktop environments and applications can be found in the [Gentoo desktop resources](https://wiki.gentoo.org/wiki/Category:Desktop) section of our documentation.

When upgrading to xorg-server 1.9 or higher, be sure to read the [migration guide](https://wiki.gentoo.org/wiki/X_server/upgrade).

X.org provides many [FAQs](http://www.x.org/wiki/FAQ) on their website, in addition to their other documentation.
