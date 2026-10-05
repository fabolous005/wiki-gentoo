<!-- source: https://wiki.gentoo.org/wiki/Steam/Client_troubleshooting | group: Gentoo Wiki (Main) | wiki-title: Steam/Client troubleshooting -->
---
title: Steam/Client troubleshooting
url: https://wiki.gentoo.org/wiki/Steam/Client_troubleshooting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-21"
fingerprint: "9456fcff0e8226c8"
license: CC BY-SA 4.0
---

# Steam/Client troubleshooting

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article provides troubleshooting details for the Steam client on Linux systems.

## ATI video drivers

- If ATI Legacy drivers are used, and a Valve game (Counter-Strike: Source, Team Fortress 2, etc.) fails to start with the following error:

Required OpenGL extension "GL\_EXT\_texture\_sRGB\_decode" is not supported. Please update your OpenGL driver.

Update the ATI Legacy drivers to the most recent version<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

## Big Picture Mode and games not working with controller

This issue is most likely caused by incorrect uinput device node permissions.

### Sony DualShock 3

Check /dev/uinput:

crw-rw---- 1 root input 10, 223 Apr  3 21:44 /dev/uinput

In order for Big Picture Mode to see the controller, it needs the user to have write access to it. The solution is a udev rule:

**`/etc/udev/rules.d/60-dualshock3.rules`**

```
# Fix permissions to allow the DualShock 3 controller to be used by Steam
SUBSYSTEM=="usb", ATTRS{idVendor}=="054c", MODE="0666"
KERNEL=="uinput", SUBSYSTEM=="misc", MODE="0660", GROUP="input"
```
After saving the file, udev will need to be "re-triggered":

`root #``udevadm control --reload``root #``udevadm trigger`
If the user(s) that intend to use the controller with Steam are not already in the input group, they will need to be added:

`root #``gpasswd -a <user> input`
After that, log out and back in as said user. Steam should detect the controller without issue. A good way to test this is to make sure Steam has focus and press the `PlayStation` button. If Big Picture mode starts, the controller is detected and configured correctly.

### Microsoft Xbox 360 controller

The Microsoft Xbox 360 controller should work out of the box with [steam-overlay](https://wiki.gentoo.org/wiki/Steam#External_repositories) and *CONFIG\_JOYSTICK\_XPAD* compiled into the kernel.

## Wayland

### Big Picture Mode not launching on Wayland

If Big Picture Mode fails to launch on Wayland, check if you do not have set the environment variable `SDL_VIDEODRIVER=wayland`, or override it on launching Steam:

`user $``SDL_VIDEODRIVER=x11 steam`
When using the provided desktop file to launch Steam, the workaround can be made permanent by editing its `Exec` part:

**`/usr/share/applications/steam.desktop`**

```
Exec=env SDL_VIDEODRIVER=x11 /usr/bin/steam %U
```
### Steam client freezing shortly after startup

The Steam client may consistently freeze shortly after startup when run under KDE Wayland. This appears to be related to a failure of the steam client to properly handle its notification animations.

The easiest workaround is to click and dismiss any notification after steam has launched. It has been reported that using a c[ustom skin for steam that disables notification animations](https://github.com/ValveSoftware/steam-for-linux/issues/8693#issuecomment-1368045868) resolves this issue for some (but not all) users.

Track the upstream issue [here](https://github.com/ValveSoftware/steam-for-linux/issues/8693).

## Direct rendering is not being used

If Steam starts with the following error:

Error: OpenGL GLX context is not using direct rendering, which may cause performance problems.

Confirm if direct rendering is enabled with glxinfo, which is provided by the [x11-apps/mesa-progs](https://packages.gentoo.org/packages/x11-apps/mesa-progs) package:

`root #``glxinfo | grep "direct rendering"`
direct rendering: Yes

If direct rendering is not enabled, ensure that the correct OpenGL implementation is [selected](https://wiki.gentoo.org/wiki/Eselect):

`root #``eselect opengl list`
Next, ensure that the user running Steam has sufficient permissions to access direct rendering. If the `USE` variable is set to `acl` and elogind or [systemd](https://wiki.gentoo.org/wiki/Systemd) is being used, permissions will be handled automatically. Otherwise, add the user running Steam to the video group:

`root #``gpasswd -a` *user* video
If direct rendering is enabled and the correct OpenGL implementation is selected, then this issue may be caused by [app-eselect/eselect-opengl](https://packages.gentoo.org/packages/app-eselect/eselect-opengl) 1.3\*.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>. This issue has been fixed for users of the steam-launcher ebuild. Otherwise, run the following as a temporary workaround:

For ATI drivers:

`user $``LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/lib32/opengl/ati/lib" steam`
For NVIDIA drivers:

`user $``LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/lib32/opengl/nvidia/lib" steam`
If direct rendering is *still* not enabled, refer to the Steam Knowledge Base [article](https://support.steampowered.com/kb_article.php?ref=9938-EYZB-7457) on direct rendering for possible solutions.

## Flatpak client

### SSL errors

When installing the Flatpak version, certain items may show "Invalid SSL certificate" when trying to load (like News and Friends). This is caused by a [bug](https://gitlab.com/freedesktop-sdk/freedesktop-sdk/-/merge_requests/2255) in [app-crypt/p11-kit](https://packages.gentoo.org/packages/app-crypt/p11-kit). Make sure the version of [app-crypt/p11-kit](https://packages.gentoo.org/packages/app-crypt/p11-kit) is equal to or greater than 0.23.20.

### Steam systray icon is not visible

There is currently no solution<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>. Instead the "empty space" in the systray can be clicked to open Steam.

### Vulkan breaks after nvidia-drivers update

After updating [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers), Proton and Vulkan-based games may fail to work (fallback to OpenGL). On a laptop with multiple GPUs, Proton/Vulkan games use the integrated GPU instead of the NVIDIA discrete GPU after updating [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers).

Other symptoms when starting Vulkan-based games:

Could not create Vulkan instance :
ERROR\_EXTENSION\_NOT\_PRESENT
FATAL ERROR: vkCreateInstance failed with error (VK\_ERROR\_EXTENSION\_NOT\_PRESENT)

This issue<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup> is likely due to a driver version mismatch. Flatpak applications run within a container (or "sandbox"), and the NVIDIA libraries within the container must match the drivers loaded and running on the Gentoo host.

To fix this, run

`user $``flatpak update --app com.valvesoftware.Steam`
to update the required runtime for `com.valvesoftware.Steam`. Check if flatpak updates the `org.freedesktop.Platform.GL[32].nvidia-VERSION-NUM` package to match the version number of [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers) on the Gentoo host.

## glxChooseVisual failed

If you encounter this error:

glXChooseVisual failed
glXChooseVisual failedsrc/steamUI/Main.cpp (409) : Assertion Failed: Fatal Error: glXChooseVisual failed
src/steamUI/Main.cpp (409) : Assertion Failed: Fatal Error: glXChooseVisual failed

It usually indicates [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers) needs to be compiled with the **abi\_x86\_32** USE flag for 32-bit support.
Add it to your package.use,

**`/etc/portage/package.use`**

When using Wayland, with [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers), the [X](https://packages.gentoo.org/useflags/X) `USE` flag needs to be enabled.

**`/etc/portage/package.use`**

When the necessary USE flags have been enabled, do a world update.

`root #``emerge -avuDU @world`
## Friends list offline and store not rendering

When running Steam in the GNOME desktop environment the store (and special offers) no longer properly render; the friend's list is constantly in an offline state and status cannot be changed to online. When viewing Steam client debugging output, the following line is repeatably displayed:

./steamwebhelper: symbol lookup error: /usr/lib/gio/modules/libdconfsettings.so: undefined symbol: g\_log\_structured\_standard

The issue is caused by Steam not properly setting the `GSETTINGS_BACKEND` environment variable, not setting the `GIO_MODULE_DIR` environment variable to its own GIO/modules path, and not shipping a newer version of glib library in its included runtime.

The workaround is to set the `GSETTINGS_BACKEND` environment variable to `memory` when launching Steam[\[5\]](https://wiki.gentoo.org#cite_note-5)<sup>[\[6\]](https://wiki.gentoo.org#cite_note-6)</sup>. This is best completed by editing the `Exec` line in the steam.desktop file used to launch Steam:

**`/usr/share/applications/steam.desktop`**

```
Exec=env GSETTINGS_BACKEND=memory /usr/bin/steam %U
```
## Hardened Gentoo

It seems that the Steam binary has `rwx` bits set, and needs to be PaX marked in order to work on a hardened system:

`user $``paxctl-ng -m ~/.local/share/Steam/ubuntu12_32/steam`
The binaries of most games should also be PaX marked:

`user $``paxctl-ng -m ~/.local/share/Steam/steamapps/common/World\ of\ Goo/WorldOfGoo``user $``paxctl-ng -m ~/.local/share/Steam/steamapps/common/Uplink/uplink.bin.x86_64``user $``paxctl-ng -m ~/.local/share/Steam/steamapps/common/Team\ Fortress\ 2/hl2_linux`
Failure to perform PaX marking will result in the game failing to run, with little information given. To check if a game needs to be PaX marked, run the game's startup script or binary file (found in \~/.local/share/Steam/steamapps or \~/.local/share/Steam/steamapps/common) under a debugger. This can be accomplished with some of Valve's provided startup scripts by setting the `GAME_DEBUGGER` environment variable to `gdb`:

`user $``env GAME_DEBUGGER=gdb ./hl2.sh`
If a binary needs to be PaX marked, gdb should output something similar to:

warning: Cannot call inferior functions, Linux kernel PaX protection forbids return to non-executable pages!

and/or:

Cannot access memory at address 0x80486c6.

After an update in July 2013, Steam also needs a PaX marked /bin/bash when the OpenGL libraries require `rwx` markings, otherwise games will fail to run from the Steam client<sup>[\[7\]](https://wiki.gentoo.org#cite_note-7)</sup>:

`user $``sudo paxctl-ng -m /bin/bash`
However, this results in Bash failing to run. It is also a security issue, and it is strongly recommended to try without PaX marking. If it works when using the proprietary NVIDIA drivers, please make a note of it on this page.

## libGL fails to load driver

The Steam runtime overrides various system libraries, including libgcc, libstdc++, and libgpg-error with its own bundled versions<sup>[\[8\]](https://wiki.gentoo.org#cite_note-8)</sup>. This can result in Steam failing to start with the following error:

libGL error: unable to load driver: \<driver\_name>.so

To workaround this issue, locate the problematic library by starting Steam with libGL debugging enabled:

`user $``LIBGL_DEBUG=verbose /usr/bin/steam`
libGL: dlopen /usr/lib32/dri/i965\_dri.so failed (/usr/lib32/libgcrypt.so.20: symbol gpgrt\_lock\_lock, version GPG\_ERROR\_1.0 not defined in file libgpg-error.so.0 with link time reference)
libGL error: unable to load driver: i965\_dri.so

Next, delete the problematic library from the Steam runtime to force the use of system library instead:

`user $``find ~/.steam/root/ -name "libgpg-error.so*" -print`
Repeat the above to discover other problematic libraries in the Steam runtime. The bundled libraries may reappear after every Steam update. In this case simply delete the libraries again from the Steam runtime.

If the above solution does not work, run the following as a temporary workaround:

For NVIDIA drivers:

`user $``LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/lib32/opengl/nvidia/lib" steam`
### libGL fails to load driver with AMDGPU

When using [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU), Steam may fail to start with the following error:

libGL error: unable to load driver: radeonsi\_dri.so
libGL error: driver pointer missing
libGL error: failed to load driver: radeonsi
libGL error: unable to load driver: radeonsi\_dri.so
libGL error: driver pointer missing
libGL error: failed to load driver: radeonsi
libGL error: unable to load driver: swrast\_dri.so
libGL error: failed to load driver: swrast

To workaround this issue, disable the problematic libraries from the Steam runtime to force the use of system libraries instead<sup>[\[9\]](https://wiki.gentoo.org#cite_note-9)</sup>:

`user $````
mv ~/.local/share/Steam/ubuntu12_32/steam-runtime/i386/lib/i386-linux-gnu/libgcc_s.so.1{,.disable}
```
`user $````
mv ~/.local/share/Steam/ubuntu12_32/steam-runtime/i386/usr/lib/i386-linux-gnu/libstdc++.so.6{,.disable}
```
## Memory corruption

If Steam starts with the following error:

\*\*\* glibc detected \*\*\* zenity: malloc(): memory corruption: 0x00000000016cf020 \*\*\*

Installing the [x11-libs/libXi](https://packages.gentoo.org/packages/x11-libs/libXi) package should fix the issue.

`root #``emerge --ask x11-libs/libXi`
## Missing fonts

If Steam is having issues with missing [fonts](https://wiki.gentoo.org/wiki/Fontconfig), installing the [media-fonts/font-bitstream-100dpi](https://packages.gentoo.org/packages/media-fonts/font-bitstream-100dpi) and [media-fonts/corefonts](https://packages.gentoo.org/packages/media-fonts/corefonts) packages may fix the issue.

`root #``emerge --ask media-fonts/font-bitstream-100dpi media-fonts/corefonts`
If the X server does not recognize the newly installed fonts, run the following:

`user $````
xset +fp /usr/share/fonts/100dpi
```
`user $````
xset +fp /usr/share/fonts/corefonts
```
`user $````
xset fp rehash
```
## Reset the installation

To reset (i.e. wipe) the Steam installation, including installed games, and reinstall Steam without losing data:

`user $``steam --reset`
## Reversed X cursor

If an X cursor theme has not been set by the desktop environment or window manager, Steam will override the default X cursor theme. This can result in a reversed X cursor from left to right. The issue can be fixed by setting an X cursor theme, if one is available, or by installing an X cursor theme:

`root #``emerge --ask x11-themes/vanilla-dmz-xcursors`
If the X cursor is still reversed, even after exiting Steam, run the following to fix the issue:

`user $``xsetroot -cursor_name left_ptr`
## Segfault when remember my password is selected

Selecting the `Remember my password` option at the Steam login dialog when Steam is running without D-Bus, will cause Steam to segfault the next time it is started<sup>[\[10\]](https://wiki.gentoo.org#cite_note-10)</sup>. This issue can be fixed by running the following:

`user $``rm -fr ~/.local/share/Steam/config`
## Segfault when starting Steam

Steam segfaults when [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) is not accessible. Check that dbus service is runnning and that the `DBUS_SESSION_BUS_ADDRESS` variable is set:

`user $``echo $DBUS_SESSION_BUS_ADDRESS`
dbus-J2zA6AVqHR,guid=e86f44f3b96c6c03f68d01a66029c172

This may happen when [KDE is started from the command line](https://wiki.gentoo.org/wiki/KDE#No_display_manager) and omit to use `dbus-launch`.

If Steam fails to start with the following error:

munmap\_chunk(): invalid pointer: 0xf75aca24
free(): invalid pointer: 0xffca16d0

Setting the locale to `C` may fix the issue<sup>[\[11\]](https://wiki.gentoo.org#cite_note-11)</sup>:

`user $``LANG=C steam`

If Steam fails to start with the following error:

The futex facility returned an unexpected error code.

The kernel option `CONFIG_COMPAT_32BIT_TIME` needs to be activated.

## Segfault when using apulse

Using Steam with [media-sound/apulse](https://packages.gentoo.org/packages/media-sound/apulse) while D-Bus is *not* running, will cause Steam to segfault with a `Connection refused` message. This issue can be fixed by ensuring that D-Bus is running:

`root #``rc-service dbus start`
## Segfault when using libcxx

When using [sys-libs/libcxx](https://packages.gentoo.org/packages/sys-libs/libcxx) system-wide, a [Segmentation Fault when running Steam](https://github.com/ValveSoftware/steam-for-linux/issues/8923) may occur. To fix this, make sure to set LD\_PRELOAD to the following:

**`/usr/share/applications/steam.desktop`**

`user $``LD_PRELOAD="/usr/lib/gcc/x86_64-pc-linux-gnu/12/32/libstdc++.so" steam`
Depending on the desktop environment being used, the Steam taskbar button may persist even when the Steam window is closed or minimized to the system tray<sup>[\[12\]](https://wiki.gentoo.org#cite_note-12)</sup>. To correct this behavior, force the Steam window to close instead of minimize:

`user $``STEAM_FRAME_FORCE_CLOSE=1 steam`
To set the `STEAM_FRAME_FORCE_CLOSE` [environment variable](https://wiki.gentoo.org/wiki/Handbook:Parts/Working/EnvVar) permanently, add the following to the [shell](https://wiki.gentoo.org/wiki/Shell) login initialization file:

**`~/.bash_profile`**

**Setting a local environment variable with Bash**

Log out and back in to have the changes take effect.

## Steam hangs when installing a game

If clicking the Install button on a Steam game page causes the Steam client to hang with the following console output:

GameAction \[AppID 255710, ActionID 1\] : LaunchApp failed with AppError\_18 with ""              
GameAction \[AppID 255710, ActionID 1\] : LaunchApp changed task to Failed with ""

It might be possible to fix this by running Steam with:

`user $``STEAM_RUNTIME=1 STEAM_RUNTIME_PREFER_HOST_LIBRARIES=1 steam`
## Use system libraries

Steam bundles many libraries which are used instead of the system libraries. To force Steam to use the system libraries, disable the Steam runtime:

`user $``STEAM_RUNTIME=0 steam`
To set the `STEAM_RUNTIME` [environment variable](https://wiki.gentoo.org/wiki/Handbook:Parts/Working/EnvVar) permanently, add the following to the [shell](https://wiki.gentoo.org/wiki/Shell) login initialization file:

**`~/.bash_profile`**

**Setting a local environment variable with Bash**

Log out and back in to have the changes take effect.

## Windows games crash immediately on launch

If Windows-only games crash without even displaying a window, and the Steam log includes a line that begins:

wine: Unhandled exception 0x20474343 in thread..

or a series of errors similar to:

Vulkan missing requested extension 'VK\_KHR\_surface'.
Vulkan missing requested extension 'VK\_KHR\_xlib\_surface'.
BInit - Unable to initialize Vulkan!

the cause might be that the system Mesa library (on which Steam depends) is not compiled with Vulkan support. Follow the instructions on the [Vulkan](https://wiki.gentoo.org/wiki/Vulkan) page to rectify this, and ensure that the [media-libs/vulkan-loader](https://packages.gentoo.org/packages/media-libs/vulkan-loader) package is compiled with X11 support:

**`/etc/portage/package.use`**

## xterm launches briefly and then closes

This issue is caused by the shell being set to something other than a POSIX compliant shell (i.e. fish). Change the shell to a POSIX compliant shell and accept the Steam license agreement. The shell can be set back afterwards.

## System information can not be detected

Sometimes it may be useful to obtain system information, as explained here: [https://steamcommunity.com/sharedfiles/filedetails/?id=390278662](https://steamcommunity.com/sharedfiles/filedetails/?id=390278662) .
If the client is not able to detect the distribution in which it is running or any related information, install [sys-apps/lsb-release](https://packages.gentoo.org/packages/sys-apps/lsb-release).

`root #``emerge --ask sys-apps/lsb-release`
## Library and store display a black screen

This is related to the following error:

D-Bus library appears to be incorrectly set up; failed to read machine uuid: Failed to open "/etc/machine-id": No such file or directory

In this case it is remedied by generating a machine ID with the following command

`root #``systemd-machine-id-setup`
## Games immediately crash/don't start

This is related to the following error:

eventfd: Too many open files
eventfd: Too many open files
eventfd: Too many open files
eventfd: Too many open files
eventfd: Too many open files
eventfd: Too many open files
eventfd: Too many open files
eventfd: Too many open files
wine: Unhandled page fault on read access to 0000000000000000 at address 0000000141510EDA (thread 0114), starting debugger...
eventfd: Too many open files

Try [cranking](https://www.reddit.com/r/SteamPlay/comments/9kqisk/tip_for_those_using_proton_no_esync1/) up the ulimit (user limit) for Steam.

**`/etc/security/limits.d/steam.conf`**

Log out and log back in.

What this does is allow steam to open 1048576 [file descriptors](https://en.wikipedia.org/wiki/File_descriptor) at once. If this value seems too high or low, adjust accordingly!

This config will allow all users and groups to use the new limit. To set the new limit to a particular user only, the \* in the beginning must be replaced with a username.

### systemd additional step

**`/etc/systemd/system.conf`**

**`/etc/systemd/user.conf`**

Then

`root #``systemctl daemon-reexec`
Log out and log back in.

## wine: RLIMIT\_NICE is \<= 20

RLIMIT\_NICE has a range from 1 (corresponding to a nice value of 19, lowest priority) to 40 (corresponding to a nice value of -20, highest priority).

When a game is run using Proton, the wine message might show up in the logs. The way to fix this is to edit the limits config:

**`/etc/security/limits.d/steam.conf`**

Log out and log back in.

This allows steam to run processes with nice values as low as -20 (highest priority).

This config will allow all users and groups to use the new limit. To set the new limit to a particular user only, the \* in the beginning must be replaced with a username.

## SteamVR crashes amdgpu driver when started with valve index

Find out the name of HMD screen:

`user $``xrandr`
Screen 0: minimum 320 x 200, current 1920 x 1080, maximum 16384 x 16384
DisplayPort-0 disconnected (normal left inverted right x axis y axis)
DisplayPort-1 disconnected (normal left inverted right x axis y axis)
   2880x1600     90.00 + 144.00   120.02    80.00  
   1920x1200     90.00  
   1920x1080     90.00  
   1600x1200     90.00  
   1680x1050     90.00  
   1280x1024     90.00  
   1440x900      90.00  
   1280x800      90.00  
   1280x720      90.00  
   1024x768      90.00  
   800x600       90.00  
   640x480       90.00  
DisplayPort-2 connected primary 1920x1080+0+0 (normal left inverted right x axis y axis) 480mm x 270mm
   1920x1080     74.97\*+  60.00    50.00    59.94  
   1680x1050     59.95  
   1400x1050     59.98  
   1600x900      60.00  
   1280x1024     60.02  
   1440x900      59.89  
   1280x800      59.81  
   1280x720      60.00    50.00    59.94  
   1024x768      60.00  
   800x600       60.32  
   720x576       50.00  
   720x480       60.00    59.94  
   640x480       60.00    59.94  
HDMI-A-0 disconnected (normal left inverted right x axis y axis)

In this example it's DisplayPort-1. Execute this ugly script before starting SteamVR and crash will be prevented.

**`fix-vr`**

## No sound in SteamVR Home

Move libSDL2-2.0.so.0 files somewhere to force loading libSDL2-2.0.so.0 from the system:

`user $``mv ~/.local/share/Steam/steamapps/common/SteamVR/tools/steamvr_environments/game/bin/linuxsteamrt64/libSDL2-2.0.so.0 ~/linuxsteamrt64-libSDL2-2.0.so.0``user $``mv ~/.local/share/Steam/steamapps/common/SteamVR/bin/linux64/libSDL2-2.0.so.0 ~/linux64-libSDL2-2.0.so.0`
If it doesn't help, follow the commented advice from \~/.local/share/Steam/steamapps/common/SteamVR/tools/steamvr\_environments/game/steamtours.sh and execute:

`user $``export GAME_DEBUGGER="strace -f -o strace.log"``user $``steam`
The output will be available in \~/.local/share/Steam/steamapps/common/SteamVR/tools/steamvr\_environments/game/strace.log. Additional instructions can apply to [Half-Life: Alyx](https://wiki.gentoo.org/wiki/Steam/Games_troubleshooting#No_sound_in_.22Half-Life:_Alyx.22).

## "Only one instance of the game can be running at one time." error in SteamVR

Stop steam and delete steam-related files from /tmp.

`user $``rm -r /tmp/qipc_systemsem_vrmonitor* /tmp/runtime-steam/ /tmp/source_engine_*.lock /tmp/steam_chrome_shmem_*`
If you have other steam files there, probably it's a good idea to delete them too. Then start steam again.

## Sharing a games library between Windows and Linux on a NTFS-backed directory

Official Valve instructions for using Proton to share games on an NTFS partition between Linux and Windows can be found here: [https://github.com/ValveSoftware/Proton/wiki/Using-a-NTFS-disk-with-Linux-and-Windows](https://github.com/ValveSoftware/Proton/wiki/Using-a-NTFS-disk-with-Linux-and-Windows)

## Client is not scaling properly

### Environment variables

As of 2023-06-15<sup>[\[13\]](https://wiki.gentoo.org#cite_note-13)</sup> the Steam Beta Client honors the following environment variable which can to manually adjust the scaling factor of the client's display window:

`user $``STEAM_FORCE_DESKTOPUI_SCALING=<float> steam`
Replace `<float>` as necessary for the scaling factor. It also should honor the scaling factor for GNOME systems via the org.gnome.desktop.interface/text-scaling-factor configuration value. This value can be inspected using gsettings.

### CLI parameters

As of 2023-05-05<sup>[\[14\]](https://wiki.gentoo.org#cite_note-14)</sup>, the Steam Beta Client provides the following command line argument to manually adjust the scaling factor of the client's display window:

`user $``steam -forcedesktopscaling <float>`
Replace `<float>` as necessary for the scaling factor. For example, when running a 4K display, a factor of `2.0` is generally sufficient to present the client as it would appear without scaling on a 1080p display:

`user $``steam -forcedesktopscaling 2.0`
## Increase virtual memory mapping

Certain games like Dayz and Counter-Strike 2 may run out of memory if the default value of 65,530 is set in the /etc/sysctl.conf file. It is recommended to increase the default virtual memory map to a value of 1048576 in order to avoid memory related issues with certain games. This will affect all processes running on the system, so proceed with the risk in mind:

`root #``sysctl -w vm.max_map_count=1048576`
## SteamVR doesn't work after it was updated (as steam package) and no workaround exists

In this case it's possible do downgrade SteamVR steam package as described here [https://github.com/ValveSoftware/SteamVR-for-Linux/issues/639#issuecomment-1817538299](https://github.com/ValveSoftware/SteamVR-for-Linux/issues/639#issuecomment-1817538299) . This workflow should also be able to downgrade other steam packages.

## Slow download or limited

Disable HTTP2 for faster downloads. Some systems and configurations seem to have issues with HTTP2. Disabling HTTP2 will probably yield faster downloads on those configurations. Create the file as described below and add the following lines to the file

Try just one line first, if there is no improvement then use both

Steam Overlay Native Installs, create:

`user $``echo "@nClientDownloadEnableHTTP2PlatformLinux 0" >> ~/.steam/steam/steam_dev.cfg`
Restart Steam and check download speed.

If the first option doesn't work, try adding another line

`user $``echo "@fDownloadRateImprovementToAddAnotherConnection 1.0" >> ~/.steam/steam/steam_dev.cfg`
Restart Steam and check download speed.

Flatpak Steam Installs, create:

`user $``echo "@nClientDownloadEnableHTTP2PlatformLinux 0" >> ~/.var/app/com.valvesoftware.Steam/.steam/steam/steam_dev.cfg` Restart Steam and check download speed.

If the first option doesn't work, try adding another line

`user $``echo "@fDownloadRateImprovementToAddAnotherConnection 1.0" >> ~/.var/app/com.valvesoftware.Steam/.steam/steam/steam_dev.cfg`
Restart Steam and check download speed.

## Steam crashed when executing steam.desktop but do not crash when running from terminal multi-gpu

When the system has multiple configured GPU's, this can cause the steam client to end up in a crashloop.

If the steam client get in a crashloop when started from /usr/share/applications/steam.desktop but works normaly when exectured from the terminal.

`user $``steam`
One issue can the Environment variable DRI\_PRIME

To reproduce the error run

`user $``DRI_PRIME=1 steam`
And to check if adjustment of the variable starts steam without crashing, this causes steam to take another approach to detect the GPU to use.

`user $``DRI_PRIME=0 steam`
If this solves the problem the /usr/share/applications/steam.desktop can be modified to

**`/usr/share/applications/steam.desktop`**

## unable to use steam input/overlay

in order for steam input or the steam overlay to work games must be laucnhed as a x11 window or threw a gamescope session that both steam and the game are under. if you want to use wayland specfic funcitality like HDR and need support for steam input or the steam overlay you must run steam like this

`user $``gamescope -f -W 3840 -H 2160 --hdr-enabled --hdr-debug-force-support -- steam --steamos`
## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) Mark Browning. [\[WORKAROUND\] Allow TF2 to run with legacy ATI drivers 12.6.](https://steamcommunity.com/app/221410/discussions/0/846938351012409765), [Steam for Linux Steam Community](https://steamcommunity.com/app/221410), December 4th, 2012. Retrieved on May 30th, 2015.
2. [↑](https://wiki.gentoo.org#cite_ref-2) Michał Górny. [steam-launcher likely broken with eselect-opengl-1.3\*](https://github.com/anyc/steam-overlay/issues/121), [Steam Overlay](https://github.com/anyc/steam-overlay), January 2nd, 2015. Retrieved on May 27th, 2015.
3. [↑](https://wiki.gentoo.org#cite_ref-3) pyroxar. [Missing icon when Steam update or install runtime while first run](https://github.com/flathub/com.valvesoftware.Steam/issues/311), [Flatpak wrapper for Steam using Freedesktop Runtime](https://github.com/flathub/com.valvesoftware.Steam), January 28th 2019. Retrieved on April 13th, 2020.
4. [↑](https://wiki.gentoo.org#cite_ref-4) Shished. [Vulkan games stopped working after Arch update](https://github.com/flathub/com.valvesoftware.Steam/issues/541), March 22nd, 2020. Retrieved on September 2nd, 2020.
5. [↑](https://wiki.gentoo.org#cite_ref-5) leio. [symbol lookup error: libdconfsettings.so](https://github.com/anyc/steam-overlay/issues/232#issuecomment-490614171), [Steam Overlay](https://github.com/anyc/steam-overlay), May 9th, 2019. Retrieved on October 24th, 2019.
6. [↑](https://wiki.gentoo.org#cite_ref-6) Leio. [GNOME 3.30 for all init systems](https://forums.gentoo.org/viewtopic-p-8333248.html#8333248), [Gentoo Forums](https://forums.gentoo.org/), May 8th, 2019. Retrieved on October 24th, 2019.
7. [↑](https://wiki.gentoo.org#cite_ref-7) Alex Efros. [Compatibility with PaX/GrSecurity](https://github.com/ValveSoftware/steam-for-linux/issues/254), [Steam for Linux](https://github.com/ValveSoftware/steam-for-linux), December 22nd, 2012. Retrieved on May 28th, 2015.
8. [↑](https://wiki.gentoo.org#cite_ref-8) jsa1983. [Problem with libstdc++.so.6 bundled with steam-runtime 2014-04-15](https://github.com/ValveSoftware/steam-runtime/issues/13), [Steam for Linux](https://github.com/ValveSoftware/steam-for-linux), April 26th, 2014. Retrieved on December 26th, 2015.
9. [↑](https://wiki.gentoo.org#cite_ref-9) itsnikolay. [Problem with installing Steam on Ubuntu 15.04+](https://askubuntu.com/questions/614422/problem-with-installing-steam-on-ubuntu-15-04/616988#616988), [Ask Ubuntu](https://askubuntu.com/), May 1st, 2015. Retrieved on September 19th, 2018.
10. [↑](https://wiki.gentoo.org#cite_ref-10) Shished. [Steam segfaults when "remember my password" is checked](https://github.com/ValveSoftware/steam-for-linux/issues/3415), [Steam for Linux](https://github.com/ValveSoftware/steam-for-linux), July 28th, 2014. Retrieved on May 25th, 2015.
11. [↑](https://wiki.gentoo.org#cite_ref-11) pnrao. [Steam client not launching](https://github.com/ValveSoftware/steam-for-linux/issues/4887), [Steam for Linux](https://github.com/ValveSoftware/steam-for-linux), March 6th, 2017. Retrieved on June 11th, 2017.
12. [↑](https://wiki.gentoo.org#cite_ref-12) Kevin Cox. [Close to Tray](https://github.com/ValveSoftware/steam-for-linux/issues/1025), [Steam for Linux](https://github.com/ValveSoftware/steam-for-linux), January 30th, 2013. Retrieved on May 27th, 2015.
13. [↑](https://wiki.gentoo.org#cite_ref-13) [https://store.steampowered.com/news/app/593110/view/3687931965598323436](https://store.steampowered.com/news/app/593110/view/3687931965598323436)
14. [↑](https://wiki.gentoo.org#cite_ref-14) [https://store.steampowered.com/news/group/4397053/view/3705943193607193985?l=english](https://store.steampowered.com/news/group/4397053/view/3705943193607193985?l=english)
