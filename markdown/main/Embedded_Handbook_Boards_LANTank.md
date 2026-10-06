<!-- source: https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/LANTank | group: Gentoo Wiki (Main) | wiki-title: Embedded Handbook/Boards/LANTank -->
---
title: Embedded Handbook/Boards/LANTank
url: https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/LANTank
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-06"
fingerprint: "429e3f626fbe7074"
license: CC BY-SA 4.0
---

# Embedded Handbook/Boards/LANTank

From Gentoo Wiki

\< [Embedded Handbook](https://wiki.gentoo.org/wiki/Embedded_Handbook) | [Boards](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Little-endian SuperH based NAS (using internal IDE) from I-O Data.

## IO-Data LANTANK specifications

Board specifications:

## /proc/cpuinfo

CPU info:

FILE **`/proc/cpuinfo`**

```
machine         : LANDISK
processor       : 0
cpu family      : sh4
cpu type        : SH7751R
cpu flags       : fpu ptea
cache type      : split (harvard)
icache size     : 16KiB (2-way)
dcache size     : 32KiB (2-way)
bogomips        : 266.24
master_clk      : 266.66MHz
module_clk      : 33.33MHz
bus_clk         : 133.33MHz
cpu_clk         : 266.66MHz
tmu0_clk        : 8.33MHz
```
## Cross compile preparation

Setup:

`root #``emerge --ask crossdev``root #``crossdev sh4-unknown-linux-gnu`
Emerge wrapper script:

FILE **`lantank-merge`**

```
#!/bin/sh
CHOST=sh4-unknown-linux-gnu
#export CBUILD=$(portageq envvar CBUILD)
export SYSROOT="/usr/${CHOST}"
export PORTAGE_CONFIGROOT="/usr/${CHOST}"
# optional exports
export enable_malloc0returnsnull=yes \
        ac_cv_file__usr_share_sgml_X11_defs_ent=1 \
        ac_cv_func_setpgrp_void=yes ac_cv_func_setgrent_void=yes \
        ac_cv_func_calloc_0_nonnull=yes ac_cv_func_malloc_0_nonnull=yes \
        gl_cv_func_malloc_0_nonnull=yes ac_cv_func_realloc_0_nonnull=yes \
        ac_cv_func_memcmp_working=yes ac_cv_func_strnlen_working=yes
# optional export for glib:2
export glib_cv_uscore=no glib_cv_stack_grows=no \
        glib_cv_stack_grows=no  glib_cv_has__inline=yes \
        glib_cv_has__inline__=yes glib_cv_hasinline=yes \
        glib_cv_sane_realloc=yes glib_cv_va_copy=yes \
        glib_cv___va_copy=yes glib_cv_va_val_copy=no \
        glib_cv_rtldglobal_broken=no glib_cv_uscore=no \
        ac_cv_func_posix_getpwuid_r=yes \
        ac_cv_func_posix_getgrgid_r=yes \
        ac_cv_header_pwd_h=yes \
        ac_cv_func_getpwuid_r=yes \
        glib_cv_sizeof_gmutex=40 
FAKEROOT=
if [[ $(id -u) != 0 ]]; then
	if [[ $(type -p fakeroot) != "" ]]; then
		FAKEROOT=fakeroot
	fi
fi
${FAKEROOT} emerge -q "$@"
```
FILE **`/usr/sh4-unknown-linux-gnu/etc/make.conf`**

```
CHOST=sh4-unknown-linux-gnu
CBUILD=x86_64-pc-linux-gnu
ARCH="sh"
ROOT=/usr/${CHOST}/
ACCEPT_KEYWORDS="sh ~sh"
USE="${ARCH} zlib bindist make-symlinks minimal \
        input_devices_keyboard input_devices_evdev \
        video_cards_fbdev video_cards_dummy"
VIDEO_CARDS="fbdev dummy"
INPUT_DEVICES="evdev keyboard mouse touchscreen"
USE_EXPAND="video_cards input_devices"
MARCH_TUNE="-m4"
CFLAGS="-Os -pipe ${MARCH_TUNE} -fomit-frame-pointer -I${ROOT}/usr/include/ -I${ROOT}/include/"
CXXFLAGS="${CFLAGS}"
LDFLAGS="-L${ROOT}/usr/lib -L${ROOT}/lib"
PKG_CONFIG_PATH="${ROOT}/usr/lib/pkgconfig/"
MAKEOPTS="-j8"
FEATURES="-collision-protect sandbox buildpkg noman noinfo nodoc"
PORTDIR_OVERLAY="/usr/portage/local/"
PKGDIR=${ROOT}/packages/
PORTAGE_TMPDIR=${ROOT}/tmp/
PORTAGE_WORKDIR_MODE=2775
PORTAGE_ECLASS_WARNING_ENABLE=0
CLEAN_DELAY=0
EPAUSE_IGNORE=1
EBEEP_IGNORE=1
```
## External resources
