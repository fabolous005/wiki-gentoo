<!-- source: https://wiki.gentoo.org/wiki/Safe_CFLAGS | group: Gentoo Wiki (Main) | wiki-title: Safe CFLAGS -->
---
title: Safe CFLAGS
url: https://wiki.gentoo.org/wiki/Safe_CFLAGS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-09"
fingerprint: a79fdb55e5871bfa
license: CC BY-SA 4.0
---

# Safe CFLAGS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

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

Also possible suboptimal `-march=native` detection - full `l2-cache-size` to single CPU thread on multi-core CPUs. Currently it used only for prefetching, but sometimes good choice to fallback to default `--param=l2-cache-size=512` or own calculated value - to reduce cache concurrency on high SMP load. But this is in theory and not for all tasks - do nothing if unsure.

Additional information can be found at the [GCC optimization](https://wiki.gentoo.org/wiki/GCC_optimization) page.

## Determining CPU type in order to set CFLAGS manually

These tools can report system CPU information that can then be matched a CPU from the list further down this page, to get some suggested `CFLAGS` that are "safe" for that system.

These settings should be used, especially when unsure which `CFLAGS` the processor needs.

### CPU detection with resolve-march-native

A tool exists to *automagically* determine `-march=native` resolution values: [app-misc/resolve-march-native](https://packages.gentoo.org/packages/app-misc/resolve-march-native). After installing it, issue:

`user $``resolve-march-native`
### Reporting the CPU type with /proc/cpuinfo

To identify the model of the CPU, take a look inside /proc/cpuinfo for the "cpu family" and "model" numbers like so:

`user $``grep -m1 -A3 "vendor_id" /proc/cpuinfo`
## Safe CFLAGS list

### x86/amd64

#### Generic psABI levels

If using a [distcc](https://wiki.gentoo.org/wiki/Distcc) farm with slightly different CPUs, it might make more sense to generate code that is just old enough to work for all of them, without bogging down to the *really* generic code. The psABI microarchitecture levels aims to provide just that for common eras of amd64 CPUs. See [Wikipedia:x86-64#Microarchitecture\_levels](https://en.wikipedia.org/wiki/x86-64#Microarchitecture_levels) for a description of the levels.

#### Intel

##### Alder Lake

| **Core i3/i5/i7 12th Gen** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=alderlake -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Skylake, Kaby Lake, Kaby Lake R, Coffee Lake, Comet Lake

| **Core i3/i5/i7 and Xeon E3/E5 \*V5** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=skylake -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Broadwell

| **Core i3/i5/i7 and Xeon E3/E5 \*V4** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=broadwell -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Haswell

| **Core i3/i5/i7 and Xeon E3/E5/E7 \*V3** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=haswell -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Ivy Bridge

| **Core i3/i5/i7 and Xeon E3/E5/E7 \*V2** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=ivybridge -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **Pentium** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=ivybridge -mno-avx -mno-aes -mno-rdrnd -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Sandy Bridge

| **Core i3/i5/i7 and Xeon E3/E5/E7** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=sandybridge -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **Pentium** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=sandybridge -mno-avx -mno-aes -mno-rdrnd -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Nehalem

| **Core i3/i5/i7** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=nehalem -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Westmere

| **Core i3/i5/i7** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=westmere -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Intel Core

| **Intel Core** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=core2 -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Older microarchitecture

| **Pentium M (Dothan)** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=pentium-m -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **Pentium 4 (Prescott)** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=nocona -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

64-bit capable models: 505, 505J, 506, 511, 516, 517, 519K, 521, 524, 531, 541, 551, 561, 571, 6xx and the 3.73(3)GHz Pentium 4 Extreme Edition.

| **All other Prescotts** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=prescott -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

#### AMD

##### Ryzen (Zen family)

| **1000 and 2000 series** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=znver1 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **3000, 4000, 5000, and EPYC 7xx2 series** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=znver2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **5000 and EPYC 7xx3 series** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=znver3 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **7xx0 series** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=znver4 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **AI 300, 9000 and EPYC 9xx5 series** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=znver5 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### A6/A8/A9/A10/A12-8XXX/9XXX (Excavator)

| **Carrizo, Bristol Ridge, and Stoney Ridge** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=bdver4 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### A4/A6/A8/A10-7XXX/8XXX (Steamroller)

| **Kaveri and Godavari** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=bdver3 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### E1/E2-XXXX, A4/A6/A8/A10-XXXX (Jaguar, Puma)

| **Kabini, Temash, Beema, Mullins, and Carrizo-L** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=btver2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### A4/A6/A8/A10-4XXX/5XXX/6XXX (Piledriver)

| **Trinity and Richland** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=bdver2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### FX-XXXX

| **Bulldozer and Piledriver** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=bdver1 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Z-XX, C-X0, E-XX0, E1/E2-1X00, E2-2000 (Bobcat)

| **Ontario, Hondo, Desna, and Zacate** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=btver1 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### A4/A6/A8-3XXX/3XXXM (12h)

| **Llano** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=amdfam10 -mcx16 -mpopcnt -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Phenom/Phenom II, Athlon II, Sempron (10h)

| **Agena, Deneb, Thuban, and derivatives** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=amdfam10 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### Older microarchitectures

| **E+ revisions - Athlon 64, Athlon 64 X2/FX, Sempron (0Fh)** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=opteron-sse3 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **Geode LX** | FILE **`/etc/portage/make.conf`** ``` CHOST="i486-pc-linux-gnu" COMMON_FLAGS="-Os -pipe -march=geode -mmmx -m3dnow -fomit-frame-pointer" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **Pre-E revisions - Athlon 64, Athlon 64 FX, Sempron (0Fh)** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -march=opteron -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

### ARM

#### Cortex-A

##### ARMv7-A/Cortex-A9 MPCore

| with optional VFPv3 FPU  | FILE **`/etc/portage/make.conf`** ``` CHOST="armv7a-hardfloat-linux-gnueabi" COMMON_FLAGS="-O2 -march=cortex-a9 -mfpu=vfpv3-d16 -mfloat-abi=hard -pipe -fomit-frame-pointer" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### ARMv8-A/BCM2837

| **AArch32 with neon FPU** | FILE **`/etc/portage/make.conf`** ``` CHOST="armv7a-hardfloat-linux-gnueabi" COMMON_FLAGS="-O2 -pipe -march=armv7-a -mfpu=neon-vfpv4 -mfloat-abi=hard" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

| **AArch64** | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=armv8-a+crc -mtune=cortex-a53 -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

#### ARM11

##### ARMv6/ARM1176JZF-S

|  | FILE **`/etc/portage/make.conf`** ``` CHOST="armv6j-hardfloat-linux-gnueabi" COMMON_FLAGS="-O2 -pipe -mcpu=arm1176jzf-s -mfpu=vfp -mfloat-abi=hard" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### ARMv6/ARM1136JF-S

|  | FILE **`/etc/portage/make.conf`** ``` CHOST="armv6j-hardfloat-linux-gnueabi" COMMON_FLAGS="-Os -mcpu=arm1136jf-s -mfpu=vfp -mfloat-abi=hard -pipe -fomit-frame-pointer" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

### PowerPC / PowerPC 64

#### POWER8

|  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-mcpu=power8 -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

#### Cell

|  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-mcpu=cell -O2 -pipe -mabi=altivec -maltivec -mno-string -mno-multiple" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

#### PPC 970 (G5)

Compatible processors are IBM PPC970, PPC970FX, PPC970MP and PPC970GX.

|  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-mcpu=970 -O2 -maltivec -mabi=altivec -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

#### G4 (PPC 74xx)

##### PPC 7450 family

Compatible processors are Motorola/Freescale MPC7450, MPC7440, MPC7451, MPC7441, MPC7455, MPC7445, MPC7457, MPC7447, MPC7447/A, and MPC7448.

|  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-mcpu=7450 -O2 -maltivec -mabi=altivec -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

##### PPC 7400 family

Compatible processors are Motorola MPC7400 and MPC7410. Note: IBM manufactured the MPC7400 as 06K5319 and 10K8298 when Motorola was not able to fulfill Apple's demands.

|  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-mcpu=7400 -O2 -maltivec -mabi=altivec -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

#### G3 (PPC 7XX)

Compatible processors are Motorola/Freescale MPC750, MPC740, MPC755 and MPC745 as well as IBM PPC750, PPC740, PPC750L, PPC740L, PPC750CX, PPC750CXe, PPCDBK ("Gekko"), PPC750FX, PPC750GX, PPC750CXr, PPC750CL ("Broadway"), PPC750GL and PPC750FL. The BAE Systems RAD750 is a radiation hardened variant of the PPC750. The "Espresso" (following the "Gekko" and "Broadway") is also based on the PPC750.

For CPUs for embedded systems such as the Gekko (PPCDBK, used in the Nintendo GameCube) additional CFLAGS (like `-meabi`) will be required.

|  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-mcpu=750 -Os -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

### Sparc

#### UltraSparc T2 (Niagara2)

`-mcpu=niagara2` targets the UltraSPARC T2/T2+ pipeline & VIS 2.0 instructions.

|  | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-O2 -mcpu=niagara2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

### RISC-V

| processor       : 0 hart            : 1 isa             : rv64imafdc mmu             : sv39 uarch           : sifive,u74-mc | FILE **`/etc/portage/make.conf`** ``` COMMON_FLAGS="-march=rv64imafdc_zicsr_zba_zbb -mcpu=sifive-u74 -mtune=sifive-7-series -O2 -pipe" CFLAGS="${COMMON_FLAGS}" CXXFLAGS="${COMMON_FLAGS}"  ```  | 

## See also

- [CFLAGS and CXXFLAGS (AMD64 Handbook)](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage#CFLAGS_and_CXXFLAGS)
- [CPU\_FLAGS\_\*](https://wiki.gentoo.org/wiki/CPU_FLAGS_*) — a `[USE_EXPAND](https://wiki.gentoo.org/wiki//etc/portage/make.conf#USE_EXPAND)` variable containing instruction set and other CPU-specific features.
- [GCC optimization](https://wiki.gentoo.org/wiki/GCC_optimization) — an introduction to optimizing compiled code using safe, sane [`CFLAGS` and `CXXFLAGS`](https://en.wikipedia.org/wiki/CFLAGS).
- [Gentoo documentation page on backtraces](https://wiki.gentoo.org/wiki/Project:Quality_Assurance/Backtraces)
- [RUSTFLAGS](https://wiki.gentoo.org/wiki/Rust#Environment_variables)
