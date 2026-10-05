<!-- source: https://wiki.gentoo.org/wiki/VMware | group: Gentoo Wiki (Main) | wiki-title: VMware -->
---
title: VMware
url: https://wiki.gentoo.org/wiki/VMware
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-10"
fingerprint: d3e73bde6daf225d
license: CC BY-SA 4.0
---

# VMware

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



VMware, Inc. sells a variety of closed-source hypervisors. "VMware" can refer to both the company or its products.

## Installation

When installing VMware Workstation on Gentoo you need to download the bundle from [VMware's website](https://blogs.vmware.com/workstation/2024/05/vmware-workstation-pro-now-available-free-for-personal-use.html).

`root #``bash VMware-Workstation-Full-16.1.2-17966106.x86_64.bundle`
### Required kernel options

To install and run VMware Workstation on Gentoo you need to enable next kernel options:

**Kernel configuration for VMware Workstation/Player hosts**

```
Processor type and features  --->
    [*] IOPERM and IOPL Emulation 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_X86_IOPL_IOPERM</code> to find this item.
[*] Enable loadable module support ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_MODULES</code> to find this item.
File systems  --->
    <*/M> FUSE (Filesystem in Userspace) support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_FUSE_FS</code> to find this item.
Device Drivers  --->
   Misc devices  --->
       <*/M> VMware VMCI Driver [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_VMWARE_VMCI</code> to find this item.
If you require VMware VMCI Sockets for host-guest/guest-guest communication, you need to also enable CONFIG\_VMWARE\_VMCI\_VSOCKETS. This is an optional feature and isn't required to run VMware guests.

**Optional configuration for VMware Workstation/Player hosts**

```
[*] Networking support  --->
    Networking options  --->
        <*/M> Virtual Socket protocol 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_VSOCKETS</code> to find this item.
        <*/M>   VMware VMCI transport for Virtual Sockets [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_VMWARE_VMCI_VSOCKETS</code> to find this item.
### Kernel modules

VMware Workstation 16.0 is known to support up to linux 5.8, and 16.1 works on 5.10 from my testing. If you use an older version of Workstation it will require an older kernel, 15.5 supports up to 5.4, 14.1.7 suppots up to 4.18, and 12.5.9 supports up to 4.12. After a kernel upgrade you will have to rebuild the VMware modules. You can do this by running the following command.

`root #``vmware-modconfig --console --install-all`

For kernels greater than 6.9 the following workaround enables VMWare Workstation 17.5:

`root #``cd vmware-host-modules/``root #``make``root #``make install`
To get modules working for VMware Workstation 17.6.3 you need to go deeper in the git and get some unmerged code (as of this writing). I tested this on a 6.12.25 kernel.

`user $``cd vmware-host/modules/``user $``git fetch origin pull/303/head:pr-303``user $``git checkout pr-303``user $``make``root #``make install`
### systemd services

If you are using systemd you might want to create some systemd service files to start VMware services on startup using systemd.

**`/etc/systemd/system/vmware.service`**

**`/etc/systemd/system/vmware-usbarbitrator.service`**

If you want to enable networking, add this service:

**`/etc/systemd/system/vmware-networks-server.service`**

If you want to connect to your VMware Workstation from another server:

**`/etc/systemd/system/vmware-workstation-server.service`**

### Uninstallation

If you use systemd and created systemd service files, you should delete them first:

`root #``systemctl disable --now vmware.service vmware-networks-server.service vmware-workstation-server.service``root #``rm /etc/systemd/system/vmware.service /etc/systemd/system/vmware.service /etc/systemd/system/vmware-networks-server.service /etc/systemd/system/vmware-workstation-server.service`
VMware Workstation has an uninstaller, and can be uninstalled.

`root #``vmware-installer -u vmware-workstation`
## Gentoo guests

Running Gentoo Linux as a guest inside of VMware Workstation requires enabling some kernel modules and installing [app-emulation/open-vm-tools](https://packages.gentoo.org/packages/app-emulation/open-vm-tools).

### Kernel Configuration

When working with VMware ESXi, despite the Ethernet emulator stating it would be an Intel e1000e, the guest OS was presented with an AMD Ethernet adapter. Both are included for completeness as well as the e1000.

The keywords for the above options are:

- CONFIG\_FUSION
- CONFIG\_FUSION\_SPI
- CONFIG\_NET\_VENDOR\_AMD
- CONFIG\_AMD8111\_ETH
- CONFIG\_PCNET32
- CONFIG\_NET\_VENDOR\_INTEL
- CONFIG\_E1000
- CONFIG\_E1000E
- CONFIG\_KEYBOARD\_ATKBD
- CONFIG\_VMWARE\_BALLOON
- CONFIG\_VMWARE\_VMCI
- CONFIG\_VMWARE\_PVSCSI
- CONFIG\_VMXNET3
- CONFIG\_MOUSE\_PS2\_VMMOUSE
- CONFIG\_DRM\_VMWGFX
- CONFIG\_DRM\_VMWGFX\_MKSSTATS
- CONFIG\_FB
- CONFIG\_FUSE\_FS

If you require VMware VMCI Sockets for host-guest/guest-guest communication, you need to also enable CONFIG\_VMWARE\_VMCI\_VSOCKETS. This is an optional feature and isn't required to run VMware guests.

### Emerge

Install [app-emulation/open-vm-tools](https://packages.gentoo.org/packages/app-emulation/open-vm-tools):

`root #``emerge --ask app-emulation/open-vm-tools`
### vmware-tools service

Start the service:

`root #``rc-service vmware-tools start`
And, add the vmware-tools service to the default run level.

`root #``rc-update add vmware-tools`
## Troubleshooting

### VMware's GUI window closes when the cursor is moved out of the window

This is due to a [x11-libs/libX11](https://packages.gentoo.org/packages/x11-libs/libX11) bug when running VMware under a Wayland session. As of December 6th, 2025, the patch that solves this has yet to be merged, but a temporary fix can be applied via [user patches](https://wiki.gentoo.org/wiki//etc/portage/patches#Adding_user_patches). The specific patch to be added to /etc/portage/patches/x11-libs/libX11-1.8.12 is from this merge request: [https://gitlab.freedesktop.org/xorg/lib/libx11/-/merge\_requests/293](https://gitlab.freedesktop.org/xorg/lib/libx11/-/merge_requests/293).

## See also

- [Virtualization](https://wiki.gentoo.org/wiki/Virtualization) — the concept and technique that permits running software in an environment separate from a computer operating system.
