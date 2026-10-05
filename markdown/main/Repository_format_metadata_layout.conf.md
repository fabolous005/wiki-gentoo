<!-- source: https://wiki.gentoo.org/wiki/Repository_format/metadata/layout.conf | group: Gentoo Wiki (Main) | wiki-title: Repository format/metadata/layout.conf -->
---
title: Repository format/metadata/layout.conf
url: https://wiki.gentoo.org/wiki/Repository_format/metadata/layout.conf
hostname: gentoo.org
sitename: Repository format/metadata/layout.conf
date: "2026-01-09"
fingerprint: a903b89c06213be0
license: CC BY-SA 4.0
---

# Repository format/metadata/layout.conf

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The metadata/layout.conf file is a file describing global properties of a repository.

Its behaviour is defined by GLEP 82. Implementations may implement extensions but these cannot be relied upon by ebuilds or repositories wishing to be portable.

## File format

- The file consists of a number of key-value pairs, one pair per line, with the two separated by `=` (equal sign).
- Comment lines start with `#`.

**`metadata/layout.conf`**

**An example showing the format of the file**

```
key1 = value1
# comment
key2 = value2
```
## Possible keys

This page contains only some of the possible keys and values in layout.conf.

For a thorough listing, refer to:

- GLEP 82 (authoritative)
- the Portage manual: man 5 portage

### repo-name

This setting specifies the name of the repository. It takes precedence over an existing value in [profiles/repo\_name](https://wiki.gentoo.org/wiki/Repository_format/profiles/repo_name):

**`metadata/layout.conf`**

**Specifies repository name**

```
repo-name = foo-overlay
```
### aliases

Allows multiple names (separate from the primary name, see `repo-name`) to be used to reference the repository. Unset by default. Space-separated.

**`metadata/layout.conf`**

**Specifies repository aliases**

```
# gen2 is a fork of Gentoo that can be used in place of the Gentoo
# repository
repo-name = gen2
aliases = gentoo
```
### masters

The *masters* key specifies a list of master repositories for this particular repository. Whenever installing an ebuild from the particular repository, the package manager can use ebuilds, eclasses, and (depending on the repository format used) profiles from one or more master repositories.

The most common example is a repository (overlay) which provides additional packages for Gentoo. Such a package uses resources from the *gentoo* ebuild repository:

**`metadata/layout.conf`**

**An example for repository relying on*gentoo* ebuild repository ebuilds, eclasses, and possibly profiles**

```
masters = gentoo
```
A particular repository may have more than a single *masters* entry. These are not inherited over repositories. In the following example, everything from gentoo take precedence over everything in python that shares the same name:

**`metadata/layout.conf`**

**An example for repository relying on two other repositories**

```
masters = python gentoo
```
Finally, a stand-alone repository like *gentoo* should provide an empty masters list. This means that all ebuilds (dependencies), eclasses, and so on in that repository must be found in that repository:

**`metadata/layout.conf`**

**An example for a stand-alone repository**

```
masters =
```
### eapis-banned

This setting bans [EAPIs](https://wiki.gentoo.org/wiki/EAPI) (repoman will fail):

**`metadata/layout.conf`**

**Bans EAPI 0 and EAPI 1**

```
eapis-banned = 0 1
```
### profile-eapis-banned

Similar to `eapis-banned` but for the EAPI used in profiles.

### eapis-deprecated

This setting marks EAPIs as deprecated (repoman will prompt a warning):

**`metadata/layout.conf`**

**Marks EAPI 2 and EAPI 3 as deprecated**

```
eapis-deprecated = 2 3
```
### profile-eapis-deprecated

Similar to `eapis-deprecated` but for the EAPI used in profiles.

### sign-commits

If enabled, the commits made in this repository will be signed:

**`metadata/layout.conf`**

**Enabling commit signing**

```
sign-commits = true
```
This applies only to git repositories. It requires git 1.7.9 or newer.

The key used to sign commits can be set through:

`user $``git config user.signingkey` *<keyid>*
### manifest-hashes

See [Repository format/package/Manifest](https://wiki.gentoo.org/wiki/Repository_format/package/Manifest) too.

Tools will support these hash algorithms when reading and creating Manifests for ebuilds. They may use a subset if some are unrecognized or not supported. Space-separated.

The default is implementation-defined.

**`metadata/layout.conf`**

**Defining hashes used for Manifests**

```
manifest-hashes = BLAKE2B SHA512
```
### manifest-required-hashes

This defines the list of hashes which **must** be present in a Manifest. Tools must regenerate (and refetch if required)
Manifests if these hashes are not present.

**`metadata/layout.conf`**

**Defining mandatory hashes used for Manifests**

```
# Allow using BLAKE2B and/or SHA512
manifest-hashes = BLAKE2B SHA512
# But require BLAKE2B to be present at least
# If unset, would require both BLAKE2B and SHA512.
manifest-required-hashes = BLAKE2B
```
### sign-manifests

*Enabled by default.* If enabled, the manifest files will be [signed](https://wiki.gentoo.org/wiki/Repository_format/package/Manifest#Manifest_signing) whenever committing to this repository from a GPG-enabled client:

**`metadata/layout.conf`**

**Disabling Manifest signing**

```
sign-manifests = false
```
In Portage, the manifest signing is enabled by adding `sign` to the `[FEATURES](https://wiki.gentoo.org/wiki/FEATURES)` variable in /etc/portage/make.conf.

### thin-manifests

*Disabled by default*. If enabled, [thin manifests](https://wiki.gentoo.org/wiki/Repository_format/package/Manifest#Thin_Manifest) will be used inside the repository instead of the regular manifests:

**`metadata/layout.conf`**

**Enabling thin Manifests**

```
thin-manifests = true
```
Repositories using thin-manifests will have only `DIST` entries in their Manifests.

### use-manifests

*Strict by default*. Enforces a correct [manifest](https://wiki.gentoo.org/wiki/Repository_format/package/Manifest) for each package:

**`metadata/layout.conf`**

**Enforce correct manifests**

```
use-manifests = strict
```
Possible values are:

- `strict` to enforce manifest usage;
- `true` to allow and create Manifests, but mismatches are ignored;
- `false` to disable manifest usage.

### update-changelog

Controls whether tools should write a ChangeLog. false by default.

**`metadata/layout.conf`**

**Enabling ChangeLogs**

```
# Note that this is OFF by default.
update-changelog = true
```
### cache-formats

Controls cache formats used by the repository. Default is implementation-defined.

**`metadata/layout.conf`**

**Cache format**

```
# Modern repo
cache-formats = md5-dict
# Backwards compatibility case (very old versions of Portage, <2.2.0_alpha68)
cache-formats = md5-dict pms
```
### profile-formats

Control functionality available to profiles in this repo such as which files may be dirs, or the syntax available in parent files.  Use `portage-2` if unsure. Default is `portage-1-compat` even though it is invalid to set this manually.

**`metadata/layout.conf`**

**Use repository relative profile paths**

```
profile-formats = portage-2
```
Possible values are `pms` `portage-1` `portage-2` `profile-bashrcs` `profile-set` `profile-default-eapi` `build-id`, and `profile-repo-deps`. Multiple options can be set (space-separated).

- `pms` is the format specified in the [Package Manager Specification](https://wiki.gentoo.org/wiki/Package_Manager_Specification) which has no extensions: the Portage extensions below exist to supplement functionality in this base specification
- `portage-1` indicates that this repo contains profiles that may use directories for USE and mask configuration instead of just files
- `portage-2` indicate that paths such as gentoo:targets/desktop or :targets/desktop in profile parent files can be used to express paths relative to the root profiles directory of a repository
- `profile-bashrcs` enables the per-profile bashrc mechanism package.bashrc
- `profile-set` enables support for using the profile packages file to add atoms to the `@profile` package set
- `profile-default-eapi` enables support for the `profile_eapi_when_unspecified` attribute of layout.conf
- `build-id` allows dependency atoms in the profile to refer to specific builds (see the binpkg-multi-instance [FEATURES](https://wiki.gentoo.org/wiki/FEATURES) setting in make.conf(5)). A build-id atom is identical to a version-specific atom, except that the version is followed by a hyphen and an integer build-id
- `profile-repo-deps` allows dependency atoms in the profile to refer to specific repositories
- `profile-license` enables package.license in profiles

### properties-allowed

Controls the list of `PROPERTIES` tokens/values which are permitted for ebuilds within the repository. If set, development tools will warn on tokens in ebuilds which are not listed. If unset, all tokens are permitted.

**`metadata/layout.conf`**

**Allow PROPERTIES=live**

```
properties-allowed = live
```
### restrict-allowed

Controls the list of `RESTRICT` tokens/values which are permitted for ebuilds within the repository. If set, development tools will warn on tokens in ebuilds which are not listed. If unset, all tokens are permitted.

**`metadata/layout.conf`**

**Allow RESTRICT=primaryuri**

```
restrict-allowed = primaryuri
```
## External resources

- [https://www.gentoo.org/glep/glep-0082.html#configuration-keys](https://www.gentoo.org/glep/glep-0082.html#configuration-keys) - GLEP 82: Repository configuration file (layout.conf)
