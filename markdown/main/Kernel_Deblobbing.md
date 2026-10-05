<!-- source: https://wiki.gentoo.org/wiki/Kernel/Deblobbing | group: Gentoo Wiki (Main) | wiki-title: Kernel/Deblobbing -->
---
title: Kernel/Deblobbing
url: https://wiki.gentoo.org/wiki/Kernel/Deblobbing
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-30"
fingerprint: d7c37635cc0b1f96
license: CC BY-SA 4.0
---

# Kernel/Deblobbing

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

From Wikipedia: *A binary blob is a closed-source binary-only piece of software without publicly available source code.* Since 1996, Linux kernel includes an increasing amount of binary blobs to handle devices with closed-sources firmware only (and among these devices, those without technical documentation which would authorize free firmware development for them). Proprietary software always introduces freedom, security, or privacy concerns.

Deblobbing is the operation which removes binary blobs from source code to get a completely blob-free kernel. This is done with two shell scripts written by Brian Brazil, Jeff Moe, and Alexandre Oliva, named deblob-check and deblob-\<version>, where *\<version>* represents the kernel revision. A third script which is for Linux tarballs, deblob-main, is not used here. See the scripts [README](https://www.fsfla.org/svn/fsfla/software/linux-libre/scripts/README) to get information about them.

Those scripts are provided by the Free Software Foundation Latin America, for each kernel revision. FSFLA mainly provides Linux-libre distribution.

Once deblobed the kernel is compiled as usual. Deblobbing kernel obviously means that **devices that exclusively support proprietary firmware cannot be used**. Generally this includes all new Intel wireless cards. Nvidia graphic cards will only use the nouveau open source driver.

See below how to get a list of removed blobs without kernel compilation.

## Deblobbing rt-sources

For [sys-kernel/rt-sources](https://packages.gentoo.org/packages/sys-kernel/rt-sources), set the USE flag `deblob` in /etc/portage/make.conf, sources will be automatically deblobed after being emerged.

## Deblobbing gentoo-sources (or any other sources)

### Emerging sources

Emerge Gentoo sources:

`root #``emerge --ask sys-kernel/gentoo-sources`
Then set /usr/src/linux symlink to the emerged sources if `symlink` USE flag is not set for Gentoo sources:

First get the list of available sources:

`root #``eselect kernel list`
\[1\] linux-6.7.8-gentoo \*
\[2\] linux-6.8.1-gentoo
etc.

… then choose the right ones:

`root #``eselect kernel set linux-6.8.1-gentoo`
Then change directory to /usr/src/linux:

`root #``cd /usr/src/linux`
### Downloading deblob scripts

Scripts are on [Linux-libre server](https://linux-libre.fsfla.org/pub/linux-libre/releases/). First make some vars to simplify lines to be entered:

- `version` will contain sources version and is the only one to be adapted, a minor version number is not mandatory;
- `main` will be generated, containing version without minor revision if needed, `6.8` for `6.8.1`;
*(If there is a release candidate string, `6.9-rc7` for example, enter `main` by hand —there is usually no rc string with gentoo-sources.)*
- `url` will contain downloading URL.

Making the vars:

`root #````
version=6.8.1
```
`root #````
if [ `echo $version | tr -cd '.' | wc -c` == 2 ]; then main=`echo ${version%.*}`; else main=$version; fi
```
Downloading:

`root #````
wget $url$version-gnu/deblob-$main
```
`root #````
wget $url$version-gnu/deblob-$main.sign
```
`root #````
wget $url$version-gnu/deblob-check
```
`root #````
wget $url$version-gnu/deblob-check.sign
```
Make the scripts executable by root only:

`root #``chmod 744 deblob-$main deblob-check`
### Verifying signatures

First import or update the Linux-libre server key:

`root #``gpg --keyserver keys.gnupg.net --recv-key BCB7CF877E7D47A7`
Then verify the signatures:

`root #````
gpg --verify deblob-$main.sign deblob-$main
```
`root #````
gpg --verify deblob-check.sign deblob-check
```
### Python version

Deblob scripts use the Python 3.8 interpreter. Emerge it:

`root #``emerge --ask --noreplace --oneshot dev-lang/python:3.8`
### Deblob command

Deblobbing can now be started (remember the `main` var):

`root #``PYTHON="python3.8" ./deblob-$main`
During the operation, which may be long, all deblobbing information is displayed. After kernel compilation **-gnu** suffix will be appended to it's name, `6.8.1-gentoo-gnu` in this example.

### List of removed blobs

To get the list of removed blobs with their kernel symbol names, redirect the deblob command output to a file:

`root #``PYTHON="python3.8" ./deblob-$main > /path/to/file/deblob-$version.log`
As kernel sources can be re-emerged after being deblobed, this is a convenient way, without kernel compilation, to investigate if targeted hardware should work without binary blobs.

## Troubleshooting

`ERROR: tools/testing/selftests/tc-testing/action-ebpf does not exist, something is wrong`
There is "rm" command for this folder in gentoo-sources ebuild file at "prepare" phase. One of ways to fix this with proper version:

`root #``ebuild /var/db/repos/localrepo/sys-kernel/gentoo-sources/gentoo-sources-6.12.24.ebuild unpack``root #``cp -r /var/tmp/portage/sys-kernel/gentoo-sources-6.12.24-r2/work/linux-6.12.24-gentoo/tools/testing/selftests/tc-testing/action-ebpf /usr/src/linux/tools/testing/selftests/tc-testing/`
