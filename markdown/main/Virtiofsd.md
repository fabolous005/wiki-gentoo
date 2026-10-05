<!-- source: https://wiki.gentoo.org/wiki/Virtiofsd | group: Gentoo Wiki (Main) | wiki-title: Virtiofsd -->
---
title: Virtiofsd
url: https://wiki.gentoo.org/wiki/Virtiofsd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-06"
fingerprint: db99d919dbcb59e9
license: CC BY-SA 4.0
---

# Virtiofsd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

virtiofsd is a [Virtiofs](https://wiki.gentoo.org/wiki/Virtiofs) vhost-user device daemon that exports a
host filesystem to a virtual machine.

## Installation

### Emerge

`root #``emerge --ask app-emulation/virtiofsd`
### Additional software

[app-emulation/qemu](https://packages.gentoo.org/packages/app-emulation/qemu) or another compatible virtualization
stack is required to provide the Virtiofs device to the guest.

## Configuration

### Invocation

Export a host directory through a UNIX domain socket:

`root #``virtiofsd --socket-path=/run/virtiofsd.sock --shared-dir=/path/to/shared/directory`
The socket is connected to the virtualization software's Virtiofs device.

The filesystem tag is configured by the virtualization software and is used by the guest when mounting the filesystem.

### Environment variables

No environment variables are required.

### Files

[app-emulation/virtiofsd](https://packages.gentoo.org/packages/app-emulation/virtiofsd) does not require a dedicated configuration file.
Configuration is specified through command-line options.

## Usage

### Invocation

Start virtiofsd with a UNIX domain socket and shared directory:

`root #``/usr/libexec/virtiofsd --socket-path=/run/virtiofsd.sock --shared-dir=/path/to/shared/directory`
The socket is connected to the virtualization software's Virtiofs device.

### OpenRC

virtiofsd does not provide an OpenRC service. Start it as part of the virtualization configuration.

### systemd

virtiofsd does not provide an OpenRC service. Start it as part of the virtualization configuration.

For QEMU configuration, see [QEMU/Host § Virtiofs](https://wiki.gentoo.org/wiki/User:Egberts/Drafts/QEMU/Host#Virtiofs).

## Caveats

### Permissions

virtiofsd must be able to access the exported directory.

The daemon normally runs with elevated privileges and drops privileges where possible. Upstream supports running it as an unprivileged user with additional UID/GID considerations.

### UNIX domain socket

The socket used by virtiofsd must be accessible to the virtualization software providing the Virtiofs device.

## Tips

### Virtiofs tag

The filesystem tag is configured by the virtualization software rather than virtiofsd. The guest uses the tag when mounting the filesystem.

## Troubleshooting

### Guest cannot mount the filesystem

Verify that the guest kernel has Virtiofs support enabled and that the mount tag matches the tag configured for the Virtiofs device.

Verify the permissions and ownership of the exported directory.

### Socket connection fails

Verify that virtiofsd is running and that the virtualization software can access the configured UNIX domain socket.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-emulation/virtiofsd`
## See also

- [Virtiofs](https://wiki.gentoo.org/wiki/Virtiofs) — a shared file system that lets virtual machines access a directory tree on the host
- [QEMU/Host](https://wiki.gentoo.org/wiki/QEMU/Host) — host-side configuration and management of [QEMU](https://wiki.gentoo.org/wiki/QEMU) virtual machines
- [User:Egberts/Drafts/QEMU/Guest/Gentoo Linux](https://wiki.gentoo.org/wiki/User:Egberts/Drafts/QEMU/Guest/Gentoo_Linux)
