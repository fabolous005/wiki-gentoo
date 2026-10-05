<!-- source: https://wiki.gentoo.org/wiki/Gentoo_Alt/Contributor%27s_Guide/Technotes_Guide | group: Gentoo Wiki (Main) | wiki-title: Gentoo Alt/Contributor's Guide/Technotes Guide -->
---
title: Gentoo Alt/Contributor's Guide/Technotes Guide
url: https://wiki.gentoo.org/wiki/Gentoo_Alt/Contributor%27s_Guide/Technotes_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-01-02"
fingerprint: "9651da61c825aba9"
license: CC BY-SA 4.0
---

# Gentoo Alt/Contributor's Guide/Technotes Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

This guide contains technical notes about working with packages on a non-Linux Gentoo environment.

## General structure

### Handling users

There are differences between users in a Gentoo/Linux system and the ones in other Gentoo/Alt systems. These differences are usually a side problem that does not invalidate most of the work. Still, it's better to pay attention.

The first thing to check is to never use the root group. The group with id 0, called root on Linux is instead wheel on BSD and Darwin, (and probably other classical unices); while wheel group on Linux has GID 10. When you need to set the default permissions on a file (or a series of files), you should then use the command chown root:0 file instead, so that it takes the right permissions.

For every other case you should always use the name of groups and users, avoiding specifying them by ID as they can have different IDs on different systems.

Also, it's important to note that every ebuild that makes references to specific users and groups, should add them with enewgroup and enewuser, if it does not depend on the ebuild that adds them. So for example, cronbase adds cron user and group, as they can not be present on the users' systems.

It's not rare to create users that cannot login on a system. To do so, on Linux, the shell of those users is set to /bin/false. Unfortunately, this does not work on BSD-derived systems like Gentoo/\*BSD and Gentoo/Darwin, as they use a more sophisticated approach, and /bin/false does not exist. The enewuser function provides a simple way to create a disabled user: just use -1 as the shell for the newly created user, this way the function will take care of selecting the right shell for the disabled user, making use of /sbin/nologin where that is present (\*BSD, Darwin and Linux using app-admin/nologin) or falling back to /bin/false when nothing else can be used.

If you need to find out the home directory for a given user, you can't rely on the format of /etc/passwd or on the output of egetent, as they are different on FreeBSD and OSX. To avoid this, the egethome function (provided by portability eclass, but available inheriting eutils), returns the right field making sure that is the home directory for the user:

## Applications

### Bash and shells

Bash is the default shell of Gentoo Linux, and it's on top of this that ebuilds are written. Because of this, Gentoo/Alt projects provide bash in their base system, as /bin/bash.

Ebuilds and scripts can use bashisms without problems as bash is provided, but in case of scripts, it's important that the first line reads as #!/bin/bash instead of just #!/bin/sh, to make sure that they are being executed with bash. The same goes for calling scripts from inside ebuilds, sh somescript is deprecated in favor of bash somescript so that bash is called for sure.

While bash should take care of managing \[ .. \] tests, it's suggested to use \[\[ .. \]\] to run tests also when quoting the arguments correctly. This way, the tests are guaranteed to be expanded by bash internally and do not get executed by /bin/\[ instead.

### GNU tools and non-GNU userlands

Gentoo/Alt projects, a part from Gentoo GNU/kFreeBSD, aims to have a complete system that uses native versions of all the system's tools, libc, and so on. However, there are some parts of Gentoo's base system that does not work with BSD-like tools (not because they are broken, just because they are strictly POSIX). To work around this problem, we install a series of GNU tools g-prefixed. In a Gentoo/Alt system profile, you can always find these tools: GNU sed (as gsed), GNU make (as gmake), GNU awk (as gawk), GNU patch (as gpatch) GNU diff (as gdiff) and usually GNU cmp (as cmp, for compatibility with a couple other scripts). If you need GNU-style find command, you can also install findutils package that provides gfind and gxargs.

To allow users who prefer the GNU-style tools to be comfortable, there is an ebuild sys-apps/userland-gnu that depends on coreutils and other GNU packages, that are always g-prefixed, and then links them inside /usr/libexec/gnu without the g-prefix. After adding them to $PATH, it's possible to use GNU tools in command line.

It's always preferred to use just the POSIX options, to be able to run the same script identical on Gentoo/Linux and Gentoo/Alt. In case you cannot rewrite the code not to use the GNU extensions, just add a dependency over the right package that provides the tool, and then do a simple test:

1. Check for the g-prefixed tool
2. Check that it works as expected for a GNU tool (usually this means that it accepts version option and reports GNU, FSF or the name of the package there)

And then select the right one you need.

{{Warning|The GNU tools' aliases works only if the tools are called by themselves, they won't work inside scripts called by the ebuild, and they won't work if called by find .. -exec or by xargs. In those cases, it's usually better to check for the right tool to call. An important exception is the usage of the sed command to call GNU sed: being widely used and difficult to check or fix, Portage 2.1\_pre9 and later provides a wrapper script that calls GNU sed in any case when used from ebuilds as long as the $ESED variable is not set, in which case the called sed will be the one there indicated. This allows the usage of BSD sed when building BSD packages for instance.

To help with porting, there are a few constructs that are usually used out of habit that need to be fixed:

1. cp -a: this is a GNUism, as the -a option is not present on POSIX cp. Its replacement is told to be -dpPR, but -d is not POSIX itself, so -pPR is what you should replace -a with.
2. cp --parents: this other GNUism is used to copy a series of files from a tree maintaining their prefix. There's no equivalent option in BSD userland, so this should be avoided. If you need this, instead use the treecopy function in portability eclass, that works exactly as a cp --parents -pPR call.
3. seq doesn't exist on BSD userland, but there's a quick replacement: jot. Unfortunately its syntax is too variable, and poses a problem to manage. To avoid this, portability eclass has a seq function replacement that calls jot with the right parameters when called on a BSD userland. Still, make sure you're not trying to use some extra options to seq

### Using pmake

There are a few packages that require the use of pmake (parallel make, by NetBSD, the package we use is from Debian) to build on GNU userland, as the makefiles are incompatible with GNU make. As the name of this flavor of make varies depending on the userland you're in (it's pmake on GNU userland, bsdmake in Darwin userland and simply make in BSD userland). To avoid this trouble, you can use get\_bmake function from portability eclass.

If the package builds using ports-like interface, as done by FreeBSD system packages, bsdmk eclass (currently on Gentoo/Alt overlay) provides a simple way to handle the knobs: just add to ${mymakeopts} the list of knobs to pass during the pkg\_setup() function, and then call mkmake and mkinstall to compile and install the packages as specified.

### Note about different make

As the make command changes name depending on the system where it's being used, there is an important thing to keep in mind: recursive makefiles are often used to build subdirectories of a source tree, but sometimes they calls directly a new make command. This breaks when the makefiles are called with other versions.

To avoid this problem all the modern make commands sets an automatic variable $(MAKE) that carries the name of the command used to launch the make chain. Instead of calling a generic make, all the makefiles should then call $(MAKE) to be safe with different naming of the command.

Another fortunately less common problem is when a package decides that $(MAKE) is not an automatic variable and try to overwrite it to set to something else than what it is by default. This usually breaks compilation.

Both kind of errors requires patches to the original sources, that should be sent upstream as usual.

## Programming

### Linking to dlopen()

On glibc-based systems, the dlopen() function and other functions to handle run-time dynamic linking resides in the libdl library that must be linked explicitly in the program. On BSD systems instead those functions are provided by libc.

Some software does not check if it needs to link to libdl and always links to it. This breaks on Gentoo/\*BSD systems. To avoid this, autotool-ed projects can use a simple m4 macro, that can be found in gentoo-alt/m4s module (derived from xine-lib's macro collection) in the dl.m4 file.

This simple AM\_DL macro added to configure.ac (or configure.in) takes care of looking for the right library to link to, and sets DYNAMIC\_LD\_LIBS. Add that to the LIBADD variable for the target and it will work fine on both glibc-based and BSD-derived systems.

For packages that do not have a build system that can be fixed to recognize whether libdl is needed, it's possible to use the dlopen\_lib function in the portability eclass. This function will return -ldl where present, and nothing where it is not present, so one could use:

To make sure it gets linked where needed.

Linkers issues

A practice that can be an issue for Gentoo/Alt projects is assuming that the linker that will be used to build a package is GNU and provided by binutils. Some operating systems like Darwin use another kind of linker that is not completely compatible with the GNU one.

The most common error is to add --with-gnu-ld to the list of arguments used to call econf. This is superfluos, as autoconf is smart enough to figure out by itself when it's using GNU ld and when not. It also breaks when the ld is not GNU as expected.

Another problem is with linking of suid binaries. Gentoo Linux guidelines tell to link them with -Wl,-z,now option to get a non-lazy binding. Unfortunately this option - while supported by many other linkers - is not always called this way. To avoid this problem, you can use the bindnow-flags function (in flag-o-matic eclass), that finds the flags to append to have the non-lazy binding on the current linker.

As appending those flags is not always the best solution, it's usually preferred to patch the sources to use those flags just when linking the suid binaries. This can be tricky to allow use of non-GNU linkers. The solution is to use an undefined macro BINDNOW\_FLAGS, and then either export the variable in src\_compile or use it to call the emake command.

### malloc.h header

Some time ago, on some systems, in order to have the malloc() function, you had to include the malloc.h header. Currently, this header is deprecated and should not be included, instead the stdlib.h header is enough to get the right prototype for the function.

When using glibc or uclibc, including malloc.h just adds stdlib.h to includes. On other systems it simply does not exist. On FreeBSD, malloc.h exists, but it is a "trap". It basically throws a compilation error, stopping the build process. This can be discouraging for users, but it's a good way to know what software needs to be fixed to not try to include that file.

There are two simple ways to deal with this problem:

- Remove the line that includes malloc.h and prepare a patch for upstream devs (if needed, add stdlib.h to the includes, to have malloc's definition)
- Add a test for malloc.h in configure.ac (or configure.in), and protect the inclusion with a #ifdef HAVE\_MALLOC\_H .. #endif block, it works because the #error call bails out the preprocessor as well.

The first way is preferred, as malloc.h is a deprecated header and should not be used, although some projects that have to deal with very very very old systems would like to protect it with an additional check, even if this means having to deal with a longer configure stage.

### Locale libraries

One of the most used components in GNU software is gettext, a tool that allows the creation of i18n-capable packages (library and programs) with a relatively small amount of work and non-intrusive changes. Unfortunately, during the past years, the way gettext was integrated into GNU/Linux systems changed, creating a problem with Gentoo/FreeBSD now. Originally, the needed intl functions were always provided by gettext itself. However, recent versions of GNU libc provides them inside the standard library itself, thus not requiring anymore the linking to libintl as is required for system using other libc's.

Similarly, libiconv is a GNU library that provides ways to convert text between different characters encodings. Its functions used to be always be provided always by libiconv, but now they are also embedded inside GNU libc. gettext, to provide its functionalities, uses libiconv; at the same time, libiconv can use gettext to proivde i18n support for itself. This is not a problem for GNU libc users, as both libintl and libiconv are inside libc, but it's a problem for non-glibc systems as this would add a circular dependency. To avoid this, nls support in libiconv is forcefully disabled.

Almost every package that depends on libiconv/gettext and uses autotools has already support to link to the right libraries on non-glibc systems. Unfortunately, this is not always enough: gettext guide states what the packages must do to make NLS support optional, but this is not always followed as intended, and intl functions are called also if NLS is disabled during configure stage, causing failure in linking on non-glibc systems (this doesn't appear as a problem on glibc systems, as the functions are always available on libc, and it doesn't need to be linked against, while on systems where libiconv and libintl are used it needs to be explicitly linked against that). This can be simply avoided protecting under #ifdef ENABLE\_NLS .. #endif blocks the calls to setlocale(), testdomain() and other intl functions. This way, they will be called only when gettext is requested and linked against.

There's then the opposite problem: when building with NLS support, some packages fail with undefined references to functions like libintl\_gettext. This happens because the package does not really add the variables to link against to the linking command, that is, it misses to link to libintl when the functions are not provided by the libc. For autotooled packages the library that contains the library to link against for libintl\_\* functions is LTLIBINTL, while the one for libiconv\_\* function is LTLIBICONV. Don't be confused by the LT prefix, those are not dependent on libtool presence, but rather means that libintl/libiconv were built with libtool, so passes -L${libdir} -lintl commandline instead of pointing directly to the .so file.

The way gettext and libiconv are used by some GNU software, with respect to generic software, actually adds a couple more problems to Gentoo/\*BSD systems. Packages like tar, gettext, glib2 and libiconv themselves create, for some reason, a ${libdir}/charset.alias. This file is going to have collisions with every package that creates one, so this will not be a good thing. The solution is to let libiconv install the only instance of it, and remove it from every other package, that's why some packages have one rm -f /usr/$(get\_libdir)/charset.alias line. Remember that the -f is needed, as the file won't be created by Gentoo/Linux with glibc.

Another problem can be defining the dependencies for packages. It's always a bit of a problem knowing if gettext must be a runtime dependency or just a build time dependency, and whether it is related to the nls USE flag or not. On a glibc-based system gettext is a build-time dependency if nls USE flag is enabled (and the package does not regenerate autotools support files), or an unconditional build-time dependency when autopoint needs to be run. On a non-glibc system, gettext is also a run-time dependency when building with nls USE flag enabled. While currently gettext and libiconv are in the list of system packages for Gentoo/\*BSD ports, the complete dependencies should be stated as:

Obviously this is true when nls USE flag is present and is honored by both the ebuild and the configure script. However, please remember that --disable-nls presence on configure --help does not always mean that nls support can be disabled or is even present.

Similarly, the packages that uses the iconv() function and thus requires libiconv package when building on non-GLIBC systems, have to depend on virtual/libiconv, that doesn't add extra dependencies on GLIBC users but satisfy the dependencies for non-GLIBC users.

### yacc and byacc

Other packages that needs special treatment for Gentoo/\*BSD are yacc and byacc. Their sources are present inside the base system of FreeBSD and other BSDs, but they are different from the original yacc/byacc, so it's better to use these versions, more updated than the ones released and in portage.

For this reason, when a package depends on yacc or byacc, it should put a || ( ) dependency with the package providing the same command on FreeBSD: sys-freebsd/freebsd-ubin.

If that dependency becomes widespread, a new virtual would be proposed.
