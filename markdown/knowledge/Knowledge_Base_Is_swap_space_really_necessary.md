<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Is_swap_space_really_necessary | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Is_swap_space_really_necessary -->
---
title: Knowledge Base:Is swap space really necessary
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Is_swap_space_really_necessary
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-04-14"
fingerprint: bb3f3c091480adec
license: CC BY-SA 4.0
---

# Knowledge Base:Is swap space really necessary

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

During a Linux installation, the Gentoo Handbook [recommends to setup a swap space](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks#What_about_swap_space.3F) (preferably a partition) for the system.

Sometimes the question is asked: *is swap space really needed?*

## Environment

This article applies to all Gentoo Linux systems.

## Analysis

Linux uses a concept of *virtual memory*, causing applications to think they have access to all possible memory. Even more, they can see an entire address space at their disposal, regardless how much memory is really available in the system. This concept allows for simplified application development (as developers do not need to care that their programs will actually run simultaneously with others), isolation, etc. This also means that an applications' virtual memory needs to be mapped somehow to real, available memory. This is done by the Linux kernel's virtual memory management subsystem.

When the sum of all running applications (and their memory requirements) and the Linux kernel itself (as it also has need for memory) exceed the available physical memory in the system, then the system is in trouble: applications still see lots of free (virtual) memory, but there is shortage on real memory. When that happens, the Linux kernel has two choices:

1. If swap space is available, it will move some data from RAM to the swap space.
2. If swap space is not available, it will start killing applications to free memory.

## Resolution

It is not possible to give a clear cut answer to the question "Is swap space really needed". Of course, if a system is under "full load" and only a little memory available, then swap space should be available to have some "comfort space" when working on the system. On the other side of the spectrum, if a system is under a "full load" and lots of memory is available, then there might never be a need to move information to the swap space.

To err on the side of safety, the Handbook provides instructions to create swap space. Generally speaking, if the system actively uses swap space, then RAM should be added or memory load should be reduced (jobs killed, application closed, etc.) to free up virtual memory. Swapping to and from the hard drive can significantly increases wear and tear, especially on [solid state drives](https://wiki.gentoo.org/wiki/SSD) ([SSDs](https://wiki.gentoo.org/wiki/SSD)).

Swap space is also needed for [hibernation](https://wiki.gentoo.org/wiki/Suspend_and_hibernate). This feature saves the computer's state into swap space and then completely shuts it down, resulting in zero power consumption. The state will be restored upon turning the computer on.

## See also

- [All available memory is being used](https://wiki.gentoo.org/wiki/Knowledge_Base:All_available_memory_is_being_used) article on the Gentoo Wiki Knowledge Base.
- [SSD](https://wiki.gentoo.org/wiki/SSD) — provides guidelines for basic maintenance, such as enabling discard/trim support, for **SSD**s ([Solid State Drives](https://en.wikipedia.org/wiki/Solid-state_drive)) on Linux.
