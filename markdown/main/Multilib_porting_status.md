<!-- source: https://wiki.gentoo.org/wiki/Multilib_porting_status | group: Gentoo Wiki (Main) | wiki-title: Multilib porting status -->
---
title: Multilib porting status
url: https://wiki.gentoo.org/wiki/Multilib_porting_status
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-02-24"
fingerprint: "8b763e6820487b3"
license: CC BY-SA 4.0
---

# Multilib porting status

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**



### Some information

List of packages included in actual emul packages: [\[1\]](http://www.gentoo.org/proj/en/base/amd64/emul/emul-linux-x86-20140508.xml)

gx86 mulitilib tracker bug: [bug #454644](https://bugs.gentoo.org/show_bug.cgi?id=454644)

## Legend

| Text | Meaning | 
|---|---|
|  | Package is ported and available in testing/stable | 
|  | Package is ported but still masked | 
|  | There is an open bug | 
|  | Not ported, no open bug | 

## Emul-\* packages porting status

### emul-linux-x86-baselibs

| Package | Status | Notes | 
|---|---|---|
| [app-admin/gamin](https://packages.gentoo.org/packages/app-admin/gamin) |  | was not really part of emul libs | 
| [app-arch/bzip2](https://packages.gentoo.org/packages/app-arch/bzip2) |  | >=emul-linux-x86-baselibs-20130224-r1 | 
| [app-crypt/mit-krb5](https://packages.gentoo.org/packages/app-crypt/mit-krb5) |  | [bug #505004](https://bugs.gentoo.org/show_bug.cgi?id=505004) | 
| [app-text/libpaper](https://packages.gentoo.org/packages/app-text/libpaper) |  | >=emul-linux-x86-baselibs-20130224-r11 | 
| [dev-db/sqlite](https://packages.gentoo.org/packages/dev-db/sqlite) |  | >=emul-linux-x86-baselibs-20130224-r14 | 
| [dev-lang/perl](https://packages.gentoo.org/packages/dev-lang/perl) |  |  | 
| [dev-lang/python](https://packages.gentoo.org/packages/dev-lang/python) |  |  | 
| [dev-libs/dbus-glib](https://packages.gentoo.org/packages/dev-libs/dbus-glib) |  | >=emul-linux-x86-baselibs-20131008-r14 | 
| [dev-libs/elfutils](https://packages.gentoo.org/packages/dev-libs/elfutils) |  | >=emul-linux-x86-baselibs-20130224-r12 | 
| [dev-libs/expat](https://packages.gentoo.org/packages/dev-libs/expat) |  | >=emul-linux-x86-baselibs-20130224-r7 | 
| [dev-libs/glib](https://packages.gentoo.org/packages/dev-libs/glib) |  | >=emul-linux-x86-baselibs-20130224-r10 | 
| [dev-libs/gmp](https://packages.gentoo.org/packages/dev-libs/gmp) |  | >=emul-linux-x86-baselibs-20131008-r2 | 
| [dev-libs/json-c](https://packages.gentoo.org/packages/dev-libs/json-c) |  | >=emul-linux-x86-baselibs-20131008-r14 | 
| [dev-libs/libffi](https://packages.gentoo.org/packages/dev-libs/libffi) |  | >=emul-linux-x86-baselibs-20130224-r2 | 
| [dev-libs/libgcrypt](https://packages.gentoo.org/packages/dev-libs/libgcrypt) |  | >=emul-linux-x86-baselibs-20131008-r20 | 
| [dev-libs/libgpg-error](https://packages.gentoo.org/packages/dev-libs/libgpg-error) |  | >=emul-linux-x86-baselibs-20131008-r14 | 
| [dev-libs/libnl](https://packages.gentoo.org/packages/dev-libs/libnl) |  |  | 
| [dev-libs/libpcre](https://packages.gentoo.org/packages/dev-libs/libpcre) |  | >=emul-linux-x86-baselibs-20131008-r3 | 
| [dev-libs/libtasn1](https://packages.gentoo.org/packages/dev-libs/libtasn1) |  | >=emul-linux-x86-baselibs-20131008-r17 | 
| [dev-libs/libusb](https://packages.gentoo.org/packages/dev-libs/libusb) |  | >=emul-linux-x86-baselibs-20130224-r8 | 
| [dev-libs/libxml2](https://packages.gentoo.org/packages/dev-libs/libxml2) |  | >=emul-linux-x86-baselibs-20131008-r14 | 
| [dev-libs/libxslt](https://packages.gentoo.org/packages/dev-libs/libxslt) |  | >=emul-linux-x86-baselibs-20131008-r21 | 
| [dev-libs/lzo](https://packages.gentoo.org/packages/dev-libs/lzo) |  | >=emul-linux-x86-baselibs-20131008-r20 | 
| [dev-libs/nettle](https://packages.gentoo.org/packages/dev-libs/nettle) |  | >=emul-linux-x86-baselibs-20131008-r17 | 
| [dev-libs/nspr](https://packages.gentoo.org/packages/dev-libs/nspr) |  | >=emul-linux-x86-baselibs-20140508-r14 | 
| [dev-libs/nss](https://packages.gentoo.org/packages/dev-libs/nss) |  | >=emul-linux-x86-baselibs-20140508-r14 | 
| [dev-libs/openssl](https://packages.gentoo.org/packages/dev-libs/openssl):0 |  | [bug #488418](https://bugs.gentoo.org/show_bug.cgi?id=488418) | 
| [dev-libs/openssl](https://packages.gentoo.org/packages/dev-libs/openssl):0.9.8 |  | >=emul-linux-x86-baselibs-20140508-r5 | 
| [dev-libs/udis86](https://packages.gentoo.org/packages/dev-libs/udis86) |  | >=emul-linux-x86-baselibs-20130224-r2 | 
| [media-libs/giflib](https://packages.gentoo.org/packages/media-libs/giflib) |  | >=emul-linux-x86-baselibs-20140406-r2 | 
| [media-libs/lcms](https://packages.gentoo.org/packages/media-libs/lcms):2 |  | >=emul-linux-x86-baselibs-20130224-r11 | 
| [media-libs/lcms](https://packages.gentoo.org/packages/media-libs/lcms):0 |  |  | 
| [media-libs/libart\_lgpl](https://packages.gentoo.org/packages/media-libs/libart_lgpl) |  |  | 
| [media-libs/libmng](https://packages.gentoo.org/packages/media-libs/libmng) |  | [bug #496380](https://bugs.gentoo.org/show_bug.cgi?id=496380) | 
| [media-libs/libpng](https://packages.gentoo.org/packages/media-libs/libpng):1.2 |  | >=emul-linux-x86-baselibs-20130224-r4 | 
| [media-libs/libpng](https://packages.gentoo.org/packages/media-libs/libpng) |  | >=emul-linux-x86-baselibs-20130224-r2 | 
| [media-libs/tiff](https://packages.gentoo.org/packages/media-libs/tiff):0 |  | >=emul-linux-x86-baselibs-20130224-r10 | 
| [media-libs/tiff](https://packages.gentoo.org/packages/media-libs/tiff):3 |  | >=emul-linux-x86-baselibs-20130224-r11 | 
| [net-dialup/capi4k-utils](https://packages.gentoo.org/packages/net-dialup/capi4k-utils) |  |  | 
| [net-dns/libidn](https://packages.gentoo.org/packages/net-dns/libidn) |  |  | 
| [net-libs/gnutls](https://packages.gentoo.org/packages/net-libs/gnutls) |  | [bug #493166](https://bugs.gentoo.org/show_bug.cgi?id=493166) | 
| [net-libs/libgssglue](https://packages.gentoo.org/packages/net-libs/libgssglue) |  |  | 
| [net-libs/libsoup](https://packages.gentoo.org/packages/net-libs/libsoup) |  |  | 
| [net-libs/libtirpc](https://packages.gentoo.org/packages/net-libs/libtirpc) |  |  | 
| [net-libs/neon](https://packages.gentoo.org/packages/net-libs/neon) |  |  | 
| [net-misc/curl](https://packages.gentoo.org/packages/net-misc/curl) |  |  | 
| [net-nds/openldap](https://packages.gentoo.org/packages/net-nds/openldap) |  | [bug #493174](https://bugs.gentoo.org/show_bug.cgi?id=493174) | 
| [net-print/cups](https://packages.gentoo.org/packages/net-print/cups) |  | [bug #493172](https://bugs.gentoo.org/show_bug.cgi?id=493172) | 
| [sys-apps/acl](https://packages.gentoo.org/packages/sys-apps/acl) |  | [bug #496964](https://bugs.gentoo.org/show_bug.cgi?id=496964) | 
| [sys-apps/attr](https://packages.gentoo.org/packages/sys-apps/attr) |  | >=emul-linux-x86-baselibs-20130224-r10 | 
| [sys-apps/dbus](https://packages.gentoo.org/packages/sys-apps/dbus) |  | >=emul-linux-x86-baselibs-20130224-r5 | 
| [sys-apps/file](https://packages.gentoo.org/packages/sys-apps/file) |  | >=emul-linux-x86-baselibs-20131008-r22 | 
| [sys-apps/keyutils](https://packages.gentoo.org/packages/sys-apps/keyutils) |  | [bug #505006](https://bugs.gentoo.org/show_bug.cgi?id=505006) | 
| [sys-apps/pciutils](https://packages.gentoo.org/packages/sys-apps/pciutils) |  |  | 
| [sys-apps/tcp-wrappers](https://packages.gentoo.org/packages/sys-apps/tcp-wrappers) |  | >=emul-linux-x86-baselibs-20130224-r5 | 
| [sys-apps/util-linux](https://packages.gentoo.org/packages/sys-apps/util-linux) |  | [bug #490968](https://bugs.gentoo.org/show_bug.cgi?id=490968) | 
| [sys-auth/nss-mdns](https://packages.gentoo.org/packages/sys-auth/nss-mdns) |  |  | 
| [sys-auth/nss\_ldap](https://packages.gentoo.org/packages/sys-auth/nss_ldap) |  |  | 
| [sys-auth/pam\_ldap](https://packages.gentoo.org/packages/sys-auth/pam_ldap) |  |  | 
| [sys-devel/binutils](https://packages.gentoo.org/packages/sys-devel/binutils) |  |  | 
| [sys-devel/gettext](https://packages.gentoo.org/packages/sys-devel/gettext) |  | >=emul-linux-x86-baselibs-20131008-r14 | 
| [sys-devel/libperl](https://packages.gentoo.org/packages/sys-devel/libperl) |  |  | 
| [sys-devel/libtool](https://packages.gentoo.org/packages/sys-devel/libtool) |  | [bug #499390](https://bugs.gentoo.org/show_bug.cgi?id=499390) | 
| [sys-devel/llvm](https://packages.gentoo.org/packages/sys-devel/llvm) |  | >=emul-linux-x86-baselibs-20130224-r3 | 
| [sys-fs/e2fsprogs](https://packages.gentoo.org/packages/sys-fs/e2fsprogs) |  |  | 
| [sys-libs/cracklib](https://packages.gentoo.org/packages/sys-libs/cracklib) |  |  | 
| [sys-libs/db](https://packages.gentoo.org/packages/sys-libs/db) |  |  | 
| [sys-libs/e2fsprogs-libs](https://packages.gentoo.org/packages/sys-libs/e2fsprogs-libs) |  | >=emul-linux-x86-baselibs-20130224-r13 | 
| [sys-libs/gdbm](https://packages.gentoo.org/packages/sys-libs/gdbm) |  | >=emul-linux-x86-baselibs-20130224-r5 | 
| [sys-libs/gpm](https://packages.gentoo.org/packages/sys-libs/gpm) |  | >=emul-linux-x86-baselibs-20130224-r13 | 
| [sys-libs/libavc1394](https://packages.gentoo.org/packages/sys-libs/libavc1394) |  | >=emul-linux-x86-baselibs-20130224-r5 | 
| [sys-libs/libraw1394](https://packages.gentoo.org/packages/sys-libs/libraw1394) |  | >=emul-linux-x86-baselibs-20130224-r5 | 
| [sys-libs/ncurses](https://packages.gentoo.org/packages/sys-libs/ncurses) |  | >=emul-linux-x86-baselibs-20130224-r13 | 
| [sys-libs/pam](https://packages.gentoo.org/packages/sys-libs/pam) |  |  | 
| [sys-libs/pwdb](https://packages.gentoo.org/packages/sys-libs/pwdb) |  |  | 
| [sys-libs/readline](https://packages.gentoo.org/packages/sys-libs/readline) |  | >=emul-linux-x86-baselibs-20131008-r14 | 
| [sys-libs/slang](https://packages.gentoo.org/packages/sys-libs/slang) |  | >=emul-linux-x86-baselibs-20140406-r2 | 
| [sys-libs/talloc](https://packages.gentoo.org/packages/sys-libs/talloc) |  | [bug #491222](https://bugs.gentoo.org/show_bug.cgi?id=491222) | 
| [sys-libs/zlib](https://packages.gentoo.org/packages/sys-libs/zlib) |  | >=emul-linux-x86-baselibs-20130224-r1 | 
| [virtual/jpeg](https://packages.gentoo.org/packages/virtual/jpeg) |  | >=emul-linux-x86-baselibs-20130224-r5 | 
| [media-libs/jpeg](https://packages.gentoo.org/packages/media-libs/jpeg):62 |  | >=emul-linux-x86-baselibs-20130224-r5 | 
| [media-libs/libjpeg-turbo](https://packages.gentoo.org/packages/media-libs/libjpeg-turbo) |  | >=emul-linux-x86-baselibs-20130224-r4 | 
| [virtual/udev](https://packages.gentoo.org/packages/virtual/udev) |  | >=emul-linux-x86-baselibs-20130224-r9 | 
| [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) |  |  | 
| [sys-fs/eudev](https://packages.gentoo.org/packages/sys-fs/eudev) |  |  | 
| [sys-fs/udev](https://packages.gentoo.org/packages/sys-fs/udev) |  |  | 

### ~~emul-linux-x86-compat~~

- Package removed as of 2014-10-14, [bug #517932](https://bugs.gentoo.org/show_bug.cgi?id=517932).

| Package | Status | Notes | 
|---|---|---|
| [sys-libs/lib-compat](https://packages.gentoo.org/packages/sys-libs/lib-compat) |  | Binary package, 32-bit only | 
| [sys-libs/libstdc++-v3](https://packages.gentoo.org/packages/sys-libs/libstdc++-v3) |  | Already builds the 32-bit x86 version with USE=multilib | 

### emul-linux-x86-cpplibs

| Package | Status | Notes | 
|---|---|---|
| [dev-libs/boost](https://packages.gentoo.org/packages/dev-libs/boost) |  | [bug #512884](https://bugs.gentoo.org/show_bug.cgi?id=512884) | 
| [dev-libs/libsigc++](https://packages.gentoo.org/packages/dev-libs/libsigc++) |  |  | 

### emul-linux-x86-db

| Package | Status | Notes | 
|---|---|---|
| [dev-db/myodbc](https://packages.gentoo.org/packages/dev-db/myodbc) |  | done with >=dev-db/myodbc-5.2.7-r1 | 
| [dev-db/mysql](https://packages.gentoo.org/packages/dev-db/mysql) |  | done with >=virtual/mysql-5.6-r2 | 
| [dev-db/unixODBC](https://packages.gentoo.org/packages/dev-db/unixODBC) |  | [bug #510868](https://bugs.gentoo.org/show_bug.cgi?id=510868) >=emul-linux-x86-db-20140508-r1 | 

### ~~emul-linux-x86-glibc-errno-compat~~

- Not an emul package, only keyworded for **\~x86**.
- Package removed as of 2014-09-07, [bug #503208](https://bugs.gentoo.org/show_bug.cgi?id=503208).

### emul-linux-x86-gstplugins

### emul-linux-x86-gtklibs

| Package | Status | Notes | 
|---|---|---|
| [dev-libs/atk](https://packages.gentoo.org/packages/dev-libs/atk) |  | [bug #488608](https://bugs.gentoo.org/show_bug.cgi?id=488608) | 
| [dev-libs/libIDL](https://packages.gentoo.org/packages/dev-libs/libIDL) |  |  | 
| [gnome-base/gconf](https://packages.gentoo.org/packages/gnome-base/gconf) |  |  | 
| [gnome-base/gnome-vfs](https://packages.gentoo.org/packages/gnome-base/gnome-vfs) |  |  | 
| [gnome-base/libglade](https://packages.gentoo.org/packages/gnome-base/libglade) |  |  | 
| [gnome-base/orbit](https://packages.gentoo.org/packages/gnome-base/orbit) |  |  | 
| [media-gfx/graphite2](https://packages.gentoo.org/packages/media-gfx/graphite2) |  | [bug #488860](https://bugs.gentoo.org/show_bug.cgi?id=488860) | 
| [media-libs/harfbuzz](https://packages.gentoo.org/packages/media-libs/harfbuzz) |  | [bug #488864](https://bugs.gentoo.org/show_bug.cgi?id=488864) | 
| [media-libs/imlib](https://packages.gentoo.org/packages/media-libs/imlib) |  | >=emul-linux-x86-gtklibs-20140406-r1 | 
| [x11-libs/cairo](https://packages.gentoo.org/packages/x11-libs/cairo) |  | >=emul-linux-x86-gtklibs-20131008-r2 | 
| [x11-libs/gdk-pixbuf](https://packages.gentoo.org/packages/x11-libs/gdk-pixbuf) |  | >=emul-linux-x86-gtklibs-20131008-r3 | 
| [x11-libs/gtk+](https://packages.gentoo.org/packages/x11-libs/gtk+) |  | [bug #489000](https://bugs.gentoo.org/show_bug.cgi?id=489000) | 
| [x11-libs/libnotify](https://packages.gentoo.org/packages/x11-libs/libnotify) |  |  | 
| [x11-libs/pango](https://packages.gentoo.org/packages/x11-libs/pango) |  | >=emul-linux-x86-gtklibs-20131008-r4 | 
| [x11-libs/pangox-compat](https://packages.gentoo.org/packages/x11-libs/pangox-compat) |  | [bug #488870](https://bugs.gentoo.org/show_bug.cgi?id=488870) | 
| [x11-libs/pixman](https://packages.gentoo.org/packages/x11-libs/pixman) |  | >=emul-linux-x86-gtklibs-20131008-r1 | 
| [x11-themes/gtk-engines](https://packages.gentoo.org/packages/x11-themes/gtk-engines) |  |  | 
| [x11-themes/gtk-engines-murrine](https://packages.gentoo.org/packages/x11-themes/gtk-engines-murrine) |  |  | 
| [x11-themes/gtk-engines-xfce](https://packages.gentoo.org/packages/x11-themes/gtk-engines-xfce) |  |  | 

### emul-linux-x86-gtkmmlibs

| Package | Status | Notes | 
|---|---|---|
| [dev-cpp/atkmm](https://packages.gentoo.org/packages/dev-cpp/atkmm) |  |  | 
| [dev-cpp/cairomm](https://packages.gentoo.org/packages/dev-cpp/cairomm) |  |  | 
| [dev-cpp/glibmm](https://packages.gentoo.org/packages/dev-cpp/glibmm) |  |  | 
| [dev-cpp/gtkmm](https://packages.gentoo.org/packages/dev-cpp/gtkmm) |  |  | 
| [dev-cpp/libglademm](https://packages.gentoo.org/packages/dev-cpp/libglademm) |  |  | 
| [dev-cpp/pangomm](https://packages.gentoo.org/packages/dev-cpp/pangomm) |  |  | 

### emul-linux-x86-java

### emul-linux-x86-jna

- Tracker [bug #474464](https://bugs.gentoo.org/show_bug.cgi?id=474464)

| Package | Status | Notes | 
|---|---|---|
| [dev-java/jna](https://packages.gentoo.org/packages/dev-java/jna) |  |  | 

### emul-linux-x86-medialibs

- [dev-libs/DirectFB](https://packages.gentoo.org/packages/dev-libs/DirectFB) doesn't need to be ported, see [bug #484248](https://bugs.gentoo.org/show_bug.cgi?id=484248)

| Package | Status | Notes | 
|---|---|---|
| [app-misc/lirc](https://packages.gentoo.org/packages/app-misc/lirc) |  |  | 
| [dev-libs/fribidi](https://packages.gentoo.org/packages/dev-libs/fribidi) |  | >=emul-linux-x86-medialibs-20130224-r11 | 
| [dev-libs/libcdio](https://packages.gentoo.org/packages/dev-libs/libcdio) |  | >=emul-linux-x86-medialibs-20130224-r11 | 
| [dev-libs/liboil](https://packages.gentoo.org/packages/dev-libs/liboil) |  | >=emul-linux-x86-medialibs-20130224-r10 | 
| [media-gfx/sane-backends](https://packages.gentoo.org/packages/media-gfx/sane-backends) |  | [bug #493168](https://bugs.gentoo.org/show_bug.cgi?id=493168) | 
| [media-libs/a52dec](https://packages.gentoo.org/packages/media-libs/a52dec) |  | >=emul-linux-x86-medialibs-20130224-r9 | 
| [media-libs/faac](https://packages.gentoo.org/packages/media-libs/faac) |  | >=emul-linux-x86-medialibs-20130224-r2 | 
| [media-libs/faad2](https://packages.gentoo.org/packages/media-libs/faad2) |  | >=emul-linux-x86-medialibs-20130224-r2 | 
| [media-libs/gst-plugins-base](https://packages.gentoo.org/packages/media-libs/gst-plugins-base) |  |  | 
| [media-libs/gstreamer](https://packages.gentoo.org/packages/media-libs/gstreamer) |  | [bug #493176](https://bugs.gentoo.org/show_bug.cgi?id=493176) | 
| [media-libs/libcuefile](https://packages.gentoo.org/packages/media-libs/libcuefile) |  | >=emul-linux-x86-medialibs-20130224-r3 | 
| [media-libs/libdca](https://packages.gentoo.org/packages/media-libs/libdca) |  | >=emul-linux-x86-medialibs-20130224-r4 | 
| [media-libs/libdv](https://packages.gentoo.org/packages/media-libs/libdv) |  | >=emul-linux-x86-medialibs-20130224-r13 | 
| [media-libs/libdvdnav](https://packages.gentoo.org/packages/media-libs/libdvdnav) |  | >=emul-linux-x86-medialibs-20130224-r5 | 
| [media-libs/libdvdread](https://packages.gentoo.org/packages/media-libs/libdvdread) |  | >=emul-linux-x86-medialibs-20130224-r5 | 
| [media-libs/libgphoto2](https://packages.gentoo.org/packages/media-libs/libgphoto2) |  | [bug #493170](https://bugs.gentoo.org/show_bug.cgi?id=493170) | 
| [media-libs/libid3tag](https://packages.gentoo.org/packages/media-libs/libid3tag) |  | >=emul-linux-x86-medialibs-20130224-r7 | 
| [media-libs/libiec61883](https://packages.gentoo.org/packages/media-libs/libiec61883) |  | >=emul-linux-x86-medialibs-20130224-r8 | 
| [media-libs/libmad](https://packages.gentoo.org/packages/media-libs/libmad) |  | >=emul-linux-x86-medialibs-20130224-r4 | 
| [media-libs/libmimic](https://packages.gentoo.org/packages/media-libs/libmimic) |  | >=emul-linux-x86-medialibs-20130224-r9 | 
| [media-libs/libmms](https://packages.gentoo.org/packages/media-libs/libmms) |  | >=emul-linux-x86-medialibs-20130224-r9 | 
| [media-libs/libmpeg2](https://packages.gentoo.org/packages/media-libs/libmpeg2) |  | >=emul-linux-x86-medialibs-20130224-r10 | 
| [media-libs/libofa](https://packages.gentoo.org/packages/media-libs/libofa) |  |  | 
| [media-libs/libpostproc](https://packages.gentoo.org/packages/media-libs/libpostproc) |  |  | 
| [media-libs/libreplaygain](https://packages.gentoo.org/packages/media-libs/libreplaygain) |  | >=emul-linux-x86-medialibs-20130224-r3 | 
| [media-libs/libshout](https://packages.gentoo.org/packages/media-libs/libshout) |  | >=emul-linux-x86-medialibs-20130224-r7 | 
| [media-libs/libsidplay](https://packages.gentoo.org/packages/media-libs/libsidplay) |  | >=emul-linux-x86-medialibs-20130224-r7 | 
| [media-libs/libtheora](https://packages.gentoo.org/packages/media-libs/libtheora) |  | >=emul-linux-x86-medialibs-20130224-r2 | 
| [media-libs/libv4l](https://packages.gentoo.org/packages/media-libs/libv4l) |  | >=emul-linux-x86-medialibs-20130224-r6 | 
| [media-libs/libvisual](https://packages.gentoo.org/packages/media-libs/libvisual) |  | >=emul-linux-x86-medialibs-20130224-r10 | 
| [media-libs/libvpx](https://packages.gentoo.org/packages/media-libs/libvpx) |  | >=emul-linux-x86-medialibs-20130224-r2 | 
| [media-libs/speex](https://packages.gentoo.org/packages/media-libs/speex) |  | >=emul-linux-x86-medialibs-20130224-r4 | 
| [media-libs/taglib](https://packages.gentoo.org/packages/media-libs/taglib) |  |  | 
| [media-libs/x264](https://packages.gentoo.org/packages/media-libs/x264) |  | >=emul-linux-x86-medialibs-20130224-r8 | 
| [media-libs/xvid](https://packages.gentoo.org/packages/media-libs/xvid) |  | >=emul-linux-x86-medialibs-20130224-r2 | 
| [media-sound/lame](https://packages.gentoo.org/packages/media-sound/lame) |  | >=emul-linux-x86-medialibs-20130224-r2 | 
| [media-video/mjpegtools](https://packages.gentoo.org/packages/media-video/mjpegtools) |  |  | 
| [sys-libs/libieee1284](https://packages.gentoo.org/packages/sys-libs/libieee1284) |  | >=emul-linux-x86-medialibs-20130224-r10 | 
| [virtual/ffmpeg](https://packages.gentoo.org/packages/virtual/ffmpeg) |  |  | 
| [media-video/libav](https://packages.gentoo.org/packages/media-video/libav) |  |  | 
| [media-video/ffmpeg](https://packages.gentoo.org/packages/media-video/ffmpeg) |  |  | 

### emul-linux-x86-motif

- Tracker [bug #461916](https://bugs.gentoo.org/show_bug.cgi?id=461916)
- emul-linux-x86-motif package is fully ported.

| Package | Status | Notes | 
|---|---|---|
| [x11-libs/motif](https://packages.gentoo.org/packages/x11-libs/motif) |  |  | 

### emul-linux-x86-opengl

- Tracker [bug #468102](https://bugs.gentoo.org/show_bug.cgi?id=468102)

| Package | Status | Notes | 
|---|---|---|
| [media-libs/freeglut](https://packages.gentoo.org/packages/media-libs/freeglut) |  | >=emul-linux-x86-opengl-20131008.ebuild | 
| [media-libs/glew](https://packages.gentoo.org/packages/media-libs/glew) |  | >=emul-linux-x86-opengl-20131008.ebuild | 
| [media-libs/glu](https://packages.gentoo.org/packages/media-libs/glu) |  | >=emul-linux-x86-opengl-20131008.ebuild | 
| [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa) |  | >=emul-linux-x86-opengl-20131008.ebuild | 
| [x11-libs/libdrm](https://packages.gentoo.org/packages/x11-libs/libdrm) |  | >=emul-linux-x86-opengl-20131008.ebuild | 

### emul-linux-x86-qtlibs

- changed package category from x11-libs to dev-qt

| Package | Status | Notes | 
|---|---|---|
| [media-libs/phonon](https://packages.gentoo.org/packages/media-libs/phonon) |  |  | 
| [dev-qt/qtcore](https://packages.gentoo.org/packages/dev-qt/qtcore) |  | >=dev-qt/qtcore-4.8.6 | 
| [dev-qt/qtdbus](https://packages.gentoo.org/packages/dev-qt/qtdbus) |  | >=dev-qt/qtdbus-4.8.6 | 
| [dev-qt/qtgui](https://packages.gentoo.org/packages/dev-qt/qtgui) |  | >=dev-qt/qtgui-4.8.6 | 
| [dev-qt/qtopengl](https://packages.gentoo.org/packages/dev-qt/qtopengl) |  | >=dev-qt/qtopengl-4.8.6 | 
| [dev-qt/qtscript](https://packages.gentoo.org/packages/dev-qt/qtscript) |  | >=dev-qt/qtscript-4.8.6 | 
| [dev-qt/qtsql](https://packages.gentoo.org/packages/dev-qt/qtsql) |  | >=dev-qt/qtsql-4.8.6 | 
| [dev-qt/qtsvg](https://packages.gentoo.org/packages/dev-qt/qtsvg) |  | >=dev-qt/qtsvg-4.8.6 | 
| [dev-qt/qtwebkit](https://packages.gentoo.org/packages/dev-qt/qtwebkit) |  | >=dev-qt/qtwebkit-4.8.6 | 
| [dev-qt/qtxmlpatterns](https://packages.gentoo.org/packages/dev-qt/qtxmlpatterns) |  | >=dev-qt/qtxmlpatterns-4.8.6 | 

### emul-linux-x86-sdl

| Package | Status | Notes | 
|---|---|---|
| [media-libs/freealut](https://packages.gentoo.org/packages/media-libs/freealut) |  | >=emul-linux-x86-sdl-20140406-r1 | 
| [media-libs/libsdl](https://packages.gentoo.org/packages/media-libs/libsdl) |  | >=emul-linux-x86-sdl-20140406-r1 | 
| [media-libs/openal](https://packages.gentoo.org/packages/media-libs/openal) |  | [bug #484060](https://bugs.gentoo.org/show_bug.cgi?id=484060) >=openal-1.15.1-r1 | 
| [media-libs/sdl-image](https://packages.gentoo.org/packages/media-libs/sdl-image) |  | >=emul-linux-x86-sdl-20140406-r1 | 
| [media-libs/sdl-mixer](https://packages.gentoo.org/packages/media-libs/sdl-mixer) |  | >=emul-linux-x86-sdl-20140406-r2 | 
| [media-libs/sdl-net](https://packages.gentoo.org/packages/media-libs/sdl-net) |  | >=emul-linux-x86-sdl-20140406-r1 | 
| [media-libs/sdl-sound](https://packages.gentoo.org/packages/media-libs/sdl-sound) |  | >=emul-linux-x86-sdl-20140406-r1 | 
| [media-libs/sdl-ttf](https://packages.gentoo.org/packages/media-libs/sdl-ttf) |  | >=emul-linux-x86-sdl-20140406-r1 | 
| [media-libs/smpeg](https://packages.gentoo.org/packages/media-libs/smpeg) |  | >=emul-linux-x86-sdl-20140406-r1 | 

### emul-linux-x86-soundlibs

| Package | Status | Notes | 
|---|---|---|
| [media-libs/alsa-lib](https://packages.gentoo.org/packages/media-libs/alsa-lib) |  | >=emul-linux-x86-soundlibs-20130224-r4.ebuild | 
| [media-libs/alsa-oss](https://packages.gentoo.org/packages/media-libs/alsa-oss) |  |  | 
| [media-libs/audiofile](https://packages.gentoo.org/packages/media-libs/audiofile) |  | [bug #513780](https://bugs.gentoo.org/show_bug.cgi?id=513780) needs closing, >=emul-linux-x86-soundlibs-20130224-r4.ebuild | 
| [media-libs/flac](https://packages.gentoo.org/packages/media-libs/flac) |  | >=emul-linux-x86-soundlibs-20130224-r4.ebuild | 
| [media-libs/ladspa-sdk](https://packages.gentoo.org/packages/media-libs/ladspa-sdk) |  | >=emul-linux-x86-soundlibs-20130224-r4.ebuild | 
| [media-libs/libao](https://packages.gentoo.org/packages/media-libs/libao) |  | >=emul-linux-x86-soundlibs-20131008-r2 | 
| [media-libs/libmikmod](https://packages.gentoo.org/packages/media-libs/libmikmod) |  | >=emul-linux-x86-soundlibs-20130224-r4.ebuild | 
| [media-libs/libmodplug](https://packages.gentoo.org/packages/media-libs/libmodplug) |  | >=emul-linux-x86-soundlibs-20130224-r4.ebuild | 
| [media-libs/libogg](https://packages.gentoo.org/packages/media-libs/libogg) |  | >=emul-linux-x86-soundlibs-20130224-r4.ebuild | 
| [media-libs/libsamplerate](https://packages.gentoo.org/packages/media-libs/libsamplerate) |  | >=emul-linux-x86-soundlibs-20130224-r7.ebuild | 
| [media-libs/libsndfile](https://packages.gentoo.org/packages/media-libs/libsndfile) |  | >=emul-linux-x86-soundlibs-20130224-r7.ebuild | 
| [media-libs/libvorbis](https://packages.gentoo.org/packages/media-libs/libvorbis) |  | >=emul-linux-x86-soundlibs-20130224-r4.ebuild | 
| [media-libs/portaudio](https://packages.gentoo.org/packages/media-libs/portaudio) |  | >=emul-linux-x86-soundlibs-20130224-r9.ebuild | 
| [media-libs/webrtc-audio-processing](https://packages.gentoo.org/packages/media-libs/webrtc-audio-processing) |  | >=emul-linux-x86-soundlibs-20130224-r4.ebuild | 
| [media-plugins/alsa-plugins](https://packages.gentoo.org/packages/media-plugins/alsa-plugins) |  | [bug #488132](https://bugs.gentoo.org/show_bug.cgi?id=488132) | 
| [media-plugins/alsaequal](https://packages.gentoo.org/packages/media-plugins/alsaequal) |  | >=emul-linux-x86-soundlibs-20130224-r4.ebuild | 
| [media-plugins/caps-plugins](https://packages.gentoo.org/packages/media-plugins/caps-plugins) |  | >=emul-linux-x86-soundlibs-20130224-r4.ebuild | 
| [media-plugins/swh-plugins](https://packages.gentoo.org/packages/media-plugins/swh-plugins) |  | >=emul-linux-x86-soundlibs-20130224-r4.ebuild | 
| [media-sound/cdparanoia](https://packages.gentoo.org/packages/media-sound/cdparanoia) |  | >=emul-linux-x86-soundlibs-20130224-r5.ebuild | 
| [media-sound/gsm](https://packages.gentoo.org/packages/media-sound/gsm) |  | >=emul-linux-x86-soundlibs-20130224-r4.ebuild | 
| [media-sound/jack-audio-connection-kit](https://packages.gentoo.org/packages/media-sound/jack-audio-connection-kit) |  | >=emul-linux-x86-soundlibs-20130224-r8.ebuild | 
| [media-sound/mpg123](https://packages.gentoo.org/packages/media-sound/mpg123) |  | >=emul-linux-x86-soundlibs-20130224-r10.ebuild | 
| [media-sound/musepack-tools](https://packages.gentoo.org/packages/media-sound/musepack-tools) |  | >=emul-linux-x86-soundlibs-20130224-r6.ebuild | 
| [media-sound/pulseaudio](https://packages.gentoo.org/packages/media-sound/pulseaudio) |  | >=emul-linux-x86-soundlibs-20131008-r2 | 
| [media-sound/twolame](https://packages.gentoo.org/packages/media-sound/twolame) |  | >=emul-linux-x86-soundlibs-20130224-r7.ebuild | 
| [media-sound/wavpack](https://packages.gentoo.org/packages/media-sound/wavpack) |  | >=emul-linux-x86-soundlibs-20130224-r5.ebuild | 
| [net-wireless/bluez](https://packages.gentoo.org/packages/net-wireless/bluez) |  |  | 
| [sci-libs/fftw](https://packages.gentoo.org/packages/sci-libs/fftw) |  | >=emul-linux-x86-soundlibs-20130224-r4.ebuild | 

### emul-linux-x86-xlibs

| Package | Status | Notes | 
|---|---|---|
| [media-libs/fontconfig](https://packages.gentoo.org/packages/media-libs/fontconfig) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [media-libs/freetype](https://packages.gentoo.org/packages/media-libs/freetype) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libICE](https://packages.gentoo.org/packages/x11-libs/libICE) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libpciaccess](https://packages.gentoo.org/packages/x11-libs/libpciaccess) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libSM](https://packages.gentoo.org/packages/x11-libs/libSM) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libvdpau](https://packages.gentoo.org/packages/x11-libs/libvdpau) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libX11](https://packages.gentoo.org/packages/x11-libs/libX11) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXau](https://packages.gentoo.org/packages/x11-libs/libXau) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXaw](https://packages.gentoo.org/packages/x11-libs/libXaw) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libxcb](https://packages.gentoo.org/packages/x11-libs/libxcb) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXcomposite](https://packages.gentoo.org/packages/x11-libs/libXcomposite) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXcursor](https://packages.gentoo.org/packages/x11-libs/libXcursor) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXdamage](https://packages.gentoo.org/packages/x11-libs/libXdamage) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXdmcp](https://packages.gentoo.org/packages/x11-libs/libXdmcp) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXext](https://packages.gentoo.org/packages/x11-libs/libXext) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXfixes](https://packages.gentoo.org/packages/x11-libs/libXfixes) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXft](https://packages.gentoo.org/packages/x11-libs/libXft) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXi](https://packages.gentoo.org/packages/x11-libs/libXi) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXinerama](https://packages.gentoo.org/packages/x11-libs/libXinerama) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXmu](https://packages.gentoo.org/packages/x11-libs/libXmu) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXp](https://packages.gentoo.org/packages/x11-libs/libXp) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXpm](https://packages.gentoo.org/packages/x11-libs/libXpm) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXrandr](https://packages.gentoo.org/packages/x11-libs/libXrandr) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXrender](https://packages.gentoo.org/packages/x11-libs/libXrender) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXScrnSaver](https://packages.gentoo.org/packages/x11-libs/libXScrnSaver) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXt](https://packages.gentoo.org/packages/x11-libs/libXt) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXtst](https://packages.gentoo.org/packages/x11-libs/libXtst) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXv](https://packages.gentoo.org/packages/x11-libs/libXv) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXvMC](https://packages.gentoo.org/packages/x11-libs/libXvMC) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXxf86dga](https://packages.gentoo.org/packages/x11-libs/libXxf86dga) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 
| [x11-libs/libXxf86vm](https://packages.gentoo.org/packages/x11-libs/libXxf86vm) |  | >=emul-linux-x86-xlibs-20130224-r2.ebuild | 

## Other packages porting status

### Source packages

### Binary packages

## Profile fixing status

Legend:

1. ABI, DEFAULT\_ABI — whether the profile sets ABI and DEFAULT\_ABI to a consistent value other than 'default',
2. MULTILIB\_ABIS — whether the profile sets MULTILIB\_ABIS,
3. IUSE\_IMPLICIT — whether the profile makes the native ABI flag implicit,
4. use.force, use.mask — whether the profile unmasks flags matching MULTILIB\_ABIS and forces the flag for native ABI,
5. def. ABI\_\* — whether the profile sets default value of ABI\_\* flags for packages that don't use use.force.

| Profile | ABI, DEFAULT\_ABI | MULTILIB\_ABIS | IUSE\_IMPLICIT | use.force, use.mask | def. ABI\_\* | 
|---|---|---|---|---|---|
| Main architecture profiles |  |  |  |  |  | 
| arch/amd64 |  |  |  |  |  | 
| arch/amd64/no-multilib |  |  |  |  |  | 
| arch/amd64/x32 |  |  |  |  |  | 
| arch/amd64-fbsd |  |  |  |  |  | 
| arch/arm |  |  |  |  |  | 
| arch/arm64 |  |  |  |  |  | 
| arch/mips |  |  |  |  |  | 
| arch/mips/mips64/n32 |  |  |  |  |  | 
| arch/mips/mips64/n64 |  |  |  |  |  | 
| arch/mips/mips64/multilib/n32 |  |  |  |  |  | 
| arch/mips/mips64/multilib/n64 |  |  |  |  |  | 
| arch/mips/mips64/multilib/o32 |  |  |  |  |  | 
| arch/mips/mipsel |  |  |  |  |  | 
| arch/mips/mipsel/mips64el/n32 |  |  |  |  |  | 
| arch/mips/mipsel/mips64el/n64 |  |  |  |  |  | 
| arch/mips/mipsel/mips64el/multilib/n32 |  |  |  |  |  | 
| arch/mips/mipsel/mips64el/multilib/n64 |  |  |  |  |  | 
| arch/mips/mipsel/mips64el/multilib/o32 |  |  |  |  |  | 
| arch/powerpc/ppc32 |  |  |  |  |  | 
| arch/powerpc/ppc64 |  |  |  |  |  | 
| arch/powerpc/ppc64/32ul |  |  |  |  |  | 
| arch/s390 |  |  |  |  |  | 
| arch/s390/s390x |  |  |  |  |  | 
| arch/sparc |  |  |  |  |  | 
| arch/sparc-fbsd |  |  |  |  |  | 
| arch/x86 |  |  |  |  |  | 
| arch/x86-fbsd |  |  |  |  |  | 
| Non-multilib architectures |  |  |  |  |  | 
| arch/alpha |  |  |  |  |  | 
| arch/hppa |  |  |  |  |  | 
| arch/ia64 |  |  |  |  |  | 
| arch/m68k |  |  |  |  |  | 
| arch/sh |  |  |  |  |  | 
| Profiles that do not inherit from arch tree |  |  |  |  |  | 
| hardened/linux/musl/amd64 |  |  |  |  |  | 
| hardened/linux/musl/arm |  |  |  |  |  | 
| hardened/linux/musl/mips |  |  |  |  |  | 
| hardened/linux/musl/mips/mipsel |  |  |  |  |  | 
| hardened/linux/musl/x86 |  |  |  |  |  | 
| hardened/linux/uclibc/amd64 |  |  |  |  |  | 
| hardened/linux/uclibc/arm |  |  |  |  |  | 
| hardened/linux/uclibc/mips |  |  |  |  |  | 
| hardened/linux/uclibc/mips/mipsel |  |  |  |  |  | 
| hardened/linux/uclibc/ppc |  |  |  |  |  | 
| hardened/linux/uclibc/x86 |  |  |  |  |  |
