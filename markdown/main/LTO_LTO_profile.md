<!-- source: https://wiki.gentoo.org/wiki/LTO/LTO_profile | group: Gentoo Wiki (Main) | wiki-title: LTO/LTO profile -->
---
title: LTO/LTO profile
url: https://wiki.gentoo.org/wiki/LTO/LTO_profile
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-23"
fingerprint: "4e43017f2778a6a2"
license: CC BY-SA 4.0
---

# LTO/LTO profile

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Customized profiles**

It is possible to create a [custom profile](https://wiki.gentoo.org/wiki/Portage/Profiles/Custom_profiles) that globally enables [LTO](https://wiki.gentoo.org/wiki/LTO), with per-package LTO disabling for problematic packages.

This article assumes there is [already an ebuild repository set up](https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository) in which to create custom profiles.

### Creating the profile

Create a profile named `lto`:

`user /var/db/repos/myrepo $``cd /var/db/repos/myrepo``user /var/db/repos/myrepo $````
mkdir profiles/lto
```
`user /var/db/repos/myrepo $````
echo 8 > profiles/lto/eapi
```
`user /var/db/repos/myrepo $``echo $(portageq envvar ARCH) lto stable >> profiles/profiles.desc`
Create a make.defaults file that combines the `FLAGS` and `USE` changes required to enable LTO globally:

**`profiles/lto/make.defaults`**

```
# Enable LTO and warnings
# These warnings indicate likely runtime problems with LTO, so promote them
# to errors. If a package fails to build with these, LTO should not be used there.
CFLAGS="${CFLAGS} -flto -Werror=odr -Werror=lto-type-mismatch -Werror=strict-aliasing"
CXXFLAGS="${CFLAGS}"
FCFLAGS="${CFLAGS}"
FFLAGS="${CFLAGS}"
LDFLAGS="${CFLAGS} ${LDFLAGS}"
USE="${USE} lto"
```
### Enable the LTO profile

In order to use the profile, it must be added to the profile tree. Either add it to the **parent** of another profile that is in the tree of a profile that is currently selected, or add the existing profile as a **parent** of this new lto profile, and select this new lto profile as the system profile using [eselect profile](https://wiki.gentoo.org/wiki/Eselect#Profile).

Refer to the [custom profiles](https://wiki.gentoo.org/wiki/Portage/Profiles/Custom_profiles) article for further instructions.

### Disabling LTO for individual packages

As profiles don't support the same env method of modifying FLAGS as in /etc/portage/env, it's possible to use **profile-bashrcs** instead.

The custom repo's metadata/layout.conf must contain **profile-bashrcs** as enabled profile formats.

Since the [bashrc](https://wiki.gentoo.org/wiki//etc/portage/bashrc) file will be sourced multiple times during a single [emerge](https://wiki.gentoo.org/wiki/Emerge) run, simply appending flags to disable lto as described under [create nolto.conf](https://wiki.gentoo.org/wiki/LTO#Create_nolto.conf) will repeatedly append those flags. It will still work, but is not desirable, and is possible to work around.

Create a [bash](https://wiki.gentoo.org/wiki/Bash) function that strips the flags from the flag variables instead:

**`profiles/lto/profile.bashrc`**

```
# args: $1 = list of flags (IFS delimited)
stripflags(){
        for flags in CFLAGS CXXFLAGS FCFLAGS FFLAGS LDFLAGS; do
                _flags=
                for flag in ${!flags}; do
                        for stripflag in $1; do
                                if [ "$flag" == "$stripflag" ]; then
                                        continue 2
                                fi
                        done
                        _flags="$_flags $flag"
                done
                printf -v "$flags" '%s' "$_flags"
        done
}
```
Create a bashrc file that will strip the lto flags by calling the aforementioned function:

`user /var/db/repos/myrepo $``mkdir profiles/lto/bashrc`
**`profiles/lto/bashrc/striplto`**

```
 "-flto -Werror=odr -Werror=lto-type-mismatch -Werror=strict-aliasing"
```
Create the package.bashrc file that will source the bashrc file for each desired package:

`user /var/db/repos/myrepo $``mkdir profiles/lto/package.bashrc`
**`profiles/lto/package.bashrc/striplto`**

**Example package.bashrc/striplto file**

```
# disable LTO for problematic packages
sci-libs/opencascade striplto
dev-lang/rust striplto
sci-electronics/kicad striplto
sci-libs/vtk striplto
llvm-core/llvm striplto
llvm-core/clang striplto
sys-kernel/gentoo-kernel striplto
dev-qt/qtwebengine striplto
net-libs/webkit-gtk striplto
=app-office/abiword-3.0.6-r1 striplto
media-sound/audacity striplto
sys-libs/libcap-ng striplto
```
