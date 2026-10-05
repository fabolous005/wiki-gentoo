<!-- source: https://wiki.gentoo.org/wiki/Steve | group: Gentoo Wiki (Main) | wiki-title: Steve -->
---
title: steve
url: https://wiki.gentoo.org/wiki/Steve
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-24"
fingerprint: f21d5414db732ba8
license: CC BY-SA 4.0
---

# steve

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**steve** is a jobserver implementation for Gentoo. Its author [mgorny](https://wiki.gentoo.org/wiki/User:MGorny) has written up its design. [\[1\]](https://wiki.gentoo.org#cite_note-1)

[GNU make](https://wiki.gentoo.org/wiki/Make) and other supporting clients support requesting tokens from a [jobserver](https://www.gnu.org/software/make/manual/html_node/POSIX-Jobserver.html) for coordination of parallelism across different make (and friends) instances. It is supported by make, ninja, GCC's [LTO](https://wiki.gentoo.org/wiki/LTO) support, and [Rust](https://wiki.gentoo.org/wiki/Rust)'s Cargo.

This allows capping the total number of parallel jobs across different emerge jobs or calls.

## Installation

### Kernel Configuration

`CONFIG_CUSE` is required in the kernel to be able to have the /dev/steve character device.

Rebuild/install/reboot the kernel in the normal way if any configuration changes are required.

**Enable Character device in Userspace support**

```
File systems --->
  FUSE (Filesystem in Userspace) support --->
    <*>  Character device in Userspace support 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_CUSE</code> to find this item.
### Build steve

`root #``emerge --ask dev-build/steve`
steve itself has an extensive README in /usr/share/doc/steve\*.

## Configuration

Clients are configured via `MAKEFLAGS="--jobserver-auth=fifo:/dev/steve"`.

Note that while the the *--jobserver-auth* flag is GNU Make-specific, non-GNU Make clients usually only check `MAKEFLAGS` and not `GNUMAKEFLAGS`.

### steve daemon

steve itself can be configured via the systemd unit (systemctl edit steve) or OpenRC init script (/etc/conf.d/steve).

It defaults to handing out a maximum of `$(nproc)` tokens which can be adjusted via *--jobs=N*. It supports other limiting factors like load average (*--load-average=N*) and free memory (*--min-memory-avail=N*) too.

#### systemd

When editing steve's startup options with systemctl edit, make sure to clear the old value of `ExecStart` first.

For example, to enable debugging output:

**`N/A`**

```
### Editing /etc/systemd/system/steve.service.d/override.conf
### Anything between here and the comment below will become the contents of the drop-in file
[Service]
ExecStart=
ExecStart=/usr/bin/steve --verbose --debug
### Edits below this comment will be discarded
### /etc/systemd/system/steve.service
# [Service]
# Type=exec
# ExecStart=/usr/bin/steve
# User=steve
# Group=steve
#
# [Install]
# WantedBy=multi-user.target
```
### stevie

The steve daemon can be configured at runtime via the stevie client.

stevie -t is very useful to see how many tokens are left for the job server to allocate.

`user $``stevie --get-tokens`
31

This could be used this to dynamically reduce the number of jobs before starting a particularly heavy package via /etc/portage/bashrc hooks.

### Starting Steve

Enable and start steve via the init system:

#### systemd

`root #``systemctl enable --now steve`
#### OpenRC

`root #``rc-update add steve default``root #``/etc/init.d/steve start`
steve itself has an extensive README in /usr/share/doc/steve\*.

## Usage

### Packages

For the jobserver to be used by packages, the package manager must be told how to communicate this to build systems: Example /etc/portage/make.conf snippet:

**`/etc/portage/make.conf`**

```
# Replace 32 by the value of $(nproc)
MAKEOPTS="-j32 -l32"
NINJAOPTS="-l32"
MAKEFLAGS="-l32 --jobserver-auth=fifo:/dev/steve"
```
It is important that *-jN* is **not** passed to make or other clients, as they interpret this as a directive to not use the jobserver. Portage will defer to `MAKEFLAGS` if both `MAKEOPTS` and `MAKEFLAGS` are set.

On the other hand, `MAKEOPTS` should still be set because some packages using less popular build systems (not involving make or ninja) will extract *-jN* from it to use an appropriate level of parallelism.

Unfortunately, `GNUMAKEFLAGS` cannot be used to resolve this problem because clients like ninja only inspect `MAKEFLAGS`.

### Portage

Portage (>=3.0.74) supports claiming a jobserver token per job in emerge --jobs. This is important because of the 'implicit slot' problem. See [bug #966879](https://bugs.gentoo.org/show_bug.cgi?id=966879).

This is controlled by `FEATURES="jobserver-token"`:

**`/etc/portage/make.conf`**

```
FEATURES="${FEATURES} jobserver-token"
```
### Permissions

Access to steve's jobserver at /dev/steve is controlled by the *jobserver* group from [acct-group/jobserver](https://packages.gentoo.org/packages/acct-group/jobserver). The *portage* group is a member of *jobserver* by default.

Users who wish to access the jobserver for builds run manually as their user will need to add their user to the group.



### Tips & tricks

#### Customizing use per-package

Users often wish for 'large' packages to be treated specially in some way: emerged first, last, or serially (e.g. [bug #460712](https://bugs.gentoo.org/show_bug.cgi?id=460712)). Achieving that is challenging because there is no single definition of a *large* package, nor do all users want the same behavior for them.

One solution to this can be found by combining *stevie* (a client for querying and configuring steve at runtime) and /etc/portage/env. This is especially useful as Portage will request a job from the jobserver (see above) if configured to do so for its own phase execution, not just build systems themselves.

For example, to limit parallel jobs when [www-client/chromium](https://packages.gentoo.org/packages/www-client/chromium) is being built:

**`/etc/portage/env/www-client/chromium`**

```
() {
    # Before starting to compile Chromium, backup the old
    # number of allowed total jobs.
    _STEVE_BACKUP_JOBS=$(stevie --get-jobs)
    _STEVE_BACKUP_MEM_LIMIT=$(stevie --get-min-memory-avail)
    # Lower the number to 5.
    stevie --set-jobs 5
    # Assume 8GB per job.
    stevie --set-min-memory-avail 8192
}
post_src_compile() {
    # Reset to the old value once Chromium has been compiled.
    stevie --set-jobs ${_STEVE_BACKUP_JOBS}
    stevie --set-min-memory-avail ${_STEVE_BACKUP_MEM_LIMIT}
}
```
A similar thing could be done with stevie's *--set-min-memory-avail* to adjust the amount of memory assumed per job. One may wish to try add a *[death hook](https://wiki.gentoo.org/wiki//etc/portage/bashrc#Hook_functions)* to restore the old value if the build fails too.

Another option could be to limit parallelism whenever *check-reqs.eclass* is inherited in an ebuild:

**`/etc/portage/bashrc`**

```
() {
    if has check-reqs ${INHERITED} ; then
        # Before starting to compile a large package, backup the old
        # number of allowed total jobs.
        _STEVE_BACKUP_JOBS=$(stevie --get-jobs)
        _STEVE_BACKUP_MEM_LIMIT=$(stevie --get-min-memory-avail)
        # Lower the number to 5.
        stevie --set-jobs 5
        # Assume 8GB per job.
        stevie --set-min-memory-avail 8192
    fi
}
post_src_compile() {
    if has check-reqs ${INHERITED} ; then
        # Reset to the old value once the large package has been compiled.
        stevie --set-jobs ${_STEVE_BACKUP_JOBS}
        stevie --set-min-memory-avail ${_STEVE_BACKUP_MEM_LIMIT}
    fi
}
```
## See also

- [Guildmaster](https://wiki.gentoo.org/wiki/Guildmaster) — a simple jobserver implementation
- [Make](https://wiki.gentoo.org/wiki/Make) — a [tool to *build*](https://wiki.gentoo.org/wiki/Build_automation#Available_software) software from source code (which usually includes compiling)
