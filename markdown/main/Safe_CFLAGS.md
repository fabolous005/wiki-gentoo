<!-- source: https://wiki.gentoo.org/wiki/Safe_CFLAGS | group: Gentoo Wiki (Main) | wiki-title: Safe CFLAGS -->
---
title: Safe CFLAGS
url: https://wiki.gentoo.org/wiki/Safe_CFLAGS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-17"
fingerprint: ddfdb12b51a0b1a
license: CC BY-SA 4.0
---

# Safe CFLAGS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

*Not to be confused with[CPU\_FLAGS\_\*](https://wiki.gentoo.org/wiki/CPU_FLAGS_*).*

This article provides a summary of "safe" settings for [CFLAGS](https://en.wikipedia.org/wiki/CFLAGS) on Gentoo Linux.

Default CFLAGS can be [set in make.conf](https://wiki.gentoo.org/wiki/Make.conf#CFLAGS_and_CXXFLAGS) for Gentoo systems. CFLAGS can also be [specified per-package](https://wiki.gentoo.org/wiki/Knowledge_Base:Overriding_environment_variables_per_package).

## Automatic CPU detection by the compiler

A recommended default choice for `CFLAGS` or `CXXFLAGS` is to use `-march=native`. This enables auto-detection of the CPU's architecture. A possible entry might look like:

**`/etc/portage/make.conf`**

```
COMMON_FLAGS="-O2 -pipe -march=native"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
```
To see what GCC detects "native" to be for certain system in particular, the following command can be run:

`user $``gcc -v -E -x c /dev/null -o /dev/null -march=native 2>&1 | grep /cc1 | grep mtune`
The internal translation of `-march=native` will be visible in the output. In some cases, if the CPU is unknown to GCC's detection model, a suboptimal `-mtune=generic` (or even no `-mtune`) will be visible. In this case, select relevant `-mtune=` from manual. In some other cases there are same to detected `-march=` or common `-mtune=intel` for (too) modern Intel CPUs.

[The cpu-type may also be detected with glibc.](https://wiki.gentoo.org/wiki/GCC_optimization#cpu-type)

Also possible suboptimal `-march=native` detection - full `l2-cache-size` to single CPU thread on multi-core CPUs. Currently it used only for prefetching, but sometimes good choice to fallback to default `--param=l2-cache-size=512` or own calculated value - to reduce cache concurrency on high SMP load. But this is in theory and not for all tasks - do nothing if unsure.

Additional information can be found at the [GCC optimization](https://wiki.gentoo.org/wiki/GCC_optimization) page.

## Determining CPU type in order to set CFLAGS manually

These tools can report system CPU information that can then be matched a CPU from the list further down this page, to get some suggested `CFLAGS` that are "safe" for that system.

These settings should be used, especially when unsure which `CFLAGS` the processor needs.

### CPU detection with resolve-march-native

A tool exists to *automagically* determine `-march=native` resolution values: [app-misc/resolve-march-native](https://packages.gentoo.org/packages/app-misc/resolve-march-native). After installing it, issue:

`user $``resolve-march-native --drop-cache-sizes`
### Reporting the CPU type with /proc/cpuinfo

To identify the model of the CPU, take a look inside /proc/cpuinfo for the "cpu family" and "model" numbers like so:

`user $``grep -m1 -A3 "vendor_id" /proc/cpuinfo`
## Safe CFLAGS list

### x86/amd64

#### Generic psABI levels

If using a [distcc](https://wiki.gentoo.org/wiki/Distcc) farm with slightly different CPUs, it might make more sense to generate code that is just old enough to work for all of them, without bogging down to the *really* generic code. The psABI microarchitecture levels aims to provide just that for common eras of amd64 CPUs. See [Wikipedia:x86-64#Microarchitecture\_levels](https://en.wikipedia.org/wiki/x86-64#Microarchitecture_levels) for a description of the levels.

#### Intel

##### Alder Lake

| **Core i3/i5/i7 12th Gen**``` vendor_id       : GenuineIntel cpu family      : 6 model           : 154 model name      : 12th Gen Intel(R) Core(TM) i7-1260P  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=alderlake -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Skylake, Kaby Lake, Kaby Lake R, Coffee Lake, Comet Lake

| **Core i3/i5/i7 and Xeon E3/E5 \*V5**``` vendor_id       : GenuineIntel cpu family      : 6 model           : 94 model name      : Intel(R) Core(TM) i7-6700K CPU @ 4.00GHz ... model           : 165 model name      : Intel(R) Core(TM) i5-10400F CPU @ 2.90GHz  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=skylake -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Broadwell

| **Core i3/i5/i7 and Xeon E3/E5 \*V4**``` vendor_id       : GenuineIntel cpu family      : 6 model           : 79 model name      : Intel(R) Xeon(R) CPU E5-2683 v4 @ 2.10GHz … model           : 79 model name      : Intel(R) Core(TM) i7-5650U CPU @ 2.20GHz  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=broadwell -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Haswell

| **Core i3/i5/i7 and Xeon E3/E5/E7 \*V3**``` vendor_id      : GenuineIntel cpu family     : 6 model          : 60 model name     : Intel(R) Xeon(R) CPU E3-1271 v3 @ 3.60GHz … model          : 60 model name     : Intel(R) Core(TM) i7-4770 CPU @ 3.40GHz  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=haswell -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Ivy Bridge

| **Core i3/i5/i7 and Xeon E3/E5/E7 \*V2**``` vendor_id       : GenuineIntel cpu family      : 6 model           : 58 model name      : Intel(R) Core(TM) i7-3610QM CPU @ 2.30GHz  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=ivybridge -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **Pentium**``` vendor_id	: GenuineIntel cpu family	: 6 model		: 58 model name	: Intel(R) Pentium(R) CPU G2020 @ 2.90GHz  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=ivybridge -mno-avx -mno-aes -mno-rdrnd -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Sandy Bridge

| **Core i3/i5/i7 and Xeon E3/E5/E7**``` vendor_id	: GenuineIntel cpu family	: 6 … model		: 42 model name	: Intel(R) Core(TM) i5-2400 CPU @ 3.10GHz … model		: 45 model name	: Intel(R) Core(TM) i7-3930K CPU @ 3.20GHz …  model		: 42 model name	: Intel(R) Xeon(R) CPU E31245 @ 3.30GHz …  model           : 45 model name      : Intel(R) Xeon(R) CPU E5-2407 0 @ 2.20GHz  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=sandybridge -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **Pentium**``` vendor_id	: GenuineIntel cpu family	: 6 model		: 42 model name	: Intel(R) Pentium(R) CPU B960 @ 2.20GHz  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=sandybridge -mno-avx -mno-aes -mno-rdrnd -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Nehalem

| **Core i3/i5/i7**``` vendor_id	: GenuineIntel cpu family	: 6 model		: 30 model name	: Intel(R) Core(TM) i5-750 CPU @ 2.66GHz  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=nehalem -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Westmere

| **Core i3/i5/i7**``` vendor_id	: GenuineIntel cpu family	: 6 model		: 37 model name	: Intel(R) Core(TM) i5-650 CPU @ 3.20GHz  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=westmere -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Intel Core

| **Intel Core**``` vendor_id       : GenuineIntel cpu family      : 6 … model		: 15 model name	: Intel(R) Core(TM)2 Duo CPU     T7500  @ 2.20GHz … model           : 15 model name      : Intel(R) Xeon(R) CPU            3040  @ 1.86GHz  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=core2 -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Older microarchitecture

| **Pentium M (Dothan)**``` vendor_id	: GenuineIntel cpu family	: 6 model		: 13 model name	: Intel(R) Pentium(R) M processor 2.13GHz  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=pentium-m -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **Pentium 4 (Prescott)**``` vendor_id	: GenuineIntel cpu family	: 15 model		: 4 model name	: Intel(R) Pentium(R) 4 CPU XXXGHz  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=nocona -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

64-bit capable models: 505, 505J, 506, 511, 516, 517, 519K, 521, 524, 531, 541, 551, 561, 571, 6xx and the 3.73(3)GHz Pentium 4 Extreme Edition.

| **All other Prescotts**``` vendor_id	: GenuineIntel cpu family	: 15 model		: 3 model name	: Intel(R) Pentium(R) 4 CPU XXXGHz  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=prescott -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

#### AMD

##### Ryzen (Zen family)

| **1000 and 2000 series**``` vendor_id	: AuthenticAMD cpu family	: 23 model		: 1 model name	: AMD Ryzen 7 1800X Eight-Core Processor … cpu family	: 23 model		: 8 model name	: AMD Ryzen 7 2700X Eight-Core Processor  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=znver1 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **3000, 4000, 5000, and EPYC 7xx2 series**``` vendor_id	: AuthenticAMD cpu family	: 23 model		: 113 model name	: AMD Ryzen 9 3900X 12-Core Processor ... cpu family      : 23 model           : 49 model name      : AMD EPYC 7542 32-Core Processor  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=znver2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **5000 and EPYC 7xx3 series**``` vendor_id	: AuthenticAMD cpu family	: 25 model		: 80 model name	: AMD Ryzen 7 PRO 5850U with Radeon Graphics  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=znver3 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **7xx0 series**``` vendor_id	: AuthenticAMD cpu family	: 25 model		: 97 model name	: AMD Ryzen 7 7800X3D 8-Core Processor  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=znver4 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **AI 300, 9000 and EPYC 9xx5 series**``` vendor_id	: AuthenticAMD cpu family	: 26 model		: 36 model name	: AMD Ryzen AI 9 365 w/ Radeon 880M  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=znver5 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### A6/A8/A9/A10/A12-8XXX/9XXX (Excavator)

| **Carrizo, Bristol Ridge, and Stoney Ridge**``` vendor_id	: AuthenticAMD cpu family	: 21 model		: 96 or 101 or 112 model name	: AMD A12-9800 RADEON R7, 12 COMPUTE CORES 4C+8G  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=bdver4 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### A4/A6/A8/A10-7XXX/8XXX (Steamroller)

| **Kaveri and Godavari**``` vendor_id	: AuthenticAMD cpu family	: 21 model		: 48 or 56 model name	: AMD A10-7850K Radeon R7, 12 Compute Cores 4C+8G  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=bdver3 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### E1/E2-XXXX, A4/A6/A8/A10-XXXX (Jaguar, Puma)

| **Kabini, Temash, Beema, Mullins, and Carrizo-L**``` vendor_id	: AuthenticAMD cpu family	: 22 model		: 0 or 48 model name	: AMD A4-5000 APU with Radeon(TM) HD Graphics  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=btver2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### A4/A6/A8/A10-4XXX/5XXX/6XXX (Piledriver)

| **Trinity and Richland**``` vendor_id	: AuthenticAMD cpu family	: 21 model		: 16 or 19 model name	: AMD A8-4500M APU with Radeon(tm) HD Graphics  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=bdver2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### FX-XXXX

| **Bulldozer and Piledriver**``` vendor_id	: AuthenticAMD cpu family	: 21 model		: 1 or 2 model name	: AMD FX(tm)-8150 Eight-Core Processor  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=bdver1 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Z-XX, C-X0, E-XX0, E1/E2-1X00, E2-2000 (Bobcat)

| **Ontario, Hondo, Desna, and Zacate**``` vendor_id	: AuthenticAMD cpu family	: 20 model		: 1 or 2 model name	: AMD E-350 Processor  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=btver1 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### A4/A6/A8-3XXX/3XXXM (12h)

| **Llano**``` vendor_id	: AuthenticAMD cpu family	: 18 model		: 1 model name	: AMD A8-3500M APU with Radeon(tm) HD Graphics  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=amdfam10 -mcx16 -mpopcnt -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Phenom/Phenom II, Athlon II, Sempron (10h)

| **Agena, Deneb, Thuban, and derivatives**``` vendor_id	: AuthenticAMD cpu family	: 16 model		: X model name	: AMD Phenom(tm) II X6 1090T Processor  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=amdfam10 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Older microarchitectures

| **E+ revisions - Athlon 64, Athlon 64 X2/FX, Sempron (0Fh)**``` vendor_id	: AuthenticAMD cpu family	: 15 model		: >= 39 model name	: AMD Athlon(tm) 64 X2 Dual Core Processor 4200+  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=opteron-sse3 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **Geode LX**``` vendor_id	: AuthenticAMD cpu family	: 5 model		: 10 model name	: Geode(TM) Integrated Processor by AMD PCS  ```  | FILE **`/etc/portage/make.conf`** ``` CHOST="i486-pc-linux-gnu" COMMON_FLAGS="-Os -pipe -march=geode -mmmx -m3dnow -fomit-frame-pointer" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **Pre-E revisions - Athlon 64, Athlon 64 FX, Sempron (0Fh)**``` vendor_id	: AuthenticAMD cpu family	: 15 model		: < 39 model name	: AMD Athlon(tm) 64 Processor 3200+  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=opteron -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

### ARM

#### Cortex-A

##### ARMv7-A/Cortex-A9 MPCore

| with optional VFPv3 FPU ``` processor       : 0 model name      : ARMv7 Processor rev 0 (v7l) BogoMIPS        : 2.00 Features        : half thumb fastmult vfp edsp vfpv3 vfpv3d16 tls  CPU implementer : 0x41 CPU architecture: 7 CPU variant     : 0x1 CPU part        : 0xc09 CPU revision    : 0   processor       : 1 model name      : ARMv7 Processor rev 0 (v7l) BogoMIPS        : 2.00 Features        : half thumb fastmult vfp edsp vfpv3 vfpv3d16 tls  CPU implementer : 0x41 CPU architecture: 7 CPU variant     : 0x1 CPU part        : 0xc09 CPU revision    : 0   Hardware        : NVIDIA Tegra SoC (Flattened Device Tree) Revision        : 0000 Serial          : 0000000000000000  ```  | FILE **`/etc/portage/make.conf`** ``` CHOST="armv7a-hardfloat-linux-gnueabi" COMMON_FLAGS="-O2 -march=cortex-a9 -mfpu=vfpv3-d16 -mfloat-abi=hard -pipe -fomit-frame-pointer" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### ARMv8-A/BCM2837

| **AArch32 with neon FPU**``` processor	: 0-3 model name	: ARMv7 Processor rev 4 (v7l) BogoMIPS	: 38.40 Features	: half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm crc32  CPU implementer	: 0x41 CPU architecture: 7 CPU variant	: 0x0 CPU part	: 0xd03 CPU revision	: 4   Hardware	: BCM2709 Revision        : 0000 Serial          : 0000000000000000  ```  | FILE **`/etc/portage/make.conf`** ``` CHOST="armv7a-hardfloat-linux-gnueabi" COMMON_FLAGS="-O2 -pipe -march=armv7-a -mfpu=neon-vfpv4 -mfloat-abi=hard" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **AArch64**``` processor	: 0-3 BogoMIPS	: 38.40 Features	: fp asimd evtstrm crc32 CPU implementer	: 0x41 CPU architecture: 8 CPU variant	: 0x0 CPU part	: 0xd03 CPU revision	: 4  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=armv8-a+crc -mtune=cortex-a53 -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

#### ARM11

##### ARMv6/ARM1176JZF-S

| ``` processor	: 0 model name	: ARMv6-compatible processor rev 7 (v6l) BogoMIPS	: 697.95 Features	: half thumb fastmult vfp edsp java tls  CPU implementer	: 0x41 CPU architecture: 7 CPU variant	: 0x0 CPU part	: 0xb76 CPU revision	: 7   Hardware	: BCM2835 Revision	: 0000 Serial		: 000000000XXXXXXX  ```  | FILE **`/etc/portage/make.conf`** ``` CHOST="armv6j-hardfloat-linux-gnueabi" COMMON_FLAGS="-O2 -pipe -mcpu=arm1176jzf-s -mfpu=vfp -mfloat-abi=hard" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### ARMv6/ARM1136JF-S

| ``` Processor       : ARMv6-compatible processor rev 5 (v6l) BogoMIPS        : 791.34 Features        : swp half thumb fastmult vfp edsp java  CPU implementer : 0x41 CPU architecture: 6TEJ CPU variant     : 0x1 CPU part        : 0xb36 CPU revision    : 5   Hardware        : IMAPX200 Revision        : 0000 Serial          : 0000000000000000  ```  | FILE **`/etc/portage/make.conf`** ``` CHOST="armv6j-hardfloat-linux-gnueabi" COMMON_FLAGS="-Os -mcpu=arm1136jf-s -mfpu=vfp -mfloat-abi=hard -pipe -fomit-frame-pointer" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

### PowerPC / PowerPC 64

#### POWER8

| ``` processor       : 0 cpu             : POWER8E (raw), altivec supported clock           : 3026.000000MHz revision        : 2.1 (pvr 004b 0201)   timebase        : 512000000 platform        : pSeries model           : IBM pSeries (emulated by qemu) machine         : CHRP IBM pSeries (emulated by qemu)  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-mcpu=power8 -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

#### Cell

| ``` processor	: 0 cpu		: Cell Broadband Engine, altivec supported clock		: 3192.000000MHz revision	: 5.1 (pvr 0070 0501)   processor	: 1 cpu		: Cell Broadband Engine, altivec supported clock		: 3192.000000MHz revision	: 5.1 (pvr 0070 0501)   timebase	: 79800000 platform	: PS3 model		: SonyPS3  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-mcpu=cell -O2 -pipe -mabi=altivec -maltivec -mno-string -mno-multiple" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

#### PPC 970 (G5)

Compatible processors are IBM PPC970, PPC970FX, PPC970MP and PPC970GX.

| ``` cpu             : PPC970, altivec supported clock           : 1800MHz revision        : 2.2 (pvr 0039 0202) bogomips        : 1127.21 machine         : PowerMac7,2 motherboard     : PowerMac7,2 MacRISC4 Power Macintosh board revision  : 00000001 detected as     : 336 (PowerMac G5) pmac flags      : 00000000 L2 cache        : 512K unified pmac-generation : NewWorld  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-mcpu=970 -O2 -maltivec -mabi=altivec -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

#### G4 (PPC 74xx)

##### PPC 7450 family

Compatible processors are Motorola/Freescale MPC7450, MPC7440, MPC7451, MPC7441, MPC7455, MPC7445, MPC7457, MPC7447, MPC7447/A, and MPC7448.

| ``` processor       : 0 cpu             : 7447A, altivec supported clock           : 1666.666000MHz revision        : 1.5 (pvr 8003 0105) bogomips        : 33.28 timebase        : 8320000 platform        : PowerMac model           : PowerBook5,9 machine         : PowerBook5,9 motherboard     : PowerBook5,9 MacRISC3 Power Macintosh  detected as     : 287 (PowerBook G4 17") pmac flags      : 00000018 L2 cache        : 512K unified pmac-generation : NewWorld  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-mcpu=7450 -O2 -maltivec -mabi=altivec -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### PPC 7400 family

Compatible processors are Motorola MPC7400 and MPC7410. Note: IBM manufactured the MPC7400 as 06K5319 and 10K8298 when Motorola was not able to fulfill Apple's demands.

| ``` processor       : 0 cpu             : 7400, altivec supported clock           : 400.000000MHz revision        : 2.9 (pvr 000c 0209) bogomips        : 49.66 timebase        : 24908583 platform        : PowerMac machine         : PowerMac3,1 motherboard     : PowerMac3,1 MacRISC Power Macintosh detected as     : 65 (PowerMac G4 AGP Graphics) pmac flags      : 00000004 L2 cache        : 1024K unified pmac-generation : NewWorld  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-mcpu=7400 -O2 -maltivec -mabi=altivec -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

#### G3 (PPC 7XX)

Compatible processors are Motorola/Freescale MPC750, MPC740, MPC755 and MPC745 as well as IBM PPC750, PPC740, PPC750L, PPC740L, PPC750CX, PPC750CXe, PPCDBK ("Gekko"), PPC750FX, PPC750GX, PPC750CXr, PPC750CL ("Broadway"), PPC750GL and PPC750FL. The BAE Systems RAD750 is a radiation hardened variant of the PPC750. The "Espresso" (following the "Gekko" and "Broadway") is also based on the PPC750.

For CPUs for embedded systems such as the Gekko (PPCDBK, used in the Nintendo GameCube) additional CFLAGS (like `-meabi`) will be required.

| ``` processor       : 0 cpu             : 740/750 clock           : 400.000000MHz revision        : 131.0 (pvr 0008 8300) bogomips        : 49.93 timebase        : 24966218 platform        : PowerMac model           : PowerBook3,1 machine         : PowerBook3,1 motherboard     : PowerBook3,1 MacRISC2 MacRISC Power Macintosh detected as     : 70 (PowerBook Pismo) pmac flags      : 0000001f L2 cache        : 1024K unified pmac-generation : NewWorld  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-mcpu=750 -Os -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

### Sparc

#### UltraSparc T2 (Niagara2)

`-mcpu=niagara2` targets the UltraSPARC T2/T2+ pipeline & VIS 2.0 instructions.

| ``` cpu             : UltraSparc T2 (Niagara2) fpu             : UltraSparc T2 integrated FPU pmu             : niagara2 prom            : OBP 4.33.6.g 2016/03/11 06:06 type            : sun4v ncpus probed    : 128 ncpus active    : 128 D$ parity tl1   : 0 I$ parity tl1   : 0 cpucaps         : flush,stbar,swap,muldiv,v9,blkinit,n2,mul32,div32,v8plus,popc,vis,vis2,ASIBlkInit  ```  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -mcpu=niagara2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

### RISC-V

| processor       : 0 hart            : 1 isa             : rv64imafdc mmu             : sv39 uarch           : sifive,u74-mc | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=rv64imafdc_zicsr_zba_zbb -mcpu=sifive-u74 -mtune=sifive-7-series -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

## See also

- [CFLAGS and CXXFLAGS (AMD64 Handbook)](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage#CFLAGS_and_CXXFLAGS)
- [CPU\_FLAGS\_\*](https://wiki.gentoo.org/wiki/CPU_FLAGS_*) — a `[USE_EXPAND](https://wiki.gentoo.org/wiki//etc/portage/make.conf#USE_EXPAND)` variable containing instruction set and other CPU-specific features.
- [GCC optimization](https://wiki.gentoo.org/wiki/GCC_optimization) — an introduction to optimizing compiled code using safe, sane [`CFLAGS` and `CXXFLAGS`](https://en.wikipedia.org/wiki/CFLAGS).
- [Gentoo documentation page on backtraces](https://wiki.gentoo.org/wiki/Project:Quality_Assurance/Backtraces)
- [RUSTFLAGS](https://wiki.gentoo.org/wiki/Rust#Environment_variables)
