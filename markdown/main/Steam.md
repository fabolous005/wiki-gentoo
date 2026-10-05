<!-- source: https://wiki.gentoo.org/wiki/Steam | group: Gentoo Wiki (Main) | wiki-title: Steam -->
---
title: Steam
url: https://wiki.gentoo.org/wiki/Steam
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-31"
fingerprint: "9c5bbc5e1e02784c"
license: CC BY-SA 4.0
---

# Steam

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Steam** is a video game digital distribution service by Valve. Steam offers digital rights management (DRM), matchmaking servers, video streaming, and social networking services. It also provides the user with installation and automatic updating of games, and community features such as friends lists and groups, cloud saving, and in-game voice and chat functionality.

The Steam client is not open source software, therefore each user must accept the [Steam Subscriber Agreement](https://store.steampowered.com/subscriber_agreement/) before using the software, then typically accept [EULAs](https://en.wikipedia.org/wiki/End-user_license_agreement) or [Terms of Use](https://en.wikipedia.org/wiki/Terms_of_service) agreements for each particular title accessed through the Steam client.

Valve Corporation has collaborated with open source software organizations such as CodeWeavers ([Wine](https://wiki.gentoo.org/wiki/Wine))<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> in order to make closed source games possible to run on open source operating systems.

## Game Compatibility

With the popularization of the [Steam Deck](https://en.wikipedia.org/wiki/Steam_Deck), playing Steam games on Linux has grown considerably in recent years<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>. Game developers are increasingly incentivized to support Linux. Native Linux games can be identified with the SteamOS icon in the Store. For games without native support, Valve maintains [Proton](https://github.com/ValveSoftware/Proton/), built on [Wine](https://wiki.gentoo.org/wiki/Wine), which integrates with the client and provides an easy-to-use compatibility layer for Windows-only games on a recent Linux OS. Users who prefer bleeding-edge versions over stability may consider an actively community-maintained fork of Proton called [GE-Proton](https://github.com/GloriousEggroll/proton-ge-custom).

## Prerequisites

Steam provides 32-bit environment for most of supported games, so client itself requires a [multilib](https://wiki.gentoo.org/wiki/Multilib) [profile](<https://wiki.gentoo.org/wiki/Profile_(Portage)>) on **amd64**. That is, during Gentoo installation, when choosing profiles the [no-multilib](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation#No-multilib) option was **not** selected. This prerequisite can be ignored if installing Steam in a [chroot](https://wiki.gentoo.org/wiki/Steam#Chroot).

The Steam browser is no longer supported on 32-bit Linux distributions, and is disabled when viewing the *Store*, *Community*, or *User Profile* tabs in the Steam client<sup>[\[4\]](https://wiki.gentoo.org#cite_note-32bitonly-4)</sup>, so only available architecture is **amd64**.

### Kernel

Steam expects that /dev/shm, which requires kernel [tmpfs](https://wiki.gentoo.org/wiki/Tmpfs) support, is mounted prior to being started. /dev/shm should be mounted automatically by [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) and [systemd](https://wiki.gentoo.org/wiki/Systemd) during boot, but can also be mounted explicitly via /etc/fstab.

**`/etc/fstab`**

The following kernel option has to be set, otherwise Steam may fail to start with the error message: "The futex facility returned an unexpected error code."

**Allow 32-bit time\_t for Steam's 32-bit compatibility**

```
General architecture-dependent options  --->
  [*] Provide system calls for 32-bit time_t 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_COMPAT_32BIT_TIME</code> to find this item.
Enable user level driver support if controller support is desired.

**Enable user level drivers for input**

```
Device Drivers  --->
  Input device support  --->
    -*- Generic input layer (needed for keyboard, mouse, ...) 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_INPUT</code> to find this item.
      [*] Miscellaneous devices [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_INPUT_MISC</code> to find this item.  --->
        <*> User level driver support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_INPUT_UINPUT</code> to find this item.
Enable user namespace support in order to support launching games in Compatibility mode (i.e. with Proton):

**Enable User namespace**

```
General setup --->
    [*] Namespaces support 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NAMESPACES</code> to find this item. --->
        [*] User namespace [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_USER_NS</code> to find this item.
Enable NTSYNC when using GE-Proton or Proton 11 or newer:

**Enable NTSYNC**

```
Device Drivers --->
    Misc devices --->
        [*] NT synchronization primitive emulation 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_NTSYNC</code> to find this item.
### File Descriptors Limit

On non-systemd configurations the default PAM hard file descriptors limit of 4096 generates the following warning in the Proton debug log:

WARNING: Low file descriptor limit: 4096 (see [https://github.com/ValveSoftware/Proton/wiki/File-Descriptors](https://github.com/ValveSoftware/Proton/wiki/File-Descriptors))

The limit on the number of file descriptors that can be opened by a user logged on via PAM is controlled by the pam\_limits.so module and the hard limit can be checked by a user at runtime with the ulimit -Hn command.

Proton will not produce the warning if the hard limit is increased to 524288 or higher.

Higher limit can be specified in the /etc/security/limits.conf file or in a configuration file in the /etc/security/limits.d/ directory, for example:

**`/etc/security/limits.d/26-steam-nofile.conf`**

This config will allow all users and groups to use the new limit. To set the new limit to a particular user only, the `*` in the beginning can be replaced with a specific username.

### max\_map\_count

In Linux kernel the default [max\_map\_count](https://docs.kernel.org/admin-guide/sysctl/vm.html#max-map-count) limit of 65530 on the maximum number of memory map areas a process may have, generates the following warning in the Proton debug log:

WARNING: Low /proc/sys/vm/max\_map\_count: 65530 will prevent some games from working

Proton will not produce the warning if the limit is increased to 1048576 or higher.

At runtime the limit can be changed with the following command:

`root #``sysctl --write vm.max_map_count=1048576`
A configuration file in the /etc/sysctl.d/ directory can be used to set the limit during boot time, for example:

**`/etc/sysctl.d/steam.conf`**

## Installation

The Steam installer downloads and installs the Steam client to the user's home directory. This prevents Portage from managing the Steam client updates or the software installed by it. The Steam client is solely responsible for managing software installation and updates.

### Emerge (recommended)

The steam-launcher ebuild is available from the [steam-overlay](https://github.com/anyc/steam-overlay) repository, which is Gentoo's primary repository for the Steam client and Steam-based games. The steam-overlay repository can be added manually or with repository management tools like eselect-repository.

Install [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository) and [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git):

`root #``emerge --ask --noreplace app-eselect/eselect-repository dev-vcs/git`
Add the Steam repository:

`root #``eselect repository enable steam-overlay`
Then sync with [emaint](https://wiki.gentoo.org/wiki/Emaint):

`root #``emaint sync -r steam-overlay`
Due to the Proton runtime built into Steam, 32-bit binaries of most dependencies are included within the Steam installation. Some system dependencies remain however, but Portage should prompt for them. These packages should be added to /etc/portage/package.use/steam with their `abi_x86_32` USE flag enabled. Some required changes include:

**`/etc/portage/package.use/steam`**

For users with an Nvidia card using the proprietary drivers, these packages should be added to /etc/portage/package.use/steam with their `abi_x86_32` USE flag enabled as well:

**`/etc/portage/package.use/steam`**

Add the steam overlay to package.accept\_keywords:

**`/etc/portage/package.accept_keywords/steam`**

Now read Steam's license terms located on /var/db/repos/steam-overlay/licenses/ValveSteamLicense and if you agree with them, then add it to portage:

**`/etc/portage/package.license/steam`**

The overlay enables the Steam runtime by default. If you'd like to rely solely on Gentoo packages, then disable the `steamruntime` USE flag. Use the esteam utility later to scan your installed native Linux games for additional Gentoo packages required by them. Note that Gentoo packages do not cover the entirety of the runtime, so a small number of games may not work.

Once the repository has been added, install the steam-launcher ebuild:

`root #``emerge --ask games-util/steam-launcher`
#### Troubleshooting

If Steam is failing to emerge due to [circular dependencies](https://wiki.gentoo.org/wiki/Portage/Help/Circular_dependencies) with ncurses and gpm, try:

`root #````
USE="-gpm" emerge --ask --oneshot sys-libs/ncurses
```
`root #````
emerge --ask games-util/steam-launcher
```
`root #````
emerge --ask --oneshot sys-libs/ncurses gpm
```
If Steam is failing to emerge due to [circular dependencies](https://wiki.gentoo.org/wiki/Portage/Help/Circular_dependencies) with harfbuzz and freetype, try:

`root #````
USE="-harfbuzz" emerge --ask --oneshot media-libs/freetype media-libs/sdl2-ttf
```
`root #````
emerge --ask games-util/steam-launcher
```
`root #````
emerge --ask --oneshot media-libs/freetype media-libs/sdl2-ttf media-libs/harfbuzz
```
On pure Wayland systems with a global `-X` use flag, installing `steam-launcher` may get blocked by `x11-libs/cairo`. In such a case, `dev-cpp/cairomm` will likely be installed without support for X as well, and an `X` use flag will need to be added to both.

Furthermore, if your Wayland compositor does not support XWayland natively, `gui-apps/xwayland-satellite` is needed to run Steam.

If SteamUpdateUI fails to launch with "An X error has occured" it may indicate the user is not in the video group.

`root #``gpasswd -a larry video`
##### Migrate from flatpak to the emerge recommended install

In order to migrate from flatpak to the recommended emerge method. The instructions for the emerge install must be followed, then the flatpak-packaged steam files must be moved to the default Gentoo filesystem location:

`user $``mv ~/.var/app/com.valvesoftware.Steam/.local/share/Steam ~/.local/share/`
Some games store user data in \~/.var/app/com.valvesoftware.Steam/.local/share/ for examples mods or screenshots. These directories also need to be moved for a complete migration. For example to move Euro Truck Simulator 2 user data :

`user $``mv ~/.var/app/com.valvesoftware.Steam/.local/share/Euro\ Truck\ Simulator\ 2 ~/.local/share/`
Finally the flatpak can be uninstalled

`user $``flatpak uninstall com.valvesoftware.Steam`
### Flatpak

A quite simple, fast, and clean method (e.g. 32-bit dependencies do not need to be compiled) of installing Steam is to use the [Flatpak](https://wiki.gentoo.org/wiki/Flatpak) package `com.valvesoftware.Steam` from [Flathub](https://flathub.org/apps/details/com.valvesoftware.Steam) (also installing necessary [udev rules](https://wiki.gentoo.org/wiki/Udev#Rules) for game controllers):

`root #````
USE="X" emerge --ask sys-apps/flatpak
```
`root #````
emerge --ask games-util/game-device-udev-rules
```
`user $````
flatpak install flathub com.valvesoftware.Steam
```
`user $````
flatpak run com.valvesoftware.Steam
```
Steam will update itself and install its files in the \~/.var/app/com.valvesoftware.Steam directory.

Steam can be run in a 64-bit [multilib](https://wiki.gentoo.org/wiki/Multilib) [chroot](https://wiki.gentoo.org/wiki/Chroot) on **amd64**. The major advantage of a chroot is that Steam and its dependencies will be isolated from the root filesystem. The Steam browser is no longer supported on 32-bit Linux distributions, so only 64-bit chroot environment is available.[\[4\]](https://wiki.gentoo.org#cite_note-32bitonly-4)

Create the chroot directory:

`root #````
mkdir /usr/local/steam64
```
`root #````
cd /usr/local/steam64
```
Fetch and extract the stage3 tarball.

`root #````
wget https://distfiles.gentoo.org/releases/amd64/autobuilds/current-stage3-amd64-openrc/stage3-amd64-openrc-20250907T165007Z.tar.xz
```
`root #````
tar xpvf stage3*.tar.xz --xattrs-include='*.*' --numeric-owner
```
Copy DNS information and ensure it's world-readable:

`root #````
cp -L /etc/resolv.conf etc
```
`root #````
chmod a+r etc/resolv.conf
```
Create the ebuild repository directory:

`root #````
mkdir var/db/repos/gentoo
```
Mount the necessary filesystems:

`root #````
mount -t proc /proc proc
```
`root #````
mount -R /sys sys
```
`root #````
mount -R /dev dev
```
`root #````
mount -R /run run
```
`root #````
mount -R /var/db/repos/gentoo var/db/repos/gentoo
```
Chroot with linux64 and update the environment. The use of linux64 is not required on **amd64**, and it is only used here for consistency.

`root #````
linux64 chroot .
```
`root #````
env-update && source /etc/profile
```
`root #````
export PS1="(chroot) $PS1"
```
The chroot should now be updated and configured accordingly. It is recommended to at least configure the [timezone](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Timezone) and enable sound support by installing [media-libs/alsa-lib](https://packages.gentoo.org/packages/media-libs/alsa-lib).

Now create the Steam user with the same UID (usually 1000) as the local user. The local UID can be determined by running id -u as the local user, outside of the chroot. Using the same UID will simplify the process of granting access to the X server from inside the chroot.

`(chroot) root #````
useradd -u <UID> -m -G audio,video steam
```
Install Steam from one of the above installation methods. When complete, exit the chroot:

`(chroot) root #````
exit
```
Unmount the chroot directories:

`root #````
umount -l proc
```
`root #````
umount -l sys
```
`root #````
umount -l dev
```
`root #````
umount -l run
```
`root #````
umount -l var/db/repos/gentoo
```
Install xhost to allow access to the X server from inside the chroot:

`root #``emerge --ask --noreplace x11-apps/xhost`
Logout, and then login. This allows the [display manager](https://wiki.gentoo.org/wiki/Display_manager) or xinit to process /etc/X11/xinit/xinitrc.d/00-xhost and automatically grant all local connections to the X server from the local UID. This will not work if the Steam UID is different to that of the local UID. Either set the same UID when creating the Steam user, as was mentioned earlier, or if the Steam user already exists change the Steam UID with usermod -u \<UID> steam to match the local UID.

Alternatively, run xhost +local: to allow all local connections to the X server from *any* local UID. This is a potential security risk as any user could access the X server without authentication. To revoke access run xhost -local:

Next, create the following wrapper script to setup the chroot, substitute to the Steam user, and start Steam. The wrapper script has two user defined variables: `chroot_bits` and `chroot_dir`. The `chroot_bits` variable must be set to `64` for a 64-bit chroot. The `chroot_dir` variable should be set to the location of the chroot directory.

**`/usr/local/bin/steam-chroot`**

```
#!/bin/sh
 
# steam chroot bits
chroot_bits="64"
 
# steam chroot directory
chroot_dir="/usr/local/steam64/"
 
# check if chroot bits is valid
if [ "${chroot_bits}" = "32" ] ; then
  chroot_arch="linux32"
elif [ "${chroot_bits}" = "64" ] ; then
  chroot_arch="linux64"
else
  printf "Invalid chroot bits value '%s'. Permitted values are '32' and '64'.\n" "${chroot_bits}"
  exit 1
fi
 
# check if the chroot directory exists
if [ ! -d "${chroot_dir}" ] ; then
  printf "The chroot directory '%s' does not exist!\n" "${chroot_dir}"
  exit 1
fi
 
# mount the chroot directories
mount -v -t proc /proc "${chroot_dir}proc"
mount -vR /sys "${chroot_dir}sys"
mount --make-rslave "${chroot_dir}sys"
mount -vR /dev "${chroot_dir}dev"
mount --make-rslave "${chroot_dir}dev"
mount -vR /run "${chroot_dir}run"
mount --make-rslave "${chroot_dir}run"
mount -vR /var/db/repos/gentoo "${chroot_dir}var/db/repos/gentoo"
# the --make-rslave flags are needed for systemd support
 
# chroot, substitute user, and start steam
if [[ -n $( grep systemd /proc/1/comm ) ]]; then
  "${chroot_arch}" unshare -m chroot "${chroot_dir}" su -c 'steam' steam
else
  "${chroot_arch}" chroot "${chroot_dir}" su -c 'steam' steam
fi
# unmount the chroot directories when steam exits
umount -vl "${chroot_dir}proc"
umount -vl "${chroot_dir}sys"
umount -vl "${chroot_dir}dev"
umount -vl "${chroot_dir}run"
umount -vl "${chroot_dir}var/db/repos/gentoo"
```
Make the wrapper script executable:

`root #``chmod +x /usr/local/bin/steam-chroot`
Run the wrapper script as root to start Steam:

`root #``steam-chroot`
### Systemd and chroot

When the host system is in systemd, raw chroot is not sufficient. Instead, `unshare -m chroot` has to be used. In fact the above wrapper script supports this case.

Explanation: With bare `chroot`, the Steam client does not run, complaining "Steam now requires user namespaces to be enabled." For this Steam tests if `bwrap --bind / / true` succeeds. (This requires bwrap is set setuid.) Internally bwrap calls [pivot\_root (2)](https://www.man7.org/linux/man-pages/man2/pivot_root.2.html), of which conditions with "/" are not met under systemd. With unshare the namespace gets separated, and things work.

## Easy Anti Cheat Support

Due to DT\_HASH not being enabled by default since glibc 2.36 then the follow needs to be applied to allow EAC games to work

**`/etc/portage/package.use/glibc`**

`root #``emerge -1 sys-libs/glibc`
## Removal

Remove the steam-launcher package and depclean all dependencies:

`root #``emerge --ask --depclean --verbose games-util/steam-launcher``root #``emerge  --depclean`
Remove the Steam directory from the user account (this will delete downloaded games files, user's settings and saves):

`user $``rm -rf ~/.local/share/Steam`
## Troubleshooting

The most common game related issues are solved by enabling the `stack-realign` USE flag on the [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc) package and re-emerge the [@world set](<https://wiki.gentoo.org/wiki/World_set_(Portage)>). It is a good idea to perform this change as the first troubleshooting action item.

Some Steam and games specific troubleshooting available on [Steam/Client troubleshooting](https://wiki.gentoo.org/wiki/Steam/Client_troubleshooting) and [Steam/Games troubleshooting](https://wiki.gentoo.org/wiki/Steam/Games_troubleshooting) subpages.

If you want to play games through proton, don't forget to add the `vulkan` USE flag on the [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa) package.

The best place to ask for help is the [Steam thread](https://forums.gentoo.org/viewtopic-t-930354.html) on the Gentoo Forums. If a solution to an issue is confirmed by others, add it to this page or the relevant troubleshooting subpage. Please do not remove content without [discussion](https://wiki.gentoo.org/wiki/Talk:Steam), *unless* it is obviously wrong.

## See also

- [Games](https://wiki.gentoo.org/wiki/Games) — a landing page for many of the games (especially open source variants) available in Gentoo's main ebuild repository.
- [Steam Controller](https://wiki.gentoo.org/wiki/Steam_Controller) — a game controller developed by [Valve](https://en.wikipedia.org/wiki/Valve_Corporation).
- [Steam/Client troubleshooting](https://wiki.gentoo.org/wiki/Steam/Client_troubleshooting) — provides troubleshooting details for the Steam client on Linux systems.
- [Steam/Games troubleshooting](https://wiki.gentoo.org/wiki/Steam/Games_troubleshooting) — provides troubleshooting details for specific games running via Steam.

## External resources

- [Gentoo Forums - Native Steam client and source game engine](https://forums.gentoo.org/viewtopic-t-930354.html)
- [GitHub - Steam for Linux Client](https://github.com/ValveSoftware/steam-for-linux)
- [Steam Community - Steam for Linux](https://steamcommunity.com/linux)
