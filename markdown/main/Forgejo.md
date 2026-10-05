<!-- source: https://wiki.gentoo.org/wiki/Forgejo | group: Gentoo Wiki (Main) | wiki-title: Forgejo -->
---
title: Forgejo
url: https://wiki.gentoo.org/wiki/Forgejo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-08"
fingerprint: efb4323d87bb7fe1
license: CC BY-SA 4.0
---

# Forgejo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Forgejo** is a fork of [Gitea](https://wiki.gentoo.org/wiki/Gitea).

## Installation

As of 2024-09-18, Forgejo is not provided as a Gentoo package, but is available in the [GURU](https://wiki.gentoo.org/wiki/Project:GURU) overlay. Alternatively, Forgejo is distributed as a single binary file.

### Official binaries

#### Forgejo

The Forgejo project distributes binaries for AMD64 and ARM64 architectures, which can be downloaded [here](https://codeberg.org/forgejo/forgejo/releases). The binaries are compatible with musl-based systems.

To download and verify the binary file, follow the instructions provided [here](https://forgejo.org/download/).

The binary does not require root privileges to run and can be launched from any directory:

`user $``./forgejo-*-linux-arm64`
If there is a plan to install the binary into the system, follow the steps provided [here](https://forgejo.org/docs/next/admin/installation-binary/).

#### Forgejo Actions (self-hosted)

The runner can be downloaded from [here](https://code.forgejo.org/forgejo/runner/releases).

Once downloaded, create and copy the token via GUI as described [here](https://forgejo.org/docs/next/admin/actions/#registration).

Register the runner:

Once registered, create the minimal configuration file:

**`config.yml`**

And launch the runner as a daemon:

`user $``./forgejo-runner-* --config config.yml daemon`
To test that everything works, push the following file to the repository:

**`.forgejo/workflows/demo.yaml`**

## SELinux policy

### Current state

The policies are not ready to be used in production.

Almost every action produces a significant number of cosmetic AVC log messages, resulting in a fast-growing /var/log/audit/audit.log file that can lead to a denial of service if /var is not mounted as a separate partition.

### Reproducible environment

Once the policies are installed and Forgejo is running, the following features must be configured in the initial setup window:

- Database type: SQLite3
- Git LFS root path: leave empty to disable
- SSH server port: leave empty to disable (almost everything can be done through the REST API)
- Enable OpenID sign-in: disable
- Password hash algorithm: pbkdf2\_hi (the default value)


The policies were tested in the following profiles:

| Profile name | Status | Forgejo's version | Forgejo runner's version | Notes | 
|---|---|---|---|---|
| default/linux/arm64/23.0/musl/hardened/selinux |  | 9.0.2 | 5.0.3 |  | 

### Forgejo's policy

**`forgejo.te`**

**`forgejo.fc`**

### Forgejo runner's policy

**`forgejo-runner.te`**

**`forgejo-runner.fc`**

### Installation of policies

All .te and .fc files defined above should be in the same directory (forgejo and forgejo-runner can be separated if desired).

`root #``make -f /usr/share/selinux/strict/include/Makefile``root #``semodule --install forgejo*.pp``root #``restorecon -R /opt/forgejo``root #``restorecon -R /opt/forgejo-runner`
### Removal of policies

`root #``semodule --remove forgejo``root #``semodule --remove forgejo-runner``root #``restorecon -R /opt/forgejo``root #``restorecon -R /opt/forgejo-runner`
### Usage of policies

The forgejo.fc file requires the forgejo binary file to be placed in the /opt/forgejo directory.

The forgejo-runner.fc file requires the forgejo-runner binary file to be placed in the /opt/forgejo-runner directory.

The execution must be performed as regular users in the mentioned above directories.

The paths can be modified as desired in the appropriate .fc file.

## See also

- [Node.js as a reverse proxy for Forgejo](https://wiki.gentoo.org/wiki/Node.js#Node.js_as_a_reverse_proxy_for_Forgejo.5CGitea_.28or_anything_else.29)
- [Gitea](https://wiki.gentoo.org/wiki/Gitea) — painless self-hosted [git](https://wiki.gentoo.org/wiki/Git) service, a fork of gogs
