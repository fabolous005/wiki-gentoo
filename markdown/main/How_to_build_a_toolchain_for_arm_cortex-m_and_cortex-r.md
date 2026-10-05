<!-- source: https://wiki.gentoo.org/wiki/How_to_build_a_toolchain_for_arm_cortex-m_and_cortex-r | group: Gentoo Wiki (Main) | wiki-title: How to build a toolchain for arm cortex-m and cortex-r -->
---
title: How to build a toolchain for arm cortex-m and cortex-r
url: https://wiki.gentoo.org/wiki/How_to_build_a_toolchain_for_arm_cortex-m_and_cortex-r
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-09-28"
fingerprint: "468e2eb8504729c3"
license: CC BY-SA 4.0
---

# How to build a toolchain for arm cortex-m and cortex-r

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

For up to date information on [ARM](https://wiki.gentoo.org/wiki/ARM) toolchain building (also for Cortex-R and Cortex-M), please review the main [ARM](https://wiki.gentoo.org/wiki/ARM) article.

## Toolchain installation steps

[Crossdev](https://wiki.gentoo.org/wiki/Crossdev) can almost build a functioning toolchain for embedded arm development. The toolchain can be created with the following steps:

Step 1:

`root #``crossdev --lenv 'USE="nano -nls -threads -unicode"' -s3 -t arm-unknown-eabi`
Step 2:

`root #``crossdev --lenv 'USE="nano -nls -threads -unicode"' --genv 'USE="cxx -nls -nptl -pch -pie -ssp" EXTRA_ECONF="--with-multilib-list=rmprofile --disable-decimal-float --disable-libffi --disable-libgomp --disable-libmudflap --disable-libquadmath --disable-shared --disable-threads --disable-tls"' -s4 -t arm-unknown-eabi`
Step 3:

`root #``emerge --ask cross-arm-unknown-eabi/newlib`
Step 4:

`root #``crossdev --lenv 'USE="nano -nls -threads -unicode"' --genv 'USE="cxx -nls -nptl -pch -pie -ssp" EXTRA_ECONF="--with-multilib-list=rmprofile --disable-decimal-float --disable-libffi --disable-libgomp --disable-libmudflap --disable-libquadmath --disable-shared --disable-threads --disable-tls"' -s4 --ex-gdb -t arm-unknown-eabi`
Now you'll have a functioning multilib / multiarch for embedded arm development with small code output size.

Big thanks to the Gentoo forum user **rapsure** who posted this instructions [here](https://forums.gentoo.org/viewtopic-t-1085836.html).

## Toolchain installation steps with hardware floating point

You may also want the toolchain to contain support for hardware floating point. In that case, use these instructions instead:

Step 1:

`root #``crossdev --lenv 'USE="nano -nls -threads -unicode"' -s3 -t arm-unknown-eabi`
Step 2:

`root #``crossdev --lenv 'USE="nano -nls -threads -unicode"' --genv 'USE="cxx -nls -nptl -pch -pie -ssp" EXTRA_ECONF="--with-multilib-list=rmprofile --disable-decimal-float --disable-libffi --disable-libgomp --disable-libmudflap --disable-libquadmath --disable-shared --disable-threads --disable-tls"' -s4 -t arm-unknown-eabi`
Step 3:

`root #``emerge --ask cross-arm-unknown-eabi/newlib`
Step 4:

`root #``crossdev --lenv 'USE="nano -nls -threads -unicode" EXTRA_ECONF="--enable-newlib-hw-fp"' --genv 'USE="cxx -nls -nptl -pch -pie -ssp" EXTRA_ECONF="--with-multilib-list=rmprofile --disable-decimal-float --disable-libffi --disable-libgomp --disable-libmudflap --disable-libquadmath --disable-shared --disable-threads --disable-tls"' -s4 --ex-gdb -t arm-unknown-eabi`
