<!-- source: https://wiki.gentoo.org/wiki/Custom_Initramfs/Hibernation | group: Gentoo Wiki (Main) | wiki-title: Custom Initramfs/Hibernation -->
---
title: Custom Initramfs/Hibernation
url: https://wiki.gentoo.org/wiki/Custom_Initramfs/Hibernation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-02-18"
fingerprint: af819baba4b43feb
license: CC BY-SA 4.0
---

# Custom Initramfs/Hibernation

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes how to enable hibernation with a custom initramfs.

Hibernation (*suspend to disk*) is a special case. If you intend to use it in your [Custom Initramfs](https://wiki.gentoo.org/wiki/Custom_Initramfs), you need to be aware of the pitfalls, or you will suffer from utter and complete data loss. If you intend to use this, you should have a really good understanding of how Linux works, particularly how its various storage layers play together. Alternatives are available. You could Suspend to RAM, which does not involve *initramfs* at all. Or you could just shutdown properly...

This article assumes you already have written your custom and fully operational **/init** and now wish to add Hibernation support to it. If you are not using a [Custom Initramfs](https://wiki.gentoo.org/wiki/Custom_Initramfs), or for a less specific introduction to the topic, see [Suspend and hibernate](https://wiki.gentoo.org/wiki/Suspend_and_hibernate) instead.

## Theory

In theory, the process is simple. To suspend to disk, you could do something like

`root #``echo disk > /sys/power/state`
and the kernel will dump the entire memory state to your swap partition and power off the computer. When you turn it back on, your **/init** might do something like

`root #``echo /dev/SWAP > /sys/power/resume`
and the kernel will load the dump and continue where it left off.

### Pitfalls

Of course, things are not that simple. When you suspend to disk and the computer powers off - the computer isn't powered off at all! *It's still up and running!* Or at least, that's what the software believes. The box may be off physically, but from a kernel/userspace point of view,

- the system is still up and running,
- filesystems are still mounted read-write,
- although you forgot to save your thesis it's still open in the editor,
- your RAID is still in mid-reshape,
- your LVM is working hard to get those extents relocated,
- your database is still in the middle of a transaction,
- the movie you shot at your last vacation is still in the middle of re-encoding,

and so on and so forth. As you can see the machine is doing quite a lot while consuming zero power. It powered off without shutting down.

Do you see the problem yet? You turn your machine back on. Your *initramfs* does what it always does: it starts RAID (which automatically continues the reshape), LVM (might resume block relocations), maybe it mounts something because it needs a file or key or program, and then you resume.

The resume sets the state of memory back in time. It does not undo changes you made to disk, so ... suddenly the RAID reshapes already reshaped data (bye RAID), LVM relocates already relocated extents (bye LVM), the filesystem state is completely not what it's supposed to be (bye all your files), the machine crashes and your doctorate thesis is gone forever (use a typewriter next time, will you?).

Before Hibernation, the goal of the *initramfs* was to reach the root partition.

After Hibernation, the goal of the *initramfs* is to reach the swap partition while staying in hard read-only land.

## Kernel configuration

**CONFIG\_SWAP=y CONFIG\_HIBERNATION=y**

```
General setup  --->
    [*] Support for paging of anonymous memory (swap)
Power management and ACPI options  --->
    [*] Hibernation (aka 'suspend to disk')
```
