<!-- source: https://wiki.gentoo.org/wiki/Rust_x86_without_SSE2 | group: Gentoo Wiki (Main) | wiki-title: Rust x86 without SSE2 -->
---
title: Rust x86 without SSE2
url: https://wiki.gentoo.org/wiki/Rust_x86_without_SSE2
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-12-23"
fingerprint: bf463d32080fc2aa
license: CC BY-SA 4.0
---

# Rust x86 without SSE2

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This will allow you to use Rust on x86 i686 machines without SSE2. Rust calls this target i586 but it will work on a CHOST using i686.

## 32bit chroot

Create a 32bit chroot on a machine with sse2 using the correct tarball for your needs (I used stage3-i686-openrc).

You will need to install the following in your chroot:

`root #``emerge --ask sys-devel/clang dev-vcs/git dev-util/cmake eselect-rust`
## Rust

### Repo

The git repo is used because the tag tarballs lack the needed submodules.

`amd64 #``cd rust``amd64 #``git checkout 1.64.0``amd64 #``git submodule update --init --recursive`
### Build

Configure:

**`config.toml`**

Run the mono x.py build script:

`amd64 #``PKG_CONFIG_ALLOW_CROSS=1 ./x.py dist --build i686-unknown-linux-gnu --host i586-unknown-linux-gnu --target i586-unknown-linux-gnu -j 4`
All going well, when it terminates, there should be a fresh rustc binary:

`amd64 #``file build/i586-unknown-linux-gnu/stage2/bin/rustc`
build/i586-unknown-linux-gnu/stage2/bin/rustc: ELF 32-bit LSB pie executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 3.2.0, not stripped

### On the non sse2 machine

#### Install built Rust temporarily

Copy the files over to the destination box (i686 in this case):

- Copy over:
  - build/dist/cargo-1.65.0-i586-unknown-linux-gnu.tar.xz
  - build/dist/rust-std-1.65.0-dev-i586-unknown-linux-gnu.tar.xz
  - build/dist/rustc-1.65.0-i686-unknown-linux-gnu.tar.xz
- For each: on the other side, untar it to a temporary location, and run inside ./install.sh --prefix=/usr/local/lib/rust/1.65.0/

#### Ebuild editing

Copy the Rust ebuilds to a temporary location:

`x86 #````
mkdir -p /tmp/dev-lang/rust && cd /tmp/dev-lang/rust
```
`x86 #``cp -rv /var/db/repos/gentoo/dev-lang/rust/* .`
Modify the relevant Rust ebuild corresponding to the current latest Rust release (must match the version built from git):

1. Add 'return' below  \`python-any-r1\` in `pkg_setup`
2. Replace the rustc call in `src_configure` by just setting rust\_stage0\_root=/usr/local/lib/rust/1.65.0/
3. Comment out the `[[ -d ${rust_stage0_root} ]]` check
4. Remove the IUSE line for the sse2 check
5. Find the are with the three `-fcf-protection=none` and add `-lz`
6. \`

Then try to emerge using it:

`x86 #``PKG_CONFIG_ALLOW_CROSS=1 RUSTFLAGS="-C opt-level=0" USE=system-bootstrap ebuild rust-1.65.0.ebuild clean merge`
Once done, some manual fiddling may be needed to get `LDPATH` right initially and make eselect rust happy.

Run eselect rust update and placate any complaints it has if there's stale stuff left over from the /usr/local bits. Try env-update && . /etc/profile.

Remember to clean up /usr/local/{bin,lib/\*}.

Now do it all again using a more vanilla ebuild:

`x86 #``rm -r /tmp/dev-lang/rust``x86 #``mkdir -p /tmp/dev-lang/rust && cd /tmp/dev-lang/rust``x86 #``cp -rv /var/db/repos/gentoo/dev-lang/rust/* .`
Remove the IUSE check for SSE2 on x86.

`x86 #``USE=system-bootstrap ebuild rust-1.65.0.ebuild clean merge`
With that you now have Rust working without the need of SSE2.
