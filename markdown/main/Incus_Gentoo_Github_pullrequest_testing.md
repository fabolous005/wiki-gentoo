<!-- source: https://wiki.gentoo.org/wiki/Incus/Gentoo_Github_pullrequest_testing | group: Gentoo Wiki (Main) | wiki-title: Incus/Gentoo Github pullrequest testing -->
---
title: Incus/Gentoo Github pullrequest testing
url: https://wiki.gentoo.org/wiki/Incus/Gentoo_Github_pullrequest_testing
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-13"
fingerprint: "9d03607429851faf"
license: CC BY-SA 4.0
---

# Incus/Gentoo Github pullrequest testing

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

An easy and automated way for testing ebuild contributions via [Gentoo's Github mirror](https://github.com/gentoo/gentoo) that's based on [Incus](https://wiki.gentoo.org/wiki/Incus).

**Summary**: You keep a base container updated and in a tidy condition. You make copies of the base container, use the copies to test ebuild contributions, and delete the copies when you're done. This is automated as much as possible; in an ideal situation all you ever have to do, is give one command.

| Pros | Description | 
|---|---|
| \* safe (unprivileged) | No root privileges or access required. | 
| \* fully isolated environment | Play / test as much as you like, your live system won't break. | 
| \* runtime testing | Not just build-test, but run GUI applications too. | 
| \* discardable snapshots (fast with binpkgs & SSD/NVME) | No need to constantly run emerge --depclean in your test system. | 
| \* fast set-up for single-time use | If you simply need a container for one-time testing, it's usable in less than 5 minutes. [But there are better alternatives for that.](https://packages.gentoo.org/packages/dev-util/ebuildtester) | 
| \* use emerge against ::gentoo repository | Using emerge is much better than using ebuild to test your work. | 
| \* OpenRC \<-> systemd support | Need to test systemd environment, but you use OpenRC? [Incus can do that](https://wiki.gentoo.org/wiki/Incus#systemd_containers_on_OpenRC_host). | 
| \* testing multiple PRs in parallel | Launch as many build tests as you wish, and they won't interfere with each other. | 
| \* limit resources | Apply per-container rules for maximum amount of CPU cores, RAM, or CPU usage. | 
| \* need a musl system? | Or a clean environment to test -native-symlinks, clang, etc. | 

| Cons | Description | 
|---|---|
| \* this exact environment takes a while to set-up | But for continuous usage it's very resilient and takes little maintenance. | 
| \* I/O and storage heavy | Truly negatable when using SSD/NVME. | 
| \* lots of automation can break | Sometimes manual intervention is required, but rarely. | 
| \* prior knowledge of LXD/Incus | It helps if you've used [LXD](https://wiki.gentoo.org/wiki/LXD) or [Incus](https://wiki.gentoo.org/wiki/Incus) before. | 
| \* prior knowledge of git | It helps if you've used git before. | 

## Installation

We're going to need [Incus](https://wiki.gentoo.org/wiki/Incus) to be installed in the host system, and configured. Using `incus-user` is the recommended way.

You should expect to have at least 2-4 GB space per container, especially if they pull rust-bin, gentoo-kernel-bin etc. Therefore it might be wise to **mount --bind** /var/lib/incus somewhere with more space available, or have /var(/lib) in a separate partition. Using an **SSD/NVME** is heavily suggested, as it makes all the operations happen in an instant compared to HDDs. Note that upstream suggests using **mount --bind** rather than symlinking a location. Obviously the base-size can be trimmed by choosing pre-installed packages differently.

## Configuring Incus

Please follow the [Incus](https://wiki.gentoo.org/wiki/Incus) page if this is your first time installing Incus. You should make sure Incus is working, by launching a test container and logging into it. It's recommended to initialize and use `incus-user`.

In this example we are [building and sharing](https://wiki.gentoo.org/wiki/Incus#Sharing_files.2Fdirectories_between_container_and_host) our own binpkgs. The [official binpkg host](https://www.gentoo.org/news/2023/12/29/Gentoo-binary.html) can also be configured to be used.



## Set up your container

### Getting the correct container image

Make sure to choose an image suited for your testing needs. Alternatively, create your own with [Distrobuilder#Create\_your\_own\_container\_images](https://wiki.gentoo.org/wiki/Distrobuilder#Create_your_own_container_images).

`user $````
incus image list images:gentoo
```
`user $``incus launch images:gentoo/openrc my-gentoo-gh-test-container`
This will download a default gentoo-x86\_64 container image, set it up, name it as **my-gentoo-gh-test-container** and start it. It's possible to simply check that it works,

`user $````
incus list
```
`user $``incus exec my-gentoo-gh-test-container bash`
But for now, it's better to turn it off so it can configured it properly.

`my-gentoo-gh-test-container #``logout` `user $``incus stop my-gentoo-gh-test-container`
### Edit the container config

#### default profile

You can edit the 'default' profile which, by default, gets used by containers and contain some necessary options.

`user $``incus profile edit default`
Especially networking needs to work on each container launched with this profile.

Use a different profile specifically for testing Github PRs if you don't want to mix `default` with that.

#### Container config

##### Sharing disk location from host on to the container

First check [Incus#Sharing files/directories between container and host](https://wiki.gentoo.org/wiki/Incus#Sharing_files.2Fdirectories_between_container_and_host) to do get the basic configuration right. Below there are specific examples to get binpkg and distfile sharing done.

Edit the config file of your container.

`user $``incus config edit my-gentoo-gh-test-container````
devices:
  binpkgdir:
    path: /var/cache/binpkgs
    source: /home/larry/sde1/binpkg_incus_mtc
    type: disk
  distfiles:
    path: /var/cache/distfiles/
    source: /var/cache/distfiles/
    type: disk
profiles:
- default
```
With the example above, we're sharing our host's distfiles and binpkg directories with the containers. `incus-user` will automatically apply `raw.idmap` values for your current user onto the profile, which should give read/write access onto the source directories in host.

If you get a message saying "Disk source path not allowed", check [Incus's troubleshooting](https://wiki.gentoo.org/wiki/Incus#Adding_a_disk_device_with_incus-user_fails_to_.22Disk_source_path_not_allowed.22) for that.

Make sure your user is in portage group to share your distfiles location between all containers and host!

##### Launching GUI apps from the container

Check the [Incus#X and Wayland apps in a container](https://wiki.gentoo.org/wiki/Incus#X_and_Wayland_apps_in_a_container) main wiki page for that.

### Launching the container

Now that the relevant parts are updated, the base container can be built. The base is kept up-to-date and clean. You're going to create discardable copies where actual testing happens.

`user $````
incus start my-gentoo-gh-test-container
```
`user $``incus exec my-gentoo-gh-test-container bash`
#### Initiate configuration

First thing, grab a portage tree. Either by issuing

`my-gentoo-gh-test-container #``emerge-webrsync`
Or alternatively, wget the latest portage tree from [any of the mirror's](https://www.gentoo.org/downloads/mirrors/) /snapshots/portage-latest.tar.xz and extract it as /var/db/repos/gentoo.

Install [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git) and an editor of your choice:

`my-gentoo-gh-test-container #``emerge -av dev-vcs/git app-editors/emacs app-editors/vim`
Now initiate the git-repo, and /etc/portage/ configuration for our container. There are two ways for it:

##### Automatic configuration via a script

A fast and simple way is to use this script:

`my-gentoo-gh-test-container #``cd /root``my-gentoo-gh-test-container #``wget` [https://raw.githubusercontent.com/juippis/incus-gentoo-github-pullrequest-tester/master/container/bin/initialize_my-gentoo-gh-test-container.sh](https://raw.githubusercontent.com/juippis/incus-gentoo-github-pullrequest-tester/master/container/bin/initialize_my-gentoo-gh-test-container.sh)
`my-gentoo-gh-test-container #````
chmod +x initialize_my-gentoo-gh-test-container.sh
```
`my-gentoo-gh-test-container #``./initialize_my-gentoo-gh-test-container.sh`
When done, skip straight to [updating your container](https://wiki.gentoo.org/wiki/Incus/Gentoo_Github_pullrequest_testing#Update_your_container).

##### Alternatively, manual configuration

Prepare the usage of git sync-repo.

`my-gentoo-gh-test-container #``cd /etc/portage``my-gentoo-gh-test-container #````
mkdir repos.conf
```
`my-gentoo-gh-test-container #````
edit /etc/portage/repos.conf/gentoo.conf
```
**`/etc/portage/repos.conf/gentoo.conf`**

**Container**

`my-gentoo-gh-test-container #``rm -rf /var/db/repos/gentoo/*``my-gentoo-gh-test-container #````
emerge --sync
```
`my-gentoo-gh-test-container #````
eselect news read
```
Edit relevant /etc/portage files. Take note from [https://github.com/juippis/incus-gentoo-github-pullrequest-tester/tree/master/container/etc/portage](https://github.com/juippis/incus-gentoo-github-pullrequest-tester/tree/master/container/etc/portage)

`my-gentoo-gh-test-container #``cd /etc/portage``my-gentoo-gh-test-container #````
mkdir -p env package.accept_keywords package.env package.unmask package.use profile /var/tmp/portage/vbslogs
```
Edit all the relevant files you need to. make.conf, make.conf.custom, package.use/default, package.accept\_keywords/default, package.mask/default, env/test.conf etc.

Always find the latest container /etc/portage config files from [https://github.com/juippis/incus-gentoo-github-pullrequest-tester/tree/master/container/etc/portage](https://github.com/juippis/incus-gentoo-github-pullrequest-tester/tree/master/container/etc/portage) and use wget to get them.

If you wish to git pull only the shell scripts used in testing pull requests, and not /etc/portage files, follow these steps:

`my-gentoo-gh-test-container #``cd /root``my-gentoo-gh-test-container #``git clone --depth=1` [https://github.com/juippis/incus-gentoo-github-pullrequest-tester.git](https://github.com/juippis/incus-gentoo-github-pullrequest-tester.git) --no-checkout incus-gentoo-gh
`my-gentoo-gh-test-container #````
cd incus-gentoo-gh/
```
`my-gentoo-gh-test-container #````
git config core.sparsecheckout true
```
`my-gentoo-gh-test-container #````
echo container/bin/* > .git/info/sparse-checkout
```
`my-gentoo-gh-test-container #````
git read-tree -m -u HEAD
```
`my-gentoo-gh-test-container #``echo "PATH=\"\$PATH:~/incus-gentoo-gh/container/bin\"" >> /root/.bashrc`
#### Binhost / binpkgs

With the above settings we are enabling binpkgs to be generated, and used, for everything other except packages we're actually testing. This will speed up testing process immensively. Make sure your user has correct permissions in your host to be able to write to shared binpkg directory.



#### Update your container

Start the update. Remember to switch profiles here if you need to.

`my-gentoo-gh-test-container #``emerge -uavDN @world``my-gentoo-gh-test-container #````
perl-cleaner --all
```
`my-gentoo-gh-test-container #````
eselect news read
```
`my-gentoo-gh-test-container #````
dispatch-conf
```
Let's use the latest gcc.

`my-gentoo-gh-test-container #``gcc-config -l``my-gentoo-gh-test-container #````
gcc-config 2
```
`my-gentoo-gh-test-container #````
source /etc/profile
```
#### Install necessary tools

`my-gentoo-gh-test-container #``emerge -av app-admin/eclean-kernel app-editors/emacs app-emacs/ebuild-mode app-editors/vim app-portage/gentoolkit app-portage/pfl app-portage/portage-utils dev-util/pkgdev app-text/ansifilter dev-util/pkgcheck sys-kernel/gentoo-kernel-bin app-portage/pkg-testing-tools app-portage/iwdevtools net-misc/dhcp`
#### Container-specific modifications to pkg-testing-tools

This step is optional, and it's recommended to mask troublesome use flags via /etc/portage/profile/package.use.mask/ instead.

Our testing depends **heavily** on [https://github.com/APN-Pucky/pkg-testing-tools](https://github.com/APN-Pucky/pkg-testing-tools) - to "configure" it for specific cases, we'll have to patch it directly utilizing /etc/portage/patches/.

Get the container-specific patches from here: [https://github.com/juippis/incus-gentoo-github-pullrequest-tester/tree/master/patches](https://github.com/juippis/incus-gentoo-github-pullrequest-tester/tree/master/patches)

For example for general pull request testing, against =app-portage/pkg-testing-tools-0.2.0, we'd do:

`my-gentoo-gh-test-container #``mkdir -p /etc/portage/patches/app-portage/pkg-testing-tools-0.2.0/``my-gentoo-gh-test-container #``wget` [https://raw.githubusercontent.com/juippis/incus-gentoo-github-pullrequest-tester/master/patches/app-portage/pkg-testing-tools-0.2.0/pkg-testing-tool-0.2.0-toggle-more-flags.patch](https://raw.githubusercontent.com/juippis/incus-gentoo-github-pullrequest-tester/master/patches/app-portage/pkg-testing-tools-0.2.0/pkg-testing-tool-0.2.0-toggle-more-flags.patch) -O /etc/portage/patches/app-portage/pkg-testing-tools-0.2.0/pkg-testing-tool-0.2.0-toggle-more-flags.patch
`my-gentoo-gh-test-container #``emerge -av app-portage/pkg-testing-tools --usepkg=n`
#### Set up container scripts

If you've followed the steps above, your container should have /root/incus-gentoo-gh/container/bin/ directory with contents from [https://github.com/juippis/incus-gentoo-github-pullrequest-tester/tree/master/container/bin](https://github.com/juippis/incus-gentoo-github-pullrequest-tester/tree/master/container/bin) - if you didn't use initialize\_my-gentoo-gh-test-container.sh add this $PATH to your container's /root/.bashrc:

**`/root/.bashrc & /root/.profile`**

**Container**

```
PATH="$PATH:~/incus-gentoo-gh/container/bin"
/root/incus-gentoo-gh/container/bin/fixshm.sh
```
#### Finishing touches for your container

Do a world update and depclean remnants after editing our /etc/portage. Then run pfl to submit the initial list, and from now on running pfl from a snapshot-container or main container will only submit modified package list when comparing to base image.

`my-gentoo-gh-test-container #``emerge --update --newuse --deep @world``my-gentoo-gh-test-container #````
emerge -a --depclean --with-bdeps=n
```
`my-gentoo-gh-test-container #``pfl`
Log out, turn off container.

`my-gentoo-gh-test-container #``logout`
(**Ctrl+d** works too)

Add [app-portage/iwdevtools](https://packages.gentoo.org/packages/app-portage/iwdevtools)'s fantastic \*\*rcd\*\* function to your containers:

**`~/.bashrc`**

`user $``incus stop my-gentoo-gh-test-container`
## Set up host scripts

Note: Edit the host scripts to your needs. Stuff like pkgdiff-mg can be incus file pushed to the container.

**`~/bin/local-container_incus_start_gentoo-gh-prtester.sh`**

### Automatic way via git sync

`user $````
mkdir -p ~/bin/incus-gentoo-gh-scripts
```
`user $````
cd ~/bin/incus-gentoo-gh-scripts
```
`user $``git clone --depth=1` [https://github.com/juippis/incus-gentoo-github-pullrequest-tester.git](https://github.com/juippis/incus-gentoo-github-pullrequest-tester.git) --no-checkout incus-gentoo-gh-bin
`user $````
cd incus-gentoo-gh-bin/
```
`user $````
git config core.sparsecheckout true
```
`user $````
echo host/bin/* > .git/info/sparse-checkout
```
`user $````
git read-tree -m -u HEAD
```
`user $````
echo "PATH=\"\$PATH:~/bin/incus-gentoo-gh-scripts/incus-gentoo-gh-bin/host/bin\"" >> ~/.bashrc
```
You should now have the contents of [https://github.com/juippis/incus-gentoo-github-pullrequest-tester/tree/master/host/bin](https://github.com/juippis/incus-gentoo-github-pullrequest-tester/tree/master/host/bin) in your host's user dir \~/bin/incus-gentoo-gh-scripts/incus-gentoo-gh-bin/host/bin. git pull occassionally to get updates.

Relog or invoke bash to get $PATH updated.

### Manually by wgetting

Just take a look at [https://github.com/juippis/incus-gentoo-github-pullrequest-tester/tree/master/host/bin](https://github.com/juippis/incus-gentoo-github-pullrequest-tester/tree/master/host/bin) and wget / copy-paste manually to your host user's bin dir. Add the script dir to your $PATH via \~/.bashrc **and/or** \~/.profile, relog or invoke bash to get $PATH updated.



### Cron job to keep container up-to-date automatically

Use your desired cron system to make the host's maintenance script a cron job. In other words, \~/bin/incus-gentoo-gh-scripts/incus-gentoo-gh-bin/host/bin/container\_incus\_maintain\_gentoo-gh-prtester\_.sh can be ran to keep the base image updated.



### Individual scripts explained

#### Host

##### container\_incus\_maintain\_gentoo-gh-prtester.sh

Script that attempts to gather all container's base maintenance tasks. Can be cron'd or called manually before each "test session".

##### container\_incus\_start\_gentoo-gh-prtester.sh

Simple helper script to do manual tasks in the **base** container. Rarely used.

##### container\_incus\_start\_local-tmptestcont.sh

Sets up a generic testing environment fast. You can use this for personal stuff.

##### test-gentoo-gh-pr.sh

Gets a Github pull request as a parameter, and sets up a new environment to test it. Attempts to include every necessary step to confirm that the PR builds.

#### Container

##### disable-native-symlinks.sh

Quick and dirty script to disable native-symlinks. Ideally this gets called via test-gentoo-gh-pr.sh -n 12345.

##### gentoo\_pkg\_errors\_and\_qa\_notices.sh

Just an error grepper. Spams the screen for any pre-defined checks.

##### fixshm.sh

Required for some openrc systems to populate devices inside container.

##### fullget-gentooghpr.sh

Gets the Github pull request and applies it onto ::gentoo tree. In future, any git-format patch should be appliable.

##### initialize\_my-gentoo-gh-test-container.sh

Designed to be ran one-time only whenever a new container is launched. Makes boring tasks easier.

##### prtester.sh

Identifies .ebuild files that have changed against git-tree's HEAD state, and proceeds to test them.

##### gentoo-pkg-rdeptester.sh

Give package as a parameter and it tests reverse dependencies of given package. By default it picks 6 rdeps, but with an -a flag it tests them **all**. Good for checking library updates which don't have test phase enabled.

## Testing that everything works!

Choose any PR to your liking from [https://github.com/gentoo/gentoo/pulls](https://github.com/gentoo/gentoo/pulls). Do note that it can't be CLOSED, in other words, merged to main tree as that will cause patch merging collision. And it should edit an .ebuild file, ideally adding a new .ebuild to the tree to be tested.

`user $``test-gentoo-gh-pr.sh 12345`
You can always, at any given time log in to the container and inspect / test everything manually. This is especially handy if you spot a mistake in the .ebuild file and need to test that your modification works.

`user $````
incus list
```
`user $````
incus start my-gentoo-gh-test-container-snap-12345
```
`user $``incus exec my-gentoo-gh-test-container-snap-12345 bash`


### Examples

A couple of real-life examples.

TODO
