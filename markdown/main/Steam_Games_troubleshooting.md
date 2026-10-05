<!-- source: https://wiki.gentoo.org/wiki/Steam/Games_troubleshooting | group: Gentoo Wiki (Main) | wiki-title: Steam/Games troubleshooting -->
---
title: Steam/Games troubleshooting
url: https://wiki.gentoo.org/wiki/Steam/Games_troubleshooting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-22"
fingerprint: b406cf520e963ddc
license: CC BY-SA 4.0
---

# Steam/Games troubleshooting

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article provides troubleshooting details for specific games running via Steam.

## Steam runtime

If the Steam runtime is enabled and some games are not starting (for example Dying Light or Life Is Strange), setting `STEAM_RUNTIME_PREFER_HOST_LIBRARIES=0` may fix the issue.

## Texture compression

Many games, especially those that use the [Source](<https://en.wikipedia.org/wiki/Source_(game_engine)>) engine, require [S3 Texture Compression (S3TC)](https://en.wikipedia.org/wiki/S3_Texture_Compression) support. Without S3TC support, these games will usually have black or missing textures, or fail to start.

Confirm if S3TC support is enabled with glxinfo, which is provided by the [x11-apps/mesa-progs](https://packages.gentoo.org/packages/x11-apps/mesa-progs) package:

`root #``glxinfo | grep GL_EXT_texture_compression_s3tc````
    GL_EXT_texture_compression_s3tc, GL_EXT_texture_filter_anisotropic,
    GL_EXT_texture_compression_s3tc, GL_EXT_texture_cube_map,
```
If S3TC support is not enabled, ensure that the `VIDEO_CARDS` variable in /etc/portage/make.conf is set to the correct value, and update the video driver to most recent version.

If S3TC support is enabled, but games fail to start with the following error:

This system does not support the OpenGL extension GL\_EXT\_texture\_compression\_s3tc

Force enable S3TC support with:

`user $``force_s3tc_enable=true steam`
If [nouveau](https://wiki.gentoo.org/wiki/Nouveau) drivers are being used, installing [x11-libs/gtkglarea](https://packages.gentoo.org/packages/x11-libs/gtkglarea) may be required to fix the above error.

`root #``emerge --ask x11-libs/gtkglarea`
## Black Mesa

- If the game is launched but shows nothing, add `LD_PRELOAD="/usr/lib64/gcc/x86_64-pc-linux-gnu/<your gcc version number>/32/libstdc++.so.6" %command%` to the launch options in `Library->Black Mesa->Properties->General->Set launch options..`.

## Borderlands 2 and Borderlands: The Pre-Sequel

- Gearbox's SHiFT service assumes the underlying distribution is Ubuntu, which stores its SSL certificates in /usr/lib/ssl, whereas Gentoo (by default) stores them in /etc/ssl. This difference prevents the redemption of SHiFT codes on Gentoo. To workaround this issue, add `SSL_CERT_DIR="/etc/ssl/certs" %command%` to the launch options in `Library->{Borderlands 2,Borderlands: The Pre-Sequel}->Properties->General->Set launch options..`.

- If the game is launched but closes almost immediately try adding `-nostartupmovies` to the launch options in `Library->{Borderlands 2,Borderlands: The Pre-Sequel}->Properties->General->Set launch options..`. This fixes the `BorderlandsPreS[...] general protection ip:.. sp:.. error:0 in libopenal.so.1.18.2`. Another solution is to recompile [x11-libs/libxcb](https://packages.gentoo.org/packages/x11-libs/libxcb) with `-O1`, [media-sound/pulseaudio](https://packages.gentoo.org/packages/media-sound/pulseaudio) with `-O1` and without `-march` and [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc) without `-march`<sup>[\[2\]](https://wiki.gentoo.org#cite_note-bugreport-O1-fixes-crash-2)</sup>.

- Additionally the game may still crash at points during game play, when creating a new character or when traveling to one of the DLC's. To workaround this issue edit the willowengine.ini file. Find the section `[FullScreenMovie]` and change the value of `bForceNoMovies` from `FALSE` to `TRUE`. The files can be found at \~/.local/share/aspyr-media/borderlands 2/willowgame/config/willowengine.ini and \~/.local/share/aspyr-media/borderlands the pre-sequel/willowgame/config/willowengine.ini.

- Currently (tested: 22.11.2022 on Arch Linux + Gentoo) it can happen that Borderlands 2 immediately crashes on startup. dmesg should print something about a segfault in libc.so. In Steam one can try to simply add the following to the Launch options: `LD_PRELOAD="" %command%`. It essentially clears the LD\_PRELOAD variable, which would normally be used by Steam to add the Overlay over Games. Clearing this variable therefore also means no Steam overlay in Games.

## Cities: Skylines

- In case you have set some libraries in the environmental variable `LD_PRELOAD` of your linux session, make sure you run the game with this variable emptied or filled only with libraries directly related to the game. Empting can be done by setting `LD_PRELOAD="" %command%` in the launch options in `Library->Cities: Skylines->Properties->General->Set launch options...`. Unrelated libraries in this variable can cause the game will not start or become unstable.

## Dead Cells

Dead cells requires the [Wayland](https://wiki.gentoo.org/wiki/Wayland) libraries to function correctly, and will complain if the libraries are missing when starting.

`root #``emerge --ask --noreplace dev-libs/wayland`
## Death Road to Canada

- Death Road to Canada requires the following USE flags to function correctly and for in-game music:

**`/etc/portage/package.use/steam`**

Rebuild the [media-libs/libsdl2](https://packages.gentoo.org/packages/media-libs/libsdl2) and [media-libs/sdl2-mixer](https://packages.gentoo.org/packages/media-libs/sdl2-mixer) packages:

`root #``emerge --ask --oneshot media-libs/libsdl2 media-libs/sdl2-mixer`
Add `LD_LIBRARY_PATH="/usr/lib:$LD_LIBRARY_PATH" %command%` to the launch options in `Library->Death Road to Canada->Properties->General->Set launch options..`.

## Deus Ex: Mankind Divided

If only the cutscenes have audio/sound, but the menus and gameplay have no audio/sound, try emerging [media-sound/pulseaudio](https://packages.gentoo.org/packages/media-sound/pulseaudio).

## DiRT Showdown

- If the launcher fails to start, enable [texture compression](https://wiki.gentoo.org/wiki/Steam/Games_troubleshooting#Texture_compression) support<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>.

- If an [Intel](https://wiki.gentoo.org/wiki/Intel) GPU is being used and the launcher fails to start with the following error:

Unfortunately your machine doesn't meet the full OpenGL 4.1 requirements so the game may not perform correctly

Add `MESA_GL_VERSION_OVERRIDE=4.1 MESA_GLSL_VERSION_OVERRIDE=410 %command%` to the launch options in `Library->DiRT Showdown->Properties->General->Set launch options..`. Intel [Broadwell](<https://en.wikipedia.org/wiki/Broadwell_(microarchitecture)>) and [Skylake](<https://en.wikipedia.org/wiki/Skylake_(microarchitecture)>) based GPUs also require `INTEL_DEBUG=vec4` to be added to the launch options<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>.

- If an [Intel](https://wiki.gentoo.org/wiki/Intel) GPU is being used, the launch options can be removed after upgrading to Mesa 12.0.1.

## Dota 2

- If black textures are visible and an older (\<9.1.6) [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa) is installed, update [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa) to a recent version<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup>.

- If black textures are visible and a recent [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa) is installed, build [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa) with the `-bindist` USE flag<sup>[\[6\]](https://wiki.gentoo.org#cite_note-6)</sup>.

- If a red screen is visible during startup and textures are missing in-game, enable [texture compression](https://wiki.gentoo.org/wiki/Steam/Games_troubleshooting#Texture_compression) support<sup>[\[7\]](https://wiki.gentoo.org#cite_note-7)</sup>.

- If some textures are not clickable (e.g. the character can not move at the fountain when the right mouse button is clicked), [verify the integrity of the game cache](https://support.steampowered.com/kb_article.php?ref=2037-QEUH-3335).
- Was noticed the following issue: in some cases the game will fail to ping any server and return this message instead:

Unable to ping any region. Check your Internet connection.

The exact reason is unknown, but is suspected to be related to router. Try to plug in the Ethernet cable directly into your PC if you are experiencing this problem.

## Easy Anti-Cheat

If you get a **"Failed to load the anti-cheat module"** error with Easy Anti-Cheat on Proton games, make sure the `hash-sysv-compat` use flag is enabled on [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc)

**Launch Error 261:** Easy Anti-Cheat may be refusing to start because you have `hidepid` set to a non-zero value.

## Exiled Kingdoms

If the game produces the following error:

```
Exception in thread "LWJGL Application" java.lang.ExceptionInInitializerError
        at com.badlogic.gdx.backends.lwjgl.LwjglGraphics.setVSync(LwjglGraphics.java:591)
        at com.badlogic.gdx.backends.lwjgl.LwjglApplication$1.run(LwjglApplication.java:124)
Caused by: java.lang.ArrayIndexOutOfBoundsException: 0
        at org.lwjgl.opengl.LinuxDisplay.getAvailableDisplayModes(LinuxDisplay.java:954)
        at org.lwjgl.opengl.LinuxDisplay.init(LinuxDisplay.java:738)
        at org.lwjgl.opengl.Display.<clinit>(Display.java:138)
        ... 2 more
AL lib: (EE) alc_cleanup: 1 device not closed
```
Ensure [x11-apps/xrandr](https://packages.gentoo.org/packages/x11-apps/xrandr) is installed.

## Firewatch

- If the game fails to start with the following terminal error:

Unable to preload the following plugins: libCSteamworks.so

Try to add `LD_PRELOAD=~/.local/share/Steam/ubuntu12_32/steam-runtime/amd64/usr/lib/x86_64-linux-gnu/libSDL2-2.0.so.0 %command%` to the launch options in `Library->Firewatch->Properties->General->Set launch options...`.

## Half-Life 2

Half-Life 2 and other Source-engine 1 based games (e.g. Portal) may segfault because the stack is no longer 16-byte aligned when the hl2\_linux process calls out to glibc. A symptom that may be exhibited is a segfaulting SIMD instruction (or rather it raises a general protection error), for example `vmovdqa` in `fseek`/`_IO_file_seekoff`/`_IO_new_file_seekoff`.

To fix this, enable the `stack-realign` USE flag for [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc) which adds `-mstackrealign` to its `CFLAGS/CXXFLAGS` for x86/multilib <sup>[\[8\]](https://wiki.gentoo.org#cite_note-8)</sup>.
See [package.env](https://wiki.gentoo.org/wiki/Package.env) for information about setting per-package build options. Functions then have an automatic-alignment-fixing entry which restores the 16-byte-alignment-assumption often used for SSE2/AVX. See also [bug #616402](https://bugs.gentoo.org/show_bug.cgi?id=616402) and the [relevant Fedora bug](https://bugzilla.redhat.com/show_bug.cgi?id=1471427).

## Insurgency

If the game crashes with the following errors:

failed to dlopen /home/user/.steam/steam/steamapps/common/insurgency2/bin/engine.so error=/home/user/.steam/steam/steamapps/common/insurgency2/bin/libgcc\_s.so.1: version \`GCC\_7.0.0' not found (required by /usr/lib32/libopenal.so.1)
AppFramework : Unable to load library module engine.so!
Unable to load interface VCvarQuery001 from engine.so, requested from EXE.

Edit the file insurgency.sh in the default game directory. Remove or comment out lines 27-29, where the lib path of game is prepended to `LD_LIBRARY_PATH`. This will force the game to use system libraries instead.

## Knights of the Old Republic II

- The Linux port may crash on startup with general protection faults, check dmesg for cause:

[  911.240241] traps: KOTOR2[4818] general protection fault ip:ef68f770 sp:f0583974 error:0 in libpulsecommon-16.1.so[ef67c000+4b000]
[ 2011.447025] traps: KOTOR2[12533] general protection fault ip:f7506924 sp:f0582574 error:0 in libc.so.6[f7420000+179000]

- libc: requires glibc built with stack-realign USE, see [Steam/Games\_troubleshooting#Half-Life\_2](https://wiki.gentoo.org/wiki/Steam/Games_troubleshooting#Half-Life_2).
- Other libraries (such as libpulse): build packages with CFLAGS of "-O1" and "-march=x86-64", see [Steam/Games\_troubleshooting#Sid\_Meier.27s\_Civilization\_V](https://wiki.gentoo.org/wiki/Steam/Games_troubleshooting#Sid_Meier.27s_Civilization_V) and [/etc/portage/package.env](https://wiki.gentoo.org/wiki//etc/portage/package.env).

## Left 4 Dead 2

- If black textures are visible, enable [texture compression](https://wiki.gentoo.org/wiki/Steam/Games_troubleshooting#Texture_compression) support.
- If environment light is too dark inside the game, make sure to run the game using a dedicated graphic driver.
- If nvidia driver is intended to be used, enable 32-bit libraries support for x11-drivers/nvidia-drivers.

## Life Is Strange

- If the launcher fails to start, add `LD_LIBRARY_PATH="$HOME/.steam/root/steamapps/common/Life Is Strange/lib/x86_64:$HOME/.local/share/Steam/ubuntu12_32/steam-runtime/amd64/usr/lib/x86_64-linux-gnu:$HOME/.local/share/Steam/ubuntu12_32/steam-runtime/amd64/lib/x86_64-linux-gnu" %command%` to the launch options in `Library->Life Is Strange->Properties->General->Set launch options..`.

## Planetary Annihilation: TITANS

`root #``emerge --ask media-libs/libsdl2`
**`/etc/portage/make.conf`**

```
CURL_SSL="gnutls"
```
`root #``emerge --ask net-misc/curl`
Planetary Annihilation: TITANS is expecting to find libudev.so.0. Within the Planetary Annihilation: TITANS runtime directory, create a symbolic link to /lib/libudev.so.1:

`user $``ln -s /lib/libudev.so.1 libudev.so.0`
## Rust (legacy)

- If the launcher fails to start, add `LD_LIBRARY_PATH="/usr/lib:$LD_LIBRARY_PATH" %command%` to the launch options in `Library->Rust->Properties->General->Set launch options..`.

## Sid Meier's Civilization V

- If a black screen is visible and the introduction music is audible during startup, change the value of `FSResID`<sup>[\[9\]](https://wiki.gentoo.org#cite_note-9)</sup>:

**`~/.local/share/Aspyr/Sid Meier's Civilization 5/GraphicsSettingsDX9.ini`**

```
FSResID = 7
```
The correct value for `FSResID` appears to be system dependent, and may require setting different values before working.

- If the game crashes almost immediately and syslog/dmesg shows:

[120551.527719] traps: Civ5XP[14295] general protection fault ip:f72f2855 sp:f2a9eb34 error:0 in libxcb.so.1.1.0[f72e7000+2c000]

where the library might be libxcb.so.1.1.0, libc-2.30.so, or libasound.so.2.0.0 (amongst others), try recompiling [x11-libs/libxcb](https://packages.gentoo.org/packages/x11-libs/libxcb), [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc), [media-libs/alsa-lib](https://packages.gentoo.org/packages/media-libs/alsa-lib) and [media-sound/pulseaudio](https://packages.gentoo.org/packages/media-sound/pulseaudio) and with CFLAGS `-O1`<sup>[\[2\]](https://wiki.gentoo.org#cite_note-bugreport-O1-fixes-crash-2)</sup> and `-march=x86-64`.

- If the game crashes during gameplay on a system that has 8 or more logical cores and dmesg shows the following segfault:

[371471.978756] Civ5XP[2293]: segfault at 14 ip 000000000885bd5f sp 00000000882ff080 error 4
[371471.978762] Civ5XP[2292]: segfault at 0 ip 0000000008cd8534 sp 00000000e636afe0 error 4
[371471.978763]  in Civ5XP[8048000+22a7000]
[371471.978764]  in Civ5XP[8048000+22a7000]
[371471.978767] Code: 00 00 00 00 5b 81 c3 f2 ea 62 01 8b b4 24 88 00 00 00 8b bc 24 84 00 00 00 8b 94 24 80 00 00 00 0f b7 87 88 00 00 00 8b 4a 04 <8b>
2c 81 85 ed 0f 84 ef 00 00 00 8b 0a 8b 52 08 89 54 24 20 f3 0f
[371471.978768] Code: 44 24 20 c7 00 00 00 00 00 83 c4 0c 5e 5f 5b 5d c3 0f 0b 55 53 57 56 83 ec 0c e8 00 00 00 00 5b 8b 6c 24 2c 8b 44 24 24 8b 00 <8b>
70 14 8b 48 18 0f b7 d5 89 54 24 08 8d 14 11 8b 78 04 81 c3 ac

Try adding `taskset -c 0-3 %command%` to the launch options in `Library->Sid Meier's Civilization V->Properties->General->Set launch options...` so that the game only uses 4 physical CPU cores (and 8 threads, with hyper-threading)<sup>[\[10\]](https://wiki.gentoo.org#cite_note-10)</sup>. Note that the number of cores given as an argument to taskset depends on the system CPU, and it should be set so that the number of threads available isn't above 8.

## Starbound

- If the launcher fails to start with the following error:

This application failed to start because it could not find or load the Qt platform plugin "xcb".
Available platform plugins are: xcb.
Reinstalling the application may fix this problem.

Add `$(dirname %command%)/starbound` to the launch options in `Library->Starbound->Properties->General->Set launch options..`.

## Stealth Bastard Deluxe

- If the [media-fonts/font-misc-misc](https://packages.gentoo.org/packages/media-fonts/font-misc-misc) package is not installed, Stealth Bastard Deluxe will segfault<sup>[\[11\]](https://wiki.gentoo.org#cite_note-11)</sup>.

`root #``emerge --ask media-fonts/font-misc-misc`
Stealth Bastard Deluxe specifically requests the fonts `9x15`/`9x15b`, which can be checked for availability with [x11-apps/xlsfonts](https://packages.gentoo.org/packages/x11-apps/xlsfonts). Otherwise, add the fonts to the font path, or create a font alias:

**`/usr/share/fonts/misc/fonts.alias`**

## Stellaris

Stellaris requires [texture compression](https://wiki.gentoo.org/wiki/Steam/Games_troubleshooting#Texture_compression) support.

## Team Fortress 2

- TF2 may segfault on start if [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc) is not built with `-mstackrealign`. See [Half-Life 2](https://wiki.gentoo.org/wiki/Steam/Games_troubleshooting#Half-Life_2) section.
- If a black screen is visible for 1-2 seconds, add `-nojoy` to the launch options in `Library->Team Fortress 2->Properties->General->Set launch options..`.
- If [app-crypt/p11-kit](https://packages.gentoo.org/packages/app-crypt/p11-kit) is built with 32-bit support, Team Fortress 2 will segfault on start. The current workaround is to disable 32-bit ABI for this library<sup>[\[12\]](https://wiki.gentoo.org#cite_note-12)</sup>:

**`/etc/portage/package.use/steam`**

- If launching TF2 results in no audio, emerge [media-libs/libsdl2](https://packages.gentoo.org/packages/media-libs/libsdl2) with the `pulseaudio` USE flag.

## Terraforming Mars (or Windows-only games in general)

If Windows-only games crash without even displaying a window, and the Steam log include a line that begins:

wine: Unhandled exception 0x20474343 in thread..

the cause might be that the system Mesa library (on which Steam depends) is not compiled with Vulkan support. Follow the instructions on the [Vulkan](https://wiki.gentoo.org/wiki/Vulkan) page to rectify this.

## Terraria

- If you use [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio) and want to change the game's sound device output (switching from default output to bluetooth headset for example), see the [PulseAudio troubleshooting page](https://wiki.gentoo.org/wiki/PulseAudio/troubleshooting#In_pavucontrol.2C_unable_to_change_output_device_for_applications_that_use_OpenAL).

## The Witcher 2: Assassins of Kings

- If the game fails to start with the following terminal error after pressing "Launch game" in the launcher:

eON\_Core::init() - failed to initialise SDL2: SDL not built with haptic (force feedback) support
ERROR - eON failed to initialise!

Try to add `LD_PRELOAD=~/.local/share/Steam/ubuntu12_32/steam-runtime/amd64/usr/lib/i386-linux-gnu/libSDL2-2.0.so.0 %command%` to the launch options in `Library->The Witcher 2: Assassins of Kings->Properties->General->Set launch options...`.

## Overwatch 2

- If the game fails to launch then ensure the kernel is built with `CONFIG_CROSS_MEMORY_ATTACH` enabled.

## Transistor

### Incorrect sound card selected

Transistor uses the FMOD engine, which can sometimes detect the wrong default device. Determine the index of the card to be used with `aplay -l`'s output, then put that index into $HOME/.local/share/Transistor/FMODDriver.txt. For example, to use the first card detected by ALSA (index 0):

`user $``echo "0" > ~/.local/share/Transistor/FMODDriver.txt`
Relaunch Transistor and the chosen card should be outputting its sound.

## Unity-based games

- Many games that utilize the Unity3D engine released in late 2015 or later either display a black screen for a few seconds and segfault, or run but without sound<sup>[\[13\]](https://wiki.gentoo.org#cite_note-13)</sup>. To workaround this issue, disable the Steam runtime for the game by adding `LD_LIBRARY_PATH="" %command%` to the game's launch options. It may also be possible to run the game without Steam, but some games will force the use of Steam and keep failing.

- Other Unity-based games such as Hollow Knight or Mother Russia Bleeds will show the screen for two seconds, terminate, and then write to a log file located in /home/user/.config/unity3d/\<developer name>/\<game name>/Player.log. This error is caused by the OpenGL version not matching up to what the game is requesting. To work around this, add `MESA_GL_VERSION_OVERRIDE=3.3 MESA_GLSL_VERSION_OVERRIDE=330 %command%` to the game's launch options. This will override the OpenGL version for that particular game, allowing it to run.

- Some games, notably Wasteland 2 Director's Cut, might require PulseAudio to be started manually prior to launching the game, especially on desktop environments which do not have PulseAudio integrated. Torment: Tides of Numenera did not require PulseAudio to be started manually when the `LD_LIBRARY_PATH` method above was used.

- It might be that [Unity](https://wiki.gentoo.org/wiki/Unity) does not fall back to OpenGL, therefore it might be necessary to set the following Unity parameter as Steam launch option:

## War Thunder

If the game fails to start with the crash report dialog saying:

We are sorry, but something went wrong.

And terminal error:

double free or corruption (out)

Try to add `LD_PRELOAD=linux64/libsteam_api.so %command%` to the launch options in `Library->War Thunder->Properties->General->Set launch options...`.

## X<sup>3</sup>: Terran Conflict and X<sup>3</sup>: Albion Prelude

- If red, green and blue stripes are visible, or the launcher fails to start, enable [texture compression](https://wiki.gentoo.org/wiki/Steam/Games_troubleshooting#Texture_compression) support<sup>[\[14\]](https://wiki.gentoo.org#cite_note-14)</sup>.

## Yooka-Laylee

Yooka-Laylee will fail to start if a controller is connected (the game is only really playable with a controller).

`user $````
cd ~/.local/share/Steam/ubuntu12_32/steam-runtime/amd64/lib/x86_64-linux-gnu
```
`user $````
rm libudev.so.0
```
`user $````
ln -s /usr/lib/libudev.so libudev.so.0
```
## Proton: No audio with pipewire

If you do not receive audio output from games running in proton and see messages in your journal like `pipewire[2216]: spa.alsa: front:3: (125 missed) snd_pcm_avail after recover: Broken pipe` then (according to some assistance received in #pipewire) 'your system is too slow'.

As a workaround you can increase either the quantum or headroom.

To increase the quantum:

first, with no audio playing run the command below

`user $````
pw-metadata -n settings 0 clock.force-quantum 2048
```
Found "settings" metadata 30
set property: id:0 key:clock.force-quantum value:2048 type:(null)

If that works, set the changes permanently:

**`/etc/pipewire/pipewire.conf.d/01-inrease-quantum.conf`**

To increase the headroom:

Create a new file named `$HOME/.config/wireplumber/main.lua.d/51-custom.lua` with the following content:

**`$HOME/.config/wireplumber/main.lua.d/51-custom.lua`**

Then restart the daemons with: `systemctl --user restart pipewire{,-pulse}.socket` to apply a headroom of 64 samples to all of your audio devices.

## No sound in "Half-Life: Alyx"

The workaround is the same as for [Steam/Client troubleshooting](https://wiki.gentoo.org/wiki/Steam/Client_troubleshooting#No_sound_in_SteamVR_Home) but also move libSDL2-2.0.so.0 from hl alyx:

`user $``mv "$HOME/.local/share/Steam/steamapps/common/Half-Life Alyx/game/bin/linuxsteamrt64/libSDL2-2.0.so.0" ~/alyx-linuxsteamrt64-libSDL2-2.0.so.0`
and debugging advice is in "$HOME/.local/share/Steam/steamapps/common/Half-Life Alyx/game/hlvr.sh".

If it doesn't help you can try doing it with libSDL2-2.0.so.0 from $HOME/.local/share/Steam/steamapps/common/SteamLinuxRuntime\_sniper/..., $HOME/.local/share/Steam/steamapps/common/SteamLinuxRuntime\_soldier/... and $HOME/.local/share/Steam/ubuntu12\_32/steam-runtime/pinned\_libs\_64/.

Also [https://github.com/ValveSoftware/SteamVR-for-Linux/issues/524#issue-1292338878](https://github.com/ValveSoftware/SteamVR-for-Linux/issues/524#issue-1292338878) or combination of it and workaround described here can help.

## "Timed out waiting for response from Mongoose. Steam VR needs to be restarted" error in "Half-Life: Alyx" with valve index

Stop SteamVR. Without turning on valve index controllers, start SteamVR, start HL Alyx, wait till it loads to menu without this error. Then turn on the controllers.

## Proton: Out of memory crash (e.g Hogwarts Legacy)

Some games seem to need more mapped memory than is allowed per default

Increase it by creating a new file:[\[15\]](https://wiki.gentoo.org#cite_note-15)[\[16\]](https://wiki.gentoo.org#cite_note-16)

**`/etc/sysctl.d/99-max-map-count.conf`**

## Red Dead Redemption 2 and EA App

If the Rockstar Launcher throws "Game executable path not found. Please reinstall the game.", or the EA App says it can not connect to their server, make sure the following kernel feature is enabled:

**Enable process\_vm\_readv/writev syscalls (`CROSS_MEMORY_ATTACH`)**

League of Legends and Geometry Dash (to download songs) need this feature too.

## Ultrawide resolution not appearing in games running through [Wine](https://wiki.gentoo.org/wiki/Wine) and Xwayland

If you are using an ultrawide or other non standard Monitor as well as a secondary 16:9 monitor and ultrawide resolutions are not selectable in games running on wine through Xwayland, make sure your ultrawide monitor is set as the primary monitor for Xwayland:

`user $``xrandr`
and find your ultrawide/non standard monitor in the list, the use the ID to set it to be the primary monitor:

`user $``xrandr --verbose --output "Monitor-ID" --primary`
This has to be ran after every login.

## The game has a problem with no existing workaround after it was updated by steam

The game can be downgraded with workflow described here [Steam/Client troubleshooting](https://wiki.gentoo.org/wiki/Steam/Client_troubleshooting#SteamVR_doesn.27t_work_after_it_was_updated_.28as_steam_package.29_and_no_workaround_exists). AppID, DepotIDs, ChangelistIDs and paths should be replaced with proper values for this game.

## Counter-Strike 2 (and potentially other Source 2 games using SDL3)

If launching the game results in no audio when using PipeWire or Pulseaudio audio backends via specifying the environment variable SDL\_AUDIO\_DRIVER, ensure that [games-util/mangohud::guru](https://github.com/gentoo-mirror/guru/tree/master/games-util/mangohud) is not installed on the system.

## Proton with NTSync: The game fails to launch or completely freezes after launch

The recent Proton 11 release uses NTSync by default when the NTSync module is loaded, causing some games to fail to launch or freeze. It is due to the Linux kernel's default file descriptor limit of 4096, which is insufficient for many games. Raising this limit should resolve the issue. [\[17\]](https://wiki.gentoo.org#cite_note-17)

**`/etc/security/limits.d/30-proton.conf`**

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) Matt Turner. [Import libtxc\_dxtn's S3TC code into Mesa](https://lists.freedesktop.org/archives/mesa-dev/2017-October/171265.html), [The mesa-dev Archives](https://lists.freedesktop.org/archives/mesa-dev/), October 2nd, 2017. Retrieved on September 6th, 2018.
2. ↑ <sup>[2.0](https://wiki.gentoo.org#cite_ref-bugreport-O1-fixes-crash_2-0)</sup> <sup>[2.1](https://wiki.gentoo.org#cite_ref-bugreport-O1-fixes-crash_2-1)</sup> Rasmus Thomsen. [x11-libs/libxcb-1.12\[abi\_x86\_32\] optimizations above -O1 causing multiple applications to stop working (e.g. Civilization 5)](https://bugs.gentoo.org/616402), [Gentoo Bugzilla](https://bugs.gentoo.org/), April 23rd, 2017. Retrieved on October 14th, 2018.
3. [↑](https://wiki.gentoo.org#cite_ref-3) nativemad. [media-libs/libtxc required from DiRT Showdown](https://github.com/anyc/steam-overlay/issues/159), [Steam Overlay](https://github.com/anyc/steam-overlay), January 18th, 2016. Retrieved on January 21st, 2016.
4. [↑](https://wiki.gentoo.org#cite_ref-4) mahdi1234. [DRI\_PRIME=1 and DiRT Showdown won't launch due to opengl 4.1](https://forums.gentoo.org/viewtopic-t-1034742.html), [Gentoo Forums](https://forums.gentoo.org/), January 17th, 2016. Retrieved on January 21st, 2016.
5. [↑](https://wiki.gentoo.org#cite_ref-5) Arkady Rost. Black texture, [Dota 2 Linux and Mac client](https://github.com/ValveSoftware/Dota-2), August 14th, 2013. Retrieved on May 26th, 2015.
6. [↑](https://wiki.gentoo.org#cite_ref-6) michaelsudnick. Black ground texture on Gentoo amd64, radeonsi open source driver, [Dota 2 Linux and Mac client](https://github.com/ValveSoftware/Dota-2), July 12th, 2014. Retrieved on May 26th, 2015.
7. [↑](https://wiki.gentoo.org#cite_ref-7) proatx. Red-screen and no textures, [Dota 2 Linux and Mac client](https://github.com/ValveSoftware/Dota-2), December 16th, 2013. Retrieved on May 26th, 2015.
8. [↑](https://wiki.gentoo.org#cite_ref-8) ldaws011. [TF2 segfault on launch](https://github.com/ValveSoftware/Source-1-Games/issues/3885), [Source 1 Based Games](https://github.com/ValveSoftware/Source-1-Games), April 6, 2022. Retrieved on April 18, 2022.
9. [↑](https://wiki.gentoo.org#cite_ref-9) Nowaker. [Linux: blank/black screen after start - windowed mode maybe?](https://steamcommunity.com/app/8930/discussions/1/540744299777007287), [Sid Meier's Civilization V Steam Community](https://steamcommunity.com/app/8930), June 12th, 2014. Retrieved on May 28th, 2015.
10. [↑](https://wiki.gentoo.org#cite_ref-10) jqpdev. [New patch needed to fix segfaults in Civ 5 Linux client for CPUs with more than 8 logical cores](https://steamcommunity.com/app/8930/discussions/0/1693788384127278334/), [Sid Meier's Civilization V Steam Community](https://steamcommunity.com/app/8930), February 23rd, 2018. Retrieved on May 30th, 2019.
11. [↑](https://wiki.gentoo.org#cite_ref-11) Dirk Meijer. [Segmentation Fault in Linux](https://steamcommunity.com/app/209190/discussions/0/810923580565231302), [Stealth Bastard Deluxe Steam Community](https://steamcommunity.com/app/209190), May 3rd, 2013. Retrieved on May 27th, 2015.
12. [↑](https://wiki.gentoo.org#cite_ref-12) netfab. [TF2 segfaults on start](https://github.com/ValveSoftware/Source-1-Games/issues/2520), [Source 1 Based Games](https://github.com/ValveSoftware/Source-1-Games), February 11th, 2018. Retrieved on October 26th, 2018.
13. [↑](https://wiki.gentoo.org#cite_ref-13) ambidot. [Several Unity games segfault at "FMOD failed to get number of drivers" when PulseAudio isn't running](https://forum.unity3d.com/threads/several-unity-games-segfault-at-fmod-failed-to-get-number-of-drivers-when-pulseaudio-isnt-running.369943/), [Unity Forums](https://forum.unity3d.com/), November 25th, 2015. Retrieved on May 2nd, 2016.
14. [↑](https://wiki.gentoo.org#cite_ref-14) timon37. [X³: TC and AP - Linux support thread](https://forum.egosoft.com/viewtopic.php?t=335500), [X Universe Forums](https://forum.egosoft.com), April 13th, 2013. Retrieved on May 26th, 2015.
15. [↑](https://wiki.gentoo.org#cite_ref-15) Blisto91 [\[1\]](https://github.com/ValveSoftware/Proton/issues/6510), [Hogwarts Legacy Compatibility Report](https://github.com/ValveSoftware/Proton/), February 8th, 2023. Retrieved February 20th 2023
16. [↑](https://wiki.gentoo.org#cite_ref-16) Radish, [Proton Wiki: Requirements - Increasing The Maximum Number Of Memory Map Areas A Process May Have](https://github.com/ValveSoftware/Proton/wiki/Requirements#increasing-the-maximum-number-of-memory-map-areas-a-process-may-have,), June 2nd, 2023. Retrieved February 24th 2026
17. [↑](https://wiki.gentoo.org#cite_ref-17) Radish, [Proton Wiki: File Descriptors](https://github.com/ValveSoftware/Proton/wiki/File-Descriptors,), July 29th, 2024. Retrieved April 24th 2026
