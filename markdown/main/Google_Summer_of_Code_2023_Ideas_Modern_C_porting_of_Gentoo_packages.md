<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2023/Ideas/Modern_C_porting_of_Gentoo_packages | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2023/Ideas/Modern C porting of Gentoo packages -->
---
title: Google Summer of Code/2023/Ideas/Modern C porting of Gentoo packages
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2023/Ideas/Modern_C_porting_of_Gentoo_packages
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-08"
fingerprint: "66f1b38a6e70a769"
license: CC BY-SA 4.0
---

# Google Summer of Code/2023/Ideas/Modern C porting of Gentoo packages

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Newer versions of compilers are planning on becoming significantly stricter with the C code they will accept or reject. This is a huge problem for the general Linux ecosystem.

Even worse is that some of these failures are silent and lead to incorrect behaviour at runtime. Extra care must be taken to detect this and we must prioritise these cases because they're the most harmful. With the release of Clang 16 (expected March 2023), the following specified warnings will be treated as errors:

- -Werror=implicit-function-declaration
- -Werror=implicit-int
- -Werror=int-conversion (been enabled in clang 15)
- -Werror=incompatible-function-pointer-types (for GCC a student might have to use -Werror=incompatible-pointer-types instead)

Also, in the coming years with C2x (likely C23) additional changes like removing certain deprecated prototypes will be made.

The above changes will affect affect Gentoo packages in the following ways:

- Lots of packages fail to build with these settings
- Sometimes packages build successfully but their ./configure scripts have misdetected features or otherwise made the wrong conclusion about the system because they expect a test to succeed when it now fails.

Please read [Modern C porting](https://wiki.gentoo.org/wiki/Modern_C_porting) in detail and see also [bug #870412](https://bugs.gentoo.org/show_bug.cgi?id=870412).

Students can test and provide patches for affected packages on both glibc and musl systems in parallel, but keeping glibc as the primary target might help fixes reach users faster.




| Contacts | Required Skills | 
|---|---|
|  |  | 
| Expected Project Size | Expected Outcomes | 
| 350 |  | 
| Project Difficulty |  | 
| Medium |  |
