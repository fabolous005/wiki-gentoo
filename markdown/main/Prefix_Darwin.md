<!-- source: https://wiki.gentoo.org/wiki/Prefix/Darwin | group: Gentoo Wiki (Main) | wiki-title: Prefix/Darwin -->
---
title: Prefix/Darwin
url: https://wiki.gentoo.org/wiki/Prefix/Darwin
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-09-25"
fingerprint: e788ebdc6e35b7ef
license: CC BY-SA 4.0
---

# Prefix/Darwin

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Prefix/Darwin brings the power of Gentoo to Darwin (macOS/OS X) based systems, similar to pkgsrc, macports, and homebrew.

## General

- macOS follows the [normal bootstrap procedure](https://wiki.gentoo.org/wiki/Project:Prefix/Bootstrap)
- Bootstraps currently done via GCC (`DARWIN_USE_GCC=1` is set by default)
  - host/Apple Clang from xcode tools is used then GCC is built as soon as possible
  - Using GCC as the "system" compiler within Prefix has problems
    - [https://gcc.gnu.org/bugzilla/show\_bug.cgi?id=90709](https://gcc.gnu.org/bugzilla/show_bug.cgi?id=90709) (general tracker for macOS header/frameworks issues with GCC)
    - [https://gcc.gnu.org/bugzilla/show\_bug.cgi?id=78352](https://gcc.gnu.org/bugzilla/show_bug.cgi?id=78352) (Blocks support)
      - As a result, can't build things using e.g. Aqua (nice GUI) integrations or other Frameworks (e.g. git keychain integration).
  - We use the system (host) linker because we can't build Apple's linker independently within our prefix without Clang, because it uses Blocks
- Possibility of bootstrapping via Clang (`DARWIN_USE_GCC=0`)
  - It's *possible* but doesn't work yet!
  - Needs some work to get sysroot, SDK paths right. See what e.g. macports does for hints.
  - [bug #758167](https://bugs.gentoo.org/show_bug.cgi?id=758167) is the meta bug for this work.
  - Help very much welcome!
    - To attempt a full bootstrap using Clang (this **won't work** yet, but it'll do something): `DARWIN_USE_GCC=0 ./bootstrap-prefix.sh`
    - Suggestion: start with trying to build Clang, LLVM, and friends within a fully-built working GCC-bootstrapped Prefix and go from there
    - Suggestion: look at macports patches

## Platforms

### arm64-macos

M1 macs, etc.

It finally works! But is highly experimental, functionality of anything other than the base system is not guaranteed and will likely have issues. After extensive testing and fixing, arm64-macos can now be bootstrapped. However, support is still highly experimental and the possibility of a regression cannot be ruled out. Thus, a manual bootstrap is recommended. You should be prepared to debug and report any problem should it arise, see [Prefix/Manual Bootstrap](https://wiki.gentoo.org/wiki/Project:Prefix/Manual_Bootstrap) for more information.

By default, the "stable" snapshot is used, which was previously broken but is now known to work after several fixes. For now, it's recommended to use the default "stable" snapshot. Alternatively, `export LATEST_TREE_YES=1` can be used to use the latest code from Portage upstream during bootstrapping. Depending on phase of the moon, `export LATEST_TREE_YES=1` may either contain the latest fixes for new problems, or introduce new regressions. To track all the known bootstrap issues on macOS 13, we use "[bug #886491](https://bugs.gentoo.org/show_bug.cgi?id=886491) bootstrap-prefix.sh fails with macOS 13 (Ventura)". Make sure to watch it for any further development of the situation.

Again, since it's still highly experimental, unlike `~x64-macos`, the vast majority of the packages are currently masked on `~arm64-macos`. Almost everything needs to be unmasked manually right now (in case you see a masked package that prevents bootstrap from progressing, please report it at [bug #904474](https://bugs.gentoo.org/show_bug.cgi?id=904474)).

Finally, the toolchain still needs to be worked on. As of early 2023, we're still waiting for GCC support to be merged upstream and in a release (GCC 12?). We currently use a snapshot from GCC maintainer Iain Sandoe's personal development branch in our scripts as a temporarily solution, see [https://github.com/iains/gcc-darwin-arm64](https://github.com/iains/gcc-darwin-arm64) - all ARM64 macOS systems use this same GCC fork, including nixOS and Homebrew. Furthermore, [sys-devel/binutils-apple](https://packages.gentoo.org/packages/sys-devel/binutils-apple) fails to build, so the linker from the host is used for now.

- [bug #778014](https://bugs.gentoo.org/show_bug.cgi?id=778014) - Prefix: Big Sur ARM (M1 MacBook Pro MYD82LL/A) build failure due to missing symbols
- [bug #792780](https://bugs.gentoo.org/show_bug.cgi?id=792780) - [sys-devel/binutils-apple](https://packages.gentoo.org/packages/sys-devel/binutils-apple) fails to build during prefix bootstrap on M1 (Big Sur 11.4)

### x64-macos

Historically works okay, with a mature set of packages with the `~x64-macos` keyword.

But as of macOS 13, there were several problems. As of June 2023, all problems are believed to be fixed, and the bootstrap should work as expected by now.

To track all the known bootstrap issues on macOS 13, we use "[bug #886491](https://bugs.gentoo.org/show_bug.cgi?id=886491) bootstrap-prefix.sh fails with macOS 13 (Ventura)". Make sure to watch it for any further development of the situation.

### ppc-macos

Still somewhat supported as the supported OS versions age. But it's likely untested for a long time, not for the faint-hearted. Recommended only for experienced Gentoo users, be prepared to report and debug problems on the go.

## Non-platforms

### x86-macos

Support was dropped recently due to lack of interest, but could be restored with a sponsor/cheerleader.
