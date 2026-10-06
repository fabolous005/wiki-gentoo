<!-- source: https://wiki.gentoo.org/wiki/BOINC | group: Gentoo Wiki (Main) | wiki-title: BOINC -->
---
title: BOINC
url: https://wiki.gentoo.org/wiki/BOINC
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-03"
fingerprint: fe06bd78d26f8986
license: CC BY-SA 4.0
---

# BOINC

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**BOINC** *(Berkeley Open Infrastructure for Network Computing)* is a software system for volunteer computing. It lets people donate time on their home computers and smartphones to science research projects.

## Installation

### Kernel

To run some projects, you need vsyscall emulation enabled:

**Enable vsyscall support**

```
Processor type and features --->
    vsyscall table for legacy applications (None) --->
        (X) Emulate
```
### USE flags


### USE flags for
            [sci-misc/boinc](https://packages.gentoo.org/packages/sci-misc/boinc)
            
            The Berkeley Open Infrastructure for Network Computing

| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [cuda](https://packages.gentoo.org/useflags/cuda) | Enable NVIDIA CUDA support (computation on GPU) | 
| [gui](https://packages.gentoo.org/useflags/gui) | Enable support for a graphical user interface | 
| [opencl](https://packages.gentoo.org/useflags/opencl) | Enable OpenCL support (computation on GPU) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 

### Emerge

`root #``emerge --ask sci-misc/boinc`
### Additional software

- [app-admin/boinctui::guru](https://gpo.zugaina.org/Overlays/guru/app-admin/boinctui) – a curses-based terminal BOINC client manager.
- [net-p2p/gridcoin::guru](https://gpo.zugaina.org/Overlays/guru/net-p2p/gridcoin) – a cryptocurrency rewarding volunteer computing.

### Applications

Some applications can be built from source with CPU-native optimizations and used via [anonymous platform](https://boinc.berkeley.edu/wiki/Anonymous_platform). A couple of them are currently packaged in [GURU](https://wiki.gentoo.org/wiki/GURU):

- [sci-biology/cmdock::guru](https://gpo.zugaina.org/Overlays/guru/sci-biology/cmdock) – application for the [SiDock@home](https://sidock.si/sidock/) project.
- [sci-biology/geneathome::guru](https://gpo.zugaina.org/Overlays/guru/sci-biology/geneathome) – application for the [TN-Grid](http://gene.disi.unitn.it/test/)'s gene@home project.

## Configuration

To learn about configuration files and environment variables, refer to the [Client configuration](https://boinc.berkeley.edu/wiki/Client_configuration) article of the BOINC user manual.

### Permissions

To be able to use [CUDA](https://wiki.gentoo.org/index.php?title=CUDA&action=edit&redlink=1) or [OpenCL](https://wiki.gentoo.org/wiki/OpenCL), you should add the boinc user to the video group:

`root #``gpasswd -a boinc video`
To use the BOINC Manager and the boinccmd command-line tool, add youself to the boinc group<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>:

`root #``gpasswd -a <user> boinc`
Changes will not take effect until you sign out and then sign in again (re-login).

### Service

#### OpenRC

`root #````
rc-service boinc start
```
`root #````
rc-update add boinc default
```
The OpenRC service also supports some common tasks, such as attaching to a new project and suspending work.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose sci-misc/boinc`
Finally, remove BOINC's state directory:

`root #``rm -r /var/lib/boinc`
