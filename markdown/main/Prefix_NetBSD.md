<!-- source: https://wiki.gentoo.org/wiki/Prefix/NetBSD | group: Gentoo Wiki (Main) | wiki-title: Prefix/NetBSD -->
---
title: Prefix/NetBSD
url: https://wiki.gentoo.org/wiki/Prefix/NetBSD
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-30"
fingerprint: "810cb9794c001bb2"
license: CC BY-SA 4.0
---

# Prefix/NetBSD

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

# Instructions

Please note that some instructions date back to 2012 with edits in 2017 and 2025. Some things might be outdated and your mileage may vary. Stage 2 has been successfully completed on NetBSD 9.3 in 2025. Stage 3 fails and still needs work.

## Initial bootstrap

Start by installing *bash* via *pkgsrc* or by using this bootstrap script:

`user $````
cd ~
```
`user $````
chmod +x ./bootstrap-bash.sh
```
`user $````
./bootstrap-bash.sh "${EPREFIX}/tmp"
```
## Prefix bootstrap

### Preparation

Download this file and make it executable:

`user $````
cd ~
```
`user $````
chmod +x ~/bootstrap-prefix.sh
```
There is no Gentoo Prefix Profile for NetBSD on x86\_64 so we'll use the amd64 Linux profile with relevant overrides in *make.conf*.  We have to edit bootstrap-prefix.sh to add support for our CHOST and profile.  See comments in the patch for details on what the changes do.  To apply the patch run:

`user $``patch -p1 bootstrap-prefix.sh < bootstrap-prefix.sh.patch`
**`bootstrap-prefix.sh.patch`**

```
--- bootstrap-prefix.sh	2025-08-30 07:43:12.236559340 -0700
+++ bootstrap-prefix-netbsd.sh	2025-08-30 07:43:12.237559337 -0700
@@ -154,6 +154,14 @@
 				export CXX="${CXX-g++} -m32"
 			fi
 			;;
+		*-netbsd*)
+			# --hash-style=gnu (default) does not work on NetBSD.  Without this
+			# stage2 will fail to compile anything after binutils (ld
+			# specifically) has been built and the system ld is no longer in
+			# use.
+			export CC="${CC-gcc} -Wl,--hash-style=sysv"
+			export CXX="${CXX-g++} -Wl,--hash-style=sysv"
+			;;
 	esac
 
 	# point possible host pkg-config to stage2 files
@@ -274,6 +282,15 @@
 	if [[ ! -f ${MAKE_CONF_DIR}/0100_bootstrap_prefix_make.conf ]] ; then
 		{
 			echo "# Added by bootstrap-prefix.sh for ${CHOST}"
+			if [[ ${CHOST} == *-netbsd* ]] ; then
+				echo '# Override variables from the Linux amd64 profile for NetBSD.'
+				echo "CHOST="\""${CHOST}"\"""
+				echo 'KERNEL="NetBSD"'
+				echo 'ELIBC="NetBSD"'
+				echo '# End of NetBSD profile overrides.'
+				echo '# Disable estrip for bootstrap, to work it needs pax-utils to be installed (NetBSD).'
+				echo 'FEATURES="${FEATURES} nostrip"'
+			fi
 			echo 'USE="unicode nls"'
 			echo 'CFLAGS="${CFLAGS} -O2 -pipe"'
 			echo 'CXXFLAGS="${CFLAGS}"'
@@ -413,7 +430,9 @@
 			profile=${profile_linux/ARCH/riscv}
 			profile=${profile/17.0/20.0/rv64gc/lp64d}
 			;;
-		x86_64-pc-linux-gnu)
+		x86_64-pc-linux-gnu | x86_64-unknown-netbsd*)
+			# Reuse amd64 Linux profile for NetBSD.  Variables relevant for
+			# NetBSD will be overridden in make.conf.
 			profile=${profile_linux/ARCH/amd64}
 			profile=${profile/17.0/17.1/no-multilib}
 			;;
@@ -1113,8 +1132,9 @@
 			LDFLAGS="${LDFLAGS} -Wl,-rpath,${ROOT}/tmp/usr/lib ${libdir}"
 			LDFLAGS="${LDFLAGS} -Wl,-rpath,${libdir#-L}"
 		;;
-		*-openbsd*)
+		*-openbsd* | *-netbsd*)
 			# LLD
+			# Piggyback on OpenBSD LDFLAGS for NetBSD.
 			LDFLAGS="${LDFLAGS} -Wl,-rpath,${ROOT}/tmp/usr/lib"
 		;;
 		*-solaris*)
@@ -1694,9 +1714,16 @@
 	[[ -x ${ROOT}/tmp/usr/bin/make ]] \
 		|| [[ $(make --version 2>&1) == *GNU" Make "4* ]] \
 		|| (bootstrap_make) || return 1
+	# Do not ignore failures on NetBSD and build wget with SSL support
+	# regardless of system wget's existence.  wget needs to be build after
+	# libressl to get SSL support.  We'll need SSL enabled wget for stage2 and
+	# pkgsrc wget didn't work during stage2 on NetBSD 9.3.
 	[[ ${OFFLINE_MODE} ]] || [[ -x ${ROOT}/tmp/usr/bin/openssl ]] \
-		|| (bootstrap_libressl) # try without on failure
-	[[ ${OFFLINE_MODE} ]] || type -P wget > /dev/null \
+		|| (bootstrap_libressl) || \
+		[[ ${CHOST} != *-netbsd* ]] && return 1 # try without on failure if not NetBSD
+	[[ ${OFFLINE_MODE} ]] || \
+		([[ ${CHOST} != *-netbsd* ]] && type -P wget) || \
+		[[ -x ${ROOT}/tmp/usr/bin/wget ]] \
 		|| (bootstrap_wget) || return 1
 	[[ -x ${ROOT}/tmp/usr/bin/sed ]] \
 		|| [[ $(sed --version 2>&1) == *GNU* ]] \
@@ -2079,8 +2106,13 @@
 	echo "dev-build/cmake -server" >> "${ROOT}"/tmp/etc/portage/package.use
 
 	mkdir -p "${ROOT}"/tmp/etc/portage/profile  # site-specific overrides
-	if [[ ${CHOST} == *-solaris* ]] ; then
-		# avoid complexities with the host toolchain
+	if [[ ${CHOST} == *-solaris* ]] || [[ ${CHOST} == *-netbsd* ]] ; then
+		# avoid complexities with the host toolchain on Solaris
+		#
+		# On NetBSD this fixes configure issues in gcc/libgomp.  With pie the
+		# xgcc (what gcc builds as a staging compiler before creating the final
+		# binary) will fail configure sanity checks because the C compiler
+		# cannot create executables.
 		echo "sys-devel/gcc -pie" >> \
 			"${ROOT}"/tmp/etc/portage/profile/package.use.force
 		echo "sys-devel/gcc -pie" >> "${ROOT}"/tmp/etc/portage/package.use
@@ -2122,12 +2154,19 @@
 	chmod 755 "${ROOT}/tmp/usr/local/bin/my"{gcc,g++}
 
 	for pkg in ${compiler_stage1} ; do
+		# NetBSD, force sysv hash when building gcc, the gnu hash doesn't work.
+		# CC and CXX variables get ignored partway through the build.
+		if [[ "${pkg}" == *sys-devel/gcc* ]] && [[ ${CHOST} == *-netbsd* ]] ; then
+			EXTRA_ECONF="--with-linker-hash-style=sysv --disable-gnu-unique-object"
+		else
+			EXTRA_ECONF=''
+		fi
 		# <glibc-2.5 does not understand .gnu.hash, use
 		# --hash-style=both to produce also sysv hash.
 		# GCC apparently drops CPPFLAGS at some point, which makes it
 		# not find things like gmp which we just installed, so force it
 		# to find our prefix
-		EXTRA_ECONF="$(rapx --with-linker-hash-style=both) --with-local-prefix=${ROOT}" \
+		EXTRA_ECONF="$(rapx --with-linker-hash-style=both) --with-local-prefix=${ROOT} ${EXTRA_ECONF}" \
 		MYCMAKEARGS="-DCMAKE_USE_SYSTEM_LIBRARY_LIBUV=OFF" \
 		GCC_MAKE_TARGET=all \
 		OVERRIDE_CFLAGS="${CPPFLAGS} ${OVERRIDE_CFLAGS}" \
@@ -2451,12 +2490,33 @@
 	# in addition, avoid collisions
 	rm -Rf "${ROOT}/tmp/usr/lib/python$(python_ver)/site-packages/clang"
 
-	# Try to get ourself out of the mud, bug #575324
-	EXTRA_ECONF="--disable-compiler-version-checks $(rapx '--disable-lto --disable-bootstrap')" \
-	GCC_MAKE_TARGET="$(rapx all)" \
-	MYCMAKEARGS="-DCMAKE_USE_SYSTEM_LIBRARY_LIBUV=OFF" \
-	PYTHON_COMPAT_OVERRIDE="python$(python_ver)" \
-	pre_emerge_pkgs --nodeps "${compiler_pkgs[@]}" || return 1
+	# -pie and --with-linker-hash-style=sysv are needed for gcc to build on
+	# NetBSD.  GCC stage3 options are the same as the ones that were needed for
+	# stage2.
+	mkdir -p "${ROOT}"/etc/portage/profile  # site-specific overrides
+	if [[ ${CHOST} == *-netbsd* ]] ; then
+		# On NetBSD this fixes configure issues in gcc/libgomp.  With pie the
+		# xgcc (what gcc builds as a staging compiler before creating the final
+		# binary) will fail configure sanity checks because the C compiler
+		# cannot create executables.
+		echo "sys-devel/gcc -pie" >> \
+			"${ROOT}"/etc/portage/profile/package.use.force
+		echo "sys-devel/gcc -pie" >> "${ROOT}"/etc/portage/package.use
+	fi
+	for pkg in ${compiler_pkgs[@]} ; do
+		# NetBSD, force sysv hash when building gcc, the gnu hash doesn't work.
+		if [[ "${pkg}" == *sys-devel/gcc* ]] && [[ ${CHOST} == *-netbsd* ]] ; then
+			EXTRA_ECONF="--with-linker-hash-style=sysv --disable-gnu-unique-object"
+		else
+			EXTRA_ECONF=''
+		fi
+		# Try to get ourself out of the mud, bug #575324
+		EXTRA_ECONF="--disable-compiler-version-checks $(rapx '--disable-lto --disable-bootstrap') ${EXTRA_ECONF}" \
+		GCC_MAKE_TARGET="$(rapx all)" \
+		MYCMAKEARGS="-DCMAKE_USE_SYSTEM_LIBRARY_LIBUV=OFF" \
+		PYTHON_COMPAT_OVERRIDE="python$(python_ver)" \
+		pre_emerge_pkgs --nodeps "${pkg}" || return 1
+	done
 
 	if [[ ${CHOST}:${DARWIN_USE_GCC} == *-darwin*:0 ]] ; then
 		# At this point our libc++abi.dylib is dynamically linked to
@@ -2970,7 +3030,8 @@
 	case "${CHOST}" in
 		*-darwin*)
 			ncpu=$(/usr/sbin/sysctl -n hw.ncpu) ;;
-		*-freebsd* | *-openbsd*)
+		*-freebsd* | *-openbsd* | *-netbsd* )
+			# Piggyback on FreeBSD/OpenBSD CPU detection for NetBSD.
 			ncpu=$(/sbin/sysctl -n hw.ncpu) ;;
 		*-solaris*)
 			ncpu=$(/usr/sbin/psrinfo | wc -l) ;;
@@ -3236,9 +3297,10 @@
 		fi
 		# GNU and BSD variants of stat take different arguments (and
 		# format specifiers are not equivalent)
+		# Piggyback on FreeBSD/OpenBSD stat flags for NetBSD.
 		case "${CHOST}" in
-			*-darwin* | *-freebsd* | *-openbsd*) STAT='stat -f %u/%g' ;;
-			*)                                   STAT='stat -c %U/%G' ;;
+			*-darwin* | *-freebsd* | *-openbsd* | *-netbsd* ) STAT='stat -f %u/%g' ;;
+			*)                                                STAT='stat -c %U/%G' ;;
 		esac
 
 		if [[ $(${STAT} "${EPREFIX}"/.canihaswrite) != \
@@ -3540,6 +3602,14 @@
 					;;
 				esac
 				;;
+			NetBSD)
+				case $(uname -m) in
+					amd64)
+						# Detect NetBSD CHOST, use CHOST style used by Rust.
+						CHOST="x86_64-unknown-netbsd"
+					;;
+				esac
+				;;
 			OpenBSD)
 				case $(uname -m) in
 					amd64)
```
### Environment setup

Set the *EPREFIX* and *PATH* variables. Adding these paths makes sure that they will be available later on in the process.

`user $````
export EPREFIX="${HOME}/gentoo"
```
`user $````
export PATH="${EPREFIX}/usr/bin:${EPREFIX}/bin:${EPREFIX}/tmp/usr/bin:${EPREFIX}/tmp/bin:/usr/bin:/bin:/usr/pkg/bin"
```
### Stage 1

With a patched bootstrap script you should be able to just run the script like so:

`user $````
./bootstrap-prefix.sh "${EPREFIX}" stage1
```
Hopefully you'll see **\* stage1 successfully finished** once the command finishes.

#### Notes on possible failure scenarios (old)

Expand this section with the button on the right.

*m4* and *bison* must be installed \*before\* *bash*. We can just go down the list until we get to *m4*

`user $````
./bootstrap-prefix.sh $EPREFIX/tmp make
```
`user $````
./bootstrap-prefix.sh $EPREFIX/tmp wget
```
`user $````
./bootstrap-prefix.sh $EPREFIX/tmp sed
```
`user $````
./bootstrap-prefix.sh $EPREFIX/tmp m4
```
Bootstrapping *m4* will probably fail due to problems with fseeko.c and fflush.c. Download [patch-lib-fflush.c](http://mechalogic.net:8080/public/patch-lib-fflush.c) and also [patch-lib-fseeko.c](http://mechalogic.net:8080/public/patch-lib-fseeko.c) and put them in your home directory. Because the script will overwrite everything each time its run, we have to patch the source tarball and replace the original.

`user $````
cd $EPREFIX/tmp/usr/portage/distfiles
```
`user $````
mkdir tmp
```
`user $````
cd tmp
```
`user $````
tar zxvf ../m4-1.4.15.tar.gz
```
`user $````
cd m4-1.4.15/lib
```
`user $````
patch fflush.c < ~/patch-lib-fflush.c
```
`user $````
patch fseeko.c < ~/patch-lib-fseeko.c
```
`user $````
cd ../..
```
`user $````
tar zcvf m4-1.4.15.tar.gz m4-1.4.15/
```
`user $````
cd ..
```
`user $````
cp tmp/m4-1.4.15.tar.gz .
```
`user $````
rm -rf tmp
```
`user $````
cd
```
This time it should work with our fixed tarball

`user $````
./bootstrap-prefix.sh $EPREFIX/tmp m4
```
`user $````
./bootstrap-prefix.sh $EPREFIX/tmp bison
```
Now we can go ahead and bootstrap *bash*

`user $````
./bootstrap-bash.sh $EPREFIX/tmp
```
`user $````
hash -r
```
Next up... *coreutils* which also has fflush.c and fseeko.c. It will fail but we run it first to download the source.

`user $``./bootstrap-prefix.sh $EPREFIX/tmp coreutils`
When it aborts we go ahead and patch the source tarball exactly like we did for *m4*. Just patch the *coreutils-8.16* not both. With luck it will build cleanly and install itself.


We get a breather now, several packages should bootstrap without issues.

`user $``./bootstrap-prefix.sh $EPREFIX/tmp findutils``user $````
./bootstrap-prefix.sh $EPREFIX/tmp tar
```
`user $``./bootstrap-prefix.sh $EPREFIX/tmp patch``user $````
./bootstrap-prefix.sh $EPREFIX/tmp grep
```
`user $``./bootstrap-prefix.sh $EPREFIX/tmp gawk``user $````
./bootstrap-prefix.sh $EPREFIX/tmp bash
```
`user $``./bootstrap-prefix.sh $EPREFIX/tmp zlib``user $````
./bootstrap-prefix.sh $EPREFIX/tmp python
```
At this point we are going to get the *tree* and *portage*. We will use *latest\_tree* to hopefully pick up some bug fixes.

`user $````
./bootstrap-prefix.sh $EPREFIX latest_tree
```
`user $````
./bootstrap-prefix.sh $EPREFIX portage
```
`user $````
hash -r
```
Soon we will have serious errors with *libtool* unless something is done. Edit your \~/.bashrc and insert this:

**`~/.bashrc`**

```
export CONFIG_SHELL="$EPREFIX/bin/bash"
```
### Stage 2

`user $``./bootstrap-prefix.sh "${EPREFIX}" stage2`
You should see **\* stage2 successfully finished** once done.

### Stage 3

`user $``./bootstrap-prefix.sh "${EPREFIX}" stage3`
The build will fail when building *flex* with an error like *eltpatch: cannot execute: required file not found*.  This is because *eltpatch* hard codes */bin/bash* as the shebang and it might not be available on NetBSD.  It's not the prefix *bash* anyway.  We can do a dirty fix and edit ${EPREFIX}/usr/bin/eltpatch.  Replace *#!/bin/bash* on the first line with *#!/usr/bin/env bash* and save the file.  See [this](https://bugs.gentoo.org/962141) bug report requesting an upstream fix.  Now just run the bootstrap script again, it will resume from where it left off.

`user $``./bootstrap-prefix.sh "${EPREFIX}" stage3`
Stage 3 fails on *acct-group/root* with an error like *GID 0 taken already*.  This is because on NetBSD *wheel* has GID 0, and *root* group does not exist.

Edit ${EPREFIX}/var/db/repos/gentoo/acct-group/root/root-0-r2.ebuild and replace the uncommented parts with this (makes the ebuild a no-op):

**`root-0-r2.ebuild`**

Making the ebuild a no-op is not an issue since it just enforces the *root* group exists, in a prefix we're not *root* anyway.

Run this to update the checksums for the ebuild:

`user $``ebuild "${EPREFIX}/var/db/repos/gentoo/acct-group/root/root-0-r2.ebuild" digest`
Run stage3 again:

`user $``./bootstrap-prefix.sh "${EPREFIX}" stage3`
It will fail to build *sys-devel/patch* with an error like *utimens.c:541:10: error: implicit declaration of function 'utimens'; did you mean 'futimens'?*.  More research is required to fix these issues.
