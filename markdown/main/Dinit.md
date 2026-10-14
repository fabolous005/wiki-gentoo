<!-- source: https://wiki.gentoo.org/wiki/Dinit | group: Gentoo Wiki (Main) | wiki-title: Dinit -->
---
title: Dinit
url: https://wiki.gentoo.org/wiki/Dinit
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-14"
fingerprint: "1308a1c7614488e5"
license: CC BY-SA 4.0
---

# Dinit

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

- Cover more of `dinitctl`.
- Explain user services

**Dinit**  is a service supervisor with dependency support which can also act as an [init system](https://wiki.gentoo.org/wiki/Init_system).

It was created with the intention of providing a portable init system with dependency management and process supervision that was functionally superior to many existing init systems; its development goals include clean design, robustness, portability, usability, and avoiding feature bloat.

Dinit is designed to integrate with rather than subsume or replace other system software.

## Installation

### USE flags

The available USE flags may be retrieved with the equery utility from [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit):

`user $``equery uses sys-apps/dinit`
### Emerge

The Dinit ebuild is available in the [GURU](https://wiki.gentoo.org/wiki/GURU) repository, which is Gentoo's user repository. The GURU repository can be added manually or with repository management tools like [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository).

To add the `guru` overlay, run:

`root #````
emerge -va app-eselect/eselect-repository
```
`root #````
eselect repository enable guru
```
`root #````
emaint sync -r guru
```
Once the repository has been added, unmask the [sys-apps/dinit::guru](https://github.com/gentoo-mirror/guru/tree/master/sys-apps/dinit) package:

**`/etc/portage/package.accept_keywords/dinit`**

```
sys-apps/dinit
```
Finally, install the [sys-apps/dinit::guru](https://github.com/gentoo-mirror/guru/tree/master/sys-apps/dinit) package:

`root #``emerge --ask sys-apps/dinit`
## Configuration

### Service files

Service files for Dinit can be found in /etc/dinit.d. An example of how such a service file may look:

**`/etc/dinit.d/turnstiled`**

```
type        = process
command     = /usr/bin/turnstiled
logfile     = /var/log/turnstiled.log
before:     login.target
depends-on: local.target
```
For detailed information on how to write service files, run man dinit-service.

### Directories

- /etc/dinit.d - System service files
- /etc/dinit.d/boot.d - Enabled services
- /etc/dinit.d/config - System service configurations
- /usr/lib/dinit.d - Package installed service files
- /usr/lib/dinit - Package installed service helpers and scripts

## Usage

### Dinit as the system init

To use Dinit as the system init, a set of basic system services is required.

Additionally, it is necessary to unmask [virtual/service-manager](https://packages.gentoo.org/packages/virtual/service-manager), [sys-apps/dinit-services::guru](https://github.com/gentoo-mirror/guru/tree/master/sys-apps/dinit-services), and optionally [app-crypt/cryptsetup-scripts-dinit::guru](https://github.com/gentoo-mirror/guru/tree/master/app-crypt/cryptsetup-scripts-dinit):

**`/etc/portage/package.accept_keywords/dinit`**

```
sys-apps/dinit-services
virtual/service-manager
# If using the cryptsetup useflag on sys-apps/dinit-services
# app-crypt/cryptsetup-scripts-dinit
```
Now install the testing version of [virtual/service-manager](https://packages.gentoo.org/packages/virtual/service-manager):

`root #``emerge --ask --oneshot virtual/service-manager`
The [sysv-utils](https://packages.gentoo.org/useflags/sysv-utils) [USE flag must be set on](https://wiki.gentoo.org/wiki/USE_flag) [sys-apps/dinit::guru](https://github.com/gentoo-mirror/guru/tree/master/sys-apps/dinit); otherwise, you will run into issues with shutdown and reboot:

**`/etc/portage/package.use/dinit`**

```
sys-apps/dinit sysv-utils
```
Now install the service package:

`root #``emerge --ask sys-apps/dinit-services`
Lastly, emerge sys-apps/dinit with the new USE flag. Doing so will also replace OpenRC:

`root #``emerge --ask sys-apps/dinit`
### Dinitctl

Dinit services can be controlled with dinitctl. In all the following commands, `<service>` is the name of the service to be acted on:

Enable a service:

`root #``dinitctl start <service>`
Disable a service:

`root #``dinitctl disable <service>`
Start a service:

`root #``dinitctl start <service>`
Stop a service:

`root #``dinitctl stop <service>`
Restart a service:

`root #``dinitctl restart <service>`
Reload a service description:

`root #``dinitctl reload <service>`
List loaded services and their state:

`root #``dinitctl list`
Before each service, one of the following state indicators is displayed:

```
[{+}     ] — service has started.
[{ }<<   ] — service is starting.
[   <<{ }] — service is starting, will stop once started.
[{ }>>   ] — service is stopping, will start once stopped.
[   >>{ }] — service is stopping.
[     {-}] — service has stopped.
```
## See also

- [Comparison\_of\_init\_systems](https://wiki.gentoo.org/wiki/Comparison_of_init_systems) — compares and contrasts **[init systems](https://wiki.gentoo.org/wiki/Init_system)** for Unix(like) [OSs](https://en.wikipedia.org/wiki/Operating_system)
- [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) — a dependency-based [init system](https://wiki.gentoo.org/wiki/Init_system) for Unix-like systems that maintains compatibility with the system-provided init system
- [Runit](https://wiki.gentoo.org/wiki/Runit) — lightweight process supervision suite, originally inspired by [daemontools](https://wiki.gentoo.org/wiki/Daemontools) that offers fast and reliable service management.
