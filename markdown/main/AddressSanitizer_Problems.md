<!-- source: https://wiki.gentoo.org/wiki/AddressSanitizer/Problems | group: Gentoo Wiki (Main) | wiki-title: AddressSanitizer/Problems -->
---
title: AddressSanitizer/Problems
url: https://wiki.gentoo.org/wiki/AddressSanitizer/Problems
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-08-15"
fingerprint: "8523da539840ad41"
license: CC BY-SA 4.0
---

# AddressSanitizer/Problems

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Common problems when trying to compile Gentoo with [Address Sanitizer](https://wiki.gentoo.org/wiki/AddressSanitizer).

## Address Sanitizer errors

Some applications just read or write invalid memory or have use after free errors in their normal operation. These should all be fixed. The Address Sanitizer error message is already a very helpful analysis of such problems. The errors are logged in /var/log/asan. To make the error messages more helpful it often makes sense to recompile applications with `-g` in the CFLAGS (debugging symbols) and with `FEATURES="nostrip"`.

## Libtool issues

When linking shared libraries libtool will filter the LDFLAGS and only allows known good flags. The current version of libtool is unaware of Address Sanitizer and will therefore also filter the `-fsanitize=address` flag from the linker call. This breaks the compilation.

This [has been fixed upstream](https://git.savannah.gnu.org/cgit/libtool.git/commit/) and this fix is part of the libtool ebuild in the asantoo overlay. This fix is not part of a libtool release yet. It will take a while until this change takes effect. The problem is that the libtool scripts are bundled with applications. Therefore even if upstream libtool allows the `-fsanitize=address` flag, this change has to be made on the systems of the upstream packagers.

As the number of packages affected by this is huge the asantoo-base package in the overlay installs a Portage hook that will automatically patch all applications that ship an affected libtool script (the problem is in the ltmain.sh file).

## pthread linking issues

Some configure scripts test the need of the `-lpthread` compiler flag by testing whether the function `pthread_create()` is available without it. The Address Sanitizer library (libasan) provides this function. However it does not have a full pthread implementation, more advanced pthread functions aren't provided. Therefore the compilation fails.

A workaround is to add the `-lphtread` compiler flag manually, the overlay provides a package.env file which does this for some known packages with this problem.
