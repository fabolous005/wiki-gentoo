<!-- source: https://wiki.gentoo.org/wiki/Crossdev/Testing_packages_via_qemu | group: Gentoo Wiki (Main) | wiki-title: Crossdev/Testing packages via qemu -->
---
title: Crossdev/Testing packages via qemu
url: https://wiki.gentoo.org/wiki/Crossdev/Testing_packages_via_qemu
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-21"
fingerprint: d7c21cba274ddf84
license: CC BY-SA 4.0
---

# Crossdev/Testing packages via qemu

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This wiki page explains the required steps needed to be able to test packages created by [Crossdev](https://wiki.gentoo.org/wiki/Crossdev) automatically via `FEATURES="test"`, [QEMU](https://wiki.gentoo.org/wiki/QEMU) and `binfmt_misc`. Will be examined example when desired arch for testing is **arm64** and is different from the running system (e.g. **amd64**). Packages in this example are compiled via a cross toolchain made by `crossdev` without QEMU emulation.

### Basics

QEMU has two modes: full-system emulation or userspace emulation.

The ones with `qemu-system-*` are for full-system emulation.

The ones with `qemu-*` are for userspace emulation.

On the page we won't use a chroot to a downloaded stage3, only what `crossdev` has built, therefore userspace emulation is used.

### Prerequisites

Please, check [Embedded\_Handbook/General/Compiling\_with\_QEMU\_user\_chroot](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Compiling_with_QEMU_user_chroot) about installation of QEMU.

Also install [Crossdev](https://wiki.gentoo.org/wiki/Crossdev#Installation).

### Crossdev

To setup a crossdev environment:

`root #``FEATURES="-test" USE="-pgo cxx" crossdev -s4 --target aarch64-unknown-linux-gnueabi -oO /var/db/repos/crossdev/`
### QEMU wrapper

We'll need to pass additional args to QEMU (some args to make linkage work correctly), so we'll create a wrapper script (in C) that'll call QEMU with it:

**`qemu-wrapper.c`**

```
/*
 * Pass arguments to qemu binary
 */
#include <string.h>
#include <unistd.h>
int main(int argc, char **argv, char **envp) {
	char *newargv[argc + 5];
	newargv[0] = argv[0];
    newargv[1] = "-L";
	newargv[2] = "/usr/aarch64-unknown-linux-gnueabi/"; /* here you can set the correct folder made by crossdev */
	newargv[3] = "-E";
	newargv[4] = "LD_LIBRARY_PATH=/usr/lib/gcc/aarch64-unknown-linux-gnueabi/14/"; /* here you should set correct path folder with i.e. libstdc++.so.6 */
	memcpy(&newargv[5], &argv[1], sizeof(*argv) * (argc -1));
	newargv[argc + 4] = NULL;
	return execve("/usr/bin/qemu-aarch64", newargv, envp);
}
```
Compile the wrapper with:

`root #``gcc -static qemu-wrapper.c -O3 -s -o qemu-wrapper`
Then it's preferably to copy the resulted binary into a visible folder for other users (i.e. `portage`) i.e. `/usr/aarch64-unknown-linux-gnueabi/qemu-wrapper` and make it executable, if it's not:

`root #``cp ./qemu-wrapper /usr/aarch64-unknown-linux-gnueabi/``root #``chmod +x /usr/aarch64-unknown-linux-gnueabi/qemu-wrapper`
### Embedding qemu-wrapper

Follow [Embedded\_Handbook/General/Compiling\_with\_QEMU\_user\_chroot#Configuration](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Compiling_with_QEMU_user_chroot#Configuration) to enable handling execution of binaries of other architectures via QEMU.

Then, according to the [Embedded\_Handbook/General/Compiling\_with\_QEMU\_user\_chroot#Additional\_Usage](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Compiling_with_QEMU_user_chroot#Additional_Usage), for aarch64 we can embed the `qemu-wrapper`:

`root #``echo ':aarch64:M::\x7fELF\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x02\x00\xb7\x00:\xff\xff\xff\xff\xff\xff\xff\xfc\xff\xff\xff\xff\xff\xff\xff\xff\xfe\xff\xff\xff:/usr/aarch64-unknown-linux-gnueabi/qemu-wrapper:' > /proc/sys/fs/binfmt_misc/register`
This is it. Now it's possible to compile via crossdev and run tests via qemu-wrapper.

### Example of usage

TODO: insert an example

### Troubleshooting

#### numpy failed to configure in cross environment

See [bug #751325](https://bugs.gentoo.org/show_bug.cgi?id=751325) for a solution.

#### scipy failed to compile in cross environment

See [bug #950049](https://bugs.gentoo.org/show_bug.cgi?id=950049), [bug #950050](https://bugs.gentoo.org/show_bug.cgi?id=950050), [bug #950205](https://bugs.gentoo.org/show_bug.cgi?id=950205) for solutions.
