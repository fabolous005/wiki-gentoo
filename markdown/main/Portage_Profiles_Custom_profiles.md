<!-- source: https://wiki.gentoo.org/wiki/Portage/Profiles/Custom_profiles | group: Gentoo Wiki (Main) | wiki-title: Portage/Profiles/Custom profiles -->
---
title: Portage/Profiles/Custom profiles
url: https://wiki.gentoo.org/wiki/Portage/Profiles/Custom_profiles
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-24"
fingerprint: c303307ea7e5b76a
license: CC BY-SA 4.0
---

# Portage/Profiles/Custom profiles

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Customized profiles**

Users can create specialized, custom profiles not available in the Gentoo ebuild repository, and put them in an [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository).

## Creating custom profiles

Custom profiles can refer to profiles from the Gentoo ebuild repository in their parent file using the 'gentoo:' prefix, to avoid recreating all profile definitions, provided that the profile-formats key in metadata/layout.conf of the ebuild repository contains portage-2, as shown below. See also the [dedicated layout.conf article](https://wiki.gentoo.org/wiki/Repository_format/metadata/layout.conf).

Following are examples of a custom profile created locally in an ebuild repository named 'local'. The ebuild repository is assumed to be in /var/db/repos/local (i.e. [the default location](https://wiki.gentoo.org/wiki/Eselect/Repository#Files) for eselect repository), world-readable and owned by a non-root user. It is also assumed to contain a package dev-libs/test-package that installs libraries:

**`/etc/portage/repos.conf/local.conf`**

```
[local]
# 'eselect repository' default location
location = /var/db/repos/local
```
**`/var/db/repos/local/profiles/repo_name`**

**`/var/db/repos/local/metadata/layout.conf`**

```
# Slave repository rather than stand-alone
masters = gentoo
profile-formats = portage-2 # This line will be important, ensure It's there ; THIS LINE DOES NOT APPEAR IF create overlay with `eselect repository create local`, so you can add it in the case on your own.
```
`user $``ls -ld /var/db/repos/local`
drwxr-xr-x 4 user user 4096 Dec 15 11:50 /var/db/repos/local

### Example 1: Combining multiple profiles from the Gentoo ebuild repository

A profile named hardened-desktop will be created, which inherits settings from default/linux/amd64/23.0/hardened and targets/desktop in the main Gentoo ebuild repository.

First, create the hardened-desktop directory in the ebuild repository's profiles subdirectory:

`user $``(cd /var/db/repos/local/profiles && mkdir hardened-desktop && echo 8 > ./hardened-desktop/eapi )` Then, create the parent file:

**`/var/db/repos/local/profiles/hardened-desktop/parent`**

Finally, insert the newly created profile in the ebuild repository's profiles.desc file, giving it a `dev` 'stability indicator' for example:

`user $``echo $(portageq envvar ARCH) hardened-desktop dev >> /var/db/repos/local/profiles/profiles.desc`
Verify that eselect profile list lists the new profile:

`user $``eselect profile list`
Available profile symlink targets:
  ...
  \[99\]  local:hardened-desktop (dev)

### Example 2: Adding custom USE flags by profile

`user $``equery uses test-package`
\[ Legend : U - final flag setting for installation\]
\[        : I - package is installed with flag     \]
\[ Colors : set, unset
 \* Found these USE flags for dev-libs/test-package-1
 U I
 - - abi\_x86\_32 : 32-bit (x86) libraries
 - - extras     : Builds the libprofile-test-extra library

A profile named custom will be created, which inherits settings from default/linux/amd64/23.0 in the Gentoo ebuild repository, and two local profiles: 32bit, which sets the `abi_x86_32` USE flag and unsets the `extras` USE flag by default for the package, and with-extras, which does exactly the opposite, to illustrate final settings determination.

First, create the 32bit and with-extras directories in the ebuild repository's profiles subdirectory:

`user $``cd /var/db/repos/local/profiles``user $``mkdir 32bit with-extras && echo 8 >with-extras/eapi && && echo 8 >32bit/eapi`
Then create a package.use to change use flags of `dev-libs/test-package`:

**`/var/db/repos/local/profiles/32bit/package.use`**

**`/var/db/repos/local/profiles/with-extras/package.use`**

Then, create the custom directory:

`user $``mkdir custom && echo 8 >custom/eapi`
And add the parent file:

**`/var/db/repos/local/profiles/profile-name/parent`**

Finally, insert the newly created profile in the ebuild repository's profiles.desc file, giving it a `dev` 'stability indicator' for example:

`user $```echo `portageq envvar ARCH` $profile_name dev >>profiles.desc``
Verify that eselect profile list lists the new profile:

`user $``eselect profile list`
Available profile symlink targets:
  ...
  \[46\]  local:custom (dev)

Now it is possible to actually switch to the new profile using eselect profile set:

`root #``eselect profile set 46`
Check the result:

`user $``eselect profile show`
Current /etc/portage/make.profile symlink:
  local:custom

`user $``ls -l /etc/portage/make.profile`
lrwxrwxrwx 1 root root 40 Dec 15 12:00 /etc/portage/make.profile -> ../../var/db/repos/local/profiles/custom

This shows that eselect profile set updated the /etc/portage/make.profile symbolic link. In the general case, it would be necessary to run a world rebuild to apply the new settings to all packages, but checking first to make sure that the desired changes are going into effect with `--ask`:

`root #``emerge --ask --update --newuse --deep --complete-graph @world`
However, for this example, knowing that the profile settings only affect dev-libs/test-package and assuming that the previously chosen profile was default/linux/amd64/23.0, only that package's reinstallation will be done.

`user $``equery uses test-package`
\[ Legend : U - final flag setting for installation\]
\[        : I - package is installed with flag     \]
\[ Colors : set, unset
 \* Found these USE flags for dev-libs/test-package-1:
 U I
 + - abi\_x86\_32 : 32-bit (x86) libraries
 - - extras     : Builds the libprofile-test-extra library

This shows that because profile 32bit comes last in file custom/parent, its setting override those in profile with-extras, and the 32-bit version of the package's libraries is built.

**`test-package-1/Makefile`**

**Package's makefile snippet**

```
%.so: %.c
   @echo Building $@ for architecture $$ARCH and ABI $$ABI "(CHOST=$$CHOST)"
   $(CC) -shared -fPIC -Wl,-soname=$@.1 $(CPPFLAGS) $(CFLAGS) $(LDFLAGS) -o $@ $< $(LDLIBS)
```
**`/var/db/repos/local/dev-libs/test-package/test-package-1.ebuild`**

**Ebuild snippet**

```
EAPI=8
 
inherit toolchain-funcs multilib-build multilib-minimal
 
# ...
 
IUSE="extras" # abi_x86_32 comes from an eclass
 
# ...
 
multilib_src_compile() {
   tc-export CC
   emake extras=$(usex extras)
}
```
`root #``ACCEPT_KEYWORDS="~amd64" emerge --oneshot test-package`
Calculating dependencies  ... done!
 
>>> Verifying ebuild manifests
 
>>> Emerging (1 of 1) dev-libs/test-package-1::local
 \* test-package-1.tar.gz BLAKE2B SHA512 size ;-) ...                     \[ ok \]
>>> Unpacking source...
>>> Unpacking test-package-1.tar.gz to /var/tmp/portage/dev-libs/test-package-1/work
>>> Source unpacked in /var/tmp/portage/dev-libs/test-package-1/work
>>> Preparing source in /var/tmp/portage/dev-libs/test-package-1/work/test-package-1 ...
 \* Will copy sources from /var/tmp/portage/dev-libs/test-package-1/work/test-package-1
 \* abi\_x86\_32.x86: copying to /var/tmp/portage/dev-libs/test-package-1/work/test-package-1-abi\_x86\_32.x86
 \* abi\_x86\_64.amd64: copying to /var/tmp/portage/dev-libs/test-package-1/work/test-package-1-abi\_x86\_64.amd64
>>> Source prepared.
>>> Configuring source in /var/tmp/portage/dev-libs/test-package-1/work/test-package-1 ...
 \* abi\_x86\_32.x86: running multilib-minimal\_abi\_src\_configure
 \* abi\_x86\_64.amd64: running multilib-minimal\_abi\_src\_configure
>>> Source configured.
>>> Compiling source in /var/tmp/portage/dev-libs/test-package-1/work/test-package-1 ...
 \* abi\_x86\_32.x86: running multilib-minimal\_abi\_src\_compile
make extras=no

Building libprofile-test.so for architecture amd64 and ABI x86 (CHOST=i686-pc-linux-gnu)

x86\_64-pc-linux-gnu-gcc -m32 -shared -fPIC -Wl,-soname=libprofile-test.so.1  -O2 -pipe -Wl,-O1 -Wl,--as-needed -o libprofile-test.so libprofile-test.c
 \* abi\_x86\_64.amd64: running multilib-minimal\_abi\_src\_compile
make extras=no

Building libprofile-test.so for architecture amd64 and ABI amd64 (CHOST=x86\_64-pc-linux-gnu)

x86\_64-pc-linux-gnu-gcc -shared -fPIC -Wl,-soname=libprofile-test.so.1  -O2 -pipe -Wl,-O1 -Wl,--as-needed -o libprofile-test.so libprofile-test.c
>>> Source compiled.
>>> Test phase \[not enabled\]: dev-libs/test-package-1
 
>>> Install test-package-1 into /var/tmp/portage/dev-libs/test-package-1/image/ category dev-libs
 \* abi\_x86\_32.x86: running multilib-minimal\_abi\_src\_install
make DESTDIR=/var/tmp/portage/dev-libs/test-package-1/image/ libdir=/usr/lib32 extras=no install

Installing: libprofile-test.so

\* abi\_x86\_64.amd64: running multilib-minimal\_abi\_src\_install
make DESTDIR=/var/tmp/portage/dev-libs/test-package-1/image/ libdir=/usr/lib64 extras=no install

Installing: libprofile-test.so

\>>> Completed installing test-package-1 into /var/tmp/portage/dev-libs/test-package-1/image/
 
 \* Final size of build directory: 68 KiB
 \* Final size of installed tree:  32 KiB
 
strip: x86\_64-pc-linux-gnu-strip --strip-unneeded -R .comment -R .GCC.command.line -R .note.gnu.gold-version
   usr/lib32/libprofile-test.so.1
   usr/lib64/libprofile-test.so.1
 
>>> Installing (1 of 1) dev-libs/test-package-1::local
>>> Auto-cleaning packages...
 
>>> No outdated packages were found on your system.
 
 \* GNU info directory index is up-to-date.

Messages printed by the package's makefile (informational ones highlighted) show variables `ARCH`, `CHOST_x86`, `CFLAGS_x86` (which adds GCC's `-m32` option for 32-bit builds) and `LDFLAGS`, set by profile default/linux/amd64/23.0, are being inherited and are available in the ebuild's environment.

Now to see what happens if line order is changed in custom/parent:

`user $``cat <<EOF >$profile_name/parent`
\> gentoo:default/linux/amd64/23.0
> ../32bit
> ../with-extras
> EOF

`user $``equery uses test-package`
\[ Legend : U - final flag setting for installation\]
\[        : I - package is installed with flag     \]
\[ Colors : set, unset
\* Found these USE flags for dev-libs/test-package-1:
 U I
 - + abi\_x86\_32 : 32-bit (x86) libraries
 + - extras     : Builds the libprofile-test-extra library

This shows that because it now comes last in file custom/parent, settings in profile with-extras override those in profile 32bit, and, on a reinstallation, the 32-bit version of the package's libraries would be removed, and library libprofile-test-extra would be built:

`root #``ACCEPT_KEYWORDS="~amd64" emerge --oneshot test-package`
Calculating dependencies  ... done!
 
>>> Verifying ebuild manifests
 
>>> Emerging (1 of 1) dev-libs/test-package-1::local
 \* test-package-1.tar.gz BLAKE2B SHA512 size ;-) ...                     \[ ok \]
>>> Unpacking source...
>>> Unpacking test-package-1.tar.gz to /var/tmp/portage/dev-libs/test-package-1/work
>>> Source unpacked in /var/tmp/portage/dev-libs/test-package-1/work
>>> Preparing source in /var/tmp/portage/dev-libs/test-package-1/work/test-package-1 ...
 \* Will copy sources from /var/tmp/portage/dev-libs/test-package-1/work/test-package-1
 \* abi\_x86\_64.amd64: copying to /var/tmp/portage/dev-libs/test-package-1/work/test-package-1-abi\_x86\_64.amd64
>>> Source prepared.
>>> Configuring source in /var/tmp/portage/dev-libs/test-package-1/work/test-package-1 ...
 \* abi\_x86\_64.amd64: running multilib-minimal\_abi\_src\_configure
>>> Source configured.
>>> Compiling source in /var/tmp/portage/dev-libs/test-package-1/work/test-package-1 ...
 \* abi\_x86\_64.amd64: running multilib-minimal\_abi\_src\_compile
make extras=yes

Building libprofile-test.so for architecture amd64 and ABI amd64 (CHOST=x86\_64-pc-linux-gnu)

x86\_64-pc-linux-gnu-gcc -shared -fPIC -Wl,-soname=libprofile-test.so.1  -O2 -pipe -Wl,-O1 -Wl,--as-needed -o libprofile-test.so libprofile-test.c

Building libprofile-test-extra.so for architecture amd64 and ABI amd64 (CHOST=x86\_64-pc-linux-gnu)

x86\_64-pc-linux-gnu-gcc -shared -fPIC -Wl,-soname=libprofile-test-extra.so.1  -O2 -pipe -Wl,-O1 -Wl,--as-needed -o libprofile-test-extra.so libprofile-test-extra.c
>>> Source compiled.
>>> Test phase \[not enabled\]: dev-libs/test-package-1
 
>>> Install test-package-1 into /var/tmp/portage/dev-libs/test-package-1/image/ category dev-libs
 \* abi\_x86\_64.amd64: running multilib-minimal\_abi\_src\_install
make DESTDIR=/var/tmp/portage/dev-libs/test-package-1/image/ libdir=/usr/lib64 extras=yes install

Installing: libprofile-test.so libprofile-test-extra.so

\>>> Completed installing test-package-1 into /var/tmp/portage/dev-libs/test-package-1/image/
 
 \* Final size of build directory: 52 KiB
 \* Final size of installed tree:  28 KiB
 
strip: x86\_64-pc-linux-gnu-strip --strip-unneeded -R .comment -R .GCC.command.line -R .note.gnu.gold-version
   usr/lib64/libprofile-test.so.1
   usr/lib64/libprofile-test-extra.so.1
 
>>> Installing (1 of 1) dev-libs/test-package-1::local
>>> Auto-cleaning packages...
 
>>> No outdated packages were found on your system.
 
 \* GNU info directory index is up-to-date.

## Combining profiles

Profile settings can be combined in a cascading/stacking fashion, by including a file named parent in the directory that defines the profile. This makes the profile inherit settings from other profiles, which can be partially overriden. For example, by unsetting a USE flag by default that is set by default in a parent profile and vice-versa, unforcing or inverting its forced state, unmasking packages masked in a parent profile, adding or removing additional packages to/from the system set, etc. parent must contain a list of profile pathnames, relative to the directory that contains the file. As an extension, with portage-2 added to the profile-format key in metadata/layout.conf, [Portage](https://wiki.gentoo.org/wiki/Portage) also allows prepending an installed ebuild repository name (as specified in [/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf)) followed by a colon (':') to a profile pathname, which is then interpreted relative to the profiles subdirectory of the named ebuild repository.

Because parent profiles can also contain a parent file, an *inheritance tree* is produced as a result. The Package Manager Specification specifies how settings in each file of a profile directory (i.e. packages, make.defaults, package.use.force and package.use.mask, etc.) combine. In particular, it defines an ordering of parent profiles to determine the final settings: the inheritance tree is traversed [depth-first, left-to-right](https://en.wikipedia.org/wiki/Tree_traversal#Depth-first_search), with multiple occurrences of the same profile processed repeatedly. The left-to-right order is defined by line order in parent files.

The specification uniformly calls 'profile' all directories with suitable structure that are contained in an ebuild repository's profiles subdirectory. A subset designated as 'valid for use' must be listed in file profiles/profiles.desc, a text file that must contain lines with three fields separated by whitespace characters (space and TAB):

- The first one is an architecture, in the format valid as the value of `ARCH` (and for setting `KEYWORDS` in ebuilds), e.g. **amd64**, **arm**, **ppc64**, etc. These are listed in the main repository's profiles/arch.list file.
- The second one is a profile path name, relative to the profiles directory.
- The third and last one is a 'stability indicator'. The Gentoo ebuild repository, for instance, uses *stable*, *dev* and *exp* (experimental) for that field.

The eselect profile list command only shows profiles in profiles.desc files of all repositories configured in /etc/portage/repos.conf, with an architecture field that matches the machine's architecture. These are the ones that in most contexts are referred to as 'profiles' with no further qualification. Profiles not named in profiles.desc might be named in other profiles' parent files, and can be thought of as 'subprofiles' or 'profile building blocks'.

Following is an example that shows parent profiles and the inheritance tree for profile default/linux/amd64/23.0/desktop/gnome/systemd from the Gentoo ebuild repository. Profiles pathnames relative to the profiles subdirectory are shown so that the filesystem layout can be inferred, top-to-bottom ordering respects line ordering in relevant parent files.

default/linux/amd64/23.0/desktop/gnome/systemd  **17** 
 | **L** 
 +--> default/linux/amd64/23.0/desktop/gnome  **15** 
 |     | **L** 
 |     +--> default/linux/amd64/23.0/desktop  **12** 
 |     |     | **L** 
 |     |     +--> default/linux/amd64/23.0  **10** 
 |     |     |     | **L** 
 |     |     |     +--> default/linux/amd64  **3** 
 |     |     |     |     | **L** 
 |     |     |     |     +--> base  **1** 
 |     |     |     |     | **R** 
 |     |     |     |     +--> default/linux  **2** 
 |     |     |     |
 |     |     |     +--> arch/amd64/lib32  **7** 
 |     |     |     |     |
 |     |     |     |     +--> arch/amd64  **6** 
 |     |     |     |           | **L** 
 |     |     |     |           +--> arch/base  **4** 
 |     |     |     |           | **R** 
 |     |     |     |           +--> features/multilib  **5** 
 |     |     |     | **R** 
 |     |     |     +--> releases/23.0  **9** 
 |     |     |           |
 |     |     |           +--> releases  **8** 
 |     |     | **R** 
 |     |     +--> targets/desktop  **11** 
 |     | **R** 
 |     +--> targets/desktop/gnome  **14** 
 |           |
 |           +--> targets/desktop  **13** 
 | **R** 
 +--> targets/systemd  **16**


The **L** and **R** markers indicate the leftmost and rightmost branches in the corresponding tree representation, and the numbers represent the order in which profiles are considered for computing the final settings. Profiles with larger numbers override settings in profiles with smaller numbers. The following table gives a quick overview of notable settings for each of the intervening profiles, as well as the relevant files that implement them:



| Profile | Notable settings | Relevant file(s) | 
|---|---|---|
| base | Define most [USE\_EXPAND](https://wiki.gentoo.org/wiki/USE_EXPAND) and profile variables, define 'base' system set packages, set `KERNEL`, `ELIBC`, and `USERLAND` to `linux`, `glibc`, and `GNU`, respectively. | make.defaults, packages, use.force | 
| default/linux | Add packages considered essential for Linux to the system set, set USE flags, set default value of `LDFLAGS`, unmask Linux-specific USE flags | make.defaults, packages, use.mask, package.use.mask | 
| default/linux/amd64 | Add profiles base and default/linux to the inheritance tree | parent | 
| arch/base | Define the `ARCH` USE\_EXPAND variable, mask USE flags only supported for some architectures | make.defaults, use.mask, package.use.mask | 
| arch/amd64 | Set `ARCH` to `amd64`, set `CHOST`, `ABI`, `MULTILIB_ABIS` and `DEFAULT_ABI` appropriately, set default values of `ACCEPT_KEYWORDS`, `CFLAGS`, `CXXFLAGS`, `FFLAGS`, and `FCFLAGS`, define the `CPU_FLAGS_X86` USE\_EXPAND variable, unmask USE flags supported for **amd64** | make.defaults, use.mask, package.use.mask | 
| features/multilib | Unmask and unconditionally set the `multilib` USE flag | all | 
| arch/amd64/lib32 | Set `LIBDIR_x86` to `lib32`, and `SYMLINK_LIB` to `yes` to make {usr,}/lib a symlink to {usr,}/lib64 | make.defaults | 
| default/linux/amd64/23.0 | Add profiles arch/amd64/lib32 and releases/23.0 to the inheritance tree | parent | 
| releases | Set USE flags | make.defaults | 
| releases/23.0 |  | package.mask, package.use.force | 
| default/linux/amd64/23.0/desktop | Add profile targets/desktop to the inheritance tree | parent | 
| targets/desktop | Set USE flags | make.defaults, package.use, package.use.force | 
| default/linux/amd64/23.0/desktop/gnome | Add profile targets/desktop/gnome to the inheritance tree | parent | 
| targets/desktop/gnome | Set USE flags; in particular, globally set `gnome` | make.defaults, package.use | 
| default/linux/amd64/23.0/desktop/gnome/systemd | Add profile targets/systemd to the inheritance tree | parent | 
| targets/systemd | Globally set `systemd` and `udev` USE flags | make.defaults, package.mask, package.use.force | 

For information about the exact way in which settings in the different profile files combine please consult the [Package Manager Specification](https://wiki.gentoo.org/wiki/Package_Manager_Specification).
