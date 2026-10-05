<!-- source: https://wiki.gentoo.org/wiki/NVIDIA/Bumblebee | group: Gentoo Wiki (Main) | wiki-title: NVIDIA/Bumblebee -->
---
title: NVIDIA/Bumblebee
url: https://wiki.gentoo.org/wiki/NVIDIA/Bumblebee
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-19"
fingerprint: a64c137e4c3a8de7
license: CC BY-SA 4.0
---

# NVIDIA/Bumblebee

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

Bumblebee is an open source implementation of [NVIDIA Optimus](https://wiki.gentoo.org/wiki/NVIDIA/Optimus).

This is a guide to configuring a Gentoo system to make use of Hybrid Graphics using:

- The proprietary NVIDIA graphics drivers
- The Intel iGPU drivers
- Bumblebee
- bbswitch for hardware switching
- Primus for bridging

This configuration prefers Primus over Optirun/VGL.

## Pre Installation

### NVIDIA Optimus support

This is pretty much straightforward. If you have an Intel processor (2nd Gen +) and a NVIDIA GPU, then you probably have Optimus on your laptop. To check for sure, run

`root #``lspci -k | grep -i -e vga -e nvidia`
00:02.0 VGA compatible controller: Intel Corporation Device 591b (rev 04)
01:00.0 3D controller: NVIDIA Corporation GM107GLM \[Quadro M1200 Mobile\] (rev a2)

and look for VGA or 3D controllers to see your present GPUs. If you don't have an Optimus system, this guide will do nothing for you.

## Installation

### NVIDIA drivers

First configure your system to use [NVIDIA's proprietary drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) instead of the [open source Nouveau drivers](https://wiki.gentoo.org/wiki/Nouveau) and install the drivers. This is already detailed elsewhere on the wiki and is not repeated here.

### Intel drivers

Also configure and install the Intel drivers according to [the wiki](https://wiki.gentoo.org/wiki/Intel). DDX or modesetting both ought to work.

### Kernel

Ensure that you system can load and unload modules:

**Linux kernel 4.3.3+**

### Configuring /etc/portage/make.conf

#### Video cards

Set your video card variables like so:

**`/etc/portage/make.conf`**

```
VIDEO_CARDS="intel i965 nvidia"
...
```
#### USE flags

Now update your @world set:

`root #``emerge --ask --update --deep --newuse @world`
Then reboot your system

`root #``shutdown -r now`
### Installing Bumblebee, bbswitch, Primus

Emerge the packages:

`root #``emerge --ask sys-power/bbswitch x11-misc/bumblebee x11-misc/primus`
## Configuration

### User access to Bumblebee

Make sure to add your user to the video and bumblebee groups to allow use of bumblebee:

`root #``usermod -a -G video,bumblebee USER`
(Replacing USER with your username)

### Configuring /etc/init.d/bumblebee

By default, the OpenRC bumblebee module is set to depend on VGL and XDM. We are using primus over VGL. The XDM dependency can cause problems if this configuration is being setup prior to the X server. Remove the following lines (which should be near the start of the file):

**`/etc/init.d/bumblebee`**

```
() {
    need display-manager
    need vgl
}
...
```
Your final file should look something like this:

**`/etc/init.d/bumblebee`**

```
PIDFILE="${PIDFILE:-/var/run/bumblebee.pid}"
start() {
    ebegin "Starting BumbleBee Daemon"
        start-stop-daemon -S -p "${PIDFILE}" -x /usr/sbin/bumblebeed -- -D ${BUMBLEBEE_EXTRA_OPTS} --pidfile "${PIDFILE}"
    eend $?
}
stop() {
    ebegin "Stopping BumbleBee Daemon"
        start-stop-daemon -K -p "${PIDFILE}" -R SIGTERM/10
    eend $?
}
```
The important part is that it no longer has any dependencies.

### Configuring /etc/bumblebee/bumblebee.conf

The following settings need to be present in the /etc/bumblebee/bumblebee.conf file.

**`/etc/bumblebee/bumblebee.conf`**

### OpenRC

Now enable Bumblebe to load at runlevel `default`

`root #``rc-update add bumblebee default`
And once again, reboot the system:

`root #``shutdown -r now`
### Configuring /etc/modprobe.d/nvidia-rmmod.conf

Earlier, we disabled the `kms` and `uvm` USE flags for the NVIDIA drivers. This means we should only have the module `nvidia` present and not `nvidia-uvm`, `nvidia-drm`, or `nvidia-modeset`.
In order that modprobe -r nvidia functions correctly, we need it to remove only `nvidia` and nothing else. Remove all other modules from /etc/modprobe.d/nvidia-rmmod.conf so that it changes from looking like this:

**`/etc/modprobe.d/nvidia-rmmod.conf`**

to this:

**`/etc/modprobe.d/nvidia-rmmod.conf`**

## Post installation

### Check if Bumblebee is working

#### Using glxgears

Install the [x11-apps/mesa-progs](https://packages.gentoo.org/packages/x11-apps/mesa-progs) package to access glxgears:

`root #``emerge --ask x11-apps/mesa-progs`
Before we run our test, make sure the `nvidia` module is ***not*** loaded:

`root #``lsmod`
Module    Size   Used by
bbswitch  5461   0

And check the status of bbswitch using:

`root #``cat /proc/acpi/bbswitch`
0000:01:00.0 OFF

As you can see:

- bbswitch is loaded
- The `nvidia` module is ***not*** loaded
- The NVIDIA GPU (at PCI location 01:00.0) is ***off***

Now let’s check if the NVIDIA card will be switched ***on*** and the `nvidia` module will be loaded once optirun/primusrun are executed, and if it’ll switch ***off*** and the `nvidia` module will be unloaded once optirun/primusrun finish executing.

From within an Xorg Session:

`user $``optirun glxgears`
or (but not both at once):

`user $``primusrun glxgears`
A window showing glxgears should open. While it's running, check:

`root #``lsmod`
Module        Size   Used by
nvidia    10652360   51
bbswitch      5461   0

`root #``cat /proc/acpi/bbswitch`
0000:01:00.0 ON

This shows that the NVIDIA GPU is now on, its module loaded, and OpenGL is using it as expected. Once you end the optirun or primusrun command and exit glxgears, the previous checks run again should show the GPU off and the module unloaded.

#### Using glxinfo

Alternatively, glxinfo can be used to see if bumblebee is working:

`root #``glxinfo | grep OpenGL`
OpenGL vendor string: Intel Open Source Technology Center
OpenGL renderer string: Intel Mesa DRI (R) Skylake Halo GT2
OpenGL core profile version string: 3.3 (Core Profile) Mesa 11.0.6
OpenGL core profile shading language version string: 3.30
OpenGL core profile context flags: (none)
OpenGL core profile profile mask: core profile
OpenGL core profile extensions:
OpenGL version string: 3.0 Mesa 11.0.6
OpenGL shading language version string: 1.30

`root #``primusrun glxinfo | grep OpenGL`
OpenGL vendor string: NVIDIA Corporation
OpenGL renderer string: GeForce GTX 960M / PCIe / SSE2
OpenGL core profile version string: 4.4.0 NVIDIA 361.28
OpenGL core profile shading language version string: 4.40 NVIDIA via Cg compiler
OpenGL core profile context flags: (none)
OpenGL core profile profile mask: core profile
OpenGL core profile extensions:
OpenGL version string: 4.5.0 NVIDIA 361.28
OpenGL shading language version string: 4.50 NVIDIA

As you can see, running the command without primusrun runs with the iGPU but with primusrun runs with the NVIDIA GPU.

## Using Bumblebee with Steam games

In order to run a [Steam](https://wiki.gentoo.org/wiki/Steam) game with optirun or primusrun, you need to

- right click on the game within steam
- select "properties"
- "Set Launch Options"
- add either `optirun %command%` or `primusrun %command%` to the text field.

## Using the Bumblebee overlay

Some users cannot get this configuration to work and it is suggested to use the Bumblebee Overlay to emerge Bumblebee and Primus.

First add the bumblebee overlay via [eselect repository](https://wiki.gentoo.org/wiki/Eselect/Repository):

`root #````
eselect repository enable bumblebee
```
`root #````
emerge --sync bumblebee
```
Then unmask Bumblebee and Primus:

`root #``echo "= x11-misc / bumblebee-9999 ~ amd64" >> /etc/portage/package.accept_keywords/bumblebee``root #````
echo "= x11-misc / primus-9999 ~ amd64" >> /etc/portage/package.accept_keywords/bumblebee 
```
And mask the official Portage tree:

`root #``echo "x11-misc / bumblebee :: gentoo" >> /etc/portage/package.mask/bumblebee.gentoo`
Then emerge Primus:

`root #``emerge --ask x11-misc/primus`
## Configuring Xorg

Some systems require Xorg to be configured to use the Intel drivers. Do this according to the [Gentoo Intel Wiki Page](https://wiki.gentoo.org/wiki/Intel).

## Troubleshooting

### bbswitch refuses to turn off card

Try adding:

`root #``"acpi_osi=!Windows\x202013" acpi_osi=Linux nogpumanager i915.enable_hd_vgaarb=1 enable_hd_vgaarb=1`
to your kernel command-line (this has worked for a couple of users, especially those with Thinkpads and Lenovo laptops).

### Manual unloading

If the NVIDIA drivers are built with `kms` and/or `uvm` and a patched version of bumblebee then the bumblebee module will not be able to unload the `nvidia` module as it will be in use by the `nvidia-modeset` module which is itself in use by the `nvidia-drm` module. However, the modules cat be unloaded manually (using modprobe -r) and the relevant changes to /etc/modprobe.d/rmmod-nvidia.conf. The GPU can then be turned off using tee /proc/acpi/bbswitch\<\<\<OFFF. Some users have these two commands in a script accessible through a key command in their WM.

### System freezes when starting Xorg server

On some systems, it may be necessary to start Xorg with the `nvidia` module loaded and the discrete GPU turned on or else the system will hang. The module can then be unloaded and the GPU switched off.
In order to manually load the module, add `nvidia` to your modules configuration file:

**`/etc/modules-load.d/video.conf`**

The `nvidia` module must not be blacklisted.

### Bumblebee daemon doesn't start on boot

First check that bumblebee is configured in OpenRC:
Now enable Bumblebee to load at runlevel `default`

`root #``rc-update add bumblebee default`
If you still get errors indicating that the Bumblebee Daemon is not running when you attempt optirun and primusrun commands, it may be necessary to start the daemon manually on log-in. Put `exec bumblebee --daemon` into a suitable configuration file so that it will be automatically run when you start your X server. This might be in your .xinitrc or window manager (such as i3) config file depending on your setup.

### GPU doesn't automatically turn off for the first time

Some users may experience their bumblebee setup working in every respect except for that the GPU remains on until the first time a primusrun or optirun command is run. A workaround for this is to add `primusrun glxinfo` to your .xinitrc or other initializing config file (like your i3 config file).

### System hangs on any PCI call when the GPU is off

Some users may find that their setup works well but will experience random kernel crashes characterized by a complete system hang with no errors in any log files generated. If this is the case, it may be necessary to trick the acpi kernel module into thinking that the system is an earlier version of Windows by editing your grub configuration:

**`/etc/default/grub`**

The parameters `modprobe.blacklist=nvidia`, `nvidia.modeset=0`, and `pcie_port_pm=off` may also be necessary.

Then regenerate your grub config file:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
### Blacklisting the NVIDIA modules

On some systems it may be necessary to blacklist the `nvidia` modules. Edit your blacklist file as necessary:

**`/etc/modprobe.d/blacklist.conf`**

### Could not load GPU driver

If you tested with glxgears to see if Bumblebee is working and got the following output:

`user $``glxinfo | grep OpenGL`
primus: fatal: Bumblebee daemon reported: error: Could not load GPU driver

Restarting the GUI by using killall X && startx should fix the issue.

### Cannot access secondary GPU

If you run the optirun or primusrun and see the error `[ERROR] Cannot access secondary GPU - error: [XORG] (EE) /dev/dri/card0: failed to set DRM interface version 1.4: Permission denied`, include the following piece of configuration in the Bumblebee's xorg.conf.nvidia.

**`/etc/bumblebee/xorg.conf.nvidia`**

```
Section "Screen"
    Identifier "Default Screen"
    Device "DiscreteNvidia"
EndSection
```
You may need to restart the bumblebee service for this change to take effect

### libGL error: failed to load driver: swrast

If you get the following error when you run optirun:

`root #``optirun glxinfo` libGL error: No matching fbConfigs or visuals found
libGL error: failed to load driver: swrast
X Error of failed request:  GLXBadContext
  Major opcode of failed request:  150 (GLX)
  Minor opcode of failed request:  6 (X\_GLXIsDirect)
  Serial number of failed request:  49
  Current serial number in output stream:  48

The issue could be that Bumblebee cant find the NVIDIA libraries. Changing the `LibraryPath` in /etc/bumblebee.conf can fix the issue.

**`/etc/bumblebee/bumblebee.conf`**

### libGL errorː failed to load any of the libreariesː libGL.so.1

If you get the following error when you run primusrun:

`root #``primusrun glxinfo` primus: fatal: failed to load any of the libraries: /usr/lib64/opengl/nvidia/lib/libGL.so.1:/usr/lib/opengl/nvidia/lib/libGL.so.1
libGLX.so.0: cannot open shared object file: No such file or directory
/usr/lib/opengl/nvidia/lib/libGL.so.1: wrong ELF class: ELFCLASS32

Then check if the library really exists and linker does not see itː

`root #``ldd /usr/lib64/opengl/nvidia/lib/libGL.so` linux-vdso.so.1 (0x00007ffddbfa7000)
	libdl.so.2 => /lib64/libdl.so.2 (0x00007f851b6a7000)
	libGLX.so.0 => not found
	libGLdispatch.so.0 => not found
	libc.so.6 => /lib64/libc.so.6 (0x00007f851b4d7000)
	/lib64/ld-linux-x86-64.so.2 (0x00007f851b983000)

`root #``ls -la /usr/lib64/opengl/nvidia/lib/libGLX*` lrwxrwxrwx 1 root root      11 Nov 10 13:25 /usr/lib64/opengl/nvidia/lib/libGLX.so -> libGLX.so.0
-rwxr-xr-x 1 root root   65096 Nov 10 13:25 /usr/lib64/opengl/nvidia/lib/libGLX.so.0
lrwxrwxrwx 1 root root      23 Nov 10 13:25 /usr/lib64/opengl/nvidia/lib/libGLX\_nvidia.so -> libGLX\_nvidia.so.435.21
lrwxrwxrwx 1 root root      23 Nov 10 13:25 /usr/lib64/opengl/nvidia/lib/libGLX\_nvidia.so.0 -> libGLX\_nvidia.so.435.21
-rwxr-xr-x 1 root root 1147776 Nov 10 13:25 /usr/lib64/opengl/nvidia/lib/libGLX\_nvidia.so.435.21

After that you can fix it by adding the needed path to the ld.so.conf.d and reloading ldcache

**`/etc/ld.so.conf.d/60-fix-bumblebee.conf`**

`root #``rm /etc/ld.so.cache && ldconfig`
## Specific use cases

The following Wiki Pages make reference to Bumblebee and/or Bumblebee configurations:

## See also

- [NVIDIA](https://wiki.gentoo.org/wiki/NVIDIA) — a popular graphical chipset manufacturer.
- [NVIDIA/nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) — The [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers) package contains the *proprietary* graphics driver for [NVIDIA](https://wiki.gentoo.org/wiki/NVIDIA) graphic cards.
- [NVIDIA/Optimus](https://wiki.gentoo.org/wiki/NVIDIA/Optimus) — a proprietary technology that seamlessly switches between two GPUs.
