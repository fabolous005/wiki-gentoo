<!-- source: https://wiki.gentoo.org/wiki/Pkgdev | group: Gentoo Wiki (Main) | wiki-title: Pkgdev -->
---
title: pkgdev
url: https://wiki.gentoo.org/wiki/Pkgdev
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-09-12"
fingerprint: ff8dbcdf8c137bbd
license: CC BY-SA 4.0
---

# pkgdev

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**pkgdev** is a collection of tools for Gentoo development.

pkgdev uses [pkgcheck](https://wiki.gentoo.org/wiki/Pkgcheck), a [pkgcore](https://wiki.gentoo.org/wiki/Pkgcore)-based QA utility.

## Installation

### USE flags


### Emerge

`root #``emerge --ask dev-util/pkgdev`
## Configuration

A second important step in selecting mirrors is to configure the Gentoo ebuild repository via the /etc/portage/repos.conf/gentoo.conf file. This file contains the sync information needed to update the package repository (the collection of ebuilds and related files containing all the information Portage needs to download and install software packages).

Configuring the repository can be done in a few simple steps. First, if it does not exist, create the [repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf) directory:

`root #``mkdir --parents /etc/portage/repos.conf`
Next, copy the Gentoo repository configuration file provided by Portage to the (newly created) repos.conf directory:

`root #``cp /usr/share/portage/config/repos.conf /etc/portage/repos.conf/gentoo.conf`
Take a peek with a text editor or the cat command. Portage repository configuration should be .ini formatted and contain config like:

**`/etc/portage/repos.conf/gentoo.conf`**

```
[DEFAULT]
main-repo = gentoo
[gentoo]
location = /var/db/repos/gentoo
sync-type = rsync
sync-uri = rsync://rsync.gentoo.org/gentoo-portage
auto-sync = yes
sync-rsync-verify-jobs = 1
sync-rsync-verify-metamanifest = yes
sync-rsync-verify-max-age = 3
sync-openpgp-key-path = /usr/share/openpgp-keys/gentoo-release.asc
sync-openpgp-keyserver = hkps://keys.gentoo.org
sync-openpgp-key-refresh-retry-count = 40
sync-openpgp-key-refresh-retry-overall-timeout = 1200
sync-openpgp-key-refresh-retry-delay-exp-base = 2
sync-openpgp-key-refresh-retry-delay-max = 60
sync-openpgp-key-refresh-retry-delay-mult = 4
sync-webrsync-verify-signature = yes
```
The default `sync-uri` variable value listed above will determine a mirror location based on a rotation. This alleviates stress on Gentoo's infrastructure and provides a fail-safe in the event a specific mirror is offline. It's recommended to leave the default URI unchanged unless a local or private mirror is used.

Upstream's documentation [describes](https://pkgcore.github.io/pkgdev/man/pkgdev.html#config-file-support) the config file format and locations.

The `DEFAULT` section in the config will apply to all repositories unless overridden by a later section.

For example, a configuration file for the current user:

**`~/.config/pkgdev/pkgdev.conf`**

```
[DEFAULT]
# Always run 'pkgcheck scan --commits' at the point of commit
# The default is not to do this (for performance) and instead runs on 'pkgdev push'
commit.scan = true
# Add a 'Signed-off-by' tag to all commits, regardless of repository
commit.signoff = true
# Ask before creating a commit with detected serious QA issues
push.ask = true
```
To enable adding a [signoff](https://wiki.gentoo.org/wiki/Gentoo_git_workflow#Signoff) (not the same as PGP signing) automatically only for the `gentoo` repository:

**`~/.config/pkgdev/pkgdev.conf`**

```
[gentoo]
commit.signoff = true
```
## Usage

The upstream repository [covers usage](https://github.com/pkgcore/pkgdev#pkgdev) briefly.

pkgdev has the following subcommands:

- pkgdev commit - uses pkgcheck scan if configured to do so (not by default, see above)
  - pkgdev commit -e - same as above but opens $EDITOR to edit the generated commit message
  - pkgdev commit ... - any unknown arguments are passed directly to git, so check `git(1)`.
- pkgdev manifest - regenerates the [Manifest](https://wiki.gentoo.org/wiki/Repository_format/package/Manifest) for ebuilds in the current directory.
- pkgdev push - makes final QA checks (runs pkgcheck scan and aborts on fatal errors) before running git push automatically
- pkgdev mask - used for [last-riting](https://devmanual.gentoo.org/ebuild-maintenance/removal/index.html) (removing) packages

## Examples

The most common pattern is to enter a repository, navigate to a package (using [app-misc/scrub](https://packages.gentoo.org/packages/app-misc/scrub) as an example), make some changes, and run pkgdev commit, like so (example here is to [bump/update](https://wiki.gentoo.org/wiki/Basic_guide_to_write_Gentoo_Ebuilds#Demonstration_by_example) a version of an ebuild):

`user $````
cd ~/git/gentoo/app-misc/scrub
```
`user $````
sudo emerge -av1o --with-test-deps app-misc/scrub
```
`user $````
cp scrub-2.6.0.ebuild scrub-2.6.1.ebuild
```
`user $````
pkgdev manifest
```
`user $````
$EDITOR scrub-2.6.1.ebuild
```
`user $````
ebuild scrub-2.6.1.ebuild clean test install merge
```
`user $````
git add scrub-2.6.1.ebuild && pkgdev commit
```
app-misc/scrub
  DeprecatedEapi: version 2.6.0: uses deprecated EAPI 6
  RedundantVersion: version 2.6.0: slot(0) keywords are overshadowed by version: 2.6.1
  DeprecatedEapi: version 2.6.1: uses deprecated EAPI 6
\[master 65b81235dc06d\] app-misc/scrub: add 2.6.1
 2 files changed, 17 insertions(+)
 create mode 100644 app-misc/scrub/scrub-2.6.1.ebuild

## Troubleshooting

### `pkgdev bugs --stablereq` gets the "no change for" wrong

`pkgdev` checks when the ebuild was last modified using git.
If the repository was cloned with the `--depth` flag  [as suggested](https://wiki.gentoo.org/wiki/Gentoo_git_workflow#Cloning) it will not contain
the full history. That means that the tool will be only able to check for changes until the oldest commit in your local repository.

To check what is the oldest commit, so to see how far back can `pkgcheck` reach a command can be run inside the repository:

`user $````
git log --date short --max-parents 0 | grep Date
```
Date:   2024-08-25

Older commits up to a certain date can be fetched with:

`user $``git fetch --shallow-since YYYY-MM-DD`
while the full repo history can be fetched with:

`user $``git fetch --unshallow`


### pkgdev tries to use an invalid commit hash

If you get an error similar to this one, e.g. when using the `commit` sub-command

`user $````
pkgdev commit
```
pkgdev commit: error: failed running git log: fatal: Invalid revision range 7d50e72c9ce20d1dfdd730d26e9695989c2f9ca2..origin/HEAD

It's probably related to caching issues (e.g. [\[1\]](https://github.com/pkgcore/pkgcheck/issues/321)). One way to workaround it is to clear the cache while within the affected repository:

`user $````
pkgcheck cache -R
```
### pkgdev doesn't sign-off my commits

pkgdev does not sign-off ('Signed-off-by' trailer) commits by default for any repo, following git upstream behavior, to encourage a [conscious decision](https://archives.gentoo.org/gentoo-project/message/b23ccce6ec7124b28d3fb98148b044f5) to sign off.

To enable automatic sign-offs per repository or globally, see [Pkgdev#Configuration](https://wiki.gentoo.org/wiki/Pkgdev#Configuration). pkgdev only uses what git is configured to use.

### pkgdev uses new /var/cache/distfiles directory unexpectedly

See also the [pkgcheck](https://wiki.gentoo.org/wiki/Pkgcheck#error:_repos.conf:_default_repo_.27gentoo.27_is_undefined_or_invalid) article.

For systems not migrated to the modern [repository data locations](https://wiki.gentoo.org/wiki/User:Sam/Portage_help/Migrating_to_new_repository_data_locations), pkgdev manifest may try write to /var/cache/distfiles unexpectedly. This is because pkgcore doesn't read /usr/share/portage (which is where Portage's defaults reside).

There are three solutions:

1. Set `manifest.distdir = /usr/portage/distfiles` in \~/.config/pkgdev/pkgdev.conf, or
2. Set `DISTDIR=/usr/portage/distfiles` in /etc/portage/make.conf, or
3. Migrate the repository locations.

### git signing errors

When committing in a repository (like ::gentoo) with 'sign-commits = true' in [metadata/layout.conf](https://wiki.gentoo.org/wiki/Repository_format/metadata/layout.conf), pkgdev will ask git to sign commits. This requires a GPG key.

There are two options:

1. Generate a key and configure git to use it
2. **non-developers** committing to Gentoo via pull requests or patches don't actually *need* a key and can disable signing, as only the person pushing to the repository needs a key at present. They can locally set `sign-commits = false` in metadata/layout.conf but of course this change should not be committed. This will prevent pkgdev from trying to sign commits.

See [Gentoo git workflow#GPG configuration](https://wiki.gentoo.org/wiki/Gentoo_git_workflow#GPG_configuration) for details on how to generate a key and debug GPG errors. pkgdev only uses what git is configured to use.

If errors persist, run GIT\_TRACE=1 pkgdev commit ... and run the gpg ... command mentioned in its output manually to further debug.

## See also

- [Standard git workflow](https://wiki.gentoo.org/wiki/Standard_git_workflow) — describing a **modern git workflow** for contributing to Gentoo, with [pkgcheck](https://wiki.gentoo.org/wiki/Pkgcheck) and [pkgdev]
- [Gentoo git workflow](https://wiki.gentoo.org/wiki/Gentoo_git_workflow) — outlines some rules and best-practices regarding the typical workflow for ebuild developers using git.
- [pkgcheck](https://wiki.gentoo.org/wiki/Pkgcheck) — a [pkgcore](https://wiki.gentoo.org/wiki/Pkgcore)-based QA utility for ebuild repos.

## External resources

- [Devmanual](https://devmanual.gentoo.org/ebuild-maintenance/git/#committing-to-gentoo.git)
- [https://archives.gentoo.org/gentoo-dev/message/7f77d3c7729251c94bfb75a1bee3b691](https://archives.gentoo.org/gentoo-dev/message/7f77d3c7729251c94bfb75a1bee3b691) - \[gentoo-dev\] pkgdev new release v0.2.1 with breaking change for signoff default
