<!-- source: https://wiki.gentoo.org/wiki/Preserve-libs | group: Gentoo Wiki (Main) | wiki-title: Preserve-libs -->
---
title: preserve-libs
url: https://wiki.gentoo.org/wiki/Preserve-libs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-01-27"
fingerprint: "23b543eb88839188"
license: CC BY-SA 4.0
---

# preserve-libs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**preserve-libs** is a [Portage feature](https://wiki.gentoo.org/wiki/FEATURES) that preserves libraries when [sonames](https://en.wikipedia.org/wiki/Soname) change during upgrade or downgrade, only as necessary to satisfy shared library dependencies of installed consumers. Preserved libraries are automatically removed when there are no remaining consumers, which occurs when consumer packages are rebuilt or uninstalled. Ideally, rebuilds are triggered automatically during updates, in order to satisfy [slot-operator](https://wiki.gentoo.org/wiki/Sub-slots_and_Slot-Operators) dependencies. Before emerge exits after installing updates, if there are remaining preserved libraries because slot-operator dependencies have not been used to trigger automatic rebuilds, then emerge will display a message like the following:

`root #``emerge --ask --verbose --update --deep --changed-use @world`
...
!!! existing preserved libs:
>>> package: sys-libs/libfoo-1
 \* - /lib/libfoo.so.1
 \*      used by /usr/bin/bar (app-foo/bar-1)
Use emerge @preserved-rebuild to rebuild packages using these libraries

In some cases, preserved libraries may remain even after the user has rebuilt the relevant package(s). In cases like these, repeated invocations of emerge [@preserved-rebuild](https://wiki.gentoo.org/wiki/Preserved-rebuild) will not eliminate the preserved libraries. This indicates that the build system for a particular package causes it to inappropriately link against the old (preserved) version of the library, instead of the new one. If you find a package like this, then you should file a bug for the package maintainer (for example, see [bug #230257](https://bugs.gentoo.org/show_bug.cgi?id=230257)). As a workaround, you can manually remove the old library (such as /lib/libreadline.so.5.2) and then run revdep-rebuild in order to rebuild the packages which linked against it. A list of all preserved libraries may be obtained using:

`root #``portageq list_preserved_libs /`
If `preserve-libs` is **not** enabled in `FEATURES`, then users need to manually run revdep-rebuild in order detect broken library dependencies and rebuild the appropriate packages.

Note that libraries preserved by `preserve-libs` (or alternatively by the `preserve_old_lib` function of [preserve-libs.eclass](http://devmanual.gentoo.org/eclass-reference/preserve-libs.eclass/index.html)) prevent breakage that would otherwise be detectable by revdep-rebuild. Therefore revdep-rebuild (without special arguments) is not useful for rebuilding consumers of preserved libraries. Instead, if `preserve-libs` is enabled, then emerge will advise the user to run emerge @preserved-rebuild when necessary. Alternatively, if `preserve-libs` is **not** enabled and the `preserve_old_lib` function from [preserve-libs.eclass](http://devmanual.gentoo.org/eclass-reference/preserve-libs.eclass/index.html) has been called by an [ebuild](https://wiki.gentoo.org/wiki/Ebuild) in order to preserve a library, then the user will receive an ewarn message like the following:

`root #``emerge --ask --verbose --update --deep --changed-use @world`
...
 \* Old versions of installed libraries were detected on your system.
 \* In order to avoid breaking packages that depend on these old libs,
 \* the libraries are not being removed.  You need to run revdep-rebuild
 \* in order to remove these old dependencies.  If you do not have this
 \* helper program, simply emerge the 'gentoolkit' package.
 \* 
 \*   # revdep-rebuild --library '/lib/libfoo.so.1' && rm '/lib/libfoo.so.1'
