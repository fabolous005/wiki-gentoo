<!-- source: https://wiki.gentoo.org/wiki/USE_flag | group: Gentoo Wiki (Main) | wiki-title: USE flag -->
---
title: USE flag
url: https://wiki.gentoo.org/wiki/USE_flag
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-04"
fingerprint: e3a11978e6e08d10
license: CC BY-SA 4.0
---

# USE flag

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**USE flags** are keywords that embody support and dependency-information for a certain concept. They are a core feature of Gentoo, and a good understanding of how to deal with them is needed for administering a Gentoo system.

USE flags serve to configure [Portage](https://wiki.gentoo.org/wiki/Portage) to determine how each package will be configured on installation or update. USE flags can configure many aspects of a package, and the available USE flags and corresponding optional functionality depend on each individual piece of software.

USE flags can change an array of package behavior. They often set [compile-time options](https://wiki.gentoo.org/wiki/Autotools#.22configure.22_scripts), though these configure options are not systematically exposed through USE flags - it's up to the package maintainers to decide what options are useful to provide as USE flags. USE flags can specify which optional libraries or utilities will be linked with a package, often determining dependencies. Another example of what USE flags can change is which files are included in an installation, such as whether documentation is provided or not.

This ability to choose options only available when installing a package is one of the great advantages Gentoo leverages from being source based.

The whole distribution comes with reasonable USE flags by default, and these are further refined by selecting a [profile](https://wiki.gentoo.org/wiki/Portage/Profiles) to suit planned system usage. Packages each have their own set of available USE flags, depending on what can be configured for the package, and these are also set to reasonable defaults. The order in which USE flags are applied is specified by the [USE\_ORDER](https://wiki.gentoo.org/wiki/USE_ORDER) variable.

Technically, the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository) comes with a small set of default USE flags. Default USE flags are further defined by the [selected profile](https://wiki.gentoo.org/wiki/Portage/Profiles). Each package comes with a set of available USE flags, and these can also have a default state, if this is justified in the context of the package. Each layer overrides the previous, to configure Portage to set up the installation of each package.

A USE flag can have three states: **set, unset, or default**. USE flags may be set or unset globally in the [USE variable in make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf#USE), or for specific packages in [/etc/portage/package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use).

The presence of a USE flag in one of these files will *set* that USE flag. Including a USE flag preceded by a minus sign ("-") will *unset* that USE flag. If a USE flag is not present in one of these places, the *default* will be used.

To see which USE flags are presently enabled:

`user $``portageq envvar USE | xargs -n 1`
To see USE flags enabled by default:

`user $``USE_ORDER="defaults:pkginternal:repo" emerge --info|grep USE`
USE="X a52 aac acl acpi activities alsa amd64 bluetooth branding bzip2 cairo cdda cdr cet crypt cups dbus declarative dri dts dvd dvdr elogind encode exif flac gdbm gif gpm gtk gui iconv icu ipv6 jpeg kde kf6compat kwallet lcms libnotify libtirpc mad mng mp3 mp4 mpeg multilib ncurses networkmanager nls ogg opengl openmp pam pango pcre pdf pipewire plasma png policykit ppds pulseaudio qml qt5 qt6 readline screencast sdl seccomp semantic-desktop sound spell ssl startup-notification svg test-rust tiff truetype udev udisks unicode upower usb vorbis vulkan wayland widgets wxwidgets x264 xattr xcb xft xml xv xvid zlib" ABI\_X86="64" ADA\_TARGET="gcc\_12" APACHE2\_MODULES="authn\_core authz\_core socache\_shmcb unixd actions alias auth\_basic authn\_anon authn\_dbm authn\_file authz\_dbm authz\_groupfile authz\_host authz\_owner authz\_user autoindex cache cgi cgid dav dav\_fs dav\_lock deflate dir env expires ext\_filter file\_cache filter headers include info log\_config logio mime mime\_magic negotiation rewrite setenvif speling status unique\_id userdir usertrack vhost\_alias" CALLIGRA\_FEATURES="karbon sheets words" COLLECTD\_PLUGINS="df interface irq load memory rrdtool swap syslog" CPU\_FLAGS\_X86="mmx mmxext sse sse2" ELIBC="glibc" GPSD\_PROTOCOLS="ashtech aivdm earthmate evermore fv18 garmin garmintxt gpsclock greis isyncitrax navcom oceanserver oncore rtcm104v2 rtcm104v3 sirf skytraq superstar2 tsip tripmate tnt ublox" GUILE\_SINGLE\_TARGET="3-0" GUILE\_TARGETS="3-0" INPUT\_DEVICES="libinput" KERNEL="linux" LCD\_DEVICES="bayrad cfontz glk hd44780 lb216 lcdm001 mtxorb text" LUA\_SINGLE\_TARGET="lua5-1" LUA\_TARGETS="lua5-1" OFFICE\_IMPLEMENTATION="libreoffice" PHP\_TARGETS="php8-2" POSTGRES\_TARGETS="postgres16" PYTHON\_SINGLE\_TARGET="python3\_12" PYTHON\_TARGETS="python3\_12" RUBY\_TARGETS="ruby32" VIDEO\_CARDS="amdgpu dummy fbdev intel nouveau radeon radeonsi vesa" XTABLES\_ADDONS="quota2 psd pknock lscan length2 ipv4options ipp2p iface geoip fuzzy condition tarpit sysrq proto logmark ipmark dhcpmac delude chaos account"

To check if a certain USE flag is activated and which packages use it, run:

`user $````
euse -I <use_flag> # included with app-portage/gentoolkit
```
`user $````
quse <use_flag> # included with app-portage/portage-utils
```
`user $````
eix --installed-with-use <use_flag> # included with app-portage/eix
```
Replace `<use_flag>` with the USE flag to be checked.

Check which `USE` flags are enabled when merging a package and its dependencies:

`root #````
emerge --ask --verbose chromium
```
These are the packages that would be merged, in order:
 
Calculating dependencies... done!
Dependency resolution took 5.95 s (backtrack: 0/20).
 
\[ebuild  N     \] media-libs/openh264-2.4.1:0/7::gentoo  USE="plugin -test -utils" ABI\_X86="(64) -32 (-x32)" CPU\_FLAGS\_X86="avx2" 58895 KiB
\[ebuild   R    \] dev-lang/rust-bin-1.77.1:stable::gentoo  USE="profiler\* (-big-endian) -clippy -doc (-prefix) -rust-analyzer -rust-src -rustfmt -verify-sig" ABI\_X86="(64) -32 (-x32)" CPU\_FLAGS\_X86="sse2" 0 KiB
\[ebuild   R    \] virtual/rust-1.77.1:0/llvm-17::gentoo  USE="profiler\* -rustfmt" ABI\_X86="(64) -32 (-x32)" 0 KiB
\[ebuild  N     \] dev-build/gn-0.2157::gentoo  731 KiB
\[ebuild   R    \] net-libs/nodejs-20.12.1:0/20::gentoo  USE="icu inspector\* npm snapshot ssl system-icu system-ssl -corepack -debug -doc (-lto) -pax-kernel -test" CPU\_FLAGS\_X86="sse2" 0 KiB
\[ebuild  N     \] www-client/chromium-124.0.6367.60:0/stable::gentoo  USE="X cups custom-cflags hangouts official proprietary-codecs pulseaudio qt5 screencast system-harfbuzz system-icu system-png system-toolchain system-zstd vaapi wayland widevine -bindist -debug -ffmpeg-chromium -gtk4 (-headless) -kerberos (-libcxx) (-lto) -pax-kernel (-pgo) -qt6 (-selinux)" L10N="af am ar bg bn ca cs da de el en-GB es es-419 et fa fi fil fr gu he hi hr hu id it ja kn ko lt lv ml mr ms nb nl pl pt-BR pt-PT ro ru sk sl sr sv sw ta te th tr uk ur vi zh-CN zh-TW" 3352285 KiB
 
Would you like to merge these packages? \[Yes/No\]

Check which `USE` flags are enabled for a single package, without calculating all of its dependencies:

`user $````
emerge --nodeps --pretend chromium
```
\[ebuild  N     \] www-client/chromium-124.0.6367.60  USE="X cups custom-cflags hangouts official proprietary-codecs pulseaudio qt5 screencast system-harfbuzz system-icu system-png system-toolchain system-zstd vaapi wayland widevine -bindist -debug -ffmpeg-chromium -gtk4 (-headless) -kerberos (-libcxx) (-lto) -pax-kernel (-pgo) -qt6 (-selinux)" L10N="af am ar bg bn ca cs da de el en-GB es es-419 et fa fi fil fr gu he hi hr hu id it ja kn ko lt lv ml mr ms nb nl pl pt-BR pt-PT ro ru sk sl sr sv sw ta te th tr uk ur vi zh-CN zh-TW"

`USE` flags in () parenthesis are forced, masked or removed. This forcing may come from the selected profile or architecture. To understand why, you may need to analyse the profile file, such as "profiles/base/package.use.stable.mask". If you think the masking in the profile is in error, you may try to unmask it by adding the package (with no flags) to the "package.accept\_keywords" file. If the masking was not in error, you will likely end up with either a broken build or no change at all (because the `USE` flag was not functional).

The [emerge](https://wiki.gentoo.org/wiki/Portage#emerge) command has some USE flag related options like:

- `--changed-use` (`-U`)
- `--complete-graph-if-new-use < y | n >`
- [`--newuse` (`-N`)](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/USE#Adapting_the_entire_system_to_the_new_USE_flags)

For details see man 1 emerge.

The technical difference between "local" and "global" flags is simply a difference of where their descriptions are saved in the [ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository):

- "Global" USE flags are described in use.desc files for a whole [profile](https://wiki.gentoo.org/wiki/Portage/Profiles) ([example](https://gitweb.gentoo.org/repo/gentoo.git/tree/profiles/use.desc))
- "Local" USE flags are described in metadata.xml files for a single package ([example](https://gitweb.gentoo.org/repo/gentoo.git/tree/app-backup/bacula/metadata.xml))

When deciding where to define them, developers determine if a USE flag has a general function common to several packages, or a specific function for a single package.

For the end user, whether a USE flag is "global" or "local" has little importance. "Global" flags may sometimes be more suited to being set in the [USE variable](https://wiki.gentoo.org/wiki//etc/portage/make.conf#USE) in [make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf), but there is absolutely no hard rule. In any case, as much as possible, no USE flags should be set in make.conf unless necessary, and [/etc/portage/package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use) should be preferred to set USE flags on a per-package basis. Following this advice should ease system administration in the long run.

Some tools are available for analyzing and managing USE flags:

- [euse](https://wiki.gentoo.org/wiki/Gentoolkit#euse)
- [quse](https://wiki.gentoo.org/wiki/Q_applets#Looking_for_packages_that_use_some_USE_flag_.28quse.29) - for details see man 1 quse
- [equery hasuse (USE flag)](https://wiki.gentoo.org/wiki/Equery#Looking_for_packages_that_have_a_specific_USE_flag_with_hasuse_.28h.29)
- [equery uses (package name)](https://wiki.gentoo.org/wiki/Equery#Listing_per-package_USE_flags_with_uses_.28u.29)
- [ufed](https://wiki.gentoo.org/wiki/Ufed)
- [app-portage/euses](https://packages.gentoo.org/packages/app-portage/euses)
- [eix](https://wiki.gentoo.org/wiki/Eix) - See `--use`, `--installed-with-use` and `--installed-without-use` options

- [/etc/portage/package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use) — provides a more fine grained **[per-package control](https://wiki.gentoo.org/wiki/Handbook:Parts/Working/USE#Declaring_USE_flags_for_individual_packages) of [USE flags]** than the `USE` variable in [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf#USE)
- [Complete Handbook/Finishing off](https://wiki.gentoo.org/wiki/Complete_Handbook/Finishing_off)
- [Gentoo Handbook documentation on USE flags](https://wiki.gentoo.org/wiki/Handbook:Parts/Working/USE)
- [Gentoo Java USE flags](https://wiki.gentoo.org/wiki/Gentoo_Java_USE_flags)
- [Toolkit USE Flags](https://wiki.gentoo.org/wiki/Toolkit_USE_Flags) — summarizes main points from discussions about toolkit (GTK, Qt) USE flags (gtk2, gtk3, qt4, qt5, etc.).

- [https://packages.gentoo.org/useflags](https://packages.gentoo.org/useflags) - USE flags on Gentoo Packages Database.
- cat /var/db/repos/gentoo/profiles/use.desc - The common USE flag description list can be seen locally on systems with Portage installed.
- [https://devmanual.gentoo.org/general-concepts/use-flags/](https://devmanual.gentoo.org/general-concepts/use-flags/) - USE flags conceptually explained in the Gentoo Developer Handbook.
