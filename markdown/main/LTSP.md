<!-- source: https://wiki.gentoo.org/wiki/LTSP | group: Gentoo Wiki (Main) | wiki-title: LTSP -->
---
title: LTSP
url: https://wiki.gentoo.org/wiki/LTSP
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-16"
fingerprint: f4a9bc5a7506af80
license: CC BY-SA 4.0
---

# LTSP

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Feb 21, 2017**, the information in this article is probably

**outdated**. You can help the Gentoo community by verifying and

[updating this article](https://wiki.gentoo.org/index.php?title=LTSP&action=edit).

The **Linux Terminal Server Project** is a collection of scripts and documentation to create a cluster of thin clients. For instance, an entire client chroot environment is built with a single command: ltsp-build-client. This article will guide the administrator through the installation and configuration of a basic LTSP 5 system.

This guide shows how to install and configure the Gentoo LTSP 5 port, and assumes some knowledge of thin client architecture and experience in manually installing Gentoo. Also, a server and client with the specifications listed in the [LTSP manual](https://ltsp.org/docs/installation/) are required. Concerning the client network card, only PXE is included in this manual.

In addition to this guide, several other resources can be of aid while configuring the system. These can be found in the [External resources](https://wiki.gentoo.org#External_resources) section below.

## Server preparation

### Installation

Gentoo's LTSP packages are stored in the *ltsp*[ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository). To [use](https://wiki.gentoo.org/wiki/Project:Overlays/Old_User_Guide) the Gentoo LTSP-Overlay, get it with [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository).

`root #``eselect repository enable ltsp`
The LTSP server package needs a tftp and dhcp server. In this tutorial [net-dns/dnsmasq](https://packages.gentoo.org/packages/net-dns/dnsmasq) is [used](https://wiki.gentoo.org#DHCP_and_PXE-boot). It also requires a system logger which can accept client messages over tcp, for which [app-admin/syslog-ng](https://packages.gentoo.org/packages/app-admin/syslog-ng) is [used](https://wiki.gentoo.org#System_Logging) in this tutorial. Don't forget to add a window manager, ltsp-client won't log in if no window manager is installed on the server.

`root #``emerge --ask app-admin/syslog-ng net-misc/ltsp-server`
### Kernel

Besides the obvious drivers, the server kernel ought to have the following settings. If NFS is going to be used to serve the chroot environments, make sure to compile it in as well and reboot afterwards.

**LTSP server**

### DHCP and PXE-boot

First, setup the server to provide client machines with a kernel at boottime. Although the server depends on a range of possible packages, the manual will use the ones listed in this paragraph. The PXE bootloader is provided by [sys-boot/syslinux](https://packages.gentoo.org/packages/sys-boot/syslinux). Dnsmasq is a DHCP/DNS/TFTP server. Advanced TFTP is also one of the TFTP server options, and the only one to support multicast TFTP.<sup>**[1](https://wiki.gentoo.org#External_resources)**</sup> The chroot environments as well as the kernels served at boot time are stored in /opt/ltsp.

Configure [net-dns/dnsmasq](https://packages.gentoo.org/packages/net-dns/dnsmasq) as a DHCP/TFTP server. The TFTP server is used by the client nodes to retrieve the kernel, initramfs and lts.conf. For help on configuration, view the Gentoo Wiki [Dnsmasq](https://wiki.gentoo.org/wiki/Dnsmasq) page.

Setup the PXE bootloader; view the [PXE install](https://wiki.gentoo.org/wiki/Syslinux#pxelinux_bootloader_installation) section for more detail. In the example configuration, the system mounts the local client disk after booting and loading the kernel from the server. Make sure the kernel and initramfs are in /var/lib/tftpboot as well as the custom real\_init option. You can test the work so far with a working kernel and system.

**`/var/lib/tftpboot/pxelinux.cfg/default`**

**ltsp-client 5.3+**

Start the services, now and at every boot

`root #``/etc/init.d/dnsmasq start``root #``rc-update add dnsmasq default`
### NFS and Xinetd

The chroot environments are shared with NFS. Xinetd is used for **ldminfod** and **nbd** sharing. By default only the localhost is allowed access, so edit the /etc/xinetd.conf and restart the service.

**`/etc/xinetd.conf`**

`root #````
/etc/init.d/nfs start
```
`root #````
/etc/init.d/xinetd start
```
`root #````
rc-update add nfs default
```
`root #````
rc-update add xinetd default
```
### NBD

Booting from NBD is still considered testing and doesn't work quite out-of-the-box. On the server, several changes are required besides installing [sys-block/nbd](https://packages.gentoo.org/packages/sys-block/nbd). First, create an nbd-server initscript.

**`/etc/init.d/nbd-server`**

```
#!/sbin/openrc-run
command="/usr/bin/nbd-server"
pidfile="/var/run/${SVCNAME}.pid"
depend() {
        use net
}
```
For the initscript to work, a default config file has to be created also:

**`/etc/nbd-server/config`**

```
[generic]
[ltsp]
exportname = /opt/ltsp/images/i686.img
readonly = true
```
Now, start the service now and at every boot.

`root #``/etc/init.d/nbd-server start``root #``rc-update add nbd-server default`
### System logging

System logging is performed by [app-admin/sysklogd](https://packages.gentoo.org/packages/app-admin/sysklogd). Log files are not stored locally however, but sent to the server specified by `SYSLOG_HOST` in lts.conf. While executing, the ltsp-client-setup script adds the syslog-ng configuration to perform this. To allow the server to process these incoming log messages, some changes have to be made in that configuration as well. In the syslog-ng setting below, messages are logged to a file named after each client's fully qualified domain name.

**`/etc/syslog-ng/syslog-ng.conf`**

`root #``/etc/init.d/syslog-ng restart`
### Sound

As you might have seen in the list of emerged dependencies for ltsp, both for the client and the server, [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio) was among them.  In addition to [media-sound/pulseaudio](https://packages.gentoo.org/packages/media-sound/pulseaudio), its alsa-plugin needs to be installed on both client and server with the `pulseaudio` use flag enabled. Refer to the [Gentoo Wiki](https://wiki.gentoo.org/wiki/PulseAudio#PA_installation) chapter for detailed installation instructions.

## Client install

The ltsp-server package amongst others ships a command called ltsp-build-client. This command is responsible for building the entire chroot environment. And while ltsp-build-client and available plugins setup the environment, Kicktoo actually builds it. You can also use the [Quickstart](https://wiki.gentoo.org#Quickstart) alternative.

### Configuration

You can invoke the build script with command line arguments or configure the config file in /etc/ltsp/ltsp-build-client.conf. During the build, the packaged kicktoo profile in /etc/ltsp/profiles/kicktoo.profile is invoked. For both, example config files were included in the installation of ltsp-server, but other profiles can be configured. Commandline options take precedence over config file options.

#### Video cards

Make sure to check if the video card drivers that are needed are installed. By default only `fbdev` and `vesa` are included.

**`/etc/ltsp/ltsp-build-client.conf`**

```
VIDEO_CARDS="vesa intel radeon mach64"
```
#### Network cards

If Genkernel is used with non default network cards, add the `post_install_initramfs_builder()` function in the kicktoo profile to add Genkernel network modules. This will be included in the next release (5.4.5).

**`/etc/ltsp/profiles/kicktoo.profile`**

```
() {
	if [ "${initramfs_builder}" == "genkernel" ]; then
		# add you're own network drivers if needed
		# eg. "MODULES_NET=\"\${MODULES_NET}\" via-rhine"
		echo "MODULES_NET=\"\${MODULES_NET}\"" >> "${chroot_dir}/usr/share/genkernel/arch/${MAIN_ARCH}/modules_load"
	fi
}
```
#### Kernel

A separate section for the client kernel is in order. A standard Genkernel kernel is created during the installation when configuration changes are made. It's advisable to take a closer look at the client's kernel config and use the config during the client install. Make sure the network drivers are included as modules.

**LTSP client**

**`/etc/ltsp/ltsp-build-client.conf`**

```
KERNEL_SOURCES="=sys-kernel/gentoo-sources-2.6.34-r12"
KERNEL_CONFIG_URI="tftp://192.168.67.1/ltsp/x86/gentoo-sources-2.6.34-r12.config"
```
### Building the client

When all configuration has been done, start building the client.

`root #``ltsp-build-client --config=/etc/ltsp/my-ltsp-build-client.conf`
After invoking the ltsp-build-client command, the environment is preparing. For each architecture the first build takes up the most time because binary packages are created from source in the first run. These binary packages are stored in /usr/portage/packages through a bind mount on the server. Any consequent builds use these packages to speed up the process.

### Finishing the install

Some things still need to be done after building the environment. First up is the kernel, which needs to be put inside the tftproot. In the default setup, this is copied from the chroot in /opt/ltsp and copied to the tftproot in an ltsp subdir in /var/lib/tftpboot, /tftpboot or /srv/tftp, if one exists. Calling ltsp-update-kernels with a different tftproot location:

`root #``ltsp-update-kernels --tftpdirs="/opt/tftproot"`
The pxelinux configuration has to be updated to reflect the changes in the setup. See the [PXE boot section](https://wiki.gentoo.org#DHCP_and_PXE-boot) for more info.

For a user to be able to login over ssh from the thin client to the server, the client needs the server ssh-keys. Although executed when building the client, these can be updated to the clients chroot with the following command:

`root #``ltsp-update-sshkeys`
### Client configuration

While some properties of the client's environment are more or less statically set in the chroot environment, others can be changed at boot time. The lts.conf file allows properties to be set for all clients or for each workstation specifically. Explaining the syntax of the file goes beyond the scope of this tutorial, but it is explained on the [LTSP wiki](http://sourceforge.net/apps/mediawiki/ltsp/index.php?title=Ltsp_LtsConf) and in the lts.conf man page. The latter is available after emerging the ltsp-docs package.

The lts.conf file is downloaded at client boot time from a preconfigured location in the tftproot, namely /ltsp/i686/lts.conf. Create the lts.conf there and change your architecture if applicable.

**`/var/lib/tftpboot/ltsp/i686/lts.conf`**

```
[default]
 SERVER=192.168.67.1
```
The script that invokes the download is /etc/init.d/ltsp-client-setup. Together with /etc/init.d/ltsp-client it is responsible for settings like the swap configuration, sound daemon, and date among others. While ltsp-client-setup performs the environment settings, ltsp-client starts the sound daemon and the ldm login process. Some of these settings will now be discussed in detail.

### LDM

If all is well, LDM will be started by ltsp-client and it should be possible to log in with a user on the server. If not, it might be worth checking if the LDM Info Daemon is disabled in /etc/xinitd.d/ldminfod. When the X server cannot start it might help to add a customized [xorg.conf](https://wiki.gentoo.org/wiki/Xorg.conf) file. As many different xorg.conf files can exist for many different clients in the same chroot, make sure to name them properly.

**`/var/lib/tftproot/ltsp/i686/lts.conf`**

```
[acer-aspire-one]
  X_CONF = /etc/X11/xorg-acer-aspire-one.conf
[00:1E:68:C2:FF:EE]
  LIKE=acer-aspire-one
```
To choose another window manager, install it on the server and put the following in the LTSP configuration file (replace Fluxbox with the chosen window manager).

**`/var/lib/tftpboot/ltsp/i686/lts.conf`**

```
LDM_XSESSION = /usr/bin/fluxbox
```
### NBD boot

If the `nbd` USE flag was enabled, it will be possible to generate a NBD bootable image and initramfs. This is optional (and testing) for now, so there are some steps to be followed to accomplish this. First, before the client install, configure the initramfs-builder for using [Dracut](https://wiki.gentoo.org/wiki/Dracut). This generates an initramfs which allows to boot an NBD image.

**`/etc/ltsp/ltsp-build-client.conf`**

```
INITRAMFS_BUILDER="dracut"
```
An image has to be generated with ltsp-build-client. The resulting image ends up in /opt/ltsp/image/i686.img.

`root #``ltsp-update-image i686`
Because Dracut's named export function isn't bugfree yet, export names can't end with a number (like **amd64**). It is therefore advised to configure the /etc/nbd-server/config file manually. Look at the example in the [server NBD part](https://wiki.gentoo.org#NBD) of the manual for a working example.

### 5.2 client

A 5.2 client is somewhat different from a 5.3 client. The main difference is that a 5.2 client has to be specifically prepared to function as an LTSP client while a 5.3 client takes care of this during the boot init process. In theory ltsp-client-5.3 can be installed on any Gentoo system, allowing it to be booted as an LTSP client.

Starting from ltsp-server-5.3, it's possible to install a 5.2 or a 5.3 client. This can be done by setting one of the different provided build profiles. By default, the kicktoo-5.3.profile is used.

**`/etc/ltsp/ltsp-build-client.conf`**

```
INSTALLER_PROFILE=/etc/ltsp/profiles/quickstart-5.2.profile
```
Booting a 5.3 client requires the following PXE configuration, without a real\_init.

**`/var/lib/tftpboot/pxelinux.cfg/default`**

**ltsp-client-5.2**

## Tips and tricks

Several optional tips and tricks concerning LTSP can be found here. Most of them are Gentoo specific. Non-Gentoo specific tips & tricks can be found on the [LTSP Wiki](http://wiki.ltsp.org/wiki/Tips_and_Tricks).

### NBD swap from USB

The nbdswapd allows clients to use swap space through a NBD. For this to work, the ltsp-server has to be emerged with the `nbd` USE flag enabled. Also, the lts.conf needs to be updated and the nbdswapd.conf has to contain the mountpoint of your usb stick and the desired swap size (64Mb by default).

**`/etc/ltsp/nbdswapd.conf`**

```
SIZE=128
SWAPDIR=/mnt/usbswap
```
**`/var/lib/tftpboot/ltsp/i686/lts.conf`**

```
[default]
  NBD_SWAP = True
```
### Decreasing chroot size

It is possible to make the built LTSP client chroot smaller using a combination of several methods. It's possible to get a chroot less than 1Gb. First up is the EXCLUDE var in the ltsp-build-client program. This can be used to automatically unmerge packages at the end of the client build.

**`/etc/ltsp/ltsp-build-client.conf`**

```
EXCLUDE="sys-apps/man-pages sys-kernel/genkernel"
```
If there is never any intention to do any maintenance on the chroot again, you can even unmerge gcc this way. Also you can remove the build time dependencies in the chroot by [chrooting](https://wiki.gentoo.org#Chrooting) into the client chroot and executing the following command.

`root #``emerge --depclean --with-bdeps=n`
### X11 keyboard layout

At the moment, X configuration is disabled. Therefore, all LTSP X settings (in lts.conf) does not work, especially `XKBLAYOUT`. For setting the X layout of clients do the following:

`root #``mkdir /opt/ltsp/i686/etc/X11/xorg.conf.d/``root #``cp /opt/ltsp/i686/usr/share/X11/xorg.conf.d/10-evdev.conf /opt/ltsp/i686/etc/X11/xorg.conf.d/10-evdev.conf`
Then edit the file and add the line to the keyboard section of the evdev.conf

**`/opt/ltsp/i686/etc/X11/xorg.conf.d/10-evdev.conf`**

```
Option "XkbLayout" "fr"
```
### Quickstart

There is also the option to use [Quickstart](http://dev.gentoo.org/~agaffney/quickstart.php) as a possible installer for ltsp-build-client (instead of the kicktoo default). Quickstart was the foundation of Kicktoo, but is not under active development anymore. To use this option, emerge it and do the following:

**`/etc/ltsp/ltsp-build-client.conf`**

```
INSTALLER=quickstart
INSTALLER_PROFILE=/etc/ltsp/profiles/quickstart.profile
```
### Portage profile

Since ltsp-server-5.3, most of the Portage settings needed to build an ltsp-client chroot are not set in the Quickstart or Kicktoo profiles anymore. Instead they are derived from an LTSP Portage profile. This profile is present in the ltsp ebuild repository, and symlinked to the ${chroot}/make.profile during the ltsp-client install.

## Troubleshooting

**Bugs** can be reported in two locations:

Check the sections below for workarounds, and see if an existing bug has been opened *before* entering a potential duplicate bug.

### python\_get\_implementational\_package not installed

- reported on: 2011-06-06
- reported by: [Wimmuskee](https://wiki.gentoo.org/wiki/User:Wimmuskee)
- no bug

**problem**

`An emerge error for "=dev-lang/python-2.6* is not installed", with a "die "$(python_get_implementational_package) is not installed";"`
**solution**

`This means that some of your binary packages were installed against Python 2.6, Remove your binary packages to let them compile against your new python environment.`
### do not move package and distfiles dir

- reported on: 2012-02-19
- reported by: [Wimmuskee](https://wiki.gentoo.org/wiki/User:Wimmuskee)
- no bug

**problem**

`The ltsp-build-client and ltsp-chroot programs asume the portage package and distfiles dirs are at Gentoo defaults when they bind mount it into the chroot. Symlinking to another location won't work, neither is moving the dir although refering correctly in /etc/portage/make.conf, all created packages will still be made in /usr/portage version.`
**solution**

`Leave the packagedir to default.`
### virtual terminals on clients

Several programs will fight for the virtual terminals on the clients. Comment out getty in inittab:

**`/opt/ltsp/i686/etc/inittab`**

**commenting example**

### Locales

ltsp-build-client does not work witch all locale. quickstart actually requires `C` locale. So if ltsp-build-client shouts with the following message:

`root #````
ltsp-build-client ...
```
unset your locale, remove the directory and restart:

`root #``unset LANG; unset LC_ALL; ltsp-build-client --purge`
## See also

**Diskless Install**

- [Gentoo Diskless install using PXE boot](https://wiki.gentoo.org/wiki/Installation_alternatives#Diskless_install_using_PXE_and_kernel.2Finitrd.2Fsquashfs_from_the_LiveCD)
- [Diskless nodes](https://wiki.gentoo.org/wiki/Diskless_nodes) — provides instructions for creating and setting up diskless nodes with Gentoo Linux.

## External resources

**Diskless Install**

**Union Mounts**

- [Unionfs: Bringing Filesystems Together](http://www.linuxjournal.com/article/7714)
- [Aufs](http://aufs.sourceforge.net/)
- [aufs example](http://www.mail-archive.com/aufs-users@lists.sourceforge.net/msg00687.html)
- [HOWTO Simple NFS Single System Image with Genkernel 4](https://svn.gentooexperimental.org/genkernel/doc/HOWTO-Genkernel-SSI.txt)

**Other**
