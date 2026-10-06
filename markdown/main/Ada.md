<!-- source: https://wiki.gentoo.org/wiki/Ada | group: Gentoo Wiki (Main) | wiki-title: Ada -->
---
title: Ada
url: https://wiki.gentoo.org/wiki/Ada
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-07"
fingerprint: de05bf9d9db5a38a
license: CC BY-SA 4.0
---

# Ada

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Ada** is a programming language designed for large, long-lived applications — and embedded systems in particular — where safety and security are essential.

Ada is used by a number of the packages in the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository).

## Installation

The Ada community has in recent years moved towards Alire, a [language-specific package manager](https://wiki.gentoo.org/wiki/Application_level_package_management). AdaCore discontinued GNAT Community (packaged as [dev-lang/gnat-gpl](https://packages.gentoo.org/packages/dev-lang/gnat-gpl)) in favor of this. However, GNAT Community still has value in Gentoo for bootstrapping a newer version of GNAT in [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc).

Following recent changes for [bug #547358](https://bugs.gentoo.org/show_bug.cgi?id=547358), installing GCC's Ada compiler (GNAT) should be far easier.

### Emerge

Remove any legacy `PATH` hacks or configuration in /etc/portage/make.conf set from previous attempts to install/use GNAT.

First, enable [ada](https://packages.gentoo.org/useflags/ada) [for](https://wiki.gentoo.org/wiki/USE_flag) [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc):

**`/etc/portage/package.use`**

```
sys-devel/gcc ada
```
Then rebuild GCC:

`root #``emerge --ask --oneshot --changed-use sys-devel/gcc`
### Alire setup

Alire can be used without installing the toolchain locally. If the *external* toolchain from Gentoo's repositories should be used, [gcc](https://wiki.gentoo.org/wiki/Gcc) has to be recompiled (as mentioned above), and [dev-ada/gprbuild](https://packages.gentoo.org/packages/dev-ada/gprbuild) has to be emerged.

### Emacs setup

As at 2024-03-13, ada-mode is effectively unmaintained, due to the primary developer having retired.[\[1\]](https://wiki.gentoo.org#cite_note-1)

A maintained alternative for users of Emacs 29 and above is [ada-ts-mode](https://github.com/brownts/ada-ts-mode), which uses [Tree-sitter](https://tree-sitter.github.io/) and the [Language Server Protocol](https://en.wikipedia.org/wiki/Language_Server_Protocol) (LSP). Simon Wright has written [a guide to setting up ada-ts-mode](https://forward-in-code.blogspot.com/2025/01/ada-ts-mode.html).

Otherwise, to set up ada-mode:

Install ada-mode via [Alire](https://alire.ada.dev/); download and install the current Linux version of Alire if necessary.

If use of Alire is not possible for some reason, ensure [dev-ada/gnatcoll-core](https://packages.gentoo.org/packages/dev-ada/gnatcoll-core) is installed:

`root #``emerge --ask dev-ada/gnatcoll-core`
Install the latest version of ada-mode, using your preferred method of managing Emacs packages. An [app-emacs/ada-mode::gnu-elpa](https://gpo.zugaina.org/Overlays/gnu-elpa/app-emacs/ada-mode) package is available via [the gnu-elpa overlay](https://gpo.zugaina.org/Overlays/gnu-elpa).

On the command line, change to the directory in which ada-mode has been installed, e.g.:

`user $``cd ~/.config/emacs/elpa/ada-mode-8.1.0/`
and then run:

`user $``./build.sh # This can be memory-intensive!`
If the Alire alr binary is installed and configured correctly, it will be used for the build-and-install process. Otherwise, the install.sh will need to be run:

`user $``./install.sh`
By default, files are installed into ${HOME}/.local/bin/, as the appropriate [XDG directory](https://wiki.gentoo.org/wiki/XDG_directories). To specify a different location for the bin directory, pass it as the first argument to install.sh.

If not using Alire, version 8.1.0 of ada-mode requires copying the file ada\_annex\_p\_lr1\_re2c\_parse\_table.txt from the top level of the ada-mode-8.1.0 directory to the relevant installation location.

Add the installation location to the PATH, e.g. via .bash\_profile:

**`~/.bash_profile`**

```
export PATH="${PATH}:${HOME}/.local/bin"
```
and restart the login session.

Depending on the installation method, ada-mode might need to be enabled manually in the [Emacs](https://wiki.gentoo.org/wiki/Emacs) configuration file:

**`~/.config/emacs/init.el`**

```
(require 'ada-mode)
```
By default, **wisi** is used for things like faces (`ada-face-backend`) and indentation (`ada-indent-backend`); however, the AdaCore ada\_language\_server (als) can be used for cross-references, via **eglot**. Refer to the **ada** customize-group and [the ada-mode home page](https://www.nongnu.org/ada-mode/) for further information.

## Troubleshooting

### can't find a native toolchain for language 'ada'

Depending on the language configuration Alire does find the external toolchain with alr toolchain --select, but gprbuild that is used under the hood, does not. That is because gprbuild depends on gprconfig to detect suitable compilers<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>. gprconfig uses regular expressions stored in /usr/share/gprconfig/compilers.xml to detect the toolchain. For `ADA_TARGET=gcc_14` that would be, among others, the `GNAT`-section.

The relevant regular expressions are

- `gcc_version = ^[-\w]*gcc \S+ (\S+)`
- `gcc_version_major = ^[-\w]*gcc \S+ (\d+)\.\d+\.\d+`

that are matched on the output of gcc-14 -v.

`user $``gcc-14 -v` Using built-in specs.
COLLECT\_GCC=gcc-14
COLLECT\_LTO\_WRAPPER=/usr/libexec/gcc/x86\_64-pc-linux-gnu/14/lto-wrapper
OFFLOAD\_TARGET\_NAMES=nvptx-none
OFFLOAD\_TARGET\_DEFAULT=1
Target: x86\_64-pc-linux-gnu
Configured with: /var/tmp/portage/sys-devel/gcc-14.2.1\_p20241221/work/gcc-14-20241221/configure --host=x86\_64-pc-linux-gnu --build=x86\_64-pc-linux-gnu --prefix=/usr --bindir=/usr/x86\_64-pc-linux-gnu/gcc-bin/14 --includedir=/usr/lib/gcc/x86\_64-pc-linux-gnu/14/include --datadir=/usr/share/gcc-data/x86\_64-pc-linux-gnu/14 --mandir=/usr/share/gcc-data/x86\_64-pc-linux-gnu/14/man --infodir=/usr/share/gcc-data/x86\_64-pc-linux-gnu/14/info --with-gxx-include-dir=/usr/lib/gcc/x86\_64-pc-linux-gnu/14/include/g++-v14 --disable-silent-rules --disable-dependency-tracking --with-python-dir=/share/gcc-data/x86\_64-pc-linux-gnu/14/python --enable-languages=c,c++,fortran,ada --enable-obsolete --enable-secureplt --disable-werror --with-system-zlib --enable-nls --without-included-gettext --disable-libunwind-exceptions --enable-checking=release --with-bugurl=https://bugs.gentoo.org/ --with-pkgversion='Gentoo 14.2.1\_p20241221 p7' --with-gcc-major-version-only --enable-libstdcxx-time --enable-lto --disable-libstdcxx-pch --enable-shared --enable-threads=posix --enable-\_\_cxa\_atexit --enable-clocale=gnu --enable-multilib --with-multilib-list=m32,m64 --disable-fixed-point --enable-targets=all --enable-offload-defaulted --enable-offload-targets=nvptx-none --enable-libgomp --disable-libssp --enable-libada --enable-cet --disable-systemtap --disable-valgrind-annotations --disable-vtable-verify --disable-libvtv --with-zstd --without-isl --enable-default-pie --enable-host-pie --enable-host-bind-now --enable-default-ssp --disable-fixincludes --with-build-config=bootstrap-cet
Thread model: posix
Supported LTO compression algorithms: zlib zstd
gcc version 14.2.1 20241221 (Gentoo 14.2.1\_p20241221 p7)

On the last line the *gcc version* is printed: `gcc version 14.2.1 20241221 (Gentoo 14.2.1_p20241221 p7)`. gprconfig will scan that with the regular expressions and it will work fine. But image you are on a german system (`LANG=de_DE gcc -v`). In that case you will get `gcc-Version 14.2.1 20241221 (Gentoo 14.2.1_p20241221 p7)` and the regular expressions does not work anymore.

In fact that is a known issue on their issue tracker<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>.

As a workaround the regular expression can be patched like this:

`root #``sed -i 's/gcc \\S/gcc[- ]\\S/g' /usr/share/gprconfig/compilers.xml`
### sys-devel/gcc gnat1 unrecognized options during build

As noted in [bug #976762](https://bugs.gentoo.org/show_bug.cgi?id=976762), this can be a result of using `-march=native` in `*FLAGS` in [make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf).

To resolve this issue, temporarily drop `-march=native` from `*FLAGS`, build [GCC](https://wiki.gentoo.org/wiki/GCC) once, then restore the configuration.

## External resources

### Learning and reference

#### Advanced examples

### Groups

- [the Ada Forum on ada-lang.io](https://forum.ada-lang.io/)
- The [comp.lang.ada](news:comp.lang.ada) ([weblink](https://www.novabbs.com/devel/thread.php?group=comp.lang.ada)) newsgroup on Usenet. Also [readable via ada-lang.io](https://usenet.ada-lang.io/comp.lang.ada/)

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [Post on forum.ada-lang.io](https://forum.ada-lang.io/t/making-a-game-in-ada-with-raylib/704/19). Retrieved on 2024-03-13.
2. [↑](https://wiki.gentoo.org#cite_ref-2) [https://forum.ada-lang.io/t/gprconfig-cant-find-native-toolchain-for-language-ada/665](https://forum.ada-lang.io/t/gprconfig-cant-find-native-toolchain-for-language-ada/665). Retrieved on 2025-03-06.
3. [↑](https://wiki.gentoo.org#cite_ref-3) [https://github.com/AdaCore/gprconfig\_kb/issues/21](https://github.com/AdaCore/gprconfig_kb/issues/21). Retrieved on 2025-03-06.
