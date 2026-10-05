<!-- source: https://wiki.gentoo.org/wiki/QEMU/troubleshooting | group: Gentoo Wiki (Main) | wiki-title: QEMU/troubleshooting -->
---
title: QEMU/troubleshooting
url: https://wiki.gentoo.org/wiki/QEMU/troubleshooting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-06"
fingerprint: "961a991bc7a3c1d6"
license: CC BY-SA 4.0
---

# QEMU/troubleshooting

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

When trying to start a virtual machine, using the parameter `-cpu host`, one may encounter the following error:

In order to fix this, use the parameter `-enable-kvm`, which will enable KVM full virtualization support:

`user $``qemu-system-x86_64 -cpu host -enable-kvm [...]`
Sometimes, during the early boot splash, the following error message may be seen:

This indicates, that both, the Intel and the AMD kernel virtual machine settings, have been enabled in the kernel. To fix this, enable it as a *module* or disable either the Intel *or* AMD KVM option specific to the system's processor in the kernel configuration as described [above](https://wiki.gentoo.org/wiki/QEMU#Configuration).

Sometimes, this error can occur, if TUN/TAP support cannot be found in the kernel. To solve this, try loading the tun kernel module:

`root #``modprobe tun`
If this works, add `tun` to a file in /etc/modules-load.d/, so the kernel module will be loaded on every boot of the host system:

**`/etc/modules-load.d/qemu-modules.conf`**

This is usually the case, if QEMU is built without the `spice` USE flag. To resolve this issue, try to build QEMU [with the correct USE flag](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/USE#Declaring_USE_flags_for_individual_packages).

First add `spice` to via a [package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use) file:

**`/etc/portage/package.use/qemu`**

Then recompile the QEMU:

`root #``emerge --ask --changed-use app-emulation/qemu`
KVM only works for the same CPU architecture. An ARM64 host **cannot** handle x86\_64 instructions.

By default, [libvirt](https://wiki.gentoo.org/wiki/Libvirt) generates a random SELinux MCS label for the QEMU process, when it is started. If the loaded [SELinux](https://wiki.gentoo.org/wiki/SELinux) policy does not support MCS categories, the resulting security context will be invalid:

The solution is, either to *switch* to one of the policy types, which supports MCS categories or *manually set* the virtual machine's security labels, without MCS categories:

This is caused by a mismatch of [GCC](https://wiki.gentoo.org/wiki/GCC), where qemu is compiled against [sys-libs/zlib](https://packages.gentoo.org/packages/sys-libs/zlib) and [dev-libs/glib](https://packages.gentoo.org/packages/dev-libs/glib). This can be fixed, by *recompiling* both libraries, *before* recompiling [app-emulation/qemu](https://packages.gentoo.org/packages/app-emulation/qemu) again:

`root #``emerge --ask --oneshot sys-libs/zlib dev-libs/glib``root #``emerge --ask --oneshot app-emulation/qemu`
For *optimal performance*, it is recommended, that modern Windows guests (at least Windows 10 22H2 and up) run under a kernel with `CONFIG_KVM_HYPERV` enabled. If this Kernel driver is **not** enabled, VMs will fail to provision, to boot or run into a Blue Screen.

Later versions of Windows, if running as virtual machines, sometimes attempt to access hardware registers - specifically MSRs (**M**odel **S**pecific **R**egisters) - that are not actually defined for the emulated processor within the virtual environment. This is often, due to how Windows interacts with hardware, a driver trying to be overly clever or even bugs within the operating system itself. While these MSR accesses might be valid on physical processors, the virtualized environment presented by KVM may not support them.

KVM's default behavior is to attempt to emulate these MSR accesses, but when encountering an undefined register, it reports an *invalid instruction error* to the virtual Windows instance. This error is often fatal, resulting in a Blue Screen and *halting* the virtual machine.

An alternative is passing the kernel parameter `kvm.ignore_msrs=1` on the kernel command line or as a parameter to the kvm module:

**`/etc/modprobe.d/kvm.conf`**

The `ignore_msrs` parameter instructs KVM to ignore any attempts by the virtual Windows machine to access these undefined MSRs. Instead of generating an error and causing a Blue Screen, KVM silently bypasses the problematic instruction. This allows Windows to continue running, albeit potentially with some minor performance implications or masked underlying issues.

Attempting sound playback may output either choppy sound or none at all. A possible fix is to use these parameters<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>:

`user $``qemu-system-x86_64 -audiodev pa,id=Sound -device intel-hda -device hda-output,audiodev=Sound [...]`
