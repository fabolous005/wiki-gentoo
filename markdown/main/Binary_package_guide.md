<!-- source: https://wiki.gentoo.org/wiki/Binary_package_guide | group: Gentoo Wiki (Main) | wiki-title: Binary package guide -->
---
title: Binary package guide
url: https://wiki.gentoo.org/wiki/Binary_package_guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-03"
tags: ['v3.0.31']
fingerprint: "21211e3fa1575f82"
license: CC BY-SA 4.0
---

# Binary package guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Gentoo binhost**

This guide covers in-depth **binary package** creation, distribution, use, and maintenance, and a few more advanced topics near the end. This page focuses on self-built binary packages rather than the [official Gentoo binhost](https://wiki.gentoo.org/wiki/Gentoo_Binary_Host_Quickstart).

[Portage](https://wiki.gentoo.org/wiki/Portage) fully supports binary packages in addition to its well-known support for source-based [ebuilds](https://wiki.gentoo.org/wiki/Ebuild). Portage may be used to create binary packages, to install them, or to update packages already on the system from binary packages. Portage has support for fetching binary packages from *binary package hosts*.

## Why use binary packages on Gentoo?

Some reasons for using binary packages on Gentoo are:

- *Save time when keeping similar systems updated*. Building from source can take more time than installing from binaries. When maintaining several similar systems, possibly some of them with older hardware, it can be easier if only one system has to compile everything from source and the other systems use the resultant binary packages.
- *Do safe updates*. For mission-critical systems in production it is important to stay *usable* as much as possible. This can be done by a staging server that performs all updates first to itself. Once the staging server is in a good state the updates can then be applied to the critical systems via binary packages. A variant of this approach is to do the updates in a chroot on the same system and use the binaries created there to update the real system.
- *As a backup*. Often, binary packages are the only way of recovering a broken system (i.e. broken compiler). Having pre-compiled binaries around, either on a binary package server or locally, can be of great help in case of a broken toolchain.
- It can aid in *updating very old systems*. It is usually helpful to install binary packages on old systems because they do not require build-time dependencies to be installed/updated. Binaries packages also avoid failures in build processes.

Two binary package formats for use in Gentoo exist, XPAK and GPKG. Starting with [v3.0.31](https://gitweb.gentoo.org/proj/portage.git/tag/?h=portage-3.0.31), Portage supports the new binary package format GPKG. The GPKG format solves issues with the legacy XPAK format and offers the benefit of [new features](https://wiki.gentoo.org/wiki/Binary_package_guide#Binary_package_OpenPGP_signing), however it is *not* backward compatible with the legacy XPAK format.

System administrators using older versions of Portage \<=v3.0.30 (Systems older then 2021) should continue to use the legacy XPAK format, which is Portage's default setting on those versions.

Motivation for the newer GPKG format design can be found in [GLEP 78: Gentoo binary package container format](https://www.gentoo.org/glep/glep-0078.html#motivation). Bugs [bug #672672](https://bugs.gentoo.org/show_bug.cgi?id=672672) and [bug #820578](https://bugs.gentoo.org/show_bug.cgi?id=820578) also provide helpful details.

To instruct Portage to use the GPKG format, change the `BINPKG_FORMAT` value in /etc/portage/make.conf. Note that current versions of Portage use gpkg by default.

**`/etc/portage/make.conf`**

**Specify GPKG binary package format**

```
BINPKG_FORMAT="gpkg"
```
This guide mostly applies to both formats; where this is not the case it will be noted. See the [Understanding the binary package format](https://wiki.gentoo.org/wiki/Binary_package_guide#Understanding_the_binary_package_format) section for technical details on the binary package formats themselves.

### General prerequisites

For binary packages made on one system to be usable on other systems they must fulfill some requirements:

- The builder and client architecture and `[CHOST](https://wiki.gentoo.org/wiki/CHOST)` must match.
- The `CFLAGS` and `CXXFLAGS` variables used to build the binary packages must be compatible with all clients.
- USE flags for processor specific instruction set features (like MMX, SSE, etc.) must be carefully selected; all clients must support them.

### Handling \*FLAGS in detail

The [app-misc/resolve-march-native](https://packages.gentoo.org/packages/app-misc/resolve-march-native) utility can be used to find a subset of `CFLAGS` that is supported by both the server and client(s). For example, the host might return:

`user $``resolve-march-native`
-march=skylake -mabm -mrtm --param=l1-cache-line-size=64 --param=l1-cache-size=32 --param=l2-cache-size=12288

While the client might return:

`user $``resolve-march-native`
-march=ivybridge -mno-rdrnd --param=l1-cache-line-size=64 --param=l1-cache-size=32 --param=l2-cache-size=3072

In this example `CFLAGS` could be set to `-march=ivybridge -mno-rdrnd` since `-march=ivybridge` is a full subset of `-march=skylake`. `-mabm` and `-mrtm` are not included as these are not supported by the client. However, `-mno-rdrnd` is included as the client does not support `-mrdrnd`. To find which `-march`'s are subsets of others, check the [gcc manual](http://gcc.gnu.org/onlinedocs/gcc/x86-Options.html), if there is no suitable subset set e.g. `-march=x86-64`.

Optionally, it is also possible to set `-mtune=` or *some-arch*`-mtune=native` to tell gcc to tune code to a specific arch. In contrast to `-march`, the `-mtune` argument does not prevent code from being executed on other processors. For example, to compile code which is compatible with *ivybridge* and up but is tuned to run best on *skylake* set `CFLAGS` to `-march=ivybridge -mtune=skylake`. When `-mtune` is not set it defaults to whatever `-march` is set to.

When changing `-march` to a lower subset for using binary packages on a client, a full recompilation is required to make sure that all binaries are compatible with the client's processor, to save time packages that are not compiled with e.g. gcc/clang can be excluded:

`user $``emerge -e @world --exclude="acct-group/* acct-user/* virtual/* app-eselect/* sys-kernel/* sys-firmware/* dev-python/* dev-java/* dev-ruby/* dev-perl/* dev-lua/* dev-php/* dev-tex/* dev-texlive/* x11-themes/* */*-bin"`
Similarly, [app-portage/cpuid2cpuflags](https://packages.gentoo.org/packages/app-portage/cpuid2cpuflags) can be used to find a suitable subset of processor specific instruction set USE flags. For example, the host might return:

`user $``cpuid2cpuflags`
CPU\_FLAGS\_X86: aes avx avx2 f16c fma3 mmx mmxext pclmul popcnt rdrand sse sse2 sse3 sse4\_1 sse4\_2 ssse3

While the client might return:

`user $``cpuid2cpuflags`
CPU\_FLAGS\_X86: avx f16c mmx mmxext pclmul popcnt sse sse2 sse3 sse4\_1 sse4\_2 ssse3

In this example `CPU_FLAGS_X86` can be set to `avx f16c mmx mmxext pclmul popcnt sse sse2 sse3 sse4_1 sse4_2 ssse3` in /etc/portage/make.conf because these flags are supported by both the client and the host

Next to these, Portage can check if the binary package is built using the same USE flags as expected on the client. Unless using `--usepkgonly` (`-K`) or `--getbinpkgonly` (`-G`), if a package is built with a different USE flag combination, Portage will either ignore the binary package (and use source-based build) or fail, depending on the options passed to the [emerge](https://wiki.gentoo.org/wiki/Emerge) command upon invocation (see [Installing binary packages](https://wiki.gentoo.org/wiki/Binary_package_guide#Installing_binary_packages)).

On clients, a few configuration changes are needed in order for the binary packages to be used.

There are a few options that can be passed on to the emerge command that inform Portage about using binary packages:

| Option | Description | 
|---|---|
| `--usepkg` (`-k`) | Tries to use the binary package(s) in the locally available packages directory. Useful when using [NFS](https://wiki.gentoo.org/wiki/NFS) or [SSHFS](https://wiki.gentoo.org/wiki/SSHFS) mounted binary package hosts. If the binary packages are not found, a regular (source-based) installation will be performed. | 
| `--usepkgonly` (`-K`) | Similar to `--usepkg` (`-k`) but fail if the binary package cannot be found. This option is useful if *only pre-built* binary packages are to be used. | 
| `--getbinpkg` (`-g`) | Download the binary package(s) from a remote binary package host. If the binary packages are not found, a regular (source-based) installation will be performed. | 
| `--getbinpkgonly` (`-G`) | Similar to `--getbinpkg` (`-g`) but will fail if the binary package(s) cannot be downloaded. This option is useful if *only pre-built* binary packages are to be used. | 

In order to automatically use binary package installations, the appropriate option can be added to the `EMERGE_DEFAULT_OPTS` variable:

**`/etc/portage/make.conf`**

**Automatically fetch binary packages and build from source if not available**

```
EMERGE_DEFAULT_OPTS="${EMERGE_DEFAULT_OPTS} --getbinpkg"
```
There is a Portage feature that forces emerge to always try to fetch files from the binary package host:

**`/etc/portage/make.conf`**

**Enabling getbinpkg in the`FEATURES` variable**

```
FEATURES="getbinpkg"
```
Portage will try to verify the binary package's signature whenever possible, but users must first set up trusted local keys. 
[app-portage/getuto](https://packages.gentoo.org/packages/app-portage/getuto) can be used to set up a local trust anchor and update the keys in /etc/portage/gnupg. Portage calls getuto automatically with *--getbinpkg* or *--getbinpkgonly*.

This configures portage such that it trusts the [Gentoo Release Engineering keys](https://www.gentoo.org/downloads/signatures/) 
as also contained in the package [sec-keys/openpgp-keys-gentoo-release](https://packages.gentoo.org/packages/sec-keys/openpgp-keys-gentoo-release) for binary installation purposes.

Changes to the configuration can be done as root using `gpg` with the parameter `--homedir=/etc/portage/gnupg`. This way allows importing additional signing keys (e.g. for non-standard installation sources) and declare them as trusted.

To add a custom signing key:

1. Generate (or use an existing) key with signing abilities, and export the public key to a file. **This is distinct from the key and keyring that getuto will generate**. The signing key should reside in /root/.gnupg by default (`BINPKG_GPG_SIGNING_GPG_HOME` controls this).
2. Set `BINPKG_GPG_SIGNING_KEY` to be the fingerprint for the public key of the signing key just-created.
3. Run `getuto` if it has never run (for the verification keyring in /etc/portage/gnupg: `root #``getuto`
4. Use `gpg --homedir=/etc/portage/gnupg --import public.key` to import the public key of the signing key in Portage's verification keyring.
5. Trust and sign the signing key created in Step 1 using the verification key created by `getuto`. In order to do this, first get the password to unlock the key at /etc/portage/gnupg/pass, then use: `root #``gpg --homedir=/etc/portage/gnupg --edit-key YOURKEYID` Type `sign`, `yes`, paste (or type) the password. The key is now signed. To trust it, type `trust`, then `4` to trust it fully. Finally, type `save`.
6. Update the trustdb so that GPG considers the key valid: `root #``gpg --homedir=/etc/portage/gnupg --check-trustdb`

If you hit any issues, check if a pre-existing /etc/portage/gnupg existed. If it did, move it away and then repeat the above steps.

Congratulations, Portage now has a working keyring!

By default, Portage will require valid OpenPGP signatures for any remote binhost [since 2026-05](https://www.gentoo.org/support/news-items/2026-05-03-portage-binpkg-changes.html).

If the user wishes to force signature verification even for binpkgs created locally, the `binpkg-request-signature` feature needs to be enabled. This feature assumes that all packages should be signed and rejects any unsigned package.

**`/etc/portage/make.conf`**

**Enabling Portage's binpkg-request-signature feature**

```
# Require that all binpkgs be signed and reject them if they are not (or have an invalid sig)
FEATURES="binpkg-request-signature"
```
For remote binhosts, this can be configured via *verify-signature* in /etc/portage/binrepos.conf.

When using a binary package host, clients need to have the `sync-uri` variable in /etc/portage/binrepos.conf (preferred) **or** the `PORTAGE_BINHOST` variable set in /etc/portage/make.conf. Without this configuration, the client will not know where the binary packages are stored which results in Portage being unable to retrieve them.

**`/etc/portage/binrepos.conf`**

**Setting binhost sync-uri**

```
[mybinhost]
sync-uri = https://example.com/binhost
priority = 10
 
# Introduced in portage-3.0.74 for per-repo verification choices
#verify-signature = true
# Defaults to /var/cache/binhost/$NAME with >=portage-3.0.77
#location = /var/cache/binhost/mybinhost
```
For each binhost, a name can be configured in the brackets. `sync-uri` must point to the directory in which the Packages file resides. Optionally, `priority` can be set. When a package exists in multiple binary package repositories, the package is pulled from the binary package host with the highest priority. This way, a preferred binary package host can be set up.

Many Gentoo stages already come with a preinstalled /etc/portage/binrepos.conf file, which points to the corresponding binary packages generated during the stage builds.

Passing the `--rebuilt-binaries` option to emerge will reinstall every binary that has been rebuilt since the package was installed. This is useful in case rebuilding tools like revdep-rebuild are run on the binary package server.

A related option is `--rebuilt-binaries-timestamp`. It causes emerge not to consider binary packages for a re-install if those binary packages have been built before the given time stamp. This is useful to avoid re-installing all packages, if the binary package server had to be rebuild from scratch but `--rebuilt-binaries` is used otherwise.

Next to the `getbinpkg` feature, Portage also listens to the `binpkg-logs` feature. It controls if log files for successful binary package installations should be kept. It is only relevant if the `PORT_LOGDIR` variable has been set and is enabled by default.

Similar to excluding binary packages for a certain set of packages or categories, clients can be configured to exclude binary package installations for a certain set of packages or categories.

To accomplish this, use the `--usepkg-exclude` option:

`root #``emerge -uDNg @world --usepkg-exclude "sys-kernel/gentoo-sources virtual/* sys-kernel/gentoo-kernel"`
To enable such additional settings for each emerge command, add the options to the `EMERGE_DEFAULT_OPTS` variable in the make.conf file:

**`/etc/portage/make.conf`**

**Enabling emerge settings on every invocation**

### Updating packages on the binary package host

There are three main methods for creating binary packages:

1. After a regular installation, using the quickpkg application.
2. Explicitly during an emerge operation by using the `--buildpkg` (`-b`) option.
3. Automatically through the use of the `buildpkg` (build binary packages for all packages) or `buildsyspkg` (build binary packages only for the [system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>)) values in Portage's `FEATURES` variable.

All three methods will create a binary package in the directory pointed to by the `PKGDIR` variable (which defaults to /var/cache/binpkgs).

When installing software using emerge, Portage can be asked to create binary packages by using `--buildpkg` (`-b`) option:

`root #``emerge --ask --buildpkg sys-devel/gcc`
It is also possible to ask Portage to *only* create a binary package but *not* to install the software on the live system. For this, the `--buildpkgonly` (`-B`) option can be used:

`root #``emerge --ask --buildpkgonly sys-devel/gcc`
The latter approach however requires all build time dependencies to be previously installed.

The most common way to automatically create binary packages whenever a package is installed by Portage is to use the `buildpkg` feature, which can be set in /etc/portage/make.conf like so:

**`/etc/portage/make.conf`**

**Enabling Portage's buildpkg feature**

```
FEATURES="buildpkg"
```
With this feature enabled, every time Portage installs software, it will create a binary package as well.

It is possible to tell Portage not to create binary packages for a select few packages or categories. This is done by passing the `--buildpkg-exclude` option to emerge:

`root #``emerge -uDN @world --buildpkg --buildpkg-exclude "acct-*/* sys-kernel/*-sources virtual/*"`
This could be used for packages that have little to no benefit in having a binary package available. Examples would be the Linux kernel source packages or upstream binary packages (those ending with *-bin* like [www-client/firefox-bin](https://packages.gentoo.org/packages/www-client/firefox-bin)).

It is possible to use a specific compression type on binary packages. Currently, the following formats are supported: `bzip2`, `gzip`, `lz4`, `lzip`, `lzop`, `xz`, and `zstd`. Defaults to `zstd`. Review man make.conf and search for `BINPKG_COMPRESS` for the most up-to-date information.

The compression format can be specified via make.conf.

**`/etc/portage/make.conf`**

**Specify binary package compression format**

```
BINPKG_COMPRESS="lz4"
```
Note that the compression type used might require extra dependencies to be installed, for example, in this case [app-arch/lz4](https://packages.gentoo.org/packages/app-arch/lz4).

A PGP signature enables Portage to check the creator and integrity of a binary package, and to perform trust management based on PGP keys. The binary package signing feature is **disabled** by default. To use it, enable the `binpkg-signing` feature. Note that whether this feature is enabled does not affect the signature verification feature.

**`/etc/portage/make.conf`**

**Enabling Portage's binpkg-signing feature**

```
FEATURES="binpkg-signing"
```
Users also need to set the `BINPKG_GPG_SIGNING_GPG_HOME` and `BINPKG_GPG_SIGNING_KEY` variables for Portage to find the signing key.

**`/etc/portage/make.conf`**

**Configuring Portage's signing key**

```
BINPKG_GPG_SIGNING_GPG_HOME="/root/.gnupg"
BINPKG_GPG_SIGNING_KEY="0x1234567890ABCDEF"
```
Portage will only try to unlock the PGP private key at the beginning. If the user's key will expire over time, then consider enabling `gpg-keepalive` to prevent signing failures.

**`/etc/portage/make.conf`**

**Enabling Portage's gpg-keepalive feature**

```
FEATURES="gpg-keepalive"
```
Existing binpkgs are not signed by default. You can use the gpkg-sign --allow-unsigned command to sign them in place, *without* updating the package index. To sign all unsigned binpkgs:

`root #``find /var/cache/binpkgs -name '*.gpkg.tar' | xargs -n 1 -P $(nproc) gpkg-sign --skip-signed --allow-unsigned``root #``emaint binhost --fix`
The quickpkg application (included in Portage) takes one or more dependency atoms (or package sets) and creates binary packages for all *installed* packages that match that atom.

For instance, to create binary packages of all *installed* GCC versions:

`root #``quickpkg sys-devel/gcc`
To create binary packages for the system set:

`root #``quickpkg @system`
To create binary packages of all installed packages on the system, use the `*` glob:

`root #``quickpkg "*/*"`
Portage supports a number of protocols for downloading binary packages: FTP, FTPS, HTTP, HTTPS, and SSH/SFTP. This leaves room for many possible binary package host implementations.

These are all detailed in the [setting up article](https://wiki.gentoo.org/wiki/Binary_package_guide/Settingup).

Exporting and distributing the binary packages will lead to useless storage consumption if the binary package list is not actively maintained.

In the [gentoolkit](https://wiki.gentoo.org/wiki/Gentoolkit) package an application called [eclean](https://wiki.gentoo.org/wiki/Eclean) is provided. It allows for maintaining Portage-related variable files, such as downloaded source code files, but also binary packages.

The following command will remove all binary packages that have no corresponding ebuild in the installed ebuild repositories:

`root #``eclean packages`
For more details please read the [eclean](https://wiki.gentoo.org/wiki/Eclean) article.

Another tool that can be used is the [qpkg](https://wiki.gentoo.org/wiki/Q_applets#Create_or_manipulate_binary_package_.28qpkg.29) tool from the [app-portage/portage-utils](https://packages.gentoo.org/packages/app-portage/portage-utils) package. However, this tool is a bit less configurable.

To clean up *unused* binary packages (in the sense of used by the server on which the binary packages are stored):

`root #``qpkg -c`
Inside the packages directory exists a [manifest file](https://en.wikipedia.org/wiki/Manifest_file) called Packages. This file acts as a cache for the metadata of all binary packages in the packages directory. The file is updated whenever Portage adds a binary package to the directory. Similarly, eclean updates it when it removes binary packages.

If for some reason binary packages are simply deleted or copied into the packages directory, or the Packages file gets corrupted or deleted, then it must be recreated. This is done using emaint command:

`root #``emaint binhost --fix`
To clear the cache of *all* binary packages:

`root #``rm -r /var/cache/binpkgs/*`
### Same architecture (Native)

If building for two systems that share the same architecture but use different profiles, then chroot building sub-article is likely the best choice for this need.

When building for different architectures such as AMD64 host and ARM64 client, then cross compiling is the method needed to create binpkgs.

These are outlined in [Binary\_package\_guide/Building\_cross](https://wiki.gentoo.org/wiki/Binary_package_guide/Building_cross)

When deploying binary packages for a large number of client systems it might become worthwhile to create snapshots of the packages directory. The client systems then do not use the packages directory directly but use binary packages from the snapshot.

Snapshots can be created using the /usr/lib/portage/python3.11/binhost-snapshot tool that comes with Portage (note that the path to that tool may need to be adjusted to match the [python](https://wiki.gentoo.org/wiki/Python) version with which Portage is installed). It takes four arguments:

1. A source directory (the path to the packages directory).
2. A target directory (that must not exist).
3. A URI.
4. A binary package server directory.

The files from the package directory are copied to the target directory. A Packages file is then created inside the binary package server directory (fourth argument) with the provided URI.

Client systems need to use an URI that points to the binary package server directory. From there they will be redirected to the URI that was given to binhost-snapshot. This URI has to refer to the target directory.

XPAK format binary packages created by Portage have the file name ending with .tbz2. These files consist of two parts:

1. A .tar.bz2 archive containing the files that will be installed on the system.
2. A xpak archive containing package metadata, the ebuild, and the environment file.

See man xpak for a description of the format.

In [app-portage/portage-utils](https://packages.gentoo.org/packages/app-portage/portage-utils) some tools exists that are able to split or create tbz2 and xpak files.

The following command will split the tbz2 into a .tar.bz2 and an .xpak file:

`user $``qtbz2 -s <package>.tbz2`
The .xpak file can be examined using the qxpak utility.

To list the contents:

`user $``qxpak -l <package>.xpak`
The next command will extract a file called USE which contains the enabled USE flags for this package:

`user $``qxpak -x package-manager-0.xpak USE`
GPKG format binary packages created by Portage have the file name ending with .gpkg.tar. These files consist of four parts at least:

1. A gpkg-1 empty file used to identify the format.
2. A C/PV/metadata.tar{.compression} archive containing package metadata, the ebuild, and the environment file.
3. A C/PV/image.tar{.compression} archive containing the files that will be installed on the system.
4. A Manifest file containing checksums to protect against file corruption.
5. Multiple optional .sig files containing OpenPGP signature are used for integrity checking and verification of trust.

The format can be extracted by tar without the need for additional tools.

The currently used format version 2 has the following layout:

The Packages file is the major improvement (and also the trigger for Portage to know that the binary package directory uses version 2) over the first binary package directory layout (version 1). In version 1, all binary packages were also hosted inside a single directory (called All/) and the category directories only had symbolic links to the binary packages inside the All/ directory.

In portage-3.0.15 and later, `FEATURES=binpkg-multi-instance` is enabled by default:

Zoobab wrote a simple shell tool named [quickunpkg](https://github.com/zoobab/quickunpkg) to quickly unpack tbz2 files.

## Troubleshooting

### Undefined Reference Compiler Errors

When using binary packages built on a different host, it's important to ensure the build host is using the same, or a lower version of [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc) or [llvm-core/clang](https://packages.gentoo.org/packages/llvm-core/clang).

In other words, the system using the binary packages must have a compiler version greater than or equal to the binary host.

Failure to use appropriate versions can result in errors similar to:

## See also

- [Binary package quickstart](https://wiki.gentoo.org/wiki/Binary_package_quickstart) — how to install packages from the Gentoo binary package host, how to set up Portage to do this by default, and associated information on Gentoo binhost usage
- [Emerge](https://wiki.gentoo.org/wiki/Emerge) — the main command-line interface to [Portage](https://wiki.gentoo.org/wiki/Portage)
- [Portage](https://wiki.gentoo.org/wiki/Portage) — the official [package manager](https://en.wikipedia.org/wiki/Package_manager) and [distribution system](https://www.gentoo.org/get-started/about/) for Gentoo.
- [Project:Binhost](https://wiki.gentoo.org/wiki/Project:Binhost) — aims to provide readily installable, precompiled packages for a subset of configurations, via central binary package hosting

## External resources

[quickpkg](https://dev.gentoo.org/~zmedico/portage/doc/man/quickpkg.1.html) man page.
