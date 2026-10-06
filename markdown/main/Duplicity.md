<!-- source: https://wiki.gentoo.org/wiki/Duplicity | group: Gentoo Wiki (Main) | wiki-title: Duplicity -->
---
title: Duplicity
url: https://wiki.gentoo.org/wiki/Duplicity
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-09-14"
fingerprint: aa7b494665b111ca
license: CC BY-SA 4.0
---

# Duplicity

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Duplicity is backup software written in Python with native [GnuPG](https://wiki.gentoo.org/wiki/GnuPG) integration and support for many different storage backends. It is designed around the ubiquitous [Tar](https://wiki.gentoo.org/wiki/Tar), [GnuPG](https://wiki.gentoo.org/wiki/GnuPG), and the same algorithm as [Rsync](https://wiki.gentoo.org/wiki/Rsync) via [net-libs/librsync](https://packages.gentoo.org/packages/net-libs/librsync).

It supports both whole backups as well as incremental diffs, using librsync to generate small deltas. It doesn't have a complex file format for its metadata, instead using tar and rdiff.

Wrappers like [app-backup/duply](https://packages.gentoo.org/packages/app-backup/duply) or [app-backup/deja-dup](https://packages.gentoo.org/packages/app-backup/deja-dup) are also available.

## Installation

### USE flags


### USE flags for
            [app-backup/duplicity](https://packages.gentoo.org/packages/app-backup/duplicity)
            
            Secure backup system using GnuPG to encrypt data

| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [s3](https://packages.gentoo.org/useflags/s3) | Support for backing up to the Amazon S3 system | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask app-backup/duplicity`
## Configuration

duplicity has limited configuration file support, but it's not necessary either, as the CLI is simple and can be handled with just a small script. There *is* a *--config-dir* argument but the format it accepts isn't well-documented and use of this feature isn't common at all.

It recognizes the following environment variables:

- *BACKEND\_PASSWORD* (previously *FTP\_PASSWORD*)
- *PASSPHRASE* (used for encrypting backups with gpg)
- *SIGN\_PASSPHRASE* (ditto, but for unlocking the gpg signing key)
- Others depending on the storage backend

## Usage

The [man page](https://duplicity.us/stable/CHANGELOG.html) is extensive and recommended reading. Below, some basic examples are covered.

### Backing up / to Backblaze with parity

duplicity can optionally chain *par2* to add parity for a proportion of the backed up data to add some resilience in the event of corruption on the storage side. Install [app-arch/par2cmdline](https://packages.gentoo.org/packages/app-arch/par2cmdline) and prefix the destination with 'par2+' for this.

**`~/bin/backup-home`**

```
#!/bin/bash
# Duplicity uses these: specifying passwords via environment variables
# is better than exposing them by passing as command-line arguments.
export PASSPHRASE="..."
export BACKEND_PASSWORD="..."
# Configuration for the script itself
DUPLICITY_KEYID=""
DUPLICITY_BUCKET=""
DUPLICITY_ARGS=(
        # Optional verbosity (controllable)
        -v info
        --concurrency $(nproc)
        # Run an incremental backup on each invocation unless
        # it has been a month since the last full backup: in which case,
        # start a new incremental chain with a full backup.
        --full-if-older-than 1M
        #
        # https://duplicity.nongnu.org/vers7/duplicity.1.html#toc9
        #
        --exclude ~/.cache
        --exclude ~/.ccache
        --exclude ~/.debug
        --exclude /var/cache/binpkgs
        --exclude /var/cache/distfiles
        --exclude /var/tmp
        --exclude /tmp
        --exclude /sys
        --exclude /proc
        --exclude /dev
        --exclude /run
        # Backup / to b2 with par2 parity support added (https://duplicity.nongnu.org/vers7/duplicity.1.html#toc21)
        backup / par2+b2://${DUPLICITY_KEYID}@${DUPLICITY_BUCKET}
)
duplicity "${DUPLICITY_ARGS[@]}"
```
### Backing up /home to Backblaze

**`~/bin/backup-home`**

```
#!/bin/bash
# Duplicity uses these: specifying passwords via environment variables
# is better than exposing them by passing as command-line arguments.
export PASSPHRASE="..."
export BACKEND_PASSWORD="..."
# Configuration for the script itself
DUPLICITY_KEYID=""
DUPLICITY_BUCKET=""
DUPLICITY_ARGS=(
        # Optional verbosity (controllable)
        -v info
        --concurrency $(nproc)
        # Run an incremental backup on each invocation unless
        # it has been a month since the last full backup: in which case,
        # start a new incremental chain with a full backup.
        --full-if-older-than 1M
        --exclude ~/.cache
        # Backup homedir (~) to b2
        backup ~ b2://${DUPLICITY_KEYID}@${DUPLICITY_BUCKET}
)
duplicity "${DUPLICITY_ARGS[@]}"
```
