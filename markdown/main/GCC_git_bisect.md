<!-- source: https://wiki.gentoo.org/wiki/GCC/git_bisect | group: Gentoo Wiki (Main) | wiki-title: GCC/git bisect -->
---
title: GCC/git bisect
url: https://wiki.gentoo.org/wiki/GCC/git_bisect
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-20"
fingerprint: b1fcbe4cf83d79ad
license: CC BY-SA 4.0
---

# GCC/git bisect

[GCC](https://wiki.gentoo.org/wiki/GCC)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This will explain two main ways to find a commit that introduce regressions in GCC. This guide should be adjusted depending on what GCC needs and how git bisect should run.

## Requirements

Git is needed:

`root #``emerge --ask dev-vcs/git`
Make gcc directory, then 3 subdirectories for build, src, and install.

`user $````
mkdir gcc && cd gcc
```
`user $````
mkdir build
```
`user $``git clone` [https://gcc.gnu.org/git/gcc.git](https://gcc.gnu.org/git/gcc.git) src
`user $````
mkdir install
```
## Bisect

Remove files in build directory to reduce miscalculation from object files:

`user $``cd build && rm -r *`
Prepare the build directory to be compiled:

`user $``../src/configure --prefix=$(pwd)/../install --disable-bootstrap --disable-multilib --disable-libgomp --disable-libsanitizer --enable-languages=c`
Some commits are needed to compile older GCC:

r13-10174-g51b9a0f7dfd244 for GCC 15 to 16

r14-12513-g046776dac7cc74 before GCC 14

r14-9360-g9970b576b7e4ae before GCC 14

In src directory, git-descr.sh RAW-COMMIT and git-undescr.sh PRETTY-COMMIT can be used to transform RAW-COMMIT to PRETTY-COMMIT and vice-versa respectively.

These commits would need to be git apply before the shell script then git apply -R so git does not complain.

Then cd to src, create a shell script that bisects GCC, and run git bisect run:

`user $``cd ../src`
**`git-bisect.sh`**

```
cd ../build
make -j9 CXXFLAGS="-O2"
make install -j9
cd ../install/bin
./gcc ../../test.c
./a.out
GIT_BISECT_RET=$?
cd ../../src
exit $GIT_BISECT_RET
```
`user $``git bisect run git-bisect.sh`
## GCC live ebuilds

When filing GCC bugs upstream, it's common to [Minimize a reproducer](https://wiki.gentoo.org/wiki/GCC/ICE_Reporting_Guide).

The following script may be useful:

**`/tmp/gcc-bisect.sh`**

```
#!/usr/bin/env bash
# Use with 'git bisect run /tmp/gcc-bisect.sh' in a checkout of gcc.git
# GPL-3+
. /lib/gentoo/functions.sh || { echo "Failed to source functions.sh!" ; exit 1 ; }
# Make sure the live ebuild can be used.
export ACCEPT_KEYWORDS="**"
# If we're poking around in a homedir, we don't want privilege dropping.
export FEATURES="ccache -collision-protect -protect-owned -userfetch -usersync -userpriv -usersandbox"
#export EVCS_OFFLINE=1
# This is needed if running 'git bisect' on the same
# repository clone used by the ebuild, not a separate one, and
# we need it to have all refs avaliable that 'git bisect' may
# choose. You only need this if EGIT_OVERRIDE_REPO_GCC is unset.
#export EGIT_CLONE_TYPE=mirror
# Path to clone of repo
# (EGIT_OVERRIDE_*_* come from the messages shown when fetching a live ebuild, see also bug 764422).
export EGIT_OVERRIDE_REPO_GCC=file:///home/sam/git/gcc
# We want to build the specific commit 'git bisect' has told us to
export EGIT_OVERRIDE_COMMIT_GCC=$(git rev-parse HEAD)
esyslog() {
        : No need.
}
die() {
        eerror "$@"
        exit 255 # Abort the bisect process
}
fatal() {
        einfo Running "$@"
        "$@" || die "$@"
}
skip() {
        einfo Running "$@"
        if ! "$@"; then
                ewarn Build failed, skipping: "$@"
                exit 125
        fi
}
gcc_ver=$(<gcc/BASE-VER)
gcc_ver=${gcc_ver%%.*}
fatal cd ~/git/gcc
# If it might flip-flop between major versions, need
# the following to pick a working version first
# because of the symlink shenanigans below:
#fatal gcc-config powerpc-unknown-linux-gnu-10
#fatal source /etc/profile
# Extra checking may be useful if debugging a difficult issue.
# --enable-checking=all
EXTRA_ECONF="--disable-bootstrap" USE="vanilla -rust -openmp -fortran -pgo -lto -jit" skip ebuild /var/db/repos/gentoo/sys-devel/gcc/gcc-${gcc_ver}.*.9999.ebuild digest clean merge
# Try hard to make sure we're actually using the newly-built thing
# (only needed if the commits flip-flop between major versions)
#gcc-config powerpc-unknown-linux-gnu-${gcc_ver}
#source /etc/profile
#export PATH="/root/bisect/bld/_pfx/bin:$PATH"
#export CC=gcc-${gcc_ver}
#export CXX=g++-${gcc_ver}
# Build Binutils with each GCC commit and consider the commit bad if Binutils fails to compile.
# Replace this with whatever the relevant test condition is for a commit being bad/good.
USE="-pgo" fatal ebuild /var/db/repos/gentoo/sys-devel/binutils/binutils-2.40.ebuild clean compile install
exit 0
```
