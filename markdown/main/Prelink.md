<!-- source: https://wiki.gentoo.org/wiki/Prelink | group: Gentoo Wiki (Main) | wiki-title: Prelink -->
---
title: Prelink
url: https://wiki.gentoo.org/wiki/Prelink
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-04-03"
fingerprint: b4c559cb818b8f06
license: CC BY-SA 4.0
---

# Prelink

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only. *Page archived as of **Feb 23, 2022**.*

TLDR:

**Do not use this article!**



This guide tells you how to make use of prelink support in Portage 2.0.46 and later.

## Introduction

### What is Prelink and how can it help me?

Most common applications make use of shared libraries. These shared libraries need to be loaded into memory at runtime and the various symbol references need to be resolved. For most small programs this dynamic linking is very quick. But for programs written in C++ and that have many library dependencies, the dynamic linking can take a fair amount of time.

On most systems, libraries are not changed very often and when a program is run, the operations taken to link the program are the same every time. Prelink takes advantage of this by carrying out the linking and storing it in the executable, in effect prelinking it.

Prelinking can cut the startup times of applications. For example, a typical KDE program's loading time can be cut by as much as 50%. The only maintenance required is re-running prelink every time a library is upgraded for a pre-linked executable.

### Summary

- Prelinking is done via a program called, surprisingly, `prelink` . It changes the binary to make it start faster.
- If an application's dependent libraries change after you have prelinked it, you need to re-prelink the application, otherwise you lose the speed advantage. This is to say, everytime you update a package via portage that updates libraries, they need to be re-prelinked.
- The change to the binary is fully reversible. `prelink` has an undo function.
- Current versions of Portage can handle, via `prelink` , the changing MD5sums and mtimes of the binaries.
- You do not need to set `FEATURES="prelink"` in your make.conf file; Portage will automatically support prelink if it can find the prelink binary.

## Setting up Prelink

### Installing the Programs

First you need to install the `prelink` tool. The emerge process automatically verifies that your system can prelink safely.

`root #``emerge --ask prelink`
A number of people get errors in emerging prelink because of the failed tests. The tests were put in for safety reasons, prelink's behavior is undefined if you disable them. The emerge errors are usually only dependent on the core packages; binutils, gcc, and glibc. Try re-emerging those packages in that order.

If you have a set of steps that reproduces the emerge error on another system please check [Bugzilla](https://bugzilla.gentoo.org) and create a bug report if it has not already been reported.

### Preparing your System

Also make sure that you do *not* have -fPIC set in your CFLAGS/CXXFLAGS. If you do, you will need to rebuild your entire system without.

### Configuration

Running `env-update` will generate the /etc/prelink.conf file that tells prelink which files to prelink.

`root #``env-update`
Unfortunately you cannot prelink files that were compiled by old versions of binutils. Most of these applications come from pre-compiled, binary only packages which are installed in /opt . Making the following file will tell prelink not to attempt to prelink them.

**`/etc/env.d/60prelink`**

```
PRELINK_PATH_MASK="/opt"
```
## Prelinking

### Prelink Usage

I use the following command to prelink all the binaries in the directories given by /etc/prelink.conf .

`root #``prelink -amR`
| The options explained: |  | 
|---|---|
| -a | "All": prelinks all the binaries | 
| -m | Conserve the virtual memory space. This is needed if you have a lot of libraries that need to be prelinked. | 
| -R | Random -- randomize the address ordering, this enhances security against buffer overflows. | 

### Prelink Cron Job

`sys-devel/prelink` installs a cron job in /etc/cron.daily/prelink . To enable it, edit the configuration file /etc/conf.d/prelink . This will run prelink daily in the background, as needed, saving you running the command manually.

### Speeding Up KDE After Prelinking

KDE's loading time can be greatly reduced after prelinking. If you inform KDE that it has been prelinked it will disable the loading of `kdeinit` (as it isn't required anymore) which speeds up KDE even more.

Set `KDE_IS_PRELINKED=1` in /etc/env.d/\*kdepaths\* to inform KDE about the prelinking.

### Removing prelink

If you ever change your mind about prelinking, before you unmerge prelink, you'll first need to remove the prelink cronjob from /etc/cron.daily and /etc/conf.d/prelink . Next, you'll have to remove prelinking from all binaries:

`root #``prelink -au`
Finally, unmerge the `prelink` package itself:

`root #``emerge -c prelink`
## Known Problems and Fixes

The cause of this problem is from badly compiled shared libraries that were compiled without the -fPIC gcc option for all their object files.

Here are the libraries that haven't been fixed or cannot be fixed:

- The libraries in the wine package, including winex. Prelinking wouldn't speed up MS Windows executables anyway.
- The library in media-video/mjpegtools, /usr/lib/liblavfile-1.6.so.0 .
- Nvidia OpenGl libraries, /usr/lib/opengl/nvidia/lib/libGL.so.\* . Due to performance reasons they were compiled without PIC support.

If your problem library was not listed please report it with, preferably, a patch to add `-fPIC` to the relevant CFLAGS.

### When I prelink my system some static binaries don't work anymore

Where glibc is concerned, there is no such thing as a 100% static binary. If you statically compile a binary with glibc, it may still depend on other system files. Below is an explanation by Dick Howell,

"I suppose the idea is that everything will be in the downloaded file, so nothing depends on the local libraries on the target system. Unfortunately with Linux, and I think anything else using GLIBC, this still isn't quite true. There's this "libnss" (name service switch, some people seem to call it network security system) which provides functions for accessing various databases for authentication, network information, and other things. It's supposed to make application programs independent of the separately configured actual network environment of the machine. A nice idea, but changes to GLIBC can lead to problems loading it. And you can't statically link "libnss", since it is configured for each machine individually. The problem comes, I think, mainly from statically linking other GLIBC libraries, notably "libpthread", "libm", and "libc", from which come incompatible calls to "libnss" functions."

### Prelink aborts with "prelink: dso.c:306: fdopen\_dso: Assertion \`j == k' failed."

This a known problem, kindly diagnosed [here](https://bugs.gentoo.org/show_bug.cgi?id=13878). Prelink cannot cope with UPX-compressed executables. As of prelink-20021213 there is no fix except to hide the executables while you are prelinking. See the section above for an easy way to do this.

### I use grsecurity and it seems that prelinking doesn't work.

In order to prelink on a system with grsecurity using a randomized mmap() base, it is necessary to turn "randomized mmap() base" OFF for /lib/ld-2.3.\*.so . This can be done with the `chpax` utility, but it must be done when the file is not in use (f.i. boot from a rescue CD).

### Prelink fails with error "prelink: Can't walk directory tree XXXX: Too many levels of symbolic links"

Your symlinks are nested too deeply. This happens when a symlink points to itself. For example, /usr/lib/lib -> lib is the most common. To fix this, you can find the symlink manually or use the utility provided by the `symlinks` package:

`root #````
emerge symlinks
```
`root #``symlinks -drv /`
More details can be found at [Bugzilla](https://bugs.gentoo.org/show_bug.cgi?id=82117) and this [forum post](https://forums.gentoo.org/viewtopic-t-458144-highlight-prelink.html) .

## Conclusion

Prelinking can drastically speed up the start up times for a number of large applications. Support is built into Portage. Prelinking is also safe as you can always undo the prelinking for any binary if you come across any problems. Just remember that when you update glibc or other libraries that you prelinked with, you need to rerun prelink ! In short good luck!
