<!-- source: https://wiki.gentoo.org/wiki/Boost/How_build_systems_find_boost | group: Gentoo Wiki (Main) | wiki-title: Boost/How build systems find boost -->
---
title: Boost/How build systems find boost
url: https://wiki.gentoo.org/wiki/Boost/How_build_systems_find_boost
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-05-31"
fingerprint: ffac8adb6ca1e58d
license: CC BY-SA 4.0
---

# Boost/How build systems find boost

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Since Boost does not provide any reasonable way for various build systems to find it, they use random hackery to do that. In order to choose a good location for boost files, and a way for the future *boost-utils.eclass* to enforce a particular version, this article will hold the research on how various build systems find boost currently.

## boost.m4

[boost.m4](https://github.com/tsuna/boost.m4) is one of the macro sets for autotools to find boost.

### headers

The macro adds a *--with-boost* option which allows the user to specify the path to boost installation prefix. If it is specified, the following directories are used for lookup:

"$with\_boost/include" "$with\_boost"

Otherwise, the following default paths are used:

. /opt/local/include /usr/local/include /opt/include /usr/include C:/Boost/include

For each of the fore-mentioned paths, boost lists the directory, collects all directories starting with *boost-* and sorts them using the version; starting with the newest version, it tries to compile a test application with a >= version test. If none of the *boost-* subdirs succeed, it tries one last time without a subdir.

In other words, boost.m4 always tries to find the newest boost available. However, passing *--with-boost* to a particular boost include directory should force it to use a particular version.

### libraries

If *--with-boost* was specified, *$with\_boost/lib* is used for the library lookup. Otherwise, the previously-found include path is used with *include* directory stripped. In that directory, the macro tries various potential boost library names, first with version number, then without one.

If no variant of the particular library is found in the directory established above, the search is retried in the following directories:

. /opt/local/lib\* /usr/local/lib\* /opt/lib\* /usr/lib\* "$with\_boost" C:/Boost/lib /lib\*

### Possible solutions

Any solution which would allow us to specify a *--with-boost* pointing to a directory with just one boost version would suffice. The includes can be either in the *include* subdirectory or in the directory. The libraries should either be in the *lib* subdirectory, or in a system-wide directory with a version appended. However, having them in the specified root will suffice as well as long as there are no non-version-suffixed libraries in the earlier tested directories; it will just make the macro more time-consuming.

## FindBoost.cmake

FindBoost.cmake was the official cmake module (provided by cmake upstream) to find boost. Now deprecated, since Boost itself provides corresponding installation CMake configs.

### headers

CMake looks for boost headers in the following locations:

1. *(additional CMake-specific directory vars)*
2. *${BOOST\_INCLUDEDIR}*
3. *${BOOST\_ROOT}*/include
4. *${BOOST\_ROOT}*
5. C:/boost/include
6. C:/boost
7. *$ENV{ProgramFiles}*/boost/include
8. *$ENV{ProgramFiles}*/boost
9. /sw/local/include
10. *(system paths)*

Additionally, a *boost-x\_yy* subdirectories are checked below each of the fore-mentioned paths, starting with the newest boost version known to the cmake module.

Thus, setting *${BOOST\_INCLUDEDIR}* is probably the best thing to do here, or *${BOOST\_ROOT}*.

### libraries

CMake looks for boost libraries in the following locations:

1. *(additional CMake-specific directory vars)*
2. *${BOOST\_LIBRARYDIR}*
3. *${BOOST\_ROOT}*/lib
4. *${BOOST\_ROOT}*/stage/lib
5. *${Boost\_INCLUDE\_DIR}*/lib
6. *${Boost\_INCLUDE\_DIR}*/../lib
7. *${Boost\_INCLUDE\_DIR}*/stage/lib
8. C:/boost/lib
9. C:/boost
10. *$ENV{ProgramFiles}*/boost/boost\_*${Boost\_MAJOR\_VERSION}*\_*${Boost\_MINOR\_VERSION}*\_*${Boost\_SUBMINOR\_VERSION}*/lib
11. *$ENV{ProgramFiles}*/boost/boost\_*${Boost\_MAJOR\_VERSION}*\_*${Boost\_MINOR\_VERSION}*/lib
12. *$ENV{ProgramFiles}*/boost/lib
13. *$ENV{ProgramFiles}*/boost
14. /sw/local/lib
15. *(system paths)*

The versioned boost library names (matching header version) are considered before unversioned ones.

### Possible solutions

The only solution seems to provide *-DBOOST\_INCLUDEDIR* or *-DBOOST\_ROOT* pointing to the location of correct version boost headers. Afterwards, the module should be able to locate the correct versioned libraries in system path.
