<!-- source: https://wiki.gentoo.org/wiki/Intel_Edison/specs | group: Gentoo Wiki (Main) | wiki-title: Intel Edison/specs -->
---
title: Intel Edison/specs
url: https://wiki.gentoo.org/wiki/Intel_Edison/specs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-06"
fingerprint: "2cd64e5cc516cbfb"
license: CC BY-SA 4.0
---

# Intel Edison/specs

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

FILE **`/etc/portage/make.conf`****make.conf**

```
# These settings were set by the catalyst build script that automatically
# built this stage.
# Please consult /usr/share/portage/config/make.conf.example for a more
# detailed example.
CHOST="i686-pc-linux-gnu"
CFLAGS="-march=i686 -mtune=atom -O2 -fomit-frame-pointer -pipe"
CXXFLAGS="${CFLAGS}"
# WARNING: Changing your CHOST is not something that should be done lightly.
# Please consult http://www.gentoo.org/doc/en/change-chost.xml before changing.
CHOST="i686-pc-linux-gnu"
# These are the USE flags that were used in addition to what is provided by the
# profile used for building.
USE="-X -alsa -ipv6 -doc -debug -gdbm -gmp mmx wifi bindist -systemd -nls -berkdb"
FEATURES="buildpkg"
PYTHON_TARGETS="python2_7"
USE_PYTHON="2.7"
PORTDIR="/usr/portage"
DISTDIR="${PORTDIR}/distfiles"
PKGDIR="${PORTDIR}/packages"
```
FILE **`emerge --info`****/emerge --info**

```
Portage 2.3.0 (python 2.7.12-final-0, unavailable, gcc-4.9.4, glibc-2.22-r4, 3.10.17-yocto-standard-r2 i686)
=================================================================
System uname: Linux-3.10.17-yocto-standard-r2-i686-Genuine_Intel-R-_CPU_4000_@_500MHz-with-gentoo-2.3
KiB Mem:      983516 total,    898980 free
KiB Swap:          0 total,         0 free
sh bash 4.3_p48-r1
ld GNU ld (Gentoo 2.25.1 p1.1) 2.25.1
dev-lang/python:          2.7.12::gentoo, 3.4.5::gentoo
sys-devel/autoconf:       2.69::gentoo
sys-devel/automake:       1.14.1::gentoo, 1.15::gentoo
sys-devel/binutils:       2.25.1-r1::gentoo
sys-devel/libtool:        2.4.6-r2::gentoo
sys-kernel/linux-headers: 4.4::gentoo (virtual/os-headers)
Repositories:
gentoo
    location: /usr/portage
    sync-type: rsync
    sync-uri: rsync://rsync.gentoo.org/gentoo-portage
    priority: -1000
x-portage
    location: /usr/portage
    masters: gentoo
ACCEPT_LICENSE="* -@EULA"
CFLAGS="-march=i686 -mtune=atom -O2 -fomit-frame-pointer -pipe"
CHOST="i686-pc-linux-gnu"
CONFIG_PROTECT="/etc"
CONFIG_PROTECT_MASK="/etc/ca-certificates.conf /etc/env.d /etc/gentoo-release /etc/sandbox.d /etc/terminfo"
CXXFLAGS="-march=i686 -mtune=atom -O2 -fomit-frame-pointer -pipe"
DISTDIR="/usr/portage/distfiles"
FEATURES="assume-digests binpkg-logs buildpkg config-protect-if-modified distlocks ebuild-locks fixlafiles merge-sync news parallel-fetch preserve-libs protect-owned sandbox sfperms strict unknown-features-warn unmerge-logs unmerge-orphans userfetch userpriv usersandbox usersync xattr"
GENTOO_MIRRORS="http://distfiles.gentoo.org"
PKGDIR="/usr/portage/packages"
PORTAGE_TMPDIR="/var/tmp"
USE="bindist mmx wifi"
Unset:  ACCEPT_KEYWORDS, EMERGE_DEFAULT_OPTS, PORTAGE_BUNZIP2_COMMAND
```
FILE **`/proc/cpuinfo`****/proc/cpuinfo**

```
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 74
model name      : Genuine Intel(R) CPU   4000  @  500MHz
stepping        : 8
microcode       : 0x810
cpu MHz         : 500.000
cache size      : 1024 KB
physical id     : 0
siblings        : 2
core id         : 0
cpu cores       : 2
apicid          : 0
initial apicid  : 0
fdiv_bug        : no
f00f_bug        : no
coma_bug        : no
fpu             : yes
fpu_exception   : yes
cpuid level     : 11
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe nx rdtscp lm constant_tsc arch_perfmon pebs bts xtopology nonstop_tsc aperfmperf nonstop_tsc_s3 pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 cx16 xtpr pdcm sse4_1 sse4_2 movbe popcnt tsc_deadline_timer aes rdrand lahf_lm 3dnowprefetch ida arat epb dtherm tpr_shadow vnmi flexpriority ept vpid tsc_adjust smep erms
bogomips        : 998.40
clflush size    : 64
cache_alignment : 64
address sizes   : 36 bits physical, 48 bits virtual
power management:
processor       : 1
vendor_id       : GenuineIntel
cpu family      : 6
model           : 74
model name      : Genuine Intel(R) CPU   4000  @  500MHz
stepping        : 8
microcode       : 0x810
cpu MHz         : 500.000
cache size      : 1024 KB
physical id     : 0
siblings        : 2
core id         : 1
cpu cores       : 2
apicid          : 2
initial apicid  : 2
fdiv_bug        : no
f00f_bug        : no
coma_bug        : no
fpu             : yes
fpu_exception   : yes
cpuid level     : 11
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe nx rdtscp lm constant_tsc arch_perfmon pebs bts xtopology nonstop_tsc aperfmperf nonstop_tsc_s3 pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 cx16 xtpr pdcm sse4_1 sse4_2 movbe popcnt tsc_deadline_timer aes rdrand lahf_lm 3dnowprefetch ida arat epb dtherm tpr_shadow vnmi flexpriority ept vpid tsc_adjust smep erms
bogomips        : 998.40
clflush size    : 64
cache_alignment : 64
address sizes   : 36 bits physical, 48 bits virtual
power management:
```
