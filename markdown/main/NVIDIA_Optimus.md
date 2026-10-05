<!-- source: https://wiki.gentoo.org/wiki/NVIDIA/Optimus | group: Gentoo Wiki (Main) | wiki-title: NVIDIA/Optimus -->
---
title: NVIDIA/Optimus
url: https://wiki.gentoo.org/wiki/NVIDIA/Optimus
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-19"
fingerprint: "96019e7a38ffbd8d"
license: CC BY-SA 4.0
---

# NVIDIA/Optimus

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

**NVIDIA Optimus** is a proprietary technology that seamlessly switches between two GPUs. It is typically used on systems that have an integrated [Intel](https://wiki.gentoo.org/wiki/Intel) GPU and a discrete [NVIDIA](https://wiki.gentoo.org/wiki/NVIDIA) GPU. The main benefit of using NVIDIA Optimus is to extend battery life by providing maximum GPU performance only when needed.

For an open source implementation of NVIDIA Optimus, see [Bumblebee](https://wiki.gentoo.org/wiki/NVIDIA/Bumblebee).

## Installation

### Kernel

Since NVIDIA Optimus will be using the integrated Intel graphics for modesetting, the following kernel options will need to be enabled:

**Linux kernel 4.3.3+**

### USE flags


| [+X](https://packages.gentoo.org/useflags/+X) | Add support for X11 | 
| [+modules](https://packages.gentoo.org/useflags/+modules) | Build the kernel modules | 
| [+static-libs](https://packages.gentoo.org/useflags/+static-libs) | Install the XNVCtrl static library for accessing sensors and other features | 
| [+strip](https://packages.gentoo.org/useflags/+strip) | Allow symbol stripping to be performed by the ebuild for special files | 
| [+tools](https://packages.gentoo.org/useflags/+tools) | Install additional tools such as nvidia-settings | 
| [dist-kernel](https://packages.gentoo.org/useflags/dist-kernel) | Enable subslot rebuilds on Distribution Kernel upgrades | 
| [kernel-open](https://packages.gentoo.org/useflags/kernel-open) | Use the open source variant of the drivers (only works for Turing/Ampere or newer GPUs, aka GTX 1650+ -- recommended with >=560.xx drivers if usable and is \*required\* for 50xx Blackwell or newer GPUs -- always-enabled regardless of USE in >=595.xx) | 
| [modules-compress](https://packages.gentoo.org/useflags/modules-compress) | Install compressed kernel modules (if kernel config enables module compression) | 
| [modules-sign](https://packages.gentoo.org/useflags/modules-sign) | Cryptographically sign installed kernel modules (requires CONFIG\_MODULE\_SIG=y in the kernel) | 
| [persistenced](https://packages.gentoo.org/useflags/persistenced) | Install the persistence daemon for keeping devices state when unused (e.g. for headless) | 
| [powerd](https://packages.gentoo.org/useflags/powerd) | Install the NVIDIA dynamic boost support daemon (only useful with specific laptops, ignore if unsure) | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 

### Emerge

Installing NVIDIA drivers is easy, run the following:

`root #``emerge --ask x11-drivers/nvidia-drivers`
Also, Make sure [Xrandr](https://wiki.gentoo.org/wiki/Xrandr) is installed, as it is needed later in the setup (it is always pulled in automatically). If not, emerge it as well:

`root #``emerge --ask x11-apps/xrandr`
## Configuration

Configuring a system to use NVIDIA's proprietary driver is not easy as the installation. There are several configuration files that will need to be modified in order for a system to work properly.

### Kernel modules

If the user has chosen to not use built-in modules, then the init system should load the necessary modules on system boot. If /proc/config.gz (or /boot/config-\*-gentoo) is available, this can verified by running the following command:

`user $``zgrep "CONFIG_MODULES=" /proc/config.gz`
If the output returns `CONFIG_MODULES` set to `N`, then the kernel will need recompiled with support to load modules. Information about that can be found over here. After module loading support has been added, return to this article and continue reading.

Create a new file called nvidia.conf in the /etc/modules-load.d directory. It should contain the NVIDIA module name:

**`/etc/modules-load.d/nvidia.conf`**

#### OpenRC

Verify the modules init script has been added to the boot runlevel (it should be by default, but double check):

`root #``rc-update add modules boot`
The output should look like:

\* rc-update: modules already installed in runlevel \`boot'; skipping

#### Systemd

Check the status of the systemd-modules-load.service to verify things are running smoothly. If issues arise this service unit will be the place to check:

`root #``systemctl start systemd-modules-load.service`
### xorg.conf

The best way to set the system's xorg.conf correctly would be to read the documentation NVIDIA has provided. The documentation can be found in a couple of locations. To save time, consider reading only the pages on Optimus and XRandR, as they are vital to correct configuration. If the driver has already been emerged (done in the installation step above), the documentation can be found locally at /usr/share/doc/nvidia-drivers-\*/README.bz2.

Example: Use the less command to read the local documentation:

`user $``less /usr/share/doc/nvidia-drivers-*/README.bz2`
It is also possible to read the documentation at NVIDIA's website by following these (external) links:

Replace 343.36 with your version of nvidia-drivers e.g. 390.42 to get the most suitable configuration.

For a quick example here on the wiki, view [this xorg.conf file](https://wiki.gentoo.org/wiki/NVIDIA/Optimus/xorg.conf).

#### How to find BusID

Invoke lspci the BusID is at the beginning of each device:

01:00.0 3D controller: NVIDIA Corporation GK104M \[GeForce GTX 870M\] (rev a1)

Where `01:00.0` is BusID.

#### Automatic Xorg.conf Configuration

The driver comes with an automatic tool to create an appropriate Xorg.conf for using Optimus. If you have a custom xorg.conf, it is prudent to create a backup just in case (although the tool makes a backup of its own).

`root #``nvidia-xconfig --prime`
### Using a specific monitor via EDID

It is probably best to first try a simple configuration first like described in the NVIDIA driver manual:

#### Saving the monitor's EDID

Some laptops/notebooks may benefit from saving the EDID screen information to a file so it can be passed to the Intel modesetting driver. The EDID information can be saved using the read-edid utility.

`root #``emerge --ask x11-misc/read-edid``root #``mkdir -p /lib/firmware/edid``root #``get-edid > /lib/firmware/edid/1920x1080_Clevo_W670SR.bin`
The EDID information is provided to the Intel GPU (Graphics Processing Unit) by specifying its location in the kernel boot parameter:

`drm_kms_helper.edid_firmware=edid/1920x1080_clevo_W670SR.bin`

If the GRUB2 bootloader is being used, this can be configured in the file /etc/default/grub

**`/etc/default/grub`**

```
GRUB_CMDLINE_LINUX_DEFAULT="drm_kms_helper.edid_firmware=edid/1920x1080_clevo_W670SR.bin"
GRUB_GFXMODE=1920x1080
```
Note: If using Sabayon Linux, the kernel boot parameters should be specified in the /etc/default/sabayon-grub file instead of /etc/default/grub file.

#### Example xorg.conf for EDID

See [EDID xorg.conf Example](https://wiki.gentoo.org/wiki/NVIDIA/Optimus/EDID_Xorg.conf_Example) to view an example xorg.conf using an EDID for a specific monitor.

### Before starting X

Per NVIDIA's [instructions](http://us.download.nvidia.com/XFree86/Linux-x86/346.22/README/randr14.html), the following commands are required before starting X:

This is to say any Display Manager that starts X-Windows then asks the user to log in ***will*** result in a black screen unless the above xrandr commands are run *before* asking the user to log in.

NOTE: If you get a black screen with no back-lighting from the previous steps, creating .xsessionrc and placing the xinitrc commands in there COULD fix it.

Use the xrandr command to find the appropriate graphics device:

`root #``xrandr --listproviders`
### Display manager configuration

The following shows a list of where to add the required xrandr commands, sorted by desktop.

#### Qingy

Add the xrandr commands to the end of the /etc/X11/Sessions/KDE-4 file:

**`/etc/X11/Sessions/KDE-4`**

**KDE-4's X session file**

```
 --setprovideroutputsource modesetting NVIDIA-0
xrandr --auto
```
Add the xrandr commands to the end of the \~/.xsession file.

##### Qingy DirectFB

In the /etc/directfbrc configuration file. It is necessary to set the `busid` variable to the BusID of the Intel graphics card as reported by the lspci command:

`root #``lspci | grep VGA`
For example, if lspci says the Intel graphics card is on BusID 00:02.0, then add the following line to /etc/directfbrc

**`/etc/directfbrc`**

```
busid=0:02:0
```
#### The Console Display Manager (CDM)

Add the **xrandr** commands to \~/.xinitrc file:

#### Simple Desktop Display Manager (SDDM)

First, edit the sddm configuration to have it look for the commands:

**`/etc/sddm.conf`**

```
[X11]
DisplayCommand=/etc/sddm/scripts/Xsetup
```
Next create the directory /etc/sddm/scripts

`root #``mkdir -p /etc/sddm/scripts`
.

Then, Add the xrandr commands to the /etc/sddm/scripts/Xsetup file

**`/etc/sddm/scripts/Xsetup`**

```
#!/bin/sh
xrandr --setprovideroutputsource modesetting NVIDIA-0
xrandr --auto
```
Finally set execute permissions on the file /etc/sddm/scripts/Xsetup.

`root #``chmod a+x /etc/sddm/scripts/Xsetup`
#### Mint Desktop Manager (MDM)

Add the xrandr commands to the /etc/X11/mdm/Init/Default file:

#### X Display Manager (XDM)

**2020-07-28**, the information in this section is probably

**outdated**. You can help the Gentoo community by verifying and

[updating this section](https://wiki.gentoo.org/index.php?title=NVIDIA/Optimus&action=edit).

Add the xrandr commands to the /usr/lib/X11/xdm/Xsetup\_0 file and protect them like described above.

**NOTE:** if the system is a 32-bit system, add the commands to the /usr/lib64/X11/xdm/Xsetup\_0 file.

If using a 64-bit system, edit the /etc/X11/xdm/xdm-config configuration file and change the following line to point to the Xsetup\_0 file created above:

**`/etc/X11/xdm/xdm-config`**

**X Display Manager Example**

#### LXDE Display Manager (LXDM)

Add the following lines to /etc/lxdm/LoginReady:

**`/etc/lxdm/LoginReady`**

**LXDE Display Manager Example**

```
 --setprovideroutputsource modesetting NVIDIA-0
xrandr --auto
```
#### Gnome Display Manager (GDM)

Create two .desktop files:

**`/etc/xdg/autostart/optimus.desktop & /usr/share/gdm/greeter/autostart/optimus.desktop`**

**Desktop Entries**

```
[Desktop Entry]
Type=Application
Name=Optimus
Exec=sh -c "xrandr --setprovideroutputsource modesetting NVIDIA-0; xrandr --auto"
NoDisplay=true
X-GNOME-Autostart-Phase=DisplayServer
```
Make sure that GDM uses X as default backend (this does only affect [gnome-base/gdm](https://packages.gentoo.org/packages/gnome-base/gdm). [gnome-base/gnome-shell](https://packages.gentoo.org/packages/gnome-base/gnome-shell) will still use wayland when USE="wayland" is enabled):

**`/etc/gdm/custom.conf`**

**custom.conf**

```
# GDM configuration storage
[daemon]
# Uncoment the line below to force the login screen to use Xorg
WaylandEnable=false
[security]
[xdmcp]
[chooser]
[debug]
# Uncomment the line below to turn on debugging
#Enable=true
```
#### LightDM

Prepare a script in /usr/local/bin/:

**`/usr/local/bin/prepare-optimus.sh`**

```
#!/bin/bash
xrandr --setprovideroutputsource modesetting NVIDIA-0
xrandr --auto
# Run the rest of the arguments.
# -
$($@)
```
Make it executable and update lightdm.conf with the location of that script:

**`/etc/lightdm/lightdm.conf`**

## Troubleshooting

Since there are many files to configure and because the NVIDIA's proprietary support for Optimus in Linux is buggy, it is rather easy to create a faulty Optimus configuration. It is possible something was typed incorrectly, or a certain configuration was not compatible with the hardware being used. Whatever the case, a broken configuration means that debugging is required.

To debug, carefully read the logs from dmesg (/var/log/dmesg) and Xorg (/var/log/Xorg.0.log) with a favorite text editor; they are the best indicators to find issues. If something irregular is discovered, make changes to the respective configuration files. Other areas to inspect for debugging include any of the configuration files that were modified through the course of this article (the kernel's {Path|.config}}, kernel boot parameters passed at /etc/default/grub, the Xorg's /etc/X11/xorg.conf file, etc.). Continue checking the files as necessary then reboot the system and try again. Many attempts may be required in order to obtain a working configuration! It is not exciting process; time *could* be spent on something more interesting, but if debugging is required in order to get Optimus working then it needs to happen.

To aid in distinguishing between important and unimportant messages in /var/log/dmesg and /var/log/Xorg.0.log files, working examples have been provided at these sub-articles:

### Specific models

- [Lenovo Thinkpad W530](https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_W530) - In short, for all the screens to work, set the configuration to discrete mode in the motherboard firmware.

### D-Bus

### Xorg


In case Xorg fails immediately after boot but works fine when launched later, it might be due to a race condition in which the Intel driver doesn't load in time and Xorg complains that there is no /dev/dri/card0. In that case you should load the Intel driver to the initramfs:

**`/etc/dracut.conf.d/nvidiaoptimus.conf`**

Regenerate the initramfs image:

`root #``dracut --force`
## See also

- [Nouveau & nvidia-drivers switching](https://wiki.gentoo.org/wiki/Nouveau_%26_nvidia-drivers_switching) — describes how to switch between [NVIDIA's binary driver](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) and the open source [nouveau](https://wiki.gentoo.org/wiki/Nouveau) driver.
- [NVIDIA/nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) — The [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers) package contains the *proprietary* graphics driver for [NVIDIA](https://wiki.gentoo.org/wiki/NVIDIA) graphic cards.
