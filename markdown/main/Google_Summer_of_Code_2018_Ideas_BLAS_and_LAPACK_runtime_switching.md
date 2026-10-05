<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2018/Ideas/BLAS_and_LAPACK_runtime_switching | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2018/Ideas/BLAS and LAPACK runtime switching -->
---
title: Google Summer of Code/2018/Ideas/BLAS and LAPACK runtime switching
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2018/Ideas/BLAS_and_LAPACK_runtime_switching
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-11"
fingerprint: c9ec0aa41cf78ef9
license: CC BY-SA 4.0
---

# Google Summer of Code/2018/Ideas/BLAS and LAPACK runtime switching

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

BLAS (Basic Linear Algebra Subroutines) and LAPACK (Linear Algebra Package) are important mathematical libraries widely used in science, engineering, data science and other areas.

Gentoo supports a large number of BLAS and LAPACK implementations, but [switching between them is not implemented properly](https://bugs.gentoo.org/632624). There are [several approaches](https://github.com/gentoo/sci/issues/805) proposed and even [draft implementation for build-time switching](https://github.com/gentoo/sci/pull/837) is available.

Your goal will be to implement blas and lapack eclasses for run-time switching and port at least some of existing ebuilds to the new framework.



| Contacts | Required Skills | 
|---|---|
|  |  |
