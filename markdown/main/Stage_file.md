<!-- source: https://wiki.gentoo.org/wiki/Stage_file | group: Gentoo Wiki (Main) | wiki-title: Stage file -->
---
title: Stage file
url: https://wiki.gentoo.org/wiki/Stage_file
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-15"
fingerprint: "943abaea6c8b0fa6"
license: CC BY-SA 4.0
---

# Stage file

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A **stage file** (also known as a *stage tarball*) is an archive that contains all the files to run a minimal Gentoo environment, typically to serve as a seed for a Gentoo installation.

Gentoo provides a selection of stage 3 files for [download](https://www.gentoo.org/downloads/), to cater to different installation configurations, on supported architectures.

Other stage files are used for internal Gentoo Linux development purposes. Customized stage files can also be [created](https://wiki.gentoo.org#Creating_stage_files) for specific requirements, if needed.

## Official Gentoo stage files

Gentoo's [Release Engineering](https://wiki.gentoo.org/wiki/Project:RelEng) team offers stage 3 files for [download](https://www.gentoo.org/downloads/) for installing Gentoo Linux. They are distributed via the [mirror system](https://www.gentoo.org/downloads/mirrors/).

The Gentoo stage files are built for a given [architecture](https://wiki.gentoo.org/wiki/Handbook:Main_Page#Architectures) for selected [profiles](https://wiki.gentoo.org/wiki/Portage/Profiles).

The internal system uses [Catalyst](https://wiki.gentoo.org/wiki/Catalyst) to create the stage files according to a set of [spec files](https://gitweb.gentoo.org/proj/releng.git/tree/releases/specs/amd64).

### Choosing a stage file for installation

First think about what a system will be used for, the two main consideration are:

- If the installation is to be a desktop system to be used with a graphical interface, or system purely used form the Command Line Interface, such as a server system
- Whether the system will use the OpenRC or systemd init system

The choice of stage 3 to use for installation is tightly coupled with what profile will be selected later on in the installation. While it's possible to easily switch between certain profiles after installation, other switches can be more involved (e.g. switching init systems), with some requiring substantial effort and consideration, or even be impossible without complete re-installation. Thus it is important to choose wisely, and conservative choices are recommend for anyone who doesn't have specifc needs that require other stage 3/profile choices.

The most basic stages will only have the name of the init system in their name (OpenRC or systemd),  *stage3-\<architecture>-openrc* or *stage3-\<architecture>-systemd*. These stages are for Command Line Interface only systems, such as for servers, for use in containers, etc.

### Desktop stages

Desktop stages target a desktop environments (graphical/GUI). The USE flags on desktop stage file are fine tuned to enable all the basic requirements a desktop system will need such as video and audio playback.

These stages include packages such as [llvm-core/llvm](https://packages.gentoo.org/packages/llvm-core/llvm) and [dev-lang/rust-bin](https://packages.gentoo.org/packages/dev-lang/rust-bin) and can save many hours off the install, over trying to convert one of the other stages to a desktop system.

These stage 3 files have the word "desktop" in their name.

### Experimental stages

These are often used by [testers](https://wiki.gentoo.org/wiki/Project:AMD64_Arch_Testers). They should only be used when specifically required, for general-use pick a *stable* stage file.

[LLVM](https://wiki.gentoo.org/wiki/LLVM) and/or [Musl](https://wiki.gentoo.org/wiki/Musl) stages are for advanced and specific installs types. As a general rule thumb the user will know when there is a need for them, rather then looking for a need to use one.

Note that musl users will find many common programs no longer being able to work without the use of workarounds. [Steam](https://wiki.gentoo.org/wiki/Steam) is  one of the common issues user have with not working under musl.

LLVM users will find many programs are no longer able to compile for various reasons. For that reason alone it is expected for users to be able to report issues, look for patches or write their own and report this back to Gentoo and upstream project where possible.

It is not possible to switch back to glibc or GCC system without reinstalling Gentoo.

### No-multilib stages

The multilib profile uses 64-bit libraries when possible, and falls back to the 32-bit versions when necessary. This is an excellent option for the majority of installations because it provides a great amount of flexibility for customization in the future.

Selecting a no-multilib tarball to be the base of the system excludes 32-bit system libraries. This will stop some apps from working, such as most games on [Steam](https://wiki.gentoo.org/wiki/Steam). There are workarounds for this, however they can be very involved, so if any use of 32 bit software might be required in the future, no-multilib stage file should not be used.

It should be noted that if a user wishs to switch back to multilib at a later time, then reinstalling is often the only practical way to do so.

## Stages by number

### Stage 3

Stage 3 files are available on main website's [downloads page](https://www.gentoo.org/downloads/) and are hosted on [distfiles.gentoo.org](https://distfiles.gentoo.org/releases/) (navigate to the \<arch>/autobuilds/ directory).

Downloading and decompressing a stage 3 file, as described in the handbook's [stage file section](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage), will install an almost-complete and mostly-functional system (the most important parts still missing are a kernel and a bootloader).

Stage 3 files are compiled from stage 2 files, but contain a [@system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>) of packages.

Not including sub-profiles, the base file used for the system set of all profiles can be found at: [/var/db/repos/gentoo/profiles/base/packages](https://gitweb.gentoo.org/repo/gentoo.git/tree/profiles/base/packages).

### Stage 4

Official "stage 4" files were [previously available](https://blog.mthode.org/posts/2016/Jan/stage4-tarballs-minimal-and-cloud/) in January 2016 for the **amd64** architecture.

A cloud "stage 4" has been created to aid in the process of virtual machine provisioning. These stage 4 files can be used with diskimage-builder (available via [app-emulation/diskimage-builder](https://packages.gentoo.org/packages/app-emulation/diskimage-builder)). See the [Gentoo README upstream](https://github.com/openstack/diskimage-builder/tree/master/diskimage_builder/elements/gentoo) and [official diskimage-builder documentation](https://docs.openstack.org/developer/diskimage-builder/) for more information.

[Catalyst](https://wiki.gentoo.org/wiki/Catalyst/Stage_Creation) is a tool for power users to create a stage4 with all the packages they require by default.

### Internal development stages

Being mostly for development purposes, stage 1 or stage 2 files are unavailable for download.

#### Stage 1

Stage 1 files are generated from a packages.build file (each [system profile](<https://wiki.gentoo.org/wiki/Profile_(Portage)>) may have a *slightly* different packages.build file).

#### Stage 2

Stage 2 files contain the same packages as a stage 1 file with one caveat: stage 2 files are compiled *from* a stage 1 file. This is to ensure the stage 1 file contains the tool chain necessary to reproduce itself.

## Creating stage files

Stage files are [generated with catalyst](https://wiki.gentoo.org/wiki/Catalyst/Stage_Creation) using appropriate [specs files](https://wiki.gentoo.org/wiki/Catalyst#Specs_files).

## See also

- [Bootable media](https://wiki.gentoo.org/wiki/Bootable_media) — Gentoo offers **bootable media** that can be used to [install](https://wiki.gentoo.org/wiki/Installation), maintain, or try out Gentoo Linux
- [Installation](https://wiki.gentoo.org/wiki/Installation) — an overview of the principles and practices of installing Gentoo on a running system.
- [Live image](https://wiki.gentoo.org/wiki/Live_image) — an operating system (OS) environment contained within a file that can be used to [boot](https://en.wikipedia.org/wiki/Booting) a system

## External resources

- [https://www.gentoo.org/downloads/](https://www.gentoo.org/downloads/) - Gentoo download page with links to official stage files.
