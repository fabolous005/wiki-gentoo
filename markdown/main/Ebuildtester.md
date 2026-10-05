<!-- source: https://wiki.gentoo.org/wiki/Ebuildtester | group: Gentoo Wiki (Main) | wiki-title: Ebuildtester -->
---
title: ebuildtester
url: https://wiki.gentoo.org/wiki/Ebuildtester
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-27"
fingerprint: "3ea4d43a15253f51"
license: CC BY-SA 4.0
---

# ebuildtester

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**ebuildtester** is a [Python](https://wiki.gentoo.org/wiki/Python) script to help automate parts of the [ebuild](https://wiki.gentoo.org/wiki/Ebuild) testing process by generating [Docker](https://wiki.gentoo.org/wiki/Docker) containers that replicate fresh Gentoo installations.

ebuildtester compiles a docker container holding the current Gentoo [stage3](https://wiki.gentoo.org/wiki/Stage3), allowing testing in a clean environment.

This environment is configured by invoking ebuildtester with appropriate command-line parameters. On execution, ebuildtester either installs the specified package, or puts the user into a shell, inside the container.

## Installation

### USE flags


### Emerge

`root #``emerge --ask dev-util/ebuildtester`
## Usage

`user $``ebuildtester --help````
usage: ebuildtester [-h] [--version] [--atom ATOM [ATOM ...]] [--binhost BINHOST] [--live-ebuild]
                    [--manual] --portage-dir PORTAGE_DIR [--overlay-dir OVERLAY_DIR] [--update]
                    [--install-basic-packages] [--threads N] [--use USE [USE ...]]
                    [--global-use GLOBAL_USE [GLOBAL_USE ...]] [--unmask ATOM] [--unstable] [--gcc-version VER]
                    [--python-single-target PYTHON_SINGLE_TARGET] [--python-targets PYTHON_TARGETS]
                    [--rm] [--storage-opt STORAGE_OPT [STORAGE_OPT ...]] [--with-X] [--with-vnc]
                    [--profile PROFILE] [--features FEATURES [FEATURES ...]] [--docker-image DOCKER_IMAGE]
                    [--docker-command DOCKER_COMMAND] [--pull] [--show-options]
                    [--ccache CCACHE_DIR] [--batch] [--debug]
A dockerized approach to test a Gentoo package within a clean stage3.
options:
  -h, --help            show this help message and exit
  --version             show program's version number and exit
  --atom ATOM [ATOM ...]
                        The package atom(s) to install
  --binhost BINHOST     Binhost URI
  --live-ebuild         Unmask the live ebuild of the atom
  --manual              Install package manually
  --portage-dir PORTAGE_DIR
                        The local portage directory
  --overlay-dir OVERLAY_DIR
                        Add overlay dir (can be used multiple times)
  --update              Update container before installing atom
  --install-basic-packages
                        Install basic packages after container starts
  --threads N           Use N (default 20) threads to build packages
  --use USE [USE ...]   The use flags for the atom
  --global-use GLOBAL_USE [GLOBAL_USE ...]
                        Set global USE flag
  --unmask ATOM         Unmask atom (can be used multiple times)
  --unstable            Globally 'unstable' system, i.e. ~amd64
  --gcc-version VER     Use gcc version VER
  --python-single-target PYTHON_SINGLE_TARGET
                        Specify a PYTHON_SINGLE_TARGET
  --python-targets PYTHON_TARGETS
                        Specify a PYTHON_TARGETS
  --rm                  Remove container after session is done
  --storage-opt STORAGE_OPT [STORAGE_OPT ...]
                        Storage driver options for all volumes (same as Docker param)
  --with-X              Globally enable the X USE flag
  --with-vnc            Install VNC server to test graphical applications
  --profile PROFILE     The profile to use (default = default/linux/amd64/23.0)
  --features FEATURES [FEATURES ...]
                        Set FEATURES in Gentoo Wiki (default = ['-sandbox', '-usersandbox', 'userfetch'])
  --docker-image DOCKER_IMAGE
                        Specify the docker image to use (default = gentoo/stage3)
  --docker-command DOCKER_COMMAND
                        Specify the docker command
  --pull                Download latest docker image
  --show-options        Show currently selected options and defaults
  --ccache CCACHE_DIR   Path to mount that contains ccache cache
  --batch               Do not drop into interactive shell
  --debug               Add some debugging output
```
An example command for reference could look like this:

## Troubleshooting

### No network in the container

There might be several reasons explaining why the network doesn’t works inside the container.

#### Host running with NFTables block forwarding packets for the container(s)

Forwarding packets is needed for Docker to works.

**Docker works along IPTables and does not support NFTables** (officially, but this is a [work-in-progress](https://github.com/docker/for-linux/issues/1472)).

It could works, but running these two firewalls together in this scenario is not recommended because it could make the system(s) unresponsive(s) to the network (host, container or both), as it requires some tweaking to allow IPTables to get the required packets.

For now there is no official way to properly make them works together. Docker’s IPTables rule set needs to be able to forward packets to the proper network interface and if NFTable is dropping the packets before hands, it won’t works and the container will have no network access.

##### Dirty workaround

A few solutions exist, but none are perfects.

This is a decision that should be made by the administrator, regarding the needs to protect with a firewall the machine or simply what tools suits best the user-case:

1. Drop or flush the forwarding rules for NFTables, if any, while using docker/ebuildtester.
2. Replace NFTables with IPTables and let docker/ebuildtester add the needed rules when using it.
3. **DANGEROUS!** Disabling NFTables while using docker/ebuildtester.
4. Running the container on a machine that does not need any firewall.
5. Using any other containers management system that does not rely on IPTables.


**(1)** Allow the administrator to keep most of the actual (and working) configuration and require simply a command before and after running ebuildtester.

Keeping in mind that the system won’t drop anymore the forwarding packets is important, *which could be seen as a security issue*.

It depends on the administrator to estimate if it is worth it.

It is a risk, but all the other rules for filtering are still actives. This is needed when having more than one interface (which is needed by docker). If the containers have to be working all the time, that probably means the forwarding rules should be disabled for good (or until the containers are not needed anymore).

See [NFTables documentation](https://wiki.nftables.org/wiki-nftables/index.php/Configuring_tables) and the [Nftables](https://wiki.gentoo.org/wiki/Nftables) wiki page about it for more details.

**(2)** It would require some works, translating rules from a firewall to another. The good part of this solution is probably that Docker needs IPTables and it won’t be an issue anymore, at least until the project add support for NFTables. See the [Iptables](https://wiki.gentoo.org/wiki/Iptables) wiki page about it.

**(3)** The third is also the worst one (**DANGEROUS**), because all the rule sets are disabled which left the system reachable on any ports worldwide, unless the network’s router already block them.

**(4)** Allow the administrator to easily get around the issue, without actually solving it.

**(5)** If not afraid to start over with a new tools to create container. This means also the use of ebuildtester is impossible.

Docker project is working on an implementation of NFTables for the moment, which could in the future solve this issue. See this [opened issue #1472 from the project](https://github.com/docker/for-linux/issues/1472) for more details.

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose dev-util/ebuildtester`
## See also

- [Package testing](https://wiki.gentoo.org/wiki/Package_testing) — provides information for ebuild developers on **testing ebuilds**.
- [Test environment](https://wiki.gentoo.org/wiki/Test_environment)
