<!-- source: https://wiki.gentoo.org/wiki/Csync2 | group: Gentoo Wiki (Main) | wiki-title: Csync2 -->
---
title: Csync2
url: https://wiki.gentoo.org/wiki/Csync2
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-02-04"
fingerprint: "78376d4f1e899986"
license: CC BY-SA 4.0
---

# Csync2

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

csync2 is a tool for asynchronous file synchronization in clusters.

## Installation

### USE flags

Cannot load package information. Is the atom *sys-cluster/csync2* correct?

### Emerge

`root #``emerge --ask sys-cluster/csync2`
## Configuration

### Create ssl certificates for transfers

`root #``emerge --config sys-cluster/csync2`
### Create key for authentication with peers

`root #``csync2 -k /etc/csync2/csync2.key_mygroup`
### Setup synchronization

FILE **`/etc/csync2/csync2.cfg`**

```
group mygroup
{
	host hostname1 host2_ip (hostname3); # (hostname3) can only download changes. - #
	key /etc/csync2/csync2.key_mygroup;  # Identical file for all in group. - #
	include /mnt/shared;                 # Same list for all. - #
	exclude /mnt/shared/local*;
}
```
### Things to note

- All nodes (of a group) share a common key, for example /etc/csync2/csync2.key\_mygroup. This means it has to be **copied** to each node.
- The *shared* folder paths need to be identical for all nodes (of a group). The configuration file need *not* be identical.
- Although the `host` parameter requires the actual `hostname` (or `ip`) to be used, it seems to work according to the /etc/hosts file.
  - The following command shows what csync2 sees as hostname:
  - `root #``csync2 -Tvv |& head`

### Service

#### OpenRC

To enable the service to run at system boot:

`root #``rc-update add csync2 default`
To start the daemon now:

`root #``rc-service csync2 start`
## Usage

### Sample parameters

```
        -x [-d] [[-r] file..]   Run checks for all given files and update
                                remote hosts.
        -d      Dry-run on all remote update operations
        -P peer1,peer1,...
                Only update these peers (still mark all as dirty).
                Only show files for these peers in -o (compare) mode.
        -f [-r] file..          Force files to win next conflict resolution.
        -r      Recursive operation over subdirectories.
        -M      List all dirty files from status db.
```
### Invocation

- Update all with this one:
  - `root #``csync2 -x`
  - See failed changes:
    - `root #``csync2 -M`
- Dry-run an update towards `hostname3`:
  - `root #``csync2 -P hostname3 -xd`
  - Increased verbosity with `-vvv`.
- Force local files onto others:
  - `root #``csync2 -fr /mnt/shared/force_these``root #``csync2 -x`

#### Debug receiver

To see the messages of the receiver, after stopping any csync2 daemon, for a single execution:

`root #``csync2 -iii -vvv |& less`
## Troubleshooting

### remote host remote\_host did not accept my identification

If working on both sides of an iptables NAT and getting this error from the inner side:

`root #``csync2 -Tv this_host remote_host /path/to/file`
Connecting to host remote\_host (SSL) ...
Connect to remote\_ip:30865 (remote\_host).
ERROR: remote host remote\_host did not accept my identification.
Connection closed.
Finished with 2 errors.

It might be because of this command on the router (that translates inner node addresses to the router's):

`root #``iptables -t nat -A POSTROUTING -o ${WAN} -j MASQUERADE`
A solution to which, is to *prepend* the following, in order to exclude the `csync2` port:

`root #``iptables -t nat -A POSTROUTING -o ${WAN} -p TCP --dport csync2 -j RETURN`
### response from peer(/path/to/file): remote\_host \[15\] \<- Permission denied!

Verify that the file mentioned is included in the remote host's /etc/csync2/csync2.cfg.

## See also

- [git](https://wiki.gentoo.org/wiki/Git) — widely used, open source, distributed [version control system](https://wiki.gentoo.org/wiki/Version_control_systems)
- [rsync](https://wiki.gentoo.org/wiki/Rsync) — a powerful file sync program capable of efficient file transfers and directory synchronization.
