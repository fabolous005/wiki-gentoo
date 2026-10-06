<!-- source: https://wiki.gentoo.org/wiki/Catalyst/Stage_Creation | group: Gentoo Wiki (Main) | wiki-title: Catalyst/Stage Creation -->
---
title: Catalyst/Stage Creation
url: https://wiki.gentoo.org/wiki/Catalyst/Stage_Creation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-10"
fingerprint: "788872392f0ed3f2"
license: CC BY-SA 4.0
---

# Catalyst/Stage Creation

[Catalyst](https://wiki.gentoo.org/wiki/Catalyst)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a guide describing how to **create to deployable images for different machines using [Catalyst](https://wiki.gentoo.org/wiki/Catalyst)**. This can be useful for getting Gentoo running on weaker systems, or for creating specialized flavors of Gentoo.

## Build Environment Setup

Install dev-util/catalyst, making sure the iso useflag is set:

`root #``emerge --ask dev-util/catalyst`
Create working directories:

`root #``mkdir -p /var/tmp/catalyst/builds/default``root #``mkdir -p /var/tmp/catalyst/config/stages/`
/var/tmp/catalyst/build/default is where the stage3 seed tarball will be placed to build a custom stage4.

/var/tmp/catalyst/config/stages is used like /etc/portage to configure Catalyst to use items like package.use and package.accept\_keywords.

## Preparation

### Kernel support

Catalyst relies on [SquashFS](https://wiki.gentoo.org/wiki/SquashFS) make sure your host kernel has support for it.

### CFLAGS

Edit /usr/share/catalyst/arch/x86.toml to change the default cflags that works best with the target system:

**`/usr/share/catalyst/arch/x86.toml`**

**x86.toml**

```
[x86.pentium3]
COMMON_FLAGS = "-Os -march=pentium3 -pipe -fomit-frame-pointer"
CPU_FLAGS_X86 = [ "mmx", "mmxext", "sse",]
```
This example shows how to set the CPU instructions that the target supports and the use of CFLAGS to lower RAM usage at the cost of some CPU time.

### \~Arch Packages

To pull in packages from \~x86, it is possible to use the /var/tmp/catalyst/config/stages directory created earlier.

`root #``mkdir -p /var/tmp/catalyst/config/stages/package.accept_keywords``root #``nano /var/tmp/catalyst/config/stages/package.accept_keywords/stage4`
**`/var/tmp/catalyst/config/stages/package.keywords/stage4`**

```
sys-kernel/gentoo-sources ~x86
```
As shown, this works very similarly to adding testing packages in Portage. See [/etc/portage/package.accept keywords](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords) for further information.

## Build Stage1

### Prepare Seed

A seed stage3 is needed to build the new stage4 for the target machine. Catalyst looks for seeds in /var/tmp/catalyst/builds/default

Select the best stage3 for the needs of the target system (in this example stage3-i686-openrc-tar.xz is used.) Stage3 tarballs can be found at [https://www.gentoo.org/downloads/mirrors/](https://www.gentoo.org/downloads/mirrors/).

`/var/tmp/catalyst/builds/default #``wget <URL of file>`
### Portage Snapshot

Portage snapshots are now handled in git and can be created and updated using:

`root #``catalyst -s stable`
You will see output that states the Gentoo Portage snapshot name and it will look similar to:

**`catalyst.log`**

**Output example**

```
NOTICE:catalyst:Creating gentoo tree snapshot 0c5fd9cca1edc63e36234b3dc91c46db24647309 from /var/tmp/catalyst/repos/gentoo.git
```
In this example take note of **0c5fd9cca1edc63e36234b3dc91c46db24647309** as it will be needed for the spec file.

### Stage1 Spec File

Catalyst *spec* files are located at /var/tmp/catalyst:

`/var/tmp/catalyst #``nano stage1-Pentium3-openrc.spec`
**`/var/tmp/catalyst/stage1-Pentium3-openrc.spec`**

**stage1-Pentium3-openrc.spec**

```
subarch: Pentium3
target: stage1
version_stamp: openrc-@Timestamp@
rel_type: default
profile: default/linux/x86/23.0
snapshot_treeish: 0c5fd9cca1edc63e36234b3dc91c46db24647309
source_subpath: default/stage3-i686-openrc
compression_mode: pixz
update_seed: yes
update_seed_command: --update --deep --newuse @world
portage_confdir: /var/tmp/catalyst/config/stages
portage_prefix: releng
```
Now, build the stage1:

`root #``catalyst -f stage1-Pentium3-openrc.spec`
## Build Stage3

`root #``nano stage3-Pentium3-openrc.spec`
**`/var/tmp/catalyst/stage3-Pentium3-openrc.spec`**

**stage1-Pentium3-openrc.spec**

```
subarch: pentium3
target: stage3
version_stamp: openrc-@TIMESTAMP@
rel_type: default
profile: default/linux/x86/23.0
snapshot_treeish: 0c5fd9cca1edc63e36234b3dc91c46db24647309
source_subpath: default/stage1-i686-openrc-@TIMESTAMP@
compression_mode: pixz
portage_confdir: /var/tmp/catalyst/stages
portage_prefix: releng
```
In this stage Catalyst will use the stage1 built earlier to build a full stage3 for the target system.

`root #``catalyst -f stage3-Pentium3-openrc.spec`
## Build Stage4

This is the stage where packages and other settings can be applied to the stage tarball. The example of the options available in this stage can be found [here](https://gitweb.gentoo.org/proj/catalyst.git/tree/examples/stage4_template.spec?id=2f3a3515f58433430a8ab4775a11fc21994df2d9) .

`root #``nano stage4-Pentium3-openrc.spec`
**`/var/tmp/catalyst/stage4-Pentium3-openrc.spec`**

**stage4-Pentium3-openrc.spec**

```
subarch: pentium3
version_stamp: openrc-@TIMESTAMP@
target: stage4
rel_type: default 0c5fd9cca1edc63e36234b3dc91c46db24647309
profile: default/linux/x86/23.0
snapshot_treeish:
source_subpath: default/stage3-pentium3-openrc-@TIMESTAMP@
portage_confdir: /var/tmp/catalyst/config/portage/stages
repos: /var/db/repos/some_overlay
stage4/use:
-qt5
-ipv6
gtk
gtk3
stage4/packages:
        net-misc/dhcpcd
        sys-kernel/gentoo-sources
        sys-devel/llvm
        dev-util/cmake
        app-misc/neofetch
        app-misc/screen
        sys-boot/grub
stage4/rcadd: dbus|default display-manager|default
stage4/empty: /var/cache/distfiles /usr/src/linux
stage4/rm: /root/.bash_history
stage4/fsscript: /path/to/file/fsscript.sh
stage4/root_overlay: /root/stage4-overlay
```
### repos

Allows additional overlays to be added to the stage4 build, this is useful when using ebuilds not part of the default ::gentoo repository. Multiple repositories can be added when separated by spaces.

### stage4/packages

This section allows the user to select which extra packages should be added to the tarball to be immediately available post-install. This can greatly speed up deployment time as there is no need to wait for large packages such as LLVM, rust or qtwebengine to compile on the slower systems.

Optionally, in this section you can also specify options, similar to how you would with emerge. For example, to get binary packages:

**`/var/tmp/catalyst/stage4-binary-openrc.spec`**

**--getbinpkg example**

```
stage4/packages:
    --getbinpkg
        net-misc/dhcpcd
        sys-kernel/gentoo-sources
        sys-devel/llvm
        dev-util/cmake
        app-misc/neofetch
        app-misc/screen
        sys-boot/grub
```
Extra spec file configuration is required for this to work correctly.

### stage4/use

This section allows the setting of global USEFLAGS that are important for the intended purpose of this tarball.

### stage4/rcadd

This section puts services into the specified runlevels to be automatically enabled at boot. These are formatted as `service|runlevel`.

### stage4/empty

This section lists directories to empty after the build is complete. For example, it can be used to clear `/usr/src/linux` to save space.

### stage4/rm

This section lists files to remove after the build is complete. For example, `/root/.bash_history` can be cleared.

### stage4/fsscript

This is a call to an optional bash script that allows a user to make any changes or tweaks on a system installed using the stage4 tarball. For example:

**`fsscript.sh`**

```
#!/bin/bash
cat > /etc/sysctl.conf <<EOF
vm.swappiness = 10
dev.rtc.max-user-freq=3072
dev.hpet.max-user-freq=3072
EOF
cat > /etc/conf.d/display-manager <<EOF
CHECKVT=7
DISPLAYMANAGER="lightdm"
EOF
cat >> /etc/portage/make.conf <<EOF
ALSA_CARDS="*"
EMERGE_DEFAULT_OPTS="--quiet"
CONFIG_PROTECT="protect-owned"
MAKEOPTS="-j2"
EOF
# Enable systemd services
systemctl enable lightdm
Systemctl enable NetworkManager
```
Anything a bash script can do can be made to run on the Gentoo system.

### stage4/root\_overlay

The root\_overlay is a filesystem overlay which is inserted at the end of build (before fsscript runs). This can be used to insert arbitrary files and directories to the build.

First, create the directory specified in `stage4/root_overlay`:

`root #``mkdir /root/stage4-overlay`
Now any files can be inserted to the build. For example, to edit `/etc/fstab`, first create the `etc` directory in the overlay:

`root #``mkdir /root/stage4-overlay/etc`
And create the file:

**`/root/stage4-overlay/etc/fstab`**

```
LABEL=EFI		/boot/efi	vfat		noauto,noatime		0 2
```
### Build

`root #``catalyst -f stage4-Pentium3-openrc.spec`
Once this is finished then the stage4 can be found in `/var/tmp/catalyst/builds/default` to transfer to the target to be unpacked.


Stub: Instructions on how to unpack/install stage4 tarballs. Probably useful as helpful tips for stage4 builders to pass along to users.
