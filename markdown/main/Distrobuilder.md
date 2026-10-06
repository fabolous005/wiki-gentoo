<!-- source: https://wiki.gentoo.org/wiki/Distrobuilder | group: Gentoo Wiki (Main) | wiki-title: Distrobuilder -->
---
title: Distrobuilder
url: https://wiki.gentoo.org/wiki/Distrobuilder
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-18"
fingerprint: "1d23c3711f773d94"
license: CC BY-SA 4.0
---

# Distrobuilder

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Distrobuilder** is an integration tool used to create [LXC](https://wiki.gentoo.org/wiki/LXC) or [Incus](https://wiki.gentoo.org/wiki/Incus) container images. It takes a [YAML](https://en.wikipedia.org/wiki/YAML)-formatted configuration file as input and generates compressed image files which can be imported to a local repository or provided remotely via a repository server.

## Installation

### Kernel

Distrobuilder requires [overlay filesystem](https://wiki.gentoo.org/wiki/OverlayFS) support in order to work.

**Enable CONFIG\_OVERLAY\_FS for distrobuilder**

```
File systems  --->    
  <*> Overlay filesystem support
```
### USE flags


### USE flags for
            [app-containers/distrobuilder](https://packages.gentoo.org/packages/app-containers/distrobuilder)
            
            System container image builder for LXC and incus

| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

`root #``emerge --ask app-emulation/distrobuilder`
### Additional software

The [app-emulation/distrobuilder](https://packages.gentoo.org/packages/app-emulation/distrobuilder) package does not include any YAML templates. The upstream templates can be a useful guide when generating custom images. They may be found at [github.com/lxc/lxc-ci](https://github.com/lxc/lxc-ci).

## Usage

Like [Docker](https://wiki.gentoo.org/wiki/Docker), Distrobuilder builds images by following definitions from a YAML file.

### The YAML template

The upstream Gentoo templates are intentionally kept very slim. What follows are some sensible improvements.

### Managing packages

Package management is determined by the packages: section:

- packages: demarcates the section.
- manager: tells distrobuilder what package manager the image uses. portage is predefined.
- update: If set to `true`, all installed packages are updated at the time the image is created. The upstream templates use the stage 3 tarball and have `update: false`, which means the repo directory in the container is empty and `portage` will fail. To avoid this, set `update: true`. Please note, setting `update: true` will make the image **larger**.
- cleanup: If set to true, the package manager will perform a cleanup operation to which usually cleans up cached files. It is not clear what operation this makes `portage` perform.
- sets: Contains a list of packages following a bullet point `-` and the keyword `remove`, an `action:`, and optional filters such as `variants` and `types`.  The packages are defined using standard portage package atoms; `action:` must be set to either `install` or `remove`. The `types` determine whether a container or a virtual machine is built; this is set via a switch to distrobuilder at the time of image creation. Similarly, `variants` allow different configurations based on the same base template.

### Create your own container images

You can create your own container images based on [Gentoo's downloadable stages](https://www.gentoo.org/downloads/). You can control the image variant with **-o source.variant** option, initd variant with **-o image.variant**, and architecture with **-o image.architecture**. See examples below.

For each example below create or download gentoo.yaml:

`user $````
mkdir -p ~/distrobuilder/gentoo
```
`user $````
cd ~/distrobuilder/gentoo
```
#### Example: LLVM container

To create an llvm container for example, follow these steps:

`user $````
cd ~/distrobuilder/gentoo
```
`user $````
mkdir llvm
```
`user $````
cd llvm
```
distrobuilder requires a root access, straight by **su**ing or **sudo**ing, because it needs access to /dev etc.

`root #``distrobuilder build-incus ../gentoo.yaml -o image.architecture=amd64 -o image.variant=openrc -o source.variant=llvm-openrc``user $````
incus image import incus.tar.xz rootfs.squashfs --alias gentoo-amd64-llvm
```
`user $````
incus launch gentoo-amd64-llvm gentoo-llvm-test
```
`user $````
incus exec gentoo-llvm-test bash
```
For an **x86** arch testing container on **amd64**, just specify architecture:

`root #``distrobuilder build-incus ../gentoo.yaml -o image.architecture=x86 -o image.variant=openrc -o source.variant=openrc`
#### Example: musl container

To create an musl container for example, follow these steps:

`user $````
cd ~/distrobuilder/gentoo
```
`user $````
mkdir musl
```
`user $````
cd musl
```
`root #``distrobuilder build-incus ../gentoo.yaml -o image.architecture=amd64 -o image.variant=openrc -o source.variant=musl-openrc``user $````
incus image import incus.tar.xz rootfs.squashfs --alias gentoo-amd64-musl
```
`user $````
incus launch gentoo-amd64-musl gentoo-musl-test
```
`user $````
incus exec gentoo-musl-test bash
```
### Building Windows Image

Distrobuilder can also create Windows Image for Incus with an official Windows iso file.

#### Dependencies

The package is not shipped with these dependencies, as their are only used for Windows building:

`root #``USE=fuse emerge --ask hivex wimlib`
#### Building your own image

You'll first need to download an official iso of Windows. Distrobuilder will download the drivers to pack them with it. You just have to mount the iso inside a Incus VM and install it.

`root #``distrobuilder repack-windows Win10.iso Win10-distrobuilder.iso`
