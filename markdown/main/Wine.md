<!-- source: https://wiki.gentoo.org/wiki/Wine | group: Gentoo Wiki (Main) | wiki-title: Wine -->
---
title: Wine
url: https://wiki.gentoo.org/wiki/Wine
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-31"
fingerprint: "4e01991cc9863944"
license: CC BY-SA 4.0
---

# Wine

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Wine** (**W**ine **I**s **N**ot an **E**mulator) is an application compatibility layer that allows [Microsoft Windows](https://en.wikipedia.org/wiki/Microsoft_Windows) software to run on Linux and other [POSIX](https://en.wikipedia.org/wiki/POSIX)-compliant operating systems. This article deals with installing, configuring, and maintaining a general purpose **Wine** environment on Gentoo.

## Wine variants

- [app-emulation/wine-vanilla](https://packages.gentoo.org/packages/app-emulation/wine-vanilla): Upstream **Wine** with no external patchsets.
- [app-emulation/wine-staging](https://packages.gentoo.org/packages/app-emulation/wine-staging): **Wine** with the [Wine Staging](https://wiki.winehq.org/Wine-Staging) patchset.
- [app-emulation/wine-proton](https://packages.gentoo.org/packages/app-emulation/wine-proton): **Wine** with the [Valve Software](https://github.com/ValveSoftware/wine/) patchset.

### Choosing a variant

Additional patchsets may introduce bugs that don't exist in vanilla **Wine**, but they may also fix others that do.

The [app-emulation/wine-proton](https://packages.gentoo.org/packages/app-emulation/wine-proton) variant is only the **Wine** fork which is used to build **Proton**, and is not a stand-alone version of **Proton** which comes with [Steam](https://wiki.gentoo.org/wiki/Steam). If there are any issues with running applications with it, it would be best to test if the issues can be seen with a build of the upstream **Wine** as well, and then file a report to the upstream **Wine** project if none exists.

## Installation

### USE flags



| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [+alsa](https://packages.gentoo.org/useflags/+alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [+dbus](https://packages.gentoo.org/useflags/+dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [+fontconfig](https://packages.gentoo.org/useflags/+fontconfig) | Support for configuring and customizing font access via media-libs/fontconfig | 
| [+gecko](https://packages.gentoo.org/useflags/+gecko) | Enable mshtml support using app-emulation/wine-gecko | 
| [+gstreamer](https://packages.gentoo.org/useflags/+gstreamer) | Add support for media-libs/gstreamer (Streaming media) | 
| [+mingw](https://packages.gentoo.org/useflags/+mingw) | Build PE files using dev-util/mingw64-toolchain (more tested) rather than llvm-core/clang (newer) | 
| [+mono](https://packages.gentoo.org/useflags/+mono) | Enable .NET support using app-emulation/wine-mono | 
| [+opengl](https://packages.gentoo.org/useflags/+opengl) | Add support for OpenGL (3D graphics) | 
| [+sdl](https://packages.gentoo.org/useflags/+sdl) | Enable gamepad support using media-libs/libsdl2 | 
| [+ssl](https://packages.gentoo.org/useflags/+ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [+strip](https://packages.gentoo.org/useflags/+strip) | Allow symbol stripping to be performed by the ebuild for special files | 
| [+truetype](https://packages.gentoo.org/useflags/+truetype) | Add support for FreeType and/or FreeType2 fonts | 
| [+unwind](https://packages.gentoo.org/useflags/+unwind) | Add support for call stack unwinding and function name resolution | 
| [+vulkan](https://packages.gentoo.org/useflags/+vulkan) | Add support for 3D graphics and computing via the Vulkan cross-platform API | 
| [+wow64](https://packages.gentoo.org/useflags/+wow64) | Enable the "new" wow64 mode that allows running 32bit applications without 32bit ELF multilib by mapping to 64bit calls. Generally works well with wine-11.0 or newer, but may or may not have a small perforamce impact or other issues. If USE=mingw is set, still need the default abi\_x86\_32 set on dev-util/mingw64-toolchain but that can be done even with /no-multilib/ profiles. This can also be enabled on arm64 to allow running x86 applications via binary translation. | 
| [+xcomposite](https://packages.gentoo.org/useflags/+xcomposite) | Enable support for the Xorg composite extension | 
| [arm64ec](https://packages.gentoo.org/useflags/arm64ec) | Enable support for running x86\_64 applications on arm64 via binary translation | 
| [bluetooth](https://packages.gentoo.org/useflags/bluetooth) | Enable Bluetooth Support | 
| [crossdev-mingw](https://packages.gentoo.org/useflags/crossdev-mingw) | Use sys-devel/crossdev for the toolchain rather than dev-util/mingw64-toolchain (requires manual setting up and is mostly unsupported, try disabling if have issues) | 
| [cups](https://packages.gentoo.org/useflags/cups) | Add support for CUPS (Common Unix Printing System) | 
| [custom-cflags](https://packages.gentoo.org/useflags/custom-cflags) | Build with user-specified CFLAGS (unsupported) | 
| [dos](https://packages.gentoo.org/useflags/dos) | Pull in games-emulation/dosbox to run DOS applications | 
| [ffmpeg](https://packages.gentoo.org/useflags/ffmpeg) | Enable ffmpeg/libav-based audio/video codec support | 
| [gphoto2](https://packages.gentoo.org/useflags/gphoto2) | Add digital camera support | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [llvm-libunwind](https://packages.gentoo.org/useflags/llvm-libunwind) | Use llvm-runtimes/libunwind instead of sys-libs/libunwind | 
| [netapi](https://packages.gentoo.org/useflags/netapi) | Enable support for configuring remote shares using net-fs/samba | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [odbc](https://packages.gentoo.org/useflags/odbc) | Add ODBC Support (Open DataBase Connectivity) | 
| [opencl](https://packages.gentoo.org/useflags/opencl) | Enable OpenCL support (computation on GPU) | 
| [pcap](https://packages.gentoo.org/useflags/pcap) | Support packet capture software (e.g. wireshark) | 
| [perl](https://packages.gentoo.org/useflags/perl) | Install helpers that require perl (winedump/winemaker) | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [samba](https://packages.gentoo.org/useflags/samba) | Pull in net-fs/samba with winbind for NTLM auth support | 
| [scanner](https://packages.gentoo.org/useflags/scanner) | Add support for scanner hardware (e.g. build the sane frontend in kdegraphics) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [smartcard](https://packages.gentoo.org/useflags/smartcard) | Enable smartcard support | 
| [udev](https://packages.gentoo.org/useflags/udev) | Enable virtual/udev integration (device discovery, power and storage device support, etc) | 
| [usb](https://packages.gentoo.org/useflags/usb) | Add USB support to applications that have optional USB support (e.g. cups) | 
| [v4l](https://packages.gentoo.org/useflags/v4l) | Enable support for video4linux (using linux-headers or userspace libv4l libraries) | 
| [vaapi](https://packages.gentoo.org/useflags/vaapi) | Enable Video Acceleration API for hardware decoding | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 
| [xinerama](https://packages.gentoo.org/useflags/xinerama) | Add support for querying multi-monitor screen geometry through the Xinerama API | 


| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [+alsa](https://packages.gentoo.org/useflags/+alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [+dbus](https://packages.gentoo.org/useflags/+dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [+fontconfig](https://packages.gentoo.org/useflags/+fontconfig) | Support for configuring and customizing font access via media-libs/fontconfig | 
| [+gecko](https://packages.gentoo.org/useflags/+gecko) | Enable mshtml support using app-emulation/wine-gecko | 
| [+gstreamer](https://packages.gentoo.org/useflags/+gstreamer) | Add support for media-libs/gstreamer (Streaming media) | 
| [+mingw](https://packages.gentoo.org/useflags/+mingw) | Build PE files using dev-util/mingw64-toolchain (more tested) rather than llvm-core/clang (newer) | 
| [+mono](https://packages.gentoo.org/useflags/+mono) | Enable .NET support using app-emulation/wine-mono | 
| [+opengl](https://packages.gentoo.org/useflags/+opengl) | Add support for OpenGL (3D graphics) | 
| [+sdl](https://packages.gentoo.org/useflags/+sdl) | Enable gamepad support using media-libs/libsdl2 | 
| [+ssl](https://packages.gentoo.org/useflags/+ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [+strip](https://packages.gentoo.org/useflags/+strip) | Allow symbol stripping to be performed by the ebuild for special files | 
| [+truetype](https://packages.gentoo.org/useflags/+truetype) | Add support for FreeType and/or FreeType2 fonts | 
| [+unwind](https://packages.gentoo.org/useflags/+unwind) | Add support for call stack unwinding and function name resolution | 
| [+vulkan](https://packages.gentoo.org/useflags/+vulkan) | Add support for 3D graphics and computing via the Vulkan cross-platform API | 
| [+wow64](https://packages.gentoo.org/useflags/+wow64) | Enable the "new" wow64 mode that allows running 32bit applications without 32bit ELF multilib by mapping to 64bit calls. Generally works well with wine-11.0 or newer, but may or may not have a small perforamce impact or other issues. If USE=mingw is set, still need the default abi\_x86\_32 set on dev-util/mingw64-toolchain but that can be done even with /no-multilib/ profiles. This can also be enabled on arm64 to allow running x86 applications via binary translation. | 
| [arm64ec](https://packages.gentoo.org/useflags/arm64ec) | Enable support for running x86\_64 applications on arm64 via binary translation | 
| [bluetooth](https://packages.gentoo.org/useflags/bluetooth) | Enable Bluetooth Support | 
| [crossdev-mingw](https://packages.gentoo.org/useflags/crossdev-mingw) | Use sys-devel/crossdev for the toolchain rather than dev-util/mingw64-toolchain (requires manual setting up and is mostly unsupported, try disabling if have issues) | 
| [cups](https://packages.gentoo.org/useflags/cups) | Add support for CUPS (Common Unix Printing System) | 
| [custom-cflags](https://packages.gentoo.org/useflags/custom-cflags) | Build with user-specified CFLAGS (unsupported) | 
| [dos](https://packages.gentoo.org/useflags/dos) | Pull in games-emulation/dosbox to run DOS applications | 
| [ffmpeg](https://packages.gentoo.org/useflags/ffmpeg) | Enable ffmpeg/libav-based audio/video codec support | 
| [gphoto2](https://packages.gentoo.org/useflags/gphoto2) | Add digital camera support | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [llvm-libunwind](https://packages.gentoo.org/useflags/llvm-libunwind) | Use llvm-runtimes/libunwind instead of sys-libs/libunwind | 
| [netapi](https://packages.gentoo.org/useflags/netapi) | Enable support for configuring remote shares using net-fs/samba | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [odbc](https://packages.gentoo.org/useflags/odbc) | Add ODBC Support (Open DataBase Connectivity) | 
| [opencl](https://packages.gentoo.org/useflags/opencl) | Enable OpenCL support (computation on GPU) | 
| [pcap](https://packages.gentoo.org/useflags/pcap) | Support packet capture software (e.g. wireshark) | 
| [perl](https://packages.gentoo.org/useflags/perl) | Install helpers that require perl (winedump/winemaker) | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [samba](https://packages.gentoo.org/useflags/samba) | Pull in net-fs/samba with winbind for NTLM auth support | 
| [scanner](https://packages.gentoo.org/useflags/scanner) | Add support for scanner hardware (e.g. build the sane frontend in kdegraphics) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [smartcard](https://packages.gentoo.org/useflags/smartcard) | Enable smartcard support | 
| [udev](https://packages.gentoo.org/useflags/udev) | Enable virtual/udev integration (device discovery, power and storage device support, etc) | 
| [usb](https://packages.gentoo.org/useflags/usb) | Add USB support to applications that have optional USB support (e.g. cups) | 
| [v4l](https://packages.gentoo.org/useflags/v4l) | Enable support for video4linux (using linux-headers or userspace libv4l libraries) | 
| [vaapi](https://packages.gentoo.org/useflags/vaapi) | Enable Video Acceleration API for hardware decoding | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 
| [xinerama](https://packages.gentoo.org/useflags/xinerama) | Add support for querying multi-monitor screen geometry through the Xinerama API | 


| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [+alsa](https://packages.gentoo.org/useflags/+alsa) | Add support for media-libs/alsa-lib (Advanced Linux Sound Architecture) | 
| [+dbus](https://packages.gentoo.org/useflags/+dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [+fontconfig](https://packages.gentoo.org/useflags/+fontconfig) | Support for configuring and customizing font access via media-libs/fontconfig | 
| [+gecko](https://packages.gentoo.org/useflags/+gecko) | Enable mshtml support using app-emulation/wine-gecko | 
| [+gstreamer](https://packages.gentoo.org/useflags/+gstreamer) | Add support for media-libs/gstreamer (Streaming media) | 
| [+mingw](https://packages.gentoo.org/useflags/+mingw) | Build PE files using dev-util/mingw64-toolchain (more tested) rather than llvm-core/clang (newer) | 
| [+mono](https://packages.gentoo.org/useflags/+mono) | Enable .NET support using app-emulation/wine-mono | 
| [+sdl](https://packages.gentoo.org/useflags/+sdl) | Enable gamepad support using media-libs/libsdl2 | 
| [+ssl](https://packages.gentoo.org/useflags/+ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [+strip](https://packages.gentoo.org/useflags/+strip) | Allow symbol stripping to be performed by the ebuild for special files | 
| [+unwind](https://packages.gentoo.org/useflags/+unwind) | Add support for call stack unwinding and function name resolution | 
| [+wow64](https://packages.gentoo.org/useflags/+wow64) | Enable the "new" wow64 mode that allows running 32bit applications without 32bit ELF multilib by mapping to 64bit calls. Generally works well with wine-11.0 or newer, but may or may not have a small perforamce impact or other issues. If USE=mingw is set, still need the default abi\_x86\_32 set on dev-util/mingw64-toolchain but that can be done even with /no-multilib/ profiles. This can also be enabled on arm64 to allow running x86 applications via binary translation. | 
| [+xcomposite](https://packages.gentoo.org/useflags/+xcomposite) | Enable support for the Xorg composite extension | 
| [arm64ec](https://packages.gentoo.org/useflags/arm64ec) | Enable support for running x86\_64 applications on arm64 via binary translation | 
| [crossdev-mingw](https://packages.gentoo.org/useflags/crossdev-mingw) | Use sys-devel/crossdev for the toolchain rather than dev-util/mingw64-toolchain (requires manual setting up and is mostly unsupported, try disabling if have issues) | 
| [custom-cflags](https://packages.gentoo.org/useflags/custom-cflags) | Build with user-specified CFLAGS (unsupported) | 
| [ffmpeg](https://packages.gentoo.org/useflags/ffmpeg) | Enable ffmpeg/libav-based audio/video codec support | 
| [llvm-libunwind](https://packages.gentoo.org/useflags/llvm-libunwind) | Use llvm-runtimes/libunwind instead of sys-libs/libunwind | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [perl](https://packages.gentoo.org/useflags/perl) | Install helpers that require perl (winedump/winemaker) | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [udev](https://packages.gentoo.org/useflags/udev) | Enable virtual/udev integration (device discovery, power and storage device support, etc) | 
| [usb](https://packages.gentoo.org/useflags/usb) | Add USB support to applications that have optional USB support (e.g. cups) | 
| [v4l](https://packages.gentoo.org/useflags/v4l) | Enable support for video4linux (using linux-headers or userspace libv4l libraries) | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 
| [xinerama](https://packages.gentoo.org/useflags/xinerama) | Add support for querying multi-monitor screen geometry through the Xinerama API | 

### 32-bit vs 64-bit

More often than not, Windows applications will require 32-bit support. This may be true even in the case of 64-bit applications, because they can still use a 32-bit installer.

This means that a lot of dependencies of **Wine** will require 32-bit support as well.  If not enabled for all the packages globally, using the `autounmask` option with `emerge` can be helpful with building the list.

Below is an example /etc/portage/package.use/wine file which includes some packages that will need their `abi_x86_32` USE flag enabled for 32-bit support:

**`/etc/portage/package.use/wine`**

Compiling **Wine** with the `wow64` USE-flag enabled will enable running 32-bit applications without building **Wine** and its dependencies with 32-bit support enabled.

Until **Wine** 11.0, upstream considered this feature as experimental, and many applications might still be unusable after this point in Wine, but a lot of applications might also perform perfectly fine.

Some problems may be worked around via use of [Vulkan](https://wiki.gentoo.org/wiki/Vulkan), meaning hardware not capable of supporting **Vulkan** may suffer from more issues.

It is possible to test this by setting the `WINEARCH` variable to `wow64` when creating a new **Wine** prefix, while still building with the USE-flag disabled.

### Build-time environment variables

The live (9999) ebuilds support setting the repository branch, commit, commit date, and repo via the following environment variables:

- EGIT\_OVERRIDE\_BRANCH
- EGIT\_OVERRIDE\_COMMIT
- EGIT\_OVERRIDE\_COMMIT\_DATE
- EGIT\_OVERRIDE\_REPO

As an example, one could select the WineHQ tag "wine-2.0-rc5" to emerge the 2.0 RC 5:

`root #``EGIT_OVERRIDE_COMMIT_WINE_WINE="wine-2.0-rc5" emerge -a '=app-emulation/wine-vanilla-9999'`
One could additionally pin Wine Staging to the same release by finding the appropriate tag, "v2.0-rc5", and augmenting the previous as follows:

`root #``EGIT_OVERRIDE_COMMIT_WINE_WINE="wine-2.0-rc5" EGIT_OVERRIDE_COMMIT_WINE_WINE_STAGING="v2.0-rc5" emerge -a '=app-emulation/wine-staging-9999'`
Other variables, which affect **Wine** at runtime, are discussed [below](https://wiki.gentoo.org#Runtime_environment_variables).

### Emerge

Enable the USE flags of your choosing on the virtual and then on the variant selected (automatically by the virtual or manually by yourself) and emerge the package:

`root #``emerge --ask virtual/wine`
or

`root #``emerge --ask app-emulation/wine-${VARIANT}`
Only versions classified as "stable" by upstream will be stabilized in Gentoo, and only as the [app-emulation/wine-vanilla](https://packages.gentoo.org/packages/app-emulation/wine-vanilla) variant, as external patchsets are not considered stable.  Some users may opt to add **Wine** variants to their package.accept\_keywords file to allow for installation of development versions of **Wine**.

## Configuration

### Kernel

```
Device Drivers --->
    Misc devices --->
        <M> NT synchronization primitive emulation 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NTSYNC</code> to find this item.
The advantage of building ntsync as a [kernel module](https://wiki.gentoo.org/wiki/Kernel_Modules) is that it may only be loaded when needed. This will create the character device /dev/ntsync.

To manually load ntsync with root privileges:

`root #````
modprobe ntsync
```
Since Wine and Proton will not autoload the ntsync module, in the case of frequent use it is recommended to either build it into the kernel, or to automatically load it on startup.

**`/etc/modules-load.d/ntsync.conf`**

Alternatively, a [udev rule](https://wiki.gentoo.org/wiki/Udev#Rules) may autoload the module upon access to /dev/ntsync.

**`/etc/udev/rules.d/70-ntsync.rules`**

Ensure that /dev/ntsync is available while Wine or Proton is running.

### Runtime environment variables

The environment variables of the shell that wine is started from are made accessible to the Windows/DOS processes. Some very useful **Wine**-specific variables include, but are not limited to, `WINEPREFIX`, `WINEARCH`, and `WINEDEBUG`.

See the man wine and man wineserver manual entries for more detailed information concerning **Wine'**s environment variables.

#### WINEPREFIX

To create a **Wine** prefix in a custom location (instead of \~/.wine) without affecting the default:

`user $``WINEPREFIX="$HOME/.wine-someprefix" wineboot`
The above would create a **Wine** prefix at /home/\<user>/.wine-someprefix.

#### WINEARCH

To create a 32-bit installation instead of the default (when built) 64-bit:

`user $``WINEARCH="win32" WINEPREFIX="$HOME/.wine-someprefix" wineboot`
To create a prefix supporting both, 32-bit and 64-bit applications using the new style of WOW64, which can be done with `USE="-wow64"` builds as well:

`user $``WINEARCH="wow64" WINEPREFIX="$HOME/.wine-someprefix" wineboot`
The **Wine** executable used could be anything that runs **Wine**, such as winecfg, which often makes sense while creating a clean, new prefix.

`WINEARCH` requires that **Wine** be built with the corresponding `abi_x86` flags.

#### WINEDEBUG

Essential in finding out why an application is misbehaving when the basic terminal output or messages boxes are not enough.  See [https://wiki.winehq.org/Debug\_Channels](https://wiki.winehq.org/Debug_Channels) for examples.

### Configuration tools

The following tools include graphical and command line interfaces for managing **Wine** prefixes:

- [app-emulation/q4wine](https://packages.gentoo.org/packages/app-emulation/q4wine) – Qt5 GUI configuration tool for **Wine**.
- [app-emulation/winetricks](https://packages.gentoo.org/packages/app-emulation/winetricks) – Easy way to install DLLs needed to work around problems in **Wine**.

### Graphics drivers

[Graphics registry value](<https://wiki.winehq.org/Useful_Registry_Keys#HKEY_CURRENT_USER_(a.k.a_HKCU)>) is comma separated string and REG\_SZ type.

| Driver | Description | 
|---|---|
| x11 | Use the X11 driver | 
| wayland | Use the Wayland driver | 
| mac | Use the native quartz driver (default on macOS) | 
| null | Use the null driver (a virtual screen will be created, but not displayed; available since **Wine** 5.2) | 

Default (on linux)

`user $``wine cmd /c "reg add HKEY_CURRENT_USER\\Software\\Wine\\Drivers /v Graphics /t REG_SZ /d x11 /f"`
For enabling only experimental wayland driver:

`user $``wine cmd /c "reg add HKEY_CURRENT_USER\\Software\\Wine\\Drivers /v Graphics /t REG_SZ /d wayland /f"`
## Easy Anti Cheat Support

Due to DT\_HASH not being enabled by default since glibc 2.36 then the follow needs to be applied to allow EAC games to work

**`/etc/portage/package.use/glibc`**

`root #``emerge --ask --oneshot sys-libs/glibc`
## Upstream FAQs

Some [upstream FAQ](https://wiki.winehq.org/FAQ) entries that users might find useful:

- [How to uninstall individual applications](https://wiki.winehq.org/FAQ#How_do_I_uninstall_individual_Windows_applications.3F)
- [How to remove a **Wine** prefix](https://wiki.winehq.org/FAQ#How_do_I_wipe_the_virtual_Windows_installation.3F)
- [How to prevent **Wine** from creating menu items](https://wiki.winehq.org/FAQ#How_can_I_prevent_Wine_from_changing_the_filetype_associations_on_my_system_or_adding_unwanted_menu_entries.2Fdesktop_links.3F)

## Troubleshooting

When a user encounters a problem with an application, they should try the latest development version to see if the unwanted behavior still exists.  If **Wine** has been built with options such as `-fomit-frame-pointer` or `--hash-style=both`, the **Wine** developers will likely be unable to help with the issue, and reports including output from such builds should not be reported to the [Wine Bugzilla](https://bugs.winehq.org/).

The `strip` USE flag should be disabled for debugging MinGW builds.

For more directions on reporting bugs, see [Bugzilla and Bugs](https://wiki.winehq.org/Bugs) at wiki.winehq.

### Applications Crashing

Some applications such as games may crash on startup giving errors such as "page fault on read access". To mitigate this inside your wine scripts make sure you change directory to inside the game folder instead of running the game by specifying the path for it.

### Support

Users may find additional support in the [#gentoo-wine](ircs://irc.libera.chat/#gentoo-wine) ([webchat](https://web.libera.chat/#gentoo-wine)) channel on Libera.Chat.

## See also

- [DOSEMU](https://wiki.gentoo.org/wiki/DOSEMU) — an application compatibility layer for [MS-DOS](https://en.wikipedia.org/wiki/MS-DOS) geared more towards running MS-DOS applications than running games.
- [Game emulators](https://wiki.gentoo.org/wiki/Games/emulation) — list of game emulators packaged by Gentoo-
- [Lutris](https://wiki.gentoo.org/wiki/Lutris) — an open source gaming platform for Linux.
- [Steam](https://wiki.gentoo.org/wiki/Steam) — a video game digital distribution service by Valve.
- [Winetricks](https://wiki.gentoo.org/wiki/Winetricks) — an easy way to work around problems in [Wine].

## External resources

- [Gentoo bugtracker: known bugs](https://bugs.gentoo.org/buglist.cgi?quicksearch=app-emulation/wine&order=bug_id%20DESC) — **Wine** related bugs.
- [Wine Application Database](https://appdb.winehq.org/) — Search for the game or program to install here to see if it is stable.
- [Wine Staging at GitHub](https://github.com/wine-staging/wine-staging)
- [WineHQ Wiki](https://wiki.winehq.org/)
- [Darling](https://github.com/darlinghq/darling) — a [Darwin](<https://en.wikipedia.org/wiki/Darwin_(operating_system)>) and [macOS](https://en.wikipedia.org/wiki/MacOS) application comparability layer for Linux.
