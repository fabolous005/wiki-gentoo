<!-- source: https://wiki.gentoo.org/wiki/Dsh | group: Gentoo Wiki (Main) | wiki-title: Dsh -->
---
title: dsh
url: https://wiki.gentoo.org/wiki/Dsh
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-12-27"
fingerprint: f618584e83b19fe1
license: CC BY-SA 4.0
---

# dsh

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**dsh** is a shell that allows parallel execution of remote commands across large numbers of servers. Generally called "distributed shell" dsh was often leveraged as an orchestration tool prior to the rise of modern alternatives. Even to this day, dsh is still used by some system administrators where heavyweight tools such as [Ansible](https://wiki.gentoo.org/wiki/Ansible) are impractical or inefficient.

## Installation

### USE flags


### Emerge

`root #``emerge --ask app-shells/dsh`
## Configuration

### Environment variables

- `$DSH_FANOUT` - the maximum number of concurrent remote shell commands
- `$DSH_REMOTE_CMD` - used to specify a remote shell other than the default [rsh](https://wiki.gentoo.org/index.php?title=Rsh&action=edit&redlink=1), typically [ssh](https://wiki.gentoo.org/wiki/Ssh).
- `$DSH_NODE_LIST` - the location of the host list.
- `$WCOLL` - (deprecated) the location of the host list.
- `$DSH_PATH` - the default path for remote command execution, `$PATH` by default.

### Files

#### Global Configuration Files

- /etc/dsh/machines.list - the list of machine names to be used for when `-a` command-line option is specified.
- /etc/dsh/group/\<group\_name> - the list of machine names to be used for when `-g` group name command-line option is specified.
- /etc/dsh/dsh.conf - the configuration file containing the day-to-day default.

#### User Specific Configuration Files

- $HOME/.dsh/machines.list - the list of machine names to be used for when `-a` command-line option is specified.
- $HOME/.dsh/group/\<group\_name> - the list of machine names to be used for when `-g` group name command-line option is specified.
- $HOME/.dsh/dsh.conf - the configuration file containing the day-to-day default.

## Usage

First, tell dsh where to find the server list:

`user $``DSH_NODE_LIST=~/<server.list>`
Second, tell dsh what command to run:

`user $``dsh -v '<remote_command>'`
### Invocation

`user $``dsh --help`
Distributed Shell / Dancer's shell version 0.25.10 
Copyright 2001-2005 Junichi Uekawa, 
distributed under the terms and conditions of GPL version 2
-v --verbose                   Verbose output
-q --quiet                     Quiet
-M --show-machine-names        Prepend the host name on output
-H --hide-machine-names        Do not prepend host name on output
-i --duplicate-input           Duplicate input given to dsh
-b --bufsize                   Change buffer size used in input duplication
-m --machine \[machinename\]     Execute on machine
-n --num-topology              How to divide the machines
-a --all                       Execute on all machines
-g --group \[groupname\]         Execute on group member
-f --file \[file\]               Use the file as list of machines
-r --remoteshell \[shellname\]   Execute using shell (rsh/ssh)
-o --remoteshellopt \[option\]   Option to give to shell 
-h --help                      Give out this message
-w --wait-shell                Sequentially execute shell
-c --concurrent-shell          Execute shell concurrently
-F --forklimit \[fork limit\]    Concurrent with limit on number
-V --version                   Give out version information
