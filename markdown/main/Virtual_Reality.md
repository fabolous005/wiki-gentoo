<!-- source: https://wiki.gentoo.org/wiki/Virtual_Reality | group: Gentoo Wiki (Main) | wiki-title: Virtual Reality -->
---
title: Virtual Reality
url: https://wiki.gentoo.org/wiki/Virtual_Reality
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-11"
fingerprint: ee09961c51a61dd4
license: CC BY-SA 4.0
---

# Virtual Reality

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Virtual Reality (VR) is the process of creating a virtual, immersive experience for the user. This is achieved usually by a head-mounted display (HMD) which simulates a 3-dimensional vision by having a different display for each eye. An HMD usually features rotation as well as full movement tracking. With many VR solutions, motion controllers exist to provide an immersive way of input.

Many VR peripherals have been released, with very few featuring official Linux support and some having third-party Linux support (see compatibility chart below).

## VR standards

### OpenXR

[OpenXR](https://www.khronos.org/openxr/) is an open standard for Virtual Reality and Augmented Reality maintained by the Khronos Group.

### OpenVR

OpenVR is a de-facto standard for Virtual Reality maintained by Valve Corporation. It is used by most games and a great number of applications. Currently SteamVR provided by [Steam](https://wiki.gentoo.org/wiki/Steam) is the only fully working implementation of OpenVR.

### OSVR

OSVR was a joint effort by Sensics Inc and Razer to create an open API for VR. It is mostly abandoned, yet some applications rely on OSVR for virtual reality

## VR Hardware Setup

### Envision

[Envision](https://gitlab.com/gabmus/envision) is a UI for building, configuring and running Monado, the open source OpenXR runtime.

#### WMR Setup

##### Prerequisites

For Windows Mixed Reality headsets, Envision is recommended. As it is currently in alpha development, several steps need to be taken first.

steamvr vulkan nvidia opengl X wayland xwayland USE flags are enabled globally, although adding vulkan USE flag to media-libs/mesa may be all that is neccessary.

Add the guru repository. If you do not have eselect repository's installed,

`root #``emerge -av eselect-repositories`
Then select and update the guru repository.

`root #``eselect repository enable guru``root #``emerge --sync guru`
Then we can install openxr and other needed dependencies

`root #``emerge -av openxr-loader gui-libs/vte glew shaderc tbb`
Users of X11 will need to install x11-apps/xrandr

`root #``sudo emerge -av x11-apps/xrandr`
As envision hasn't been updated to use Catch v3, \*you may need to unmerge v3 if you already have it installed.

`user $``cd Catch2` `user $``git switch v2.x` `user $``cmake -Bbuild -H. -DBUILD_TESTING=OFF` `root #``cmake --build build/ --target install` Then install xr-hardware udev rules.

`user $``cd xr-hardware``user $``make``root #` `make install`
Nvidia users will need vulkan-layers

`user $``cd vulkan-layers``user $``cmake -G Ninja -B build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_SHARED_LIBS=ON``user $``ninja -C build``root #``ninja -C build install`
Then reboot for the changes to take effect.

##### Install

Clone install, and run envision.

`user $``cd envision``user $``meson setup build -Dprefix="$PWD/build/localprefix" -Dprofile=development``user $``ninja -C build``user $``ninja -C build install``user $``./build/localprefix/bin/envision`
##### Configuration

Select your headset. For this guide, we are using WMR - Envision Default click duplicate profile (between pen and + icon at the bottom) Name it. For Samsung Odyssey +(probably Odyssey as well) add

WMR\_RIGHT\_DISPLAY\_VIEW\_Y\_OFFSET=40

to environment variables. Towards the bottom, on the left side, you'll see a plus sign beside Enviroment variables. Other headsets may require other variables.

Save your profile, and then select build profile from the envision menu at the top left(3 lines) Once it has finished building, a pop up asking for a setcap command that requires root privileges will appear, so enter your password and click yes. Then you can click close, and plug in your headset if it's not already plugged in. X11 users will have to run

`user $``xrandr | grep connected` and the replace "HDMI-0" on the following line, with their headset port.

`user $``xrandr --output HDMI-0 --set non-desktop 1`
Wayland users do not need to do anything else Click start, and now the headset should light up. You can go ahead and start any steam game with openxr.

For SteamVR

Copy the monado plugin to /usr/share

`user $``cd /home/$USER/.local/share/Steam/steamapps/common/SteamVR/bin``root #``cp -r /home/$USER/.local/share/envision/prefixes/PREFIXDIRECTORY/share/steamvr-monado /usr/share/` `user $``./vrpathreg.sh adddriver /usr/share/steamvr-monado/`
then you can start room setup with

`user $``./vrhelper-startup.sh`
and after words you can run steamvr via

`user $``./vrmonitor.sh`
##### Troubleshooting

Sometimes starts with black screen, and it can not be closed.

Ctrl-Super-Esc and then clicking on it will kill the zombie window

The Envision client won't connect to the headset anymore but was working before.

`user $` `rm /run/user/1000/monado_comp_ipc`
Resetting the pc usually works if that doesn't. Logging out and logging back sometimes works.

### Monado

[Monado](https://gitlab.freedesktop.org/monado/monado/) is an open source XR runtime delivering immersive experiences such as VR
and AR on mobile, PC/desktop, and any other device (because gosh darn people come up with a lot of weird hardware). Monado aims to be a complete and conforming implementation of the OpenXR API made by Khronos. The project is primarily developed on GNU/Linux, but also runs on Android and Windows. "Monado" has no specific meaning and is just a name.

### ALVR

[ALVR](https://github.com/alvr-org/ALVR) is a solution to use Android-based VR-headsets like the Meta Quest (2/3/Pro) or the Pico (3/4) with SteamVR(OpenVR).

### WiVRn

[WiVRn](https://github.com/Meumeu/WiVRn) is a solution to use Android-based VR-headsets like the Meta Quest (2/3/Pro) or the Pico 3/4 through OpenXR on your computer

### OpenHMD

[OpenHMD](http://www.openhmd.net) aims to provide a Free and Open Source API and drivers for immersive technology, such as head mounted displays with built in head tracking. Our aim is to implement support for as many devices as possible in a portable, cross-platform package. It is currently unmaintained.

## VR hardware

This is an overview of the support status of VR hardware. Official support means that Linux support is done by the manufacturer and not by a third party project.

| Device | Supported | Official Support | Rotation | Position | Controller | OpenVR | OpenXR | OSVR | 
|---|---|---|---|---|---|---|---|---|
| HTC Vive |  |  |  |  |  |  |  |  | 
| Valve Index |  |  |  |  |  |  |  |  | 
| HTC Vive Pro |  |  |  |  |  |  |  |  | 
| HTC Vive Pro 2 |  | [Third-party SteamVR driver available](https://github.com/CertainLach/VivePro2-Linux-Driver) |  |  |  |  |  |  | 
| Oculus Rift DK1 |  | [Third-party support through OpenHMD](http://www.openhmd.net/) |  | n/a |  |  |  | [Yes (via OSVR-OpenHMD)](https://github.com/simlrh/OSVR-OpenHMD) | 
| Oculus Rift DK2 |  | [Third-party support through OpenHMD](http://www.openhmd.net/) |  |  |  |  |  | [Yes (via OSVR-OpenHMD)](https://github.com/simlrh/OSVR-OpenHMD) | 
| Oculus Rift CV1 |  | [Third-party support through OpenHMD](http://www.openhmd.net/) |  |  |  |  |  |  | 
| Playstation VR |  | [Third-party support through OpenHMD](http://www.openhmd.net/) |  |  |  |  |  |  | 
| Oculus Quest |  | [Third-party support through ALVR](https://github.com/alvr-org/ALVR) |  |  |  |  | [ALXR](https://github.com/korejan/ALVR/wiki/ALXR-Client) |  | 
| Oculus/Meta Quest 2 |  | [Third-party support through ALVR](https://github.com/alvr-org/ALVR) |  |  |  |  | [ALXR](https://github.com/korejan/ALVR/wiki/ALXR-Client) |  | 
| OSVR HDK |  |  |  |  | n/a |  |  |  | 
| OSVR HDK2 |  |  |  |  | n/a |  |  |  | 
| Windows Mixed Reality Headsets (like HP Reverb) |  | [Third-party support through Monado](https://gitlab.freedesktop.org/monado/monado) |  |  | [Experimental](https://gitlab.freedesktop.org/thaytan/monado) via Thaytan's branch |  |  |  |
