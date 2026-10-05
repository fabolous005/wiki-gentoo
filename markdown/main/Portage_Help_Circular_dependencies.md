<!-- source: https://wiki.gentoo.org/wiki/Portage/Help/Circular_dependencies | group: Gentoo Wiki (Main) | wiki-title: Portage/Help/Circular dependencies -->
---
title: Portage/Help/Circular dependencies
url: https://wiki.gentoo.org/wiki/Portage/Help/Circular_dependencies
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-13"
fingerprint: f299377ed5ef938e
license: CC BY-SA 4.0
---

# Portage/Help/Circular dependencies

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A circular dependency is where a cycle of packages depend on each other, e.g. `libxcrypt -> perl -> libxcrypt`.

## Examples

### harfbuzz and freetype

#### Background

On desktop profiles, `USE=truetype` is enabled which is needed for font rendering. But plenty of packages will require `USE=harfbuzz` to be enabled on [media-libs/freetype](https://packages.gentoo.org/packages/media-libs/freetype) for rendering.

This causes a circular dependency between [media-libs/harfbuzz](https://packages.gentoo.org/packages/media-libs/harfbuzz) and [media-libs/freetype](https://packages.gentoo.org/packages/media-libs/freetype). It appears there's
discussion ongoing [upstream](https://github.com/harfbuzz/harfbuzz/issues/2524) as to how to resolve the issue (perhaps by embedding code from one into the other).

The cycle looks like this:

1. `media-libs/harfbuzz[truetype]` depends on `media-libs/freetype`
2. `media-libs/freetype[harfbuzz]` depends on `media-libs/harfbuzz[truetype]`


Given so many packages need `media-libs/freetype[harfbuzz]`, it must be turned on in almost all situations - but it's frustrating as it's not immediately obvious from the two ebuilds themselves.

#### Portage output

`root #``emerge -p -uvDU @world`
\[...\]
 \* Error: circular dependencies:
(media-libs/harfbuzz-2.8.1:0/0.9.18::gentoo, ebuild scheduled for merge) depends on
 (media-libs/freetype-2.10.4:2/2::gentoo, ebuild scheduled for merge) (buildtime\_slot\_op)
  (media-libs/harfbuzz-2.8.1:0/0.9.18::gentoo, ebuild scheduled for merge) (buildtime)
 \* Note that circular dependencies can often be avoided by temporarily
 \* disabling USE flags that trigger optional dependencies.

#### Solution

Try this first:

`root #``USE="-harfbuzz" emerge --oneshot media-libs/freetype`
If it fails, read on.

##### Sledgehammer

The following works although is a little bit convoluted:

`root #````
USE=-* emerge --verbose --oneshot --nodeps media-libs/harfbuzz
```
`root #````
USE=-* emerge --verbose --oneshot --nodeps media-libs/freetype
```
`root #````
USE=-truetype emerge --verbose --oneshot --nodeps media-libs/harfbuzz
```
`root #````
USE="-* truetype harfbuzz" emerge --verbose --oneshot media-libs/freetype
```
`root #``emerge --verbose --oneshot media-libs/freetype`
Or for multilib, something like:

`root #````
USE="abi_x86_32" emerge -v1 sys-libs/zlib
```
`root #````
USE="-* abi_x86_32" emerge --verbose --oneshot --nodeps media-libs/harfbuzz
```
`root #````
USE="-* abi_x86_32" emerge --verbose --oneshot --nodeps media-libs/freetype
```
`root #````
USE="-* truetype harfbuzz abi_x86_32" emerge --verbose --oneshot media-libs/freetype
```
`root #``emerge --verbose --oneshot media-libs/freetype`
### Python and libcrypt

#### Background

See the [libcrypt migration](https://wiki.gentoo.org/wiki/Project:Toolchain/libcrypt_implementation#Circular_dependencies) article. Occurs when upgrading from stable to \~arch.

This will affect those who do not yet have e.g. Python 3.10 installed *or* are upgrading an older installation with no recent Pythons installed.

#### Portage output

`root #``emerge -p -uvDU @world````
[...]
* Error: circular dependencies:
(dev-lang/python-3.10.0_beta4:3.10/3.10::gentoo, ebuild scheduled for merge) depends on
 (virtual/libcrypt-2:0/2::gentoo, ebuild scheduled for merge) (buildtime_slot_op)
  (sys-libs/libxcrypt-4.4.23-r1:0/1::gentoo, ebuild scheduled for merge) (runtime)
   (sys-libs/glibc-2.33-r2:2.2/2.2::gentoo, ebuild scheduled for merge) (buildtime)
    (dev-lang/python-3.10.0_beta4:3.10/3.10::gentoo, ebuild scheduled for merge) (buildtime)
```
#### Solution

##### Error mentions one version not yet installed

If the error mentions Python versions **not yet installed** like Python 3.10 but you have some valid Pythons installed (like 3.9), please try emerge -a -uvDU @world --exclude python:3.10.

##### Error mentions default version not yet installed (old install)

If the error mentions Python versions that **are** installed that are the current default (Python 3.9 at time of writing in November 2021):

1. Temporarily mask `virtual/libcrypt:0/2` in /etc/portage/package.mask.
2. Temporarily unmask the 'crypt' use flag for `>=sys-libs/glibc-2.33-r2` (in /etc/portage/profile/package.use.mask, add: `>=sys-libs/glibc-2.33-r2 -crypt`) (do **not** forget the *profile* part of the path, create the directory if needed)
3. Temporarily unmask `virtual/libcrypt:0/1` in /etc/portage/package.unmask.
4. Attempt a world upgrade.
5. Undo Step 1, Step 2, and Step 3.
6. Attempt another world upgrade.

This process is temporarily delaying the libxcrypt migration to get a new version of each Python needed installed before it's queued to be rebuilt against the new libcrypt.

### Python and bluez

#### Background

Gentoo's desktop profiles enable `USE=bluetooth` which is handy for plenty of users. While it's useful for *most* packages, it's debatable whether or not it should apply to [dev-lang/python](https://packages.gentoo.org/packages/dev-lang/python) as it seems not much uses its bluetooth support.

See [bug #764623](https://bugs.gentoo.org/show_bug.cgi?id=764623) for more discussion.

The cycle looks like this:

1. `dev-lang/python[bluetooth]` depends on `net-wireless/bluez`
2. `net-wireless/bluez` unconditionally needs `dev-lang/python` to build!

#### Portage output

`root #``emerge -p -uvDU @world````
[...]
(sys-apps/dbus-1.12.20-r1:0/0::gentoo, ebuild scheduled for merge) depends on
 (x11-libs/libX11-1.7.0:0/0::gentoo, ebuild scheduled for merge) (buildtime)
  (x11-libs/libxcb-1.14:0/1.12::gentoo, ebuild scheduled for merge) (buildtime)
   (dev-lang/python-3.9.0-r1:3.9/3.9::gentoo, ebuild scheduled for merge) (buildtime)
    (net-wireless/bluez-5.55:0/3::gentoo, ebuild scheduled for merge) (buildtime)
     (sys-apps/dbus-1.12.20-r1:0/0::gentoo, ebuild scheduled for merge) (buildtime_slot_op)
It might be possible to break this cycle
by applying any of the following changes:
- net-wireless/bluez-5.55 (Change USE: -systemd)
- sys-apps/dbus-1.12.20-r1 (Change USE: -X)
- dev-lang/python-3.9.0-r1 (Change USE: -bluetooth)
```
#### Solution

In some cases, as a result of this observation, it's sufficient to run (adjust :3.9 to whichever new slot of Python is being pulled in):

`root #``emerge -p -uvDU @world --exclude dev-lang/python:3.9` Where this isn't enough, it's easiest to temporarily disable bluetooth support in [dev-lang/python](https://packages.gentoo.org/packages/dev-lang/python):

**`/etc/portage/package.use`**

```
# Avoid a circular dependency between python and bluez
dev-lang/python -bluetooth
```
Then complete a world update, then delete the entry from the file.

### Curl and nghttp2/3

#### Background

TODO

#### Portage output

`root #``emerge -p -uvDU @world`
\[...\]
 \* Error: circular dependencies:
(net-misc/curl-8.11.1-r2:0/0::gentoo, ebuild scheduled for merge) depends on
 (net-libs/nghttp3-1.7.0:0/0::gentoo, ebuild scheduled for merge) (buildtime)
  (dev-build/cmake-3.31.5:0/0::gentoo, ebuild scheduled for merge) (buildtime)
   (net-misc/curl-8.11.1-r2:0/0::gentoo, ebuild scheduled for merge) (buildtime)

#### Solution

To fix this issue, emerge curl with temporary use flags:

`root #``USE="-http2 -http3 -quic -curl_quic_openssl" emerge -va1 net-misc/curl``root #``emerge -va1 net-misc/curl`
### Mono and libgdiplus

#### Background

Almost all use cases of [dev-lang/mono](https://packages.gentoo.org/packages/dev-lang/mono) will require `USE=-minimal` which pulls in [dev-dotnet/libgdiplus](https://packages.gentoo.org/packages/dev-dotnet/libgdiplus) a library of the Mono project.
However, in order to get `dev-dotnet/libgdiplus`, we need `dev-lang/mono` as a build time dependency and herein lies the loop.

#### Portage output

`root #``emerge -p -uvDU @world`
\[...\]
 \* Error: circular dependencies:
(dev-dotnet/libgdiplus-6.2:0/0::gentoo, ebuild scheduled for merge) depends on
 (dev-lang/mono-6.14.1-r1:0/0::gentoo, ebuild scheduled for merge) (buildtime)
  (dev-dotnet/libgdiplus-6.2:0/0::gentoo, ebuild scheduled for merge) (buildtime)

#### Solution

To fix this issue, first emerge mono with the minimal flag, as to not require libgdiplus.

`root #``USE="minimal" emerge --oneshot --verbose dev-lang/mono`
Then, we can emerge [dev-lang/mono](https://packages.gentoo.org/packages/dev-lang/mono) normally, as one version will already be present to fulfill [dev-dotnet/libgdiplus](https://packages.gentoo.org/packages/dev-dotnet/libgdiplus) build time dependency requirement.

`root #``emerge --oneshot --verbose dev-lang/mono`
## TODO

\[07:32:55\]  \<@juippis> sam\_: are you collecting known circular dep cases?
\[07:33:00\]  \<@juippis> sam\_: there was one with mit-krb5
\[07:35:33\]  \<@juippis> I \_think\_ mit-krb5\[openldap\] -> openldap\[kerberos\] -> mit-krb5. Also there was one with qemu where it depends on itself

- [dev-ml/findlib](https://packages.gentoo.org/packages/dev-ml/findlib) and [dev-ml/labltk](https://packages.gentoo.org/packages/dev-ml/labltk) when findlib has USE=tk, [bug #831371](https://bugs.gentoo.org/show_bug.cgi?id=831371), [Onkobu](https://wiki.gentoo.org/wiki/User:Onkobu) ([talk](https://wiki.gentoo.org/wiki/User_talk:Onkobu)) 20:18, 17 January 2022 (UTC)
