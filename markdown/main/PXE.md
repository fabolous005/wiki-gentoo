<!-- source: https://wiki.gentoo.org/wiki/PXE | group: Gentoo Wiki (Main) | wiki-title: PXE -->
---
title: PXE
url: https://wiki.gentoo.org/wiki/PXE
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-06"
fingerprint: "1a85bf5910056796"
license: CC BY-SA 4.0
---

# PXE

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page is about using Gentoo as a **PXE** server to boot just about any operating system or bootable tool. Please extend it with your own experience. The primary focus is to boot live images and the page can get renamed once we have a better name for it.

## TFTP server

Create the TFTP root directory, where the chosen bootloader will be served from. Also make /tftproot owned by a user to not have to be root to do modifications.

`root #``mkdir /tftproot``root #``chown user:usergroup /tftproot`
Then choose a TFTP server, pick one of these:

### atftp

One of the easiest TFTP servers to setup on Gentoo is [net-ftp/atftp](https://packages.gentoo.org/packages/net-ftp/atftp).

`root #``emerge --ask net-ftp/atftp`
Add to default startup, and start service.

`root #``rc-update add atftp default``root #``/etc/init.d/atftp restart`
### tftp-hpa

Many have good experience with [net-ftp/tftp-hpa](https://packages.gentoo.org/packages/net-ftp/tftp-hpa).

`root #``emerge --ask net-ftp/tftp-hpa`
To run it temporarily, use the following command line.

`root #``in.tftpd -L --secure /tftproot`
## Bootloaders

The article about [Diskless nodes](https://wiki.gentoo.org/wiki/Diskless_nodes) contains plenty of useful information on how to setup a PXE environment.

### GRUB2

For client configuration, see for example: [Setup PXE boot with EFI Using Grub2](https://github.com/openSUSE/kiwi/wiki/Setup-PXE-boot-with-EFI-Using-GRUB2) from openSUSE wiki.

Verify that [sys-boot/grub](https://packages.gentoo.org/packages/sys-boot/grub) was installed with `GRUB_PLATFORMS` `efi-32`, `efi-64`, `pc`

Next "install" needed grub files to /tftproot

`user $``grub-mknetdir --net-directory /tftproot` Netboot directory for i386-pc created. Configure your DHCP server to point to /tftproot/boot/grub/i386-pc/core.0
Netboot directory for i386-efi created. Configure your DHCP server to point to /tftproot/boot/grub/i386-efi/core.efi
Netboot directory for x86\_64-efi created. Configure your DHCP server to point to /tftproot/boot/grub/x86\_64-efi/core.efi

### iPXE

There are a few different options to get the [iPXE](https://ipxe.org) bootloaders.

The quickest way:

But building from source (highly recommended from iPXE folks) is not hard, and allows for easy modifications.

Verify that some needed dependencies are installed:

`root #``emerge --ask --oneshot dev-vcs/git`
The package [sys-firmware/ipxe](https://packages.gentoo.org/packages/sys-firmware/ipxe) can be used, but needs `USE` flags such as `efi`, `undi` and still does not have all of the files we want.

### PXELINUX

Exists as [sys-boot/syslinux](https://packages.gentoo.org/packages/sys-boot/syslinux) in portage and can be installed with

`root #``emerge --ask sys-boot/syslinux`
Copy over needed files to our /tftproot *this is only basic files for pcbios kernel boot, other files might be needed for other platforms*

`user $````
cp /usr/share/syslinux/pxelinux.0 /tftproot
```
`user $``cp /usr/share/syslinux/ldlinux.c32 /tftproot`
## DHCP server

## Bootable images

### Gentoo installation

See [Installation Alternatives - Diskless install using PXE](https://wiki.gentoo.org/wiki/Installation_alternatives#Diskless_install_using_PXE_from_the_LiveCD) for a guide on Gentoo.

Some projects exist to help setup such an environment

- [numberly/gentoo-pxe-builder](https://github.com/numberly/gentoo-pxe-builder) with focus on [Pxelinux](https://wiki.gentoo.org#PXELINUX) and SSH automation for installation
- [NiKiZe/Gentoo-iPXE](https://github.com/NiKiZe/Gentoo-iPXE) with focus on [iPXE](https://wiki.gentoo.org#iPXE)

### Fedora installation

The following script creates a TFTP directory tree, downloads kernel and initrd images from a Fedora mirror and configures a netbootable instance of GRUB.

**`~/bin/fedora-pxe-install`**

```
#!/bin/bash -xe
# Mirror URL
mirror="http://dl.fedoraproject.org/pub/fedora/linux/"
# Releases
[ $# -eq 0 ] && releases="22 21/Server 21-Alpha/Workstation 20/Fedora" || releases="$*"
# Architecture: x86 | x86_64 | ...
architectures="x86_64 i386"
# TFTP directory path
prefix=/tftproot
test -n "$prefix"
menuentry() { cat << EOF
menuentry "Install Fedora $release $arch ($state)" {
    linux /boot/$kernel repo=$repo
    initrd /boot/$initrd
}
EOF
}
check() {
    state="$1"
    repo="$mirror$state/$release/$arch/os"
    curl --silent --fail --head "$repo/" >/dev/null
}
download() {
    test -e "$1" && { echo "Skipping: $1" >&2; return 0; }
    echo "Downloading: $1 from $2" >&2
    curl --silent --fail --output "$1" "$2"
}
grub2-mknetdir --net-directory "$prefix"
for release in $releases; do
    for arch in $architectures; do
        check releases || check development || check releases/test || { echo "# Check failed for $release $arch"; break; }
        kernel="fedora-$(echo "$release" | tr / -)-$arch-kernel"
        initrd="fedora-$(echo "$release" | tr / -)-$arch-initrd"
        pxerepo="$repo/images/pxeboot"
        download "$prefix/boot/$kernel" "$pxerepo/vmlinuz" || { echo "# Couldn't download kernel for $release $arch"; break; }
        download "$prefix/boot/$initrd" "$pxerepo/initrd.img" || { echo "# Couldn't download initrd for $release $arch"; break; }
        menuentry
    done
done | tee "$prefix/boot/grub/grub.cfg"
```
Adjust it to your needs and then run it without parameters.

`root #``fedora-pxe-install`
Now you just need to configure TFTP and DHCP so that `/tftproot` is available to the netbooted machine and so that it requests `/boot/grub/i386-pc/core.0` via TFTP.

### Ubuntu installation

To install Ubuntu to a device via PXE, simply [download an Ubuntu netboot image](http://cdimage.ubuntu.com/netboot/) for the appropriate architecture. Note that most of the files in the release and architecture-specific directory are unnecessary, you simply need `netboot.tar.gz` (about 40MB).

Extract that file to an appropriate TFTP-served directory and configure your DHCP server with the `filename` directive to point at `pxelinux.0` for the machine or subnet in question.

Note that you will also need to give the host(s) internet access as the netboot images are configured to pull packages down from the internet.
