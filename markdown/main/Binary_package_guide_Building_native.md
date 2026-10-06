<!-- source: https://wiki.gentoo.org/wiki/Binary_package_guide/Building_native | group: Gentoo Wiki (Main) | wiki-title: Binary package guide/Building native -->
---
title: Binary package guide/Building native
url: https://wiki.gentoo.org/wiki/Binary_package_guide/Building_native
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-12"
fingerprint: "7c08b22e6d441685"
license: CC BY-SA 4.0
---

# Binary package guide/Building native

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Gentoo binhost**

**Binary packages**

When building for binpkgs for a weaker system that uses the same architecture and the user doesn't want use similar Portage settings across both. Then using a container such as a chroot is the good way to achieve this goal.

This build process does depend on the host (builder) being able to support all the CPU instructions that weaker system does.

On AMD64 based CPUs it is commonly assumed that a newer CPU will support everything the previous generation did, plus more. This however has not always been the case so users should double check before starting, or at the very least check before reporting bugs/asking for support.

## Chrooting

If creating packages for a different [Portage profile](https://wiki.gentoo.org/wiki/Portage/Profiles) or system with different USE flags, a [chroot](https://wiki.gentoo.org/wiki/Chroot) can be created.

### Creating the directories

First, the directories for this chroot must be created:

`root #``mkdir --parents /var/chroot/buildenv`
### Deploying the build environment

Next, the appropriate *stage 3 tarball* must be downloaded and extracted, here the *desktop profile | openrc* tarball is being used:

This can be extracted with the following command:

`/var/chroot/buildenv/ #``tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner`
### Configuring the build environment

The build environment should be configured to match that of the system it is building for. The simplest way to do this is to copy the /etc/portage and /var/lib/portage/world files. This can be done with **rsync**:

`user $``rsync --archive --whole-file --verbose /etc/portage/* larry@remote_host:/var/chroot/buildenv/etc/portage``user $``rsync --archive --whole-file --verbose /var/db/repos/* larry@remote_host:/var/chroot/buildenv/var/db/repos`
This process should be repeated for the world file:

`user $``rsync --archive --whole-file --verbose /var/lib/portage/world larry@remote_host:/var/chroot/buildenv/var/lib/portage/world`
### Configuring the chroot

Once created, mounts must be bound for the chroot to work:

`/var/chroot/buildenv #``mount --types proc /proc proc``/var/chroot/buildenv #``mount --rbind /dev dev``/var/chroot/buildenv #``cp --dereference /etc/resolv.conf etc`
### Entering the chroot

To enter this chroot, the following command can be used:

`/var/chroot/buildenv #``chroot . /bin/bash`
Optionally, the prompt can be set to reflect the fact that the chroot is active:

`/ #``export PS1="(chroot) $PS1"`
### Enable binpkgs

As explained in the [overview article](https://wiki.gentoo.org/wiki/Binary_package_guide#Implementing_buildpkg_as_a_Portage_feature). 
Enable the binpkg building feature with:

**`/etc/portage/make.conf`**

**Enabling Portage's buildpkg feature**

```
FEATURES="buildpkg"
```
### Performing an initial build

This step is optional, but rebuilds all packages in the new *world*:

`(chroot) #``emerge --emptytree @world`
Exit the chroot and select the best option for sharing binpkgs to the client machine from the following list  [Binary\_package\_guide#Setting\_up\_a\_binary\_package\_host](https://wiki.gentoo.org/wiki/Binary_package_guide#Setting_up_a_binary_package_host).
