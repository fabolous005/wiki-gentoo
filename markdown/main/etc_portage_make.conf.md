<!-- source: https://wiki.gentoo.org/wiki//etc/portage/make.conf | group: Gentoo Wiki (Main) | wiki-title: /etc/portage/make.conf -->
---
title: "/etc/portage/make.conf"
url: https://wiki.gentoo.org/wiki//etc/portage/make.conf
hostname: gentoo.org
sitename: "/etc/portage/make.conf"
date: "2026-03-30"
fingerprint: "2c351a7a8fa39388"
license: CC BY-SA 4.0
---

# /etc/portage/make.conf

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**/etc/portage/make.conf**, previously located at /etc/make.conf, is the main configuration file used to customize the [Portage](https://wiki.gentoo.org/wiki/Portage) environment on a global level. The [/etc/portage](https://wiki.gentoo.org/wiki//etc/portage) directory contains most other Portage configuration files.

Settings in make.conf will apply to every package that is emerged. These settings control many elements of Portage functionality such as global [USE flags](https://wiki.gentoo.org/wiki/USE_flag), [language (L10N)](https://wiki.gentoo.org/wiki/L10n) options, [Portage mirrors](https://wiki.gentoo.org/wiki/GENTOO_MIRRORS), etc.

A very basic version gets installed [while extracting](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage#Unpacking_the_stage_file) the [Stage file](https://wiki.gentoo.org/wiki/Stage_file), and an example setup can be found at /usr/share/portage/config/make.conf.example.

Like many Portage configuration files, make.conf can be a directory, and its contents will get summed together as if it were a single file.

The final Portage configuration is not only based on make.conf. Global settings defined in this file can be refined (or redefined) on a per-package basis in the /etc/portage/package.use/ files as well as through environment variables. Default settings managed by the distribution are available as well (partially through the Portage package defaults, partially through the Gentoo [profile](<https://wiki.gentoo.org/wiki/Profile_(Portage)>) that is in use).

There are many possible variables to customize in make.conf. Only the most commonly used ones are explained further within this article, with an example and a link to a more detailed article (if applicable). For more information, and the full list of variables, consult the make.conf [man page](https://wiki.gentoo.org/wiki/Man_page) by running:

`user $``man make.conf`
Most variables are optional, can span multiple lines, but must not appear more than once.

The `CHOST` variable is passed through the configure step of ebuilds to set the build-host of the system.

The `CFLAGS` and `CXXFLAGS` variables define the build and compile flags that will be used for all package deployments (some exceptions notwithstanding which filter out flags known to cause problems with the package). The `CFLAGS` variable is for C based applications, while `CXXFLAGS` is meant for C++ based applications. Most users will keep the content of both variables the same.

**`/etc/portage/make.conf`**

**Commonly used sane setting for CFLAGS and CXXFLAGS**

```
CFLAGS="-march=native -O2 -pipe"
CXXFLAGS="${CFLAGS}"
```
The `CONFIG_PROTECT` variable contains a space-delimited list of files and directories that Portage will protect from automatic modification. Proposed changes to protected configuration locations will require manual merges from the system administrator (see [dispatch-conf](https://wiki.gentoo.org/wiki/Dispatch-conf) or similar merge tools).

A current list of presently protected locations can be displayed with [portageq](https://wiki.gentoo.org/wiki/Portageq):

`user $``portageq envvar CONFIG_PROTECT`
/etc /usr/share/config /usr/share/gnupg/qualified.txt

Using portageq is a short hand alternative to running a regular expression search on verbose, informational output from the emerge command:

`user $``emerge --verbose --info | grep -E '^CONFIG_PROTECT='`
CONFIG\_PROTECT="/etc /usr/share/config /usr/share/gnupg/qualified.txt"

Files or subdirectories defined within the `CONFIG_PROTECT` can be *excluded* from protection through the `[CONFIG_PROTECT_MASK](https://wiki.gentoo.org/wiki/CONFIG_PROTECT_MASK)` variable. Masking is useful when a parent directory should be protected, but a certain child file or directory beneath it should not.

The variable has a sane default setting handled by the Portage installation and the user's Gentoo [profile](https://wiki.gentoo.org/wiki/Portage/Profiles). It can be extended through the system environment (which is often used by applications that update the variable through their /etc/env.d file) and the user's [/etc/portage/make.conf] setting.

**`/etc/portage/make.conf`**

**Example`CONFIG_PROTECT` definitions**

```
CONFIG_PROTECT="/var/bind"
```
See also the [Environment variables](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/EnvVar) chapter in the Gentoo Handbook.



The `FEATURES` variable contains a list of Portage features that the user wants enabled on the system, effectively influencing Portage's behavior. It is set by default via /usr/share/portage/config/make.globals, but can be easily updated through [/etc/portage/make.conf]. Since this is an [incremental variable](https://dev.gentoo.org/~ulm/pms/head/pms.html#section-5.3.1), `FEATURES` values can be added without directly overriding the ones implemented through the Gentoo profile.

**`/etc/portage/make.conf`**

**Adding keepwork to FEATURES in Portage**

```
FEATURES="keepwork"
```
For more information, please see [Portage features](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Features) in the Gentoo Handbook and the [FEATURES](https://wiki.gentoo.org/wiki/FEATURES) article. For a complete list of available features, see man 5 make.conf.

The `MAKEOPTS` variable is used to specify arguments passed to make when packages are built from source. It defaults to `-j$(nproc) -l$(nproc)` if unset.

**`/etc/portage/make.conf`**

**Recommended setting for a dual-core processor with Hyper-Threading enabled with 8GB of RAM**

```
# We recommend that the smaller of: number of threads, or ram/2GB is used.
# So, for a dual-core processor w/ HT and 8GB of RAM, that's: min(4, 8) = 4
MAKEOPTS="-j4 -l4"
```
**EMERGE\_DEFAULT\_OPTS** is a variable for [Portage](https://wiki.gentoo.org/wiki/Portage) that defines entries to be appended to the emerge command line.

`EMERGE_DEFAULT_OPTS` allows for parallel emerge operations through the `--jobs`  and `N``--load-average`  options. `X.YEMERGE_DEFAULT_OPTS` is used by Portage to reference system load, or load average, and limit how many packages are built at a time.

To run up to three build jobs simultaneously:

**`/etc/portage/make.conf`**

**Enabling 3 parallel package builds**

```
EMERGE_DEFAULT_OPTS="--jobs 3"
```
For more information, see the [EMERGE\_DEFAULT\_OPTS](https://wiki.gentoo.org/wiki/EMERGE_DEFAULT_OPTS) article.

The `PORTAGE_TMPDIR` variable defines the location of the temporary files for Portage. The value defaults to /var/tmp, resulting in /var/tmp/portage for the build location, /var/tmp/ccache for Portage's [ccache](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Features#Caching_compilation_objects) support and so forth.

**`/etc/portage/make.conf`**

**Default PORTAGE\_TMPDIR setting**

```
PORTAGE_TMPDIR="/var/tmp"
```
On some systems, /var/tmp/ may be mounted with the `noexec` option. The following error would be displayed by emerge when building packages:

`user $``emerge --ask package`
Can not execute files in /var/tmp/portage
Likely cause is that you've mounted it with one of the
following mount options: 'noexec', 'user', 'users'
 
Please make sure that portage can execute files in this directory.

In this case, if removing the offending option from [/etc/fstab](https://wiki.gentoo.org/wiki//etc/fstab) isn't possible, `PORTAGE_TMPDIR` should be set to a different directory.

If enough memory is available, building packages can be accelerated by mounting `PORTAGE_TMPDIR` in RAM. See the article on [Portage TMPDIR on tmpfs](https://wiki.gentoo.org/wiki/Portage_TMPDIR_on_tmpfs) for more details.

The `DISTDIR` variable defines the location where Portage will store the downloaded source code archives. Its value defaults to [/var/cache/distfiles](https://wiki.gentoo.org/wiki//var/cache/distfiles) on new installations. Previously the default was ${PORTDIR}/distfiles which resolved to /usr/portage/distfiles by default.

Users can set the `DISTDIR` variable in [/etc/portage/make.conf]:

**`/etc/portage/make.conf`**

**Using a different DISTDIR location**

```
DISTDIR=/var/gentoo/distfiles
```
For more information, please refer to the [DISTDIR](https://wiki.gentoo.org/wiki/DISTDIR) article.

`PKGDIR` is the location [Portage](https://wiki.gentoo.org/wiki/Portage) keeps binary packages. By default, the location is set to /var/cache/binpkgs.

Previously, binary packages were set to ${PORTDIR}/packages, which by default resolved to /usr/portage/packages.

Run man make.conf for more information.

The `USE` variable allows the **system wide** setting or unsetting of [USE flags](https://wiki.gentoo.org/wiki/USE_flag). This variable is a space separated list and may span several lines.

```
USE="-kde -qt5 ldap"
```
The `ACCEPT_LICENSE` variable tells Portage which software licenses are allowed to be installed on the system. Packages licensed under an agreement that has not been formally accepted by the system administrator will not be installed on the system.

See [license groups](https://wiki.gentoo.org/wiki/License_groups) for a categorical breakdown of available software licenses.

```
ACCEPT_LICENSE="*"
```
The preferred way to accept all licenses is to set `-@EULA` which allows users to check over the terms of proprietary software.

**`/etc/portage/make.conf`**

**To accept all licenses on all packages except proprietary**

```
ACCEPT_LICENSE="* -@EULA"
```
**`/etc/portage/make.conf`**

**To accept free software only**

```
ACCEPT_LICENSE="-* @FREE"
```
The `USE_EXPAND` variable is a list set in [profiles/base/make.defaults](https://gitweb.gentoo.org/repo/gentoo.git/tree/profiles/base/make.defaults) as of Portage 2.0.51.20.[\[2\]](https://wiki.gentoo.org#cite_note-2)

The `CPU_FLAGS_*` variables inform Portage about the CPU flags (features) permitted by the CPU. This information is used to optimize package builds specifically for the targeted features. The currently supported variables are `CPU_FLAGS_X86` (for **amd64** and **x86** architectures), `CPU_FLAGS_ARM` (for **arm** and **arm64** architectures), and `CPU_FLAGS_PPC` (for **ppc** and **ppc64** architectures).

The cpuid2cpuflags utility (found in the [app-portage/cpuid2cpuflags](https://packages.gentoo.org/packages/app-portage/cpuid2cpuflags) package) can be used to query a complete listing of CPU flags supported by the system's processor. After emerging the package, issue:

`user $``cpuid2cpuflags`
CPU\_FLAGS\_X86: aes avx f16c mmx mmxext pclmul popcnt sse sse2 sse3 sse4\_1 sse4\_2 ssse3

Place this string into /etc/portage/packages.use as is:

**`/etc/portage/package.use/00cpu-flags`**

```
 CPU_FLAGS_X86: aes avx f16c mmx mmxext pclmul popcnt sse sse2 sse3 sse4_1 sse4_2 ssse3
```
See the [make.conf](https://wiki.gentoo.org/wiki/Xorg/Guide#make.conf) section of the Xorg/Guide article and the [possible values](https://packages.gentoo.org/useflags/input_devices_libinput).

**`/etc/portage/package.use/00local`**

```
 LINGUAS: de pt_BR en en_US en_GB
```
**`/etc/portage/package.use/00local`**

```
 L10N: de pt-BR en en-US en-GB
```
For possible values of this `USE_EXPAND` variable see `[VIDEO_CARDS](https://packages.gentoo.org/useflags/expand#video_cards)`.

| Machine | Discrete video card | VIDEO\_CARDS | 
|---|---|---|
| Intel x86 | None | See [Intel#Feature support](https://wiki.gentoo.org/wiki/Intel#Feature_support) | 
| x86/ARM | Nvidia | `nvidia` | 
| Any | Nvidia except Maxwell, Pascal and Volta | `nouveau` | 
| Any | AMD since Sea Islands | `amdgpu radeonsi` | 
| Any | ATI and older AMD | See [radeon#Feature support](https://wiki.gentoo.org/wiki/Radeon#Feature_support) | 
| Any | Intel | `intel` | 
| Raspberry Pi | N/A | `vc4` | 
| QEMU/KVM | Any | `virgl` | 
| WSL | Any | `d3d12` | 

From the common combinations of machines and video cards, substitute the name of the driver(s) to be used.

Appropriate `VIDEO_CARDS` value(s) pull in the correct driver(s).

After changing `VIDEO_CARDS` values remember to update the system using the following command so the changes take effect:

**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* intel nouveau radeon radeonsi
```
`root #``emerge --getbinpkg --ask --changed-use --deep @world`
Omit `--getbinpkg` to not use the configured binary package host.

For the average user, if a graphical desktop environment is to be used this variable *should* be explicitly defined. For further information see [the Xorg Guide make.conf section](https://wiki.gentoo.org/wiki/Xorg/Guide#make.conf).

For more details see the [AMDGPU](https://wiki.gentoo.org/wiki/AMDGPU), [Intel](https://wiki.gentoo.org/wiki/Intel), [Nouveau](https://wiki.gentoo.org/wiki/Nouveau), or [NVIDIA](https://wiki.gentoo.org/wiki/NVIDIA) articles.

- [make.conf - custom settings for Portage](https://devmanual.gentoo.org/eclass-reference/make.conf/index.html) - make.conf man page.
