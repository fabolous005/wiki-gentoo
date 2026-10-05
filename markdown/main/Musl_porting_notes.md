<!-- source: https://wiki.gentoo.org/wiki/Musl_porting_notes | group: Gentoo Wiki (Main) | wiki-title: Musl porting notes -->
---
title: Musl porting notes
url: https://wiki.gentoo.org/wiki/Musl_porting_notes
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-06"
fingerprint: "2e85fa5b6c2dabd8"
license: CC BY-SA 4.0
---

# Musl porting notes

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[musl](https://wiki.gentoo.org/wiki/Musl) is very strict regarding standards-conformance compared to the widely used GNU C Library (glibc). This means that many of the GNU extensions, as well as much of the backwards compatibility provided by glibc is completely absent, and applications that use these will often fail to build. Here are some 
**pointers on getting software to compile with musl**.

## Macro errors

Errors like these are usually the easiest, and thankfully the most common issues encountered when porting software to musl. Oftentimes it is possible to just copy the definition from glibc, and then conditionally define it. Sometimes these macros are only aliases in glibc, and if that's the case just replace the macro with the original one. The good way of dealing with this is to remove the usage of these macros, and submitting patches upstream.

### MAXNAMLEN not defined here

`MAXNAMLEN` is the BSD name for `NAME_MAX`. glibc aliases this as `NAME_MAX`, but not musl, so applications which tries to use this macro will fail to build.

To fix this:

1. Include `<limits.h>`.
2. Add a conditional "ifdef" for `NAME_MAX`, if it's defined then use it. If not, fall back to `MAXNAMLEN`. The reason to fall back to `MAXNAMELEN` is to make the BSD users and friends happy.

See also [glibc's documentation](https://www.gnu.org/software/libc/manual/html_node/Limits-for-Files.html).

### MSG\_TRYHARD undeclared

`MSG_TRYHARD` is also one of these glibc aliases (`_GNU_SOURCE` set). Just use `MSG_DONTROUTE` instead.

### S\_BLKSIZE undeclared

`S_BLKSIZE` is an alias for `DEV_BSIZE`. Just use `DEV_BSIZE` instead.

## Undefined references and missing functions

These errors are very similar to the macro errors above, but for functions instead of macros. The most common cause for undefined reference errors is that the program in question uses some GNU extension, or that musl has moved the function into a separate header that needs to be included first.

### getopt was not declared in this scope

musl moves this into its own header. To fix, include `<getopt.h>`.

### undefined reference to getpt

`getpt` is specific to glibc, instead use the portable `posix_openpt` function, for example.

Example: "net-misc/vmnet-0.4: vmnet.c:(.text+\<snip>): undefined reference to getpt" [bug #712470](https://bugs.gentoo.org/show_bug.cgi?id=712470)

### undefined reference to fts\* (ex. fts\_read)

These functions are part fts, a set of functions in glibc that are not in musl libc. There is a standalone package for this here: [sys-libs/fts-standalone](https://packages.gentoo.org/packages/sys-libs/fts-standalone). To fix this, simply add the standalone as a DEPEND for the affected package:

**`package.ebuild`**

```
DEPEND="
    ...
    elibc_musl? ( sys-libs/fts-standalone )
    ...
"
```
### undefined reference to \`libintl\_dgettext'

Example: "media-libs/fontconfig-2.13.0-r2: undefined reference to \`libintl\_dgettext' \[...\] on amd64-fbsd" [bug #652674](https://bugs.gentoo.org/show_bug.cgi?id=652674)

### strtol\_l not declared

strtol is a function to convert a string to an integer type. The '\_l' counterpart takes an additional locale parameter to be used instead of the global locale. musl only uses "C.UTF-8", so passing a local locale does not make any sense.

Check for strtol\_l in the build system and use strtol if it's not available to fix this. Platform macros can also be used, though configure checks are usually prefered.

### error: LFS64 interfaces (\*64 undeclared here, ex. pread64)

The Gentoo tracker bug for these issues is [bug #903611](https://bugs.gentoo.org/show_bug.cgi?id=903611).

The legacy "LFS64" ("large file support") interfaces, which were provided by macros remapping them to their standard names (`#define stat64 stat` and similar) have been deprecated and are no longer provided under the \_GNU\_SOURCE feature profile, only under explicit \_LARGEFILE64\_SOURCE. The latter will also be removed in a future version. [https://musl.libc.org/releases.html](https://musl.libc.org/releases.html)

The correct fix is to adjust the code to use standard `off_t` types and then to cater for glibc by passing `-D_FILE_OFFSET_BITS=64` to avoid regressing glibc systems. In autoconf, this can be done with the `AC_SYS_LARGEFILE` macro (if using this, config.h **must** be consistently included before all standard headers everywhere, or corruption may occur).

As a temporary workaround, `-D_LARGEFILE64_SOURCE` can be appended to `CPPFLAGS` by doing

**`borked.ebuild`**

```
inherit flag-o-matic
...
src_compile() {
    # Temporary workaround for musl-1.2.4 (upstream bug #123456, gentoo bug #123456)
    # XXX: This will stop working with future musl releases!
    append-cppflags "-D_LARGEFILE64_SOURCE"
}
```
See also: [musl release notes, see 1.2.4](https://musl.libc.org/releases.html)

## Missing headers

musl is relatively selective of what should go into the core musl libc codebase. The reasoning is obviously different on a case-to-case basis but usually it boils down to:

1. GNU extensions.
2. Often unused/error-prone functionality.
3. Makes more sense as a separate library.

This can often be worked around with \*-standalone packages. Be aware that some standalones, like [sys-libs/cdefs-standalone](https://packages.gentoo.org/packages/sys-libs/cdefs-standalone), are only there for user convenience. Preferably usage of these should be reported upstream.

### error.h: No such file or directory

error.h just provides extra ways to report errors. This is a GNU extension and is not provided by musl. To fix this, either use the `perror` function, or combine `fprintf(stderr, ...)` with `exit(EXIT_FAILURE)`.

[sys-libs/error-standalone](https://packages.gentoo.org/packages/sys-libs/error-standalone) is available for users' comfort when compiling third-party software, but contributors and developers should fix these errors, and preferably fix this upstream.

Example: "net-libs/iax-0.2.2-r3 : iax.c: fatal error: error.h: No such file or directory " [bug #712510](https://bugs.gentoo.org/show_bug.cgi?id=712510)

### cdefs.h: No such file or directory

Developers like to wrongly include sys/cdefs.h to use the `_*_DECLS` macros. This is a bug and the correct way to do it is to use:

**`bug.cpp`**

```
#ifdef __cplusplus
extern "C" {
#endif
```
instead of
`_BEGIN_DECLS`
and

**`bug.cpp`**

```
#ifdef __cplusplus
}
#endif
```
instead of
`_END_DECLS`

## Other build time errors

Other build time errors that do not belong to any of the above sections.

### error: assignment of read-only variable '\[stdout|stdin|stderr\]'

In musl stdout, stdin and stderr are read-only and cannot be set like in glibc.

To fix this, use freopen like this:

Instead of:

See: [glibc standard streams](https://www.gnu.org/software/libc/manual/html_node/Standard-Streams.html).

Example of this: [lvm2 fix](https://github.com/gentoo/gentoo/commit/2a99ca696dc1229ec4bbe7aa7dc4fb37533c839b#diff-049fb37dde861dc3a15ffcfe8af1ec9e0b2ff8ba4552ef6643b0739c0830bfe8)

This functionality is not implemented in musl mostly due to the "usefulness/security-risk"-ratio beeing far too low. It is however mandated by POSIX and therefore musl defines it as stubs instead of just not including it at all.

Because it's implemented with stubs instead of simply not being there it means that builds will not fail with a simple "{u,w}tmp.h" not found error as expected. Builds can instead complain about things like undefined macros such as WTMPX\_FILENAME and \_PATH\_WTMPX, or not finding a valid path to utx.log. This should almost always be solved by making the functionality optional/conditionally removing it.

Example: [AccountsService: "Do not know which filename to watch for wtmp changes"](https://gitlab.freedesktop.org/accountsservice/accountsservice/-/merge_requests/97)

## Runtime issues

## See also

- [Libc](https://wiki.gentoo.org/wiki/Libc) — a software component that allows userspace applications to interact with operating system services.
- [Musl](https://wiki.gentoo.org/wiki/Musl) — a [standard C library](https://wiki.gentoo.org/wiki/Libc) implementation that strives to be lightweight and correct in the sense of standards
- [Project:Musl](https://wiki.gentoo.org/wiki/Project:Musl) - Gentoo's musl project

## External resources

### Standalone packages

glibc includes various extra functions which are not part of POSIX, so musl does not include them.

[User:blueness](https://wiki.gentoo.org/wiki/User:Blueness) has ported and added the common ones to Gentoo:

- [sys-libs/fts-standalone](https://packages.gentoo.org/packages/sys-libs/fts-standalone) (adds [fts](http://man7.org/linux/man-pages/man3/fts.3.html) - functions to traverse directories etc)
- [sys-libs/obstack-standalone](https://packages.gentoo.org/packages/sys-libs/obstack-standalone) (obstack in [glibc](https://www.gnu.org/software/libc/manual/html_node/Obstacks.html))
- [sys-libs/queue-standalone](https://packages.gentoo.org/packages/sys-libs/queue-standalone) (queue.h)
- [sys-libs/rpmatch-standalone](https://packages.gentoo.org/packages/sys-libs/rpmatch-standalone) (used for 'yes/no' [questions](http://man7.org/linux/man-pages/man3/rpmatch.3.html))
- [sys-libs/argp-standalone](https://packages.gentoo.org/packages/sys-libs/argp-standalone) ([extends](https://www.gnu.org/software/libc/manual/html_node/Argp.html) getopt)

### Porting tasks

- Tracker [bug](https://bugs.gentoo.org/713786) for missing includes/compile errors
- Bugs with [possible patches](https://bugs.gentoo.org/buglist.cgi?f1=blocked&keywords=PATCH%2C%20&keywords_type=allwords&list_id=4532876&o1=equals&query_format=advanced&resolution=---&v1=713786) to test and commit

If a patch is discovered in another distro (or if a developer creates one themselves!), please add PATCH to the keywords (if the correct permissions are possessed) and comment with a link to the patch. File a new bug if one does not already exist.

### Patches from other distros

If stuck, it may be worth seeing what other musl-using distros have done to fix the problem.

Be aware that some distros, like Alpine, include compatibility packages by default (for now), so this will not always help.

- [sabotage's patches](https://github.com/sabotage-linux/sabotage/tree/master/KEEP)
- [dragora's patches](https://git.savannah.gnu.org/cgit/dragora.git/tree/patches)
- netbsd's pkgsrc [patches](https://github.com/GregorR/musl-pkgsrc-patches)

By all means look at Alpine or Void Linux too, but they do not seem to have an easy listing of patches like the above.

- Alpine Linux [search](https://pkgs.alpinelinux.org/) ([git](https://git.alpinelinux.org/aports/tree/main/))
- Void Linux [search](https://voidlinux.org/packages/)
- [OpenEmbedded](https://cgit.openembedded.org/meta-openembedded/)
- [Buildroot](https://git.busybox.net/buildroot/)
- [Adélie Linux](https://pkg.adelielinux.org/current/-/search)
- Miscellaneous [projects](https://wiki.musl-libc.org/projects-using-musl.html#Linux-distributions-using-musl) using musl

### Other resources

- [#gentoo-hardened](ircs://irc.libera.chat/#gentoo-hardened) ([webchat](https://web.libera.chat/#gentoo-hardened))
- [#musl](ircs://irc.libera.chat/#musl) ([webchat](https://web.libera.chat/#musl))
- musl's [FAQ](https://wiki.musl-libc.org/faq.html)
- musl's POSIX [table](https://repo.or.cz/w/musl-tools.git/blob_plain/HEAD:/tab_posix.html); useful for seeing 'new' header file names
- musl's compatibility [page](https://wiki.musl-libc.org/compatibility.html)
- Gentoo's [musl overlay](https://gitweb.gentoo.org/proj/musl.git/)
