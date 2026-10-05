<!-- source: https://wiki.gentoo.org/wiki/Test_environment | group: Gentoo Wiki (Main) | wiki-title: Test environment -->
---
title: Test environment
url: https://wiki.gentoo.org/wiki/Test_environment
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-28"
fingerprint: cd23387635e1032d
license: CC BY-SA 4.0
---

# Test environment

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Testing your ebuilds (see also [Package testing](https://wiki.gentoo.org/wiki/Package_testing)) can be a tedious task. Beside a simple re-emerge of the package in question, to see whether it merges successfully, a good testing practice usually needs to take one or more of the following questions into account:

- Having a clean gentoo installation to test with: Using your day to day desktop, might miss on dependencies, which you happen to have installed already and thus producing false positives, i.e. letting your ebuild successfully install, while on a new system it would have been failed due to missing dependencies.
- An exhaustive testing of all possible **USE flag combinations:** For ebuilds with only a few USE flags, this can be done easily by hand. For packages with a lot of USE flags, such an approach is error-prone. You might be better by writing a shell or python script to enumerate all the possibilities.
- **Profile testing:** Here testing of default vs. hardened and multilib vs. no-multilib profiles is of interest. It may for example uncover problems with PIC/PIE code in hardened profiles or problems of missing proper multiclassing in the ebuild. Usually you don't want to switch profiles just for the purpose of testing an ebuild.
- **Keyword testing:** This requires the proper hardware and is done by the arch projects.

So to run proper and efficient tests for your ebuild, a dedicated test environment seems necessary. There are several options for this, like using a chroot environment, VM's, a containerized enviroment or even dedicated hardware. The table below summarizes some of the pros and cons of these options.

| Method | Pros | Cons | 
|---|---|---|
| [chroot](https://wiki.gentoo.org/wiki/Chroot) [guide](https://wiki.gentoo.org/wiki/Project:X86/Chroot_Guide) |  | need to reset the environment after each emerge | 
| [chroot using btrfs](https://wiki.gentoo.org/wiki/Chroot_for_package_testing) |  |  | 
| [Virtual Machine](https://wiki.gentoo.org/wiki/Virtualization#Hypervisors) |  | large memory footprint | 
| [Container](https://wiki.gentoo.org/wiki/Virtualization#Containers) |  |  | 

#### Using Docker containers for ebuild testing with ebuildtester

**[ebuildtester](https://wiki.gentoo.org/wiki/Ebuildtester)** is a [Python](https://wiki.gentoo.org/wiki/Python) script for testing ebuilds in a [Docker](https://wiki.gentoo.org/wiki/Docker) container.

ebuildtester compiles a docker container with the parameters passed at invocation time and either installs the specified package or puts the user into a shell inside the container.

Usage of the script is simple, go to the [ebuildtester](https://wiki.gentoo.org/wiki/Ebuildtester) article for more details.

#### Using LXC containers for ebuild testing

These instructions make use of [unprivileged LXC containers](https://wiki.gentoo.org/wiki/LXC#Unprivileged_User_configuration).

`user $``lxc-create -t download -n proxy-maint`
Then for **Distribution**, choose *gentoo*; **Release**, choose *current*; and **Architecture**, choose *amd64*.

When it completes, start up the container with,

`user $``lxc-start -n proxy-maint`
Then configure a basic development environment using the [recommended tools](https://wiki.gentoo.org/wiki/Package_testing#Tools).

**`/etc/portage/sets/tools`**

`user $``lxc-console -n proxy-maint``root #``emerge --ask @tools`
To stop the container,

`user $``lxc-stop -n proxy-maint`
To clone the container for working on a particular ebuild,

`user $``lxc-copy -n proxy-maint -N "${PN}" # clones the original container`
To delete it when finished,

`user $``lxc-destroy -n "${PN}" # destroy the container when finished`
#### Using Kubler for ebuild testing

See [Kubler](https://wiki.gentoo.org/wiki/Kubler) for more info.

Kubler may be used to develop against any ebuild repository.

Create a new namespace, let's call it edev:

`user $``kubler new namespace edev`
Create a new builder, select `kubler/bob` as `IMAGE_PARENT`:

`user $``kubler new builder edev/bob`
Edit the new builder's build.sh and add any additional repositories:

**`/config/build.sh`**

```
() {
    # we overwrite this with a local host mount later, but this takes care of the initial overlay setup in the builder for us
    add_overlay kubler https://github.com/edannenberg/kubler-overlay.git
    # just for convenience
    echo 'cd /var/db/repos/kubler' >> ~/.bashrc
}
```
Create a new image, let's call it bench, use `kubler/bash` as `IMAGE_PARENT`:

`user $``kubler new image edev/bench`
Edit the new image's build.conf and configure the builder and ebuild repo to be mounted builder:

**`/config/build.conf`**

```
BUILDER="edev/bob"
BUILDER_MOUNTS=(
    "/data/development/ebuild-repos/gentoo:/var/db/repos/gentoo"
    "/data/development/ebuild-repos/kubler-overlay:/var/db/repos/kubler"
)
```
Start an interactive build container and get tinkering:

`user $``kubler build -i edev/bench``root #``ebuild dev-lang/foo/foo-0.4.0.ebuild manifest merge`
## See also

- [Incus/Gentoo Github pullrequest testing](https://wiki.gentoo.org/wiki/Incus/Gentoo_Github_pullrequest_testing) — easy and automated way for testing ebuild contributions via [Gentoo's Github mirror](https://github.com/gentoo/gentoo) that's based on [Incus](https://wiki.gentoo.org/wiki/Incus)
- [Package testing](https://wiki.gentoo.org/wiki/Package_testing) — provides information for ebuild developers on **testing ebuilds**.
- [Project:X86/Chroot Guide](https://wiki.gentoo.org/wiki/Project:X86/Chroot_Guide) — provides instructions on how to create a fresh Gentoo installation inside a [chroot](https://wiki.gentoo.org/wiki/Chroot) to assist in testing Gentoo packages for stabilization and for other sundry testing.
- [User:SwifT/Gensetup](https://wiki.gentoo.org/wiki/User:SwifT/Gensetup) — automates the installation of Gentoo on KVM guests for testing and development purposes.
