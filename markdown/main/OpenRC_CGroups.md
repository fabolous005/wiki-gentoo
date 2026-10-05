<!-- source: https://wiki.gentoo.org/wiki/OpenRC/CGroups | group: Gentoo Wiki (Main) | wiki-title: OpenRC/CGroups -->
---
title: OpenRC/CGroups
url: https://wiki.gentoo.org/wiki/OpenRC/CGroups
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-31"
fingerprint: be12e9dfbda035c5
license: CC BY-SA 4.0
---

# OpenRC/CGroups

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

OpenRC includes support for [cgroups](https://wiki.gentoo.org/wiki/Cgroups). Cgroup support is implemented following the [recommendations from freedesktop.org](https://www.freedesktop.org/wiki/Software/systemd/PaxControlGroups/).

OpenRC creates its own cgroup controller named *openrc* in which the service processes are put. If set in the service's options a new cgroup controller named *openrc\_${service\_name}* is created to hold its process inclusive all its child services' processes. The result is a hierarchical tree of cgroups containing the service processes.

## Configuration

### Activating cgroup feature support

The cgroup feature is only available on Linux.

To use cgroups in OpenRC turn on the following option in the main rc configuration file:

**`/etc/rc.conf`**

**Turn on the cgroup feature support**

```
rc_controller_cgroups="YES"
```

For more information about cgroups version 1, see Documentation/admin-guide/cgroup-v1/\*<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> in the Linux kernel source tree.

For more information about cgroups version 2, see Documentation/admin-guide/cgroup-v2.rst<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> in the Linux kernel source tree.

OpenRC cgroup support is handled by the `cgroups` service. It should be part of the sysinit runlevel by default, if not, it can be added using rc-update add cgroups sysinit.

#### Unified CGroups version 2

CGroups version 2 can be used exclusively or in hybrid mode. Some programs like docker only work when cgroups version 1 are available (since 20.10 docker supports CGroups version 2)<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>.

**`/etc/rc.conf`**

**Use both cgroups version 1 and 2**

```
rc_cgroup_mode="hybrid"
```
**`/etc/rc.conf`**

**Unified cgroups (v2) are default**

```
# unified mode is enabled by default
#rc_cgroup_mode="unified"
```
Controllers for cgroups version 2 need to be enabled explicitly if hybrid mode is being used, controllers listed here will not be available for cgroups version 1.

**`/etc/rc.conf`**

**Enable controllers for cgroups version 2**

```
rc_cgroup_controllers="cpuset cpu io memory hugetlb pids"
```
### Setting limits

#### CGroups version 1

For each cgroup controller one can set options in the form of:

**`/etc/rc.conf`**

**Set cgroup controller option globally for all services**

```
${CONTROLLER_NAME}="${CONTROLLER_NAME}.${OPTION_NAME} ${OPTION_VALUE}"
```
or

**`/etc/conf.d/${service_name}`**

**Set cgroup controller option for one service**

```
${CONTROLLER_NAME}="${CONTROLLER_NAME}.${OPTION_NAME} ${OPTION_VALUE}"
```
For example, you would use this to set the cpu.shares setting in the CPU controller to 512 for the service.

**`/etc/conf.d/${service_name}`**

```
rc_cgroup_cpu="cpu.shares 512"
```
#### CGroups version 2

This example limits the service to the power of 1.5 cpu cores (1500000 / 1000000), puts it under heavy reclaim pressure if 4 GiB of memory is exceeded and invokes the OOM killer if the memory consumption exceeds 5 GiB.

**`/etc/conf.d/${service_name}`**

**Set cgroup settings option for one service**

```
rc_cgroup_settings="
    cpu.max 1500000 1000000
    memory.high 4G
    memory.max 5G
"
```
### Service cleanup

To ensure the termination of a service's entire process tree within the same process cgroup upon service stop or restart, you can enable the cleanup feature in the service's rc script. This is done by setting the following option:

**`/etc/conf.d/${service_name}`**

**an example of triggering cgroup cleanup**

```
rc_cgroup_cleanup="yes"
```
Enabling this option globally can result in unexpected termination of processes belonging to other services. For instance, if the SSH daemon (sshd) is restarted, it could lead to the termination of programs started by users via SSH sessions. This includes any ongoing processes or tasks initiated within those sessions.

OpenRC does not support automatic process cgroup relocation, unlike some other services like logind, which handle process management differently. Therefore, caution must be exercised when enabling the cleanup option to avoid unintended disruptions to system functionality.

## OpenRC cgroup architecture

Cgroups, short for "control groups," are a hierarchical mechanism for organizing processes and managing system resources efficiently. They consist of two main components: the core, responsible for organising processes hierarchically, and controllers, which distribute specific system resources within the hierarchy.

Processes in the system are organized into a tree structure, with each process belonging to a single cgroup. All threads of a process are part of the same cgroup. Processes are initially placed in the cgroup of their parent process but can be moved to different cgroups. Importantly, moving a process does not affect existing descendant processes.

Cgroups support selective enabling or disabling of controllers, which determine how resources are distributed within the hierarchy. Controller behaviors are hierarchical, meaning that enabling a controller on a cgroup affects all processes within that cgroup's subtree. Restrictions set closer to the root in the hierarchy cannot be overridden further down.

Here is a pseudo-example of how cgroups may be organised in a hierarchy:

```
          root
         /  |  \
    system web  app
   /   |    |     \
 ssh httpd  php  python
       |
     worker
```
- The "root" cgroup contains all other cgroups.
- The "system" cgroup contains system-related processes.
- Under "system", there are separate cgroups for SSH and HTTP server processes.
- The "web" cgroup contains processes related to web services, including HTTP server and PHP processes.
- The "app" cgroup contains application-related processes, such as Python scripts.
- The "worker" cgroup, a child of the "httpd" cgroup, contains worker processes spawned by the "httpd" server.

### cgroup mount point

In Gentoo Linux, cgroups are mounted at different locations depending on the cgroups version and configuration:

**For cgroups v1**:

The cgroup v1 mount point is located at /sys/fs/cgroup. This mount point contains subdirectories for various cgroup controllers, such as cpu, memory, blkio, etc.

**For hybrid mode (using both cgroups v1 and v2)**:

When using hybrid mode, the cgroups v1 mount point remains the same as described above. Additionally, cgroups v2 has its own mount point located at /sys/fs/cgroup/unified. The unified directory includes subdirectories for each cgroup hierarchy, allowing for the simultaneous use of cgroups v1 and v2 controllers.

**For cgroups v2**:

When exclusively using cgroups v2, the mount point is /sys/fs/cgroup.

These mount points provide access to the cgroup filesystem, allowing users to interact with and configure cgroups and their controllers.
