<!-- source: https://wiki.gentoo.org/wiki/Drive_Migration_or_Switching_Laptops | group: Gentoo Wiki (Main) | wiki-title: Drive Migration or Switching Laptops -->
---
title: Drive Migration or Switching Laptops
url: https://wiki.gentoo.org/wiki/Drive_Migration_or_Switching_Laptops
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-01"
fingerprint: "3d951fb244df9dce"
license: CC BY-SA 4.0
---

# Drive Migration or Switching Laptops

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Moving an SSD or hard disk to a new laptop** means transplanting an
existing Gentoo installation into different hardware while preserving the
world set, toolchain, kernel, and boot configuration. The main hazards are
CPU toolchain flags, the Linux kernel and bootloader, and maybe the Gentoo
profile (rare).

## 0) Check profile compatibility

`root #````
eselect profile list
```
`root #``eselect profile show`
## 1) Architecture-dependent toolchain variables

Set these in /etc/portage/make.conf:

**`/etc/portage/make.conf`**

```
# Architecture level — override per target
ARCH_LEVEL=""
COMMON_FLAGS="-O2 -pipe -march=x86-64-v3"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
RUSTFLAGS="-C target-cpu=x86-64-v3"
LLVM_TARGETS="X86"
CPU_FLAGS_X86=""
```
## 2) Verify host baseline

`root #``emerge --info`
**C compiler (gcc)**

`root #````
gcc -march=native -Q --help=target | grep -- '-march='
```
`root #````
gcc -dumpmachine
```
**C++ compiler (g++)**

`root #````
g++ -march=native -Q --help=target | grep -- '-march='
```
`root #``g++ -dumpmachine`
**LLVM & Clang**

clang -march=native -### -x c - \< /dev/null |& grep -oE 'target-cpu" "\[a-zAZ0-9\-\_\]\*"'

`root #``clang -print-target-triple`
**Rust (rustc)**

`root #````
rustc --print target-cpus
```
`root #``rustc --print cfg`
## 3) GCC toolchain

`root #``emerge --oneshot sys-devel/binutils sys-devel/gcc sys-libs/glibc`
## 4) LLVM

`root #````
emerge --oneshot \
```
sys-libs/glibc \
 llvm-core/llvm \
 llvm-runtimes/compiler-rt \

llvm-core/clang
## 5) LLD

Confirm lld --version reports the LLVM version before proceeding:

`root #````
emerge --oneshot llvm-core/lld
```
`root #``lld --version`
## 6) Rust

`root #````
emerge --oneshot dev-lang/rust
```
`root #``emerge --oneshot dev-util/cargo-c`
Verify:

`root #````
rustc --print target-cpus
```
`root #``rustc --print cfg`
## 7) System

`root #````
emerge --oneshot @system \
```
--exclude sys-devel/binutils \

--exclude sys-devel/gcc
## 8) Network

`root #````
emerge --tree --oneshot \
```
net-misc/dhcpcd net-misc/stunnel \
 net-wireless/iw net-wireless/wireless-tools \

net-wireless/wpa_supplicant net-firewall/nftables
## 9) Dracut (--host-only vs. generic)

`root #````
emerge --oneshot sys-fs/cryptsetup app-crypt/gnupg \
  sys-fs/btrfs-progs sys-boot/plymouth
```
Host-only mode control:

\* host\_only="no" — generates a universal initramfs containing
 a broader set of drivers, filesystems and generic binaries. Essential if
 the initramfs may be moved across different machines.
\* host\_only="yes" — strips drivers not present on the current
 building machine. Avoid when building a portable image.

## 10) GRUB

`root #``grub-install --target=x86_64-efi`
### LLVM available targets

`root #``llvm-config --targets-built`
Some common:

\* X86 — Covers both 32-bit x86 and 64-bit amd64. Essential for
 standard Intel/AMD PCs.
\* AMDGPU — Recommended if you use an AMD graphics card. Used by
 Mesa, ROCm, and LLVM-based graphics drivers.
\* AArch64 — ARM 64-bit target.
\* ARM — ARM 32-bit target.
\* RISCV — RISC-V target.
\* PowerPC — PowerPC 32/64-bit target.
\* WebAssembly — WebAssembly target.

Less common embedded / experimental options include NVPTX (NVIDIA PTX/CUDA), SystemZ (IBM s390x), BPF, LoongArch, and others.
