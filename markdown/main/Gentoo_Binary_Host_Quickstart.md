<!-- source: https://wiki.gentoo.org/wiki/Gentoo_Binary_Host_Quickstart | group: Gentoo Wiki (Main) | wiki-title: Gentoo Binary Host Quickstart -->
---
title: Gentoo Binary Host Quickstart
url: https://wiki.gentoo.org/wiki/Gentoo_Binary_Host_Quickstart
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-12"
fingerprint: a1213a2c887b7f90
license: CC BY-SA 4.0
---

# Gentoo Binary Host Quickstart

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Gentoo binhost**

**Binary packages**

Gentoo offers prebuilt binary packages through  the **Gentoo binary package host**, for quicker and easier package [installation](https://wiki.gentoo.org/wiki/Emerge#Install_a_package) and [updates](https://wiki.gentoo.org/wiki/Update). Also called the **Gentoo binhost** for short, it provides thousands of packages that can be installed without the need to compile code locally, or to install build-time dependencies, giving Gentoo users even more choice and convenience!

The Gentoo binhost leverages [Portage](https://wiki.gentoo.org/wiki/Portage)'s longstanding [support for binary packages](https://wiki.gentoo.org/wiki/Binary_package_guide) to systematically provide a huge array of prebuilt packages. It hosts commonly used packages, for a range of common [USE flag](https://wiki.gentoo.org/wiki/USE_flag) configurations, for several system architectures, and with optional optimizations for more recent hardware.

The Gentoo binhost delivers resource-light package installation, for the common case where building with very specific, custom compiler flags is not required. It is important to highlight that using binary packages from the Gentoo binhost in no way diminishes the power, choice, or freedom afforded to Gentoo users; on the contrary, it provides one more useful option for many common use-cases, for an even smoother Gentoo experience. Of course, users who require packages built with specific CFLAGS not available from the Gentoo binhost will not usually use it's packages.

A common way to use the Gentoo binhost is to have Portage default to installing using binary packages whenever such a package is available for the requested USE flag configuration, but to transparently build packages locally to the user's specification when there is no appropriate binary package available. Gentoo deployments configured this way still allow the usual full control over the system, with all USE flags still available to be customized, just with the added benefit of faster package installation whenever possible.

The **amd64** (x86-64) and **arm64** (aarch64) architectures are currently much more widely supported than other architectures - see the [available packages](https://wiki.gentoo.org#Available_packages_and_update_schedule) section. The [available packages and configurations](https://wiki.gentoo.org/wiki/Gentoo_binhost/Available_packages_and_configurations) subpage gives details of what USE flags packages are available for, and wish CFLAGS they use.

This article explains how to install packages from the Gentoo binary package host, how to set up Portage to do this by default, and associated information on Gentoo binhost usage.

## The Gentoo binhost

### Appropriate use-cases

The Gentoo binhost brings some key benefits that will help determine when it suits individual use-cases:

- Faster package installation and upgrades, up to orders of magnitude quicker depending on the package and hardware
- Limited storage and RAM requirements when installing larger packages (i.e., some packages need X GB of RAM during compilation)
- Older systems can be set up rapidly and maintained without waiting longer than needed for packages to build (compile)
- Fast updates and fast set-up time for cloud instances

The Gentoo binhost can be invaluable when installing Gentoo Linux, making for easier and potentially much faster installations.

### Unsuitable use-cases

The binary packages from the Gentoo binhost are built with preset compiler options. If there is the need to set options such as `-march` to a value not available for packages in the Gentoo binhost, then of course it will not be useful. Some `-march` settings can reportedly give up to a 5% boost on x86-64 processors for some cases. Of course if these optimizations are required, packages can always be built locally, even when mixing binary packages into the installation.

The Gentoo binhost can only provide binary packages if the locally-requested [USE flag](https://wiki.gentoo.org/wiki/USE_flag) combination is not changed from the default setting(s) used for the creation of the binary packages. If the user requests non-default USE flags for a package, then the package will need to be compiled locally. Portage can automatically either install from a binary package by default, or build a package locally when needed.

### Available packages and update schedule

The Gentoo binhost currently has strong support for the **amd64** (x86-64) and **arm64** (aarch64) architectures, for which it supplies packages that are known to be commonly installed on desktop systems. Such packages include the [KDE Plasma](https://wiki.gentoo.org/wiki/KDE) and [GNOME](https://wiki.gentoo.org/wiki/GNOME) [desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment), and productivity packages such as [LibreOffice](https://wiki.gentoo.org/wiki/LibreOffice) or [TeX Live](https://wiki.gentoo.org/wiki/TeX_Live), for example. For these architectures, the binary packages are updated *daily*.

Other [architectures and system types](https://packages.gentoo.org/arches) have more basic support on the Gentoo binhost, and are furnished with core packages only. The binary packages for these architectures are updated approximately once a week.

See the [available packages and configurations](https://wiki.gentoo.org/wiki/Gentoo_binhost/Available_packages_and_configurations) subpage for more details.

## Configuration

### Make the Gentoo binhost available to Portage (binrepos.conf)

To be able to use packages from the Gentoo binhost, Portage must be configured to be able to find it. Older installations will have to configure this manually, but more recent installations use new [Stage3](https://www.gentoo.org/downloads/) files which come with the Gentoo binhost preconfigured and ready to use, and the handbook explains during installation how to [set it to be used by default](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Optional:_Adding_a_binary_package_host).

Follow this section to customize the Gentoo binhost configuration, or on systems that were installed before Gentoo binhost configuration was provided by default in Stage3 installation tarballs.

Using a [local Gentoo mirror](https://www.gentoo.org/downloads/mirrors/) is highly recommend to reduce server load and to speed up downloads on the local end. Portage uses files in the directory /etc/portage/binrepos.conf to find binary package hosts, and here are some example configuration files for different mirrors and architectures:

**`/etc/portage/binrepos.conf/gentoo.conf`**

**CDN Mirror Example, amd64**

```
[gentoo]
priority = 9999
sync-uri = https://distfiles.gentoo.org/releases/amd64/binpackages/23.0/x86-64/
# Introduced in portage-3.0.74 for per-repo verification choices
verify-signature = true
# Default value with >=portage-3.0.77
location = /var/cache/binhost/gentoo
```
**`/etc/portage/binrepos.conf/gentoo.conf`**

**UK Mirror Example, amd64**

```
[gentoo]
priority = 9999
sync-uri = https://www.mirrorservice.org/sites/distfiles.gentoo.org/releases/amd64/binpackages/23.0/x86-64/
# Introduced in portage-3.0.74 for per-repo verification choices
verify-signature = true
# Default value with >=portage-3.0.77
location = /var/cache/binhost/gentoo
```
**`/etc/portage/binrepos.conf/gentoo.conf`**

**CN Mirror Example, arm64**

```
[gentoo]
priority = 9999
sync-uri = https://mirrors.aliyun.com/gentoo/releases/arm64/binpackages/23.0/arm64
# Introduced in portage-3.0.74 for per-repo verification choices
verify-signature = true
# Default value with >=portage-3.0.77
location = /var/cache/binhost/gentoo
```
The `sync-uri` in binrepos.conf contains at its end the local system architecture and type of installation. The examples above are given for normal amd64 or arm64 installations.
.

### Configure Portage to use binary packages by default

To automatically download and use a binary package when a suitable one is available on the servers, enable the getbinpkg Portage feature:

**`/etc/portage/make.conf`**

```
FEATURES="getbinpkg"
```
If no suitable binary package can be found, the package will be compiled from source as usual.

### Package signature verification

Now, enable the `binpkg-request-signature` Portage feature to require verification of GPG signatures:

**`/etc/portage/make.conf`**

```
FEATURES="binpkg-request-signature"
```
Once binary packages have to be downloaded, emerge will automatically run the Gentoo Trust Tool known as getuto to set up a ring of GPG keys that are trusted for binary package installation.

If hitting issues, the [preexisting /etc/portage/gnupg directory](https://wiki.gentoo.org#Preexisting_.2Fetc.2Fportage.2Fgnupg_directory) may help.

### Emerge options and EMERGE\_DEFAULT\_OPTS

Below are some useful settings that can be applied via `EMERGE_DEFAULT_OPTS` in make.conf or on the emerge command line to improve the experience of working with Gentoo binary packages.

- --getbinpkg (-g)

- Adding `--getbinpkg` will automatically download and use a binary package when a suitable one is available on the servers. If no suitable binary package can be found, the package will be compiled from source as usual.

- --usepkgonly (-K)

- Using `--usepkgonly` will tell Portage to only use binary packages and exit if no suitable one can be found locally or (with -g) for download.

- --with-bdeps=y

- This can be set to y(es) or n(o) and controls whether build dependencies of packages are downloaded and/or installed.

- For binary package installation, it defaults to no. For source-based installation, the build dependencies are required and are accordingly also installed.

- --binpkg-respect-use=y

- \* By default, Portage will accept binary packages only if use flags match the precise requirements and compile the package from source otherwise. It will also log a warning: "The following binary packages have been ignored due to non matching USE"

- \* When the option is explicitly set to y(es), the warning is disabled.

- \* When the option is explicitly set to n(o), the differences between a user's configuration and the configuration used to make the binary package are ignored, and the binary package is installed anyway.  **Warning:** Dangerous.

- In some cases, it is desirable to sacrifice choice of USE flags in order to expand the number of binary packages that can be installed. Leaving the option unset is therefore useful, because portage will print possible package.use lines which can be used to opt in to those binaries. Otherwise, it is best to set the option to y(es).

## Usage

If Portage is not configured to use binary packages from the Gentoo binhost by default in the `EMERGE_DEFAULT_OPTS`, binary packages can still be used selectively when emerging packages.

### Emerging a single binary package

To install a single package using the binhost, add the `--getbinpkg` (`-g`) switch as in the example below:

`root #``emerge --ask --verbose --getbinpkg app-editors/nano`
Or the shorthand:

`root #``emerge -avg app-editors/nano`
### Update the system using binary packages

To perform a full system update using the binhost use:

`root #``emerge --ask --verbose --update --deep --changed-use --getbinpkg @world`
Or shorthand:

`root #``emerge -avuDUg @world`
## Frequently asked questions

Various questions are covered in the Gentoo news *[Gentoo goes Binary!](https://www.gentoo.org/news/2023/12/29/Gentoo-binary.html)*.

Review the frequently asked questions before [asking for help](https://wiki.gentoo.org/wiki/Support) when using the Gentoo binhost.

### I used to have a binary package but not anymore, what happened?

Builds are scheduled around 5am EST and may take a couple hours to finish building (except when dev-libs/icu has a major version bump, in which case you will have to wait all day because there is just SO MUCH to rebuild. Sorry about that!) Try waiting a bit. Getting in the habit of doing your daily updates later in the day might help too. You may also wish to keep an eye on [bug #924772](https://bugs.gentoo.org/show_bug.cgi?id=924772).

Note that some additional packages are also built by lottery every day. Winning that lottery one day doesn't guarantee it will be built the next time that package gets a version bump.

### Portage still tries to compile from source

If there is a binary package available on a binhost but Portage is not using it, it may well be because of a mismatch between the [USE flags](https://wiki.gentoo.org/wiki/USE_flag) that were requested to install the package with, and the USE flags that were applied to build the binary package.

The USE flags that were used to build the available binary package may be listed by using the following command:

`user $``emerge --pretend --verbose --getbinpkg --usepkgonly --binpkg-respect-use=n <package-X>`
To allow Portage to install from the available binary package, adjust the requested USE flags accordingly, in [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) or [/etc/portage/package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use).

### Preferring binary to source for particular packages

There is currently no way to specify that the binary version of a particular package should be preferred to the source version. The functionality is planned, but there is no specific timeline for implementation at this stage.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> Refer to [bug 463964](https://bugs.gentoo.org/463964) and [bug 924772](https://bugs.gentoo.org/924772).

## Troubleshooting

### Emerge complains with "there are no binary packages to satisfy \<package>"

If `--getbinpkg` (`-g`) isn't supplied, Portage won't search the binhost for binary packages, even if `--usepkgonly` (`-K`) is supplied.

If `--getbinpkg` is being supplied to portage, users should make sure the package exists on the binhost by visiting the URL in /etc/portage/binhost.conf in a web browser.

### keyblock resource: '/etc/portage/gnupg/pubring.kbx': No such file or directory

The keyring in /etc/portage/gnupg/ needs to be generated. If /etc/portage/gnupg/ does not exist, run getuto.

If that directory does exist, follow the instructions in the [next section](https://wiki.gentoo.org#Preexisting_.2Fetc.2Fportage.2Fgnupg_directory).

### Preexisting /etc/portage/gnupg directory

In the past, /etc/portage/gnupg may have been used for older methods of verifying the Gentoo repository. If it exists, *getuto* won't override it, but the correct settings may be missing. If hitting issues, move away the old directory, then run getuto again:

`root #``mv /etc/portage/gnupg /etc/portage/gnupg.bak ; getuto`
## See also

- [Binary package guide](https://wiki.gentoo.org/wiki/Binary_package_guide) — in-depth **binary package** creation, distribution, use, and maintenance
- [Emerge](https://wiki.gentoo.org/wiki/Emerge) — the main command-line interface to [Portage](https://wiki.gentoo.org/wiki/Portage)
- [Portage](https://wiki.gentoo.org/wiki/Portage) — the official [package manager](https://en.wikipedia.org/wiki/Package_manager) and [distribution system](https://www.gentoo.org/get-started/about/) for Gentoo.
- [Project:Binhost](https://wiki.gentoo.org/wiki/Project:Binhost) — aims to provide readily installable, precompiled packages for a subset of configurations, via central binary package hosting

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [Gentoo forums post](https://forums.gentoo.org/viewtopic-p-8819825.html#8819825). Accessed on 2024-03-17.
