<!-- source: https://wiki.gentoo.org/wiki/Minimizing_compilation_and_installation_time | group: Gentoo Wiki (Main) | wiki-title: Minimizing compilation and installation time -->
---
title: Minimizing compilation and installation time
url: https://wiki.gentoo.org/wiki/Minimizing_compilation_and_installation_time
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-12-07"
fingerprint: f711767ea5411781
license: CC BY-SA 4.0
---

# Minimizing compilation and installation time

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Most packages will install very quickly on reasonably fast modern hardware, but there are heavier packages, and a few very large ones. Here are some tips to minimize compilation and installation times on Gentoo.

Note that not all packages are compiled, in the usual sense: many are written in interpreted langues such as [Python](https://wiki.gentoo.org/wiki/Python) and do not always use a heavy build process. Some packages are installed directly in a precompiled "binary" format.

## Avoid unneeded packages

Installing software often pulls in dependencies, and some of these dependencies may be large packages. Inspect what will be installed when [emerging](https://wiki.gentoo.org/wiki/Emerge), and adjust [USE flags](https://wiki.gentoo.org/wiki/USE_flag) to avoid any unneeded dependencies, if possible.

### QtWebEngine

[QtWebEngine](https://wiki.gentoo.org/wiki/QtWebEngine) is a particularly large package that takes a long time to compile. Some packages will pull it in as a dependency by default, but this is not always required or desired.

To avoid having to compile [dev-qt/qtwebengine](https://packages.gentoo.org/packages/dev-qt/qtwebengine) as a dependency for certain packages, add `-webengine` to make.conf:

**`/etc/portage/make.conf`**

For some packages, the `gui, urlpreview, rss, qt5, pyqt5, dictionary-manager` and maybe other USE flags determine QtWebEngine dependency, so it may be useful to watch if theses flags are needed [for certain packages](https://wiki.gentoo.org/wiki//etc/portage/package.use) or not.

If QtWebEngine has already been pulled in as a dependency, and may now be removed, execute the following commands after modifying make.conf:

`root #``emaint sync --auto``root #``emerge --ask --verbose --update --deep --newuse @world``root #``emerge --ask --depclean`
Some packages depend unconditionally on QtWebEngine. If these packages are not needed, simply avoid installing them!

## Configure portage to optimize build times

Portage should be configured to use resources in a reasonable manner, in order to achieve the best possible build times. Generally things will be set up to use all processing threads available, memory capacity permitting.

Portage niceness may be set to allow using the system while Portage is merging packages, while minimally impacting build time.

The main variables to pay attention to are [EMERGE\_DEFAULT\_OPTS](https://wiki.gentoo.org/wiki/EMERGE_DEFAULT_OPTS) and [MAKEOPTS](https://wiki.gentoo.org/wiki/MAKEOPTS).

See [Portage niceness](https://wiki.gentoo.org/wiki/Portage_niceness) to set up Portage to let other processes be given priority, to allow full use of the system while installing or updating packages.

## Select compiler options for compilation speed

Some [CFLAGS](https://wiki.gentoo.org/wiki/CFLAGS) such as `-pgo` will result in much longer compile times. The `pgo` USE flag has the same effect.

## Alternative binary packages ("-bin" packages)

Some packages have a precompiled alternative, provided by the Gentoo developers. These allow fast installation of packages that otherwise take a particularly long time to compile. Note that the "-bin" packages referred to here are precompiled versions of packages available in the [Ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) - there are also binary packages in the repository for other reasons, such as for software that is only available in binary form.

The devs pay great attention to building these versions, there is usually no noticeable performance hit to using them, though usually any difference one way or the other is negligible.

For packages that have [USE flags](https://wiki.gentoo.org/wiki/USE_flag), the "-bin" versions use common defaults. If it is required to change the USE flags for a package, a "-bin" version may not be appropriate, though it is uncommon to have to do so.

### Substituting a source based dependency for "-bin" version

When installing a package would pull in a large package that would advantageously be replaced by it's "-bin" version, it is possible to substitute the former for the latter.

When noticing a large packing being pulled in as a dependency, first abort the merge before starting. Then emerge the "-bin" version without adding it to the [world file](<https://wiki.gentoo.org/wiki/Selected_set_(Portage)>):

`root #``emerge --oneshot <category>/<package>-bin`
Because dependencies on packages that have a "-bin" version are virtual, simply emerge the original package, and the "-bin" version will be used as the dependency.

## Make binary packages on a secondary machine

If another, maybe more powerful, machine is available, it could be set up to build [binary packages](https://wiki.gentoo.org/wiki/Binary_package_guide) to be used on the first machine with a [binhost](https://wiki.gentoo.org/wiki/Binary_package_guide#Setting_up_a_binary_package_host). Packages built this way may be shared among several machines, if they share the same architecture and use similar [USE flags](https://wiki.gentoo.org/wiki/USE_flag).

## Hardware

Of course, more powerful hardware will allow faster compilation times, though it will usually also be the most cumbersome and expensive way to make things faster. Some upgrades may be reasonable though, such as adding RAM on a memory limited machine (it's generally advised to have two GB per processor thread). Some systems have cheaper memory sticks available to them than others however.

An [SSD](https://wiki.gentoo.org/wiki/SSD), particularly an [NVMe](https://wiki.gentoo.org/wiki/NVMe) SSD, can sometimes be a reasonably cheap way to make installation faster, if coming from a traditional spinning hard drive.

## Put Portage TMPDIR on tmpfs

It is possible to have [Portage](https://wiki.gentoo.org/wiki/Portage) build packages in RAM with [tmpfs](https://wiki.gentoo.org/wiki/Tmpfs) instead of using a [Hard Disk Drive](https://wiki.gentoo.org/wiki/HDD) (HDD) or [Solid State Drive](https://wiki.gentoo.org/wiki/SSD) (SSD). This can sometimes speed up emerge times, for those who have enough RAM. See the [Portage TMPDIR on tmpfs](https://wiki.gentoo.org/wiki/Portage_TMPDIR_on_tmpfs) article.

## Tips

### --newuse vs --changed-use

The `--changed-use` parameter may be used to do less rebuilds while updating. Using the `--newuse` parameter however will let installed packages better reflect the state of the current [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository).

## See also

- [ccache](https://wiki.gentoo.org/wiki/Ccache) — helps avoid repeated recompilation for the same C and C++ object files by fetching the result from a cache directory.
- [distcc](https://wiki.gentoo.org/wiki/Distcc) — a program designed to distribute compiling tasks across a network to participating hosts.
- [genlop](https://wiki.gentoo.org/wiki/Genlop) — a utility for extracting information about emerged ebuilds from Portage log files - show build times
- [Portage with Git](https://wiki.gentoo.org/wiki/Portage_with_Git) — use [Git](https://wiki.gentoo.org/wiki/Git) to synchronize the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository)
- [qlop](https://wiki.gentoo.org/wiki/Q_applets#Extracting_information_from_emerge_logs_.28qlop.29) - q applets are fast Portage query utilities written in C,  show build times with qlop
