<!-- source: https://wiki.gentoo.org/wiki/Namespaces | group: Gentoo Wiki (Main) | wiki-title: Namespaces -->
---
title: Namespaces
url: https://wiki.gentoo.org/wiki/Namespaces
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-29"
fingerprint: fb8499b158d9180
license: CC BY-SA 4.0
---

# Namespaces

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Linux **namespaces**  wrap system resources, making processes within that namespace appear to have isolated instances of that resource. Changes can be made within the namespace that will not be visible outside, on the system.

## Namespace types

The following namespaces are available in Linux:

- Cgroup - Provides a new Cgroup root directory for the process.
- IPC - Provides System V IPC and POSIX message queues.
- Network - Isolated network devices, IP stacks, routing tables, firewall rules, used ports, UNIX sockets, and more.
- Mount - Isolated mount records for the process, providing distinct single-directory hierarchies.
- PID - Provides a new PID tree, starting at 1 like a typical Linux system.
- Time - Provides 2 virtual clocks for the process: `CLOCK_MONOTONIC`, and `CLOCK_BOOTTIME`.
- User - Provides isolated user security identifiers and attributes, such as: UIDs, GIDs, keyrings, capabilities.
- UTS - Isolates the process' *hostname* and *NIS domain name* using sethostname and setdomainname.

## Interacting with user namespaces

*User* namespaces, or namespaces where UID/GIDs are mapped, can be used to act as the root UID without elevated privilages.

### Checking current ID maps

The current UID/GID maps can be set or edited using /proc/$$/uid\_map and /proc/$$/gid\_map.

`user $``cat /proc/$$/uid_map`
0          0 4294967295

### Creating a new namespace

To create a new user namespace, mapped to the **root** user and group within the namespace:

`user $``unshare --map-auto -S 0 -G 0`
This new shell session has the following mapped UIDs:

`root #``cat /proc/$$/uid_map`
0     100000      65536

### Creating a new namespace with outside user privileges

To create a new shell session where **root** inside is mapped to the user running the command outside:

`user $``unshare --map-auto --map-root`
This new shell session has the following mapped UIDs:

`root #``cat /proc/$$/uid_map````
         0       1000          1
         1     100000      65536
```
### Entering an existing namespace

If a process is already running in a namespace, nsenter can be used to interact with it.

To get root user context within a namespace running on PID **12345**:

`user $``nsenter --target 12345 --setuid 0 --setgid 0 --user`
## See also

- [Cgroups](https://wiki.gentoo.org/wiki/Cgroups) — allow securely managing the system resource usage of processes

## External resources

- [namespaces(7)](https://man.archlinux.org/man/namespaces.7.en)- [Wikipedia article on Linux namespaces](https://en.wikipedia.org/wiki/Linux_namespaces)
