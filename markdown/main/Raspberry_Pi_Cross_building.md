<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi/Cross_building | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi/Cross building -->
---
title: Raspberry Pi/Cross building
url: https://wiki.gentoo.org/wiki/Raspberry_Pi/Cross_building
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-23"
fingerprint: "4c68327a17542007"
license: CC BY-SA 4.0
---

# Raspberry Pi/Cross building

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**2025-11-23**, the information in this article is probably

**outdated**. You can help the Gentoo community by verifying and

[updating this article](https://wiki.gentoo.org/index.php?title=Raspberry_Pi/Cross_building&action=edit).

Building almost any software on embedded SoC computers such as the [Raspberry Pi](https://wiki.gentoo.org/wiki/Raspberry_Pi) can take a very, very long time - especially when many dependencies are involved. Fortunately, it is possible to offload much of the heavy lifting for compilation to a more powerful build system (such as a Gentoo desktop/server with more compute cores and more memory) using distcc and crossdev. This time saving tactic is only possible for packages that are written in language that require compiling (such as C, C++, etc.).

## distcc

It is also suggested that the first package you build on the Raspberry Pi should be distcc, as it will dramatically speed up subsequent packages that require a lot of compilation.

### Raspberry Pi and build server(s)

On the Raspberry Pi and on all build servers install [sys-devel/distcc](https://packages.gentoo.org/packages/sys-devel/distcc):

`root #``emerge --ask sys-devel/distcc`
#### OpenRC

Edit the distcc config file to ensure it is on the right subnet for the network configuration. For example:

**`/etc/conf.d/distccd`**

#### Systemd

When using systemd edit the config file for the systemd service:

**`/etc/systemd/system/distccd.service.d/00gentoo.conf`**

Then register and start the distcc daemon:

`root #````
rc-update add distccd default
```
`root #````
rc-config start distccd
```
Or for systemd:

`root #````
systemctl enable distccd
```
`root #````
systemctl start distccd
```
### Raspberry Pi only

Tell Portage to use distcc:

**`/etc/portage/make.conf`**

(Optional) Also add `buildpkg` to the `FEATURES` variable to tell the Raspberry Pi to build package files for everything it builds (if you want to use the same setup on multiple Raspberry Pis without recompiling).


Edit the distcc host file to tell your Raspberry Pi to submit compile jobs to the server:

**`/etc/distcc/hosts`**

Now you will need to tell distcc the specific compiler name to use instead of just "gcc":[\[1\]](https://wiki.gentoo.org#cite_note-1)

`root #````
cd /usr/lib/distcc/bin
```
`root #``ls -l`
total 0
lrwxrwxrwx  1 root root 15 Dec 23 20:13 c++ -> /usr/bin/distcc
lrwxrwxrwx  1 root root 15 Dec 23 20:13 cc -> /usr/bin/distcc
lrwxrwxrwx  1 root root 15 Dec 23 20:13 g++ -> /usr/bin/distcc
lrwxrwxrwx  1 root root 15 Dec 23 20:13 gcc -> /usr/bin/distcc
lrwxrwxrwx  1 root root 15 Dec 23 20:13 armv6j-unknown-linux-gnueabihf-c++ -> /usr/bin/distcc
lrwxrwxrwx  1 root root 15 Dec 23 20:13 armv6j-unknown-linux-gnueabihf-g++ -> /usr/bin/distcc
lrwxrwxrwx  1 root root 15 Dec 23 20:13 armv6j-unknown-linux-gnueabihf-gcc -> /usr/bin/distcc

We need to replace those symlinks with the following script:

**`/usr/lib/distcc/bin/armv6j-unknown-linux-gnueabihf-wrapper`**

**Distcc crossdev wrapper file**

```
#!/bin/bash
exec /usr/lib/distcc/bin/armv6j-unknown-linux-gnueabihf-g${0:$[-2]} "$@"
```
`root #````
rm c++ g++ gcc cc
```
`root #````
chmod a+x /usr/lib/distcc/bin/armv6j-unknown-linux-gnueabihf-wrapper
```
`root #````
ln -s armv6j-unknown-linux-gnueabihf-wrapper cc
```
`root #````
ln -s armv6j-unknown-linux-gnueabihf-wrapper gcc
```
`root #````
ln -s armv6j-unknown-linux-gnueabihf-wrapper g++
```
`root #````
ln -s armv6j-unknown-linux-gnueabihf-wrapper c++
```
Double check that you did things right:

`root #``ls -l`
total 4
lrwxrwxrwx 1 root root 15 Dec  8 00:38 armv6j-unknown-linux-gnueabihf-c++ -> /usr/bin/distcc
lrwxrwxrwx 1 root root 15 Dec  8 00:38 armv6j-unknown-linux-gnueabihf-g++ -> /usr/bin/distcc
lrwxrwxrwx 1 root root 15 Dec  8 00:38 armv6j-unknown-linux-gnueabihf-gcc -> /usr/bin/distcc
-rwxr-xr-x 1 root root 85 Dec  8 01:04 armv6j-unknown-linux-gnueabihf-wrapper
lrwxrwxrwx 1 root root 38 Dec  8 01:05 c++ -> armv6j-unknown-linux-gnueabihf-wrapper
lrwxrwxrwx 1 root root 38 Dec  8 01:04 cc -> armv6j-unknown-linux-gnueabihf-wrapper
lrwxrwxrwx 1 root root 38 Dec  8 01:04 g++ -> armv6j-unknown-linux-gnueabihf-wrapper
lrwxrwxrwx 1 root root 38 Dec  8 01:04 gcc -> armv6j-unknown-linux-gnueabihf-wrapper

You can now check what is being dispatched to your build machines while doing an emerge operation:

`root #``DISTCC_DIR="/var/tmp/portage/.distcc/" distccmon-text 1`
## crossdev

This will setup crossdev on your build servers so that they can compile binaries compatible with the Raspberry Pi. Note that you can have multiple compilation nodes - just add them to the list of hosts on the Raspberry Pi. Distcc will decide when to distribute the compilation though, so chances are you will never be able to fully load even a single modern build server with jobs from the Raspberry Pi.

Install [sys-devel/crossdev](https://packages.gentoo.org/packages/sys-devel/crossdev):

`root #``emerge --ask sys-devel/crossdev`
You will need to maintain separate portage profiles for the Raspberry Pi and your server's default, so you must convert your existing profile files to folders.

Copy the following file to \~/convert-profile-to-files.sh, and then run it as root:

**`~/convert-profile-to-files.sh`**

**Convert profile files to folders**

```
#!/bin/bash
PROFILE_DIR="/etc/portage"
if [ ! -e ${PROFILE_DIR} ]; then
  mkdir ${PROFILE_DIR};
fi;
for PACK_DIR in package.env package.accept_keywords package.keywords package.use package.unmask package.mask; do
  CUR_DIR="${PROFILE_DIR}/${PACK_DIR}"
  if [ ! -e ${CUR_DIR} ]; then
    mkdir ${CUR_DIR}
  fi
  if [ -e ${CUR_DIR} -a ! -d ${CUR_DIR} ]; then
    mv ${CUR_DIR} ${CUR_DIR}.moving
    mkdir ${CUR_DIR}
    mv ${CUR_DIR}.moving ${CUR_DIR}/monolithic
  fi
done
echo "Completed!"
```
Create a cross toolchain for ARM: (drop the `-S` option from the command when planning to run an unstable system):

`root #``crossdev -S -t armv6j-unknown-linux-gnueabihf`
If the cross toolchain for ARM fail to build try this:

`root #``CFLAGS="-O2 -pipe" CXXFLAGS="${CFLAGS}" crossdev -S -t armv6j-unknown-linux-gnueabihf`
### crossdev with musl

Pie needs to be enabled for gcc on the build-server(s), otherwise the crossbuilt packages will segfault after throwing a `warning: creating DT_TEXTREL in a PIE` during the build phase.

`root #``crossdev --genv 'USE="pie ssp"' -S -t armv6j-unknown-linux-musleabihf` After compilation confirm similar flags of your gcc package on build-server(s) & raspberry pi

`root #``/usr/bin/armv6j-unknown-linux-musleabihf-gcc -v` For testing use the following suggestion from: [the testing section of Distcc wikipage](https://wiki.gentoo.org/wiki/Distcc#Testing)

## See also

- [distcc](https://wiki.gentoo.org/wiki/Distcc) — a program designed to distribute compiling tasks across a network to participating hosts.
- [Distcc/Cross-Compiling](https://wiki.gentoo.org/wiki/Distcc/Cross-Compiling) — shows the reader how to set up distcc for cross-compiling across different processor architectures.
