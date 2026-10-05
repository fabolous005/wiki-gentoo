<!-- source: https://wiki.gentoo.org/wiki/Distcc/Cross-Compiling | group: Gentoo Wiki (Main) | wiki-title: Distcc/Cross-Compiling -->
---
title: Distcc/Cross-Compiling
url: https://wiki.gentoo.org/wiki/Distcc/Cross-Compiling
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-31"
fingerprint: "44c83caa5dd1092d"
license: CC BY-SA 4.0
---

# Distcc/Cross-Compiling

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide shows the reader how to set up distcc for cross-compiling across different processor architectures.

## Cross-compiling with distcc

### Introduction

distcc is a tool that lets you share the burden of software compiling across several networked computers. As long as the networked boxes are all using the same toolchain built for the same processor architecture, no special distcc setup is required.

**This guide provides instructions on how to configure distcc to compile for different architectures.**

### Emerge the needed utilities

First, you will need to emerge crossdev on all the machines that will be involved in the compiling process. crossdev is a tool that makes building cross-architecture toolchains easy. Its usage is straightforward: crossdev -t sparc will build a full cross-toolchain targeting the Sparc architecture. This includes binutils, gcc, glibc, and linux-headers.

You will need to emerge the proper cross-toolchain on all the helper boxes. If you need more help, try running crossdev --help.

If you want to fine tune the cross-toolchain, here is a script that will produce a command line with the exact versions of the cross development packages to be built on the helper boxes (the script is to be run on the target box).

Next, you will need to emerge distcc on all the machines that will be involved in the process. This includes the box that will run emerge and the boxes with the cross-compilers. Please see the [Gentoo Distcc Documentation](https://wiki.gentoo.org/wiki/Distcc) for more information on setting up and using distcc.

### Arch-specific notes

#### Intel x86 subarchitectures

If you are cross-compiling between different subarchitectures for Intel **x86** (e.g. i586 and i686), you must still build a full cross-toolchain for the desired `CHOST`, or else the compilation will fail. This is because i586 and i686 are actually different CHOSTs, despite the fact that they are both considered "x86." Please keep this in mind when you build your cross-toolchains. For example, if the target box is i586, this means that you must build i586 cross-toolchains on your i686 helper boxes.

#### SPARC

Using crossdev -t sparc might fail with one of the following errors:

If this happens, try using the following command instead:

`user $``crossdev --lenv "CC=sparc-unknown-linux-gnu-gcc" -t sparc-unknown-linux-gnu`
### Configuring distcc to cross-compile correctly

In the default distcc setup, cross-compiling will *not* work properly. The problem is that many builds just call gcc instead of the full compiler name (e.g. sparc-unknown-linux-gnu-gcc). When this compile gets distributed to a distcc helper box, the native compiler gets called instead of your shiny new cross-compiler.

Fortunately, there is a workaround for this little problem. All it takes is a wrapper script and a few symlinks on the box that will be running emerge. We'll use a Sparc box as an example. Wherever you see `sparc-unknown-linux-gnu` below, you will want to insert your own `CHOST` value (`x86_64-pc-linux-gnu` for an AMD64 box, for example). When you first emerge distcc, the /usr/lib/distcc/bin directory looks like this:

`root #````
cd /usr/lib/distcc/bin
```
`root #``ls -l`
total 0
lrwxrwxrwx  1 root root 15 Dec 23 20:13 c++ -> /usr/bin/distcc
lrwxrwxrwx  1 root root 15 Dec 23 20:13 cc -> /usr/bin/distcc
lrwxrwxrwx  1 root root 15 Dec 23 20:13 g++ -> /usr/bin/distcc
lrwxrwxrwx  1 root root 15 Dec 23 20:13 gcc -> /usr/bin/distcc
lrwxrwxrwx  1 root root 15 Dec 23 20:13 sparc-unknown-linux-gnu-c++ -> /usr/bin/distcc
lrwxrwxrwx  1 root root 15 Dec 23 20:13 sparc-unknown-linux-gnu-g++ -> /usr/bin/distcc
lrwxrwxrwx  1 root root 15 Dec 23 20:13 sparc-unknown-linux-gnu-gcc -> /usr/bin/distcc

Here is what you want to do:

`root #``rm c++ g++ gcc cc`
Next, we'll create the new script on this box. Fire up your favorite editor and create a file with the following text in it, then save it as sparc-unknown-linux-gnu-wrapper. Remember to change the `CHOST` value (in this case, `sparc-unknown-linux-gnu`) to the actual `CHOST` of the box that will be running the emerge.

Next, we'll make the script executable and create the proper symlinks:

`root #````
chmod a+x sparc-unknown-linux-gnu-wrapper
```
`root #````
ln -s sparc-unknown-linux-gnu-wrapper cc
```
`root #````
ln -s sparc-unknown-linux-gnu-wrapper gcc
```
`root #````
ln -s sparc-unknown-linux-gnu-wrapper g++
```
`root #``ln -s sparc-unknown-linux-gnu-wrapper c++`
When you're done, /usr/lib/distcc/bin will look like this:

`root #``ls -l`
total 4
lrwxrwxrwx  1 root root 25 Jan 18 14:20 c++ -> sparc-unknown-linux-gnu-wrapper
lrwxrwxrwx  1 root root 25 Jan 18 14:20 cc -> sparc-unknown-linux-gnu-wrapper
lrwxrwxrwx  1 root root 25 Jan 18 14:20 g++ -> sparc-unknown-linux-gnu-wrapper
lrwxrwxrwx  1 root root 25 Jan 18 14:20 gcc -> sparc-unknown-linux-gnu-wrapper
lrwxrwxrwx  1 root root 15 Nov 21 10:42 sparc-unknown-linux-gnu-c++ -> /usr/bin/distcc
lrwxrwxrwx  1 root root 15 Nov 21 10:42 sparc-unknown-linux-gnu-g++ -> /usr/bin/distcc
lrwxrwxrwx  1 root root 15 Jul 27 10:52 sparc-unknown-linux-gnu-gcc -> /usr/bin/distcc
-rwxr-xr-x  1 root root 70 Jan 18 14:20 sparc-unknown-linux-gnu-wrapper

Next we want to make sure that these wrappers stay available after upgrading the distcc package as it will overwrite the symbolic links. We can do this through a /etc/portage/bashrc file that looks like so:

**`/etc/portage/bashrc`**

```
case ${CATEGORY}/${PN} in
                 sys-devel/distcc | sys-devel/gcc | sys-devel/clang)
			if [ "${EBUILD_PHASE}" == "postinst" ]; then
				/usr/local/sbin/distcc-fix &
			fi
		;;
esac
```
Then create one of the following files as applicable. If you are not using clang:

**`/usr/local/sbin/distcc-fix`**

```
#!/bin/bash                     
sleep 20
# We extract $TUPLE from make.conf to avoid editing the script for each architecture.
TUPLE=$(portageq envvar CHOST)
GCC_VER=$(gcc-config -c|cut -d "-" -f5)
cd /usr/lib/distcc/bin
rm cc c++ gcc g++ gcc-${GCC_VER} g++-${GCC_VER} ${TUPLE}-wrapper
echo '#!/bin/bash' > ${TUPLE}-wrapper
echo "exec ${TUPLE}-g\${0:\$[-2]}" "\"\$@\"" >> ${TUPLE}-wrapper
chmod 755 ${TUPLE}-wrapper
ln -s ${TUPLE}-wrapper cc
ln -s ${TUPLE}-wrapper c++
ln -s ${TUPLE}-wrapper gcc
ln -s ${TUPLE}-wrapper g++
ln -s ${TUPLE}-wrapper gcc-${GCC_VER}
ln -s ${TUPLE}-wrapper g++-${GCC_VER}
#those are wrappers from gcc-config. remove them so the real compiler gets used.
if [ -e /usr/lib/distcc/bin/c99 ]
then
  rm /usr/lib/distcc/bin/c99
fi
if [ -e /usr/lib/distcc/bin/c89 ]
then
rm /usr/lib/distcc/bin/c89
fi
#clang process below, now your >chromium-65 ebuilds will use distcc just like before ;) 
if [ -x "$(command -v clang)" ]
then
  CLANG_VER=$(clang --version|grep version|cut -d " " -f3|cut -d'.' -f1,2)
  rm clang clang++ clang-${CLANG_VER} clang++-${CLANG_VER} ${TUPLE}-clang-wrapper
  echo '#!/bin/bash' > ${TUPLE}-clang-wrapper
  echo "exec ${TUPLE}-\$(basename \${0}) \"\$@\"" >> ${TUPLE}-clang-wrapper
  chmod 755 ${TUPLE}-clang-wrapper
  ln -s ${TUPLE}-clang-wrapper clang
  ln -s ${TUPLE}-clang-wrapper clang++
  ln -s ${TUPLE}-clang-wrapper clang-${CLANG_VER}
  ln -s ${TUPLE}-clang-wrapper clang++-${CLANG_VER}
fi
```
Give it the proper permissions:

`root #``chmod 755 /usr/local/sbin/distcc-fix`
Congratulations; you (hopefully) now have a working cross-distcc setup.

### How this works

When distcc is called, it checks to see what it was called as (e.g. `i686-pc-linux-gnu-gcc`, `sparc-unknown-linux-gnu-g++`, etc.) When distcc then distributes the compile to a helper box, it passes along the name it was called as. The distcc daemon on the other helper box then looks for a binary with that same name. If it sees just gcc, it will look for gcc, which is likely to be the native compiler on the helper box, if it is not the same architecture as the box running emerge. When the *full* name of the compiler is sent (e.g. `sparc-unknown-linux-gnu-gcc`), there is no confusion.

### Troubleshooting

This section covers a number of common problems when using distcc for cross-compiling.

#### Remote host distccd COMPILE ERRORS

When receiving the message `COMPILE ERRORS` within a remote host's /var/log/distccd.log file, see the above notes concerning specifying the correct architecture name (ie. crossdev -t $TARGET).

Another solution is to uninstall and re-install crossdev compiler tools, using the crossdev --clean option, or ensuring /usr/$TARGET no longer exists, and then completely reinstall the cross compiler.

It might also be wise to edit the remote host's /usr/$TARGET/etc/portage/make.conf, and ensure the contents of the `CFLAGS` variable are similar on all computers or hosts performing compiler operations. Also make sure the `USE` flags for the cross compiler are sufficient: if you built GCC with `USE=graphite` on the client, you need a line like `cross-i686-pc-linux-gnu/gcc graphite` in /etc/portage/package.use too.

#### Failed to exec $TARGET-unknown-linux-gnu-gcc: No such file or directory

The wrapper scripts might fail to execute, even with correct permissions:

To resolve this, make sure to have the wrapper script created with the complete name of the architecture target:

`user $``ls -alh /usr/lib/distcc/bin/c++`
/usr/lib/distcc/bin/c++ ->./i686-pc-linux-gnu-wrapper
