<!-- source: https://wiki.gentoo.org/wiki/Elivepatch | group: Gentoo Wiki (Main) | wiki-title: Elivepatch -->
---
title: Elivepatch
url: https://wiki.gentoo.org/wiki/Elivepatch
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-21"
tags: ['Some public reports']
fingerprint: cf9791534c8927a0
license: CC BY-SA 4.0
---

# Elivepatch

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Sponsor

[Tokyo University of Technology](https://www.teu.ac.jp/english/index.html) - server sponsor

## Introduction

Flexible Distributed Linux Kernel Live Patching

[livepatch](https://www.kernel.org/doc/Documentation/livepatch/livepatch.txt) is a Linux kernel facility that allows to build a kernel module to update a running kernel without requiring a reboot.

**Elivepatch** is a client-server infrastructure to automate the creation and distribution of those modules.

![](https://wiki.gentoo.org/images/thumb/5/54/ZDzIl3CEP1L6U6XI-B43E4.png/300px-ZDzIl3CEP1L6U6XI-B43E4.png)

### Use cases

- Resource-constrained updates
  - Distributed live patch building
  - Incremental live patch (You can build live patch over the previous one)
- Automatic no-reboot updates for [security CVE](https://github.com/nluedtke/linux_kernel_cves)

## Setup

The software is mainly two components:

- **elivepatch-client**: Client to be run on the machines to be updated.
- **elivepatch-server**: Server in charge of building the livepatch modules.

The two use a RESTful API for receiving the kernel *configuration* and a source 'diff' and generate a *livepatch* module out of it using [kpatch](https://wiki.gentoo.org/wiki/Kpatch)

### Server setup

`root #``emerge --ask sys-apps/elivepatch-server`
This will install the init.d file under /etc/init.d/elivepatch and the conf.d under /etc/conf.d/elivepatch.

From the conf.d file you can change the elivepatch daemon user and permission (by default is root).

You can start elivepatch-server on machine startup with:

`root #``rc-config add elivepatch-server default`
The machine will need to have enough space to build several kernels and the toolchains used to build the original kernel.

### Client setup

`root #``emerge --ask sys-apps/elivepatch-client`
The client is as well a Python program.

## Usage examples

### One time livepatch build

`root #``elivepatch  --config <file.config> --patch <example.patch> --url <elivepatch-server_url:elivepatch-server_port>`
The **example.patch** is a diff from the original kernel sources the **file.config** belongs to (the elivepatch-client figures out the kernel version from it).

The command will contact the elivepatch server and request a livepatch module matching the patch provided.

### CVE livepatch

`root #``elivepatch  --cve --kernel <kernel_version> --url <elivepatch-server-url:port>`
It will automatically produce a **cve.patch** from the CVE advisories for the current running kernel and request a *livepatch* module.

It could be used as a cron job command to keep an always-on machine up to date security-wise.

## Limitations

### Patch creation

*elivepatch* uses **kpatch** under the hood and the system has the following limitations:

- [Patch that change data structure](https://github.com/dynup/kpatch/blob/master/doc/patch-author-guide.md#change-the-code-which-uses-the-data-structure)
- [Change content of existing variable](https://github.com/dynup/kpatch/blob/master/doc/patch-author-guide.md#use-a-kpatch-load-hook)
- [Add field to existing data structure](https://github.com/dynup/kpatch/blob/master/doc/patch-author-guide.md#use-a-shadow-variable)
- Init code changes are incompatible with kpatch
- [Header file changes](https://github.com/dynup/kpatch/blob/master/doc/patch-author-guide.md#header-file-changes)
- [Dealing with unexpected changed functions](https://github.com/dynup/kpatch/blob/master/doc/patch-author-guide.md#dealing-with-unexpected-changed-functions)
- [Removing references to static local variables](https://github.com/dynup/kpatch/blob/master/doc/patch-author-guide.md#removing-references-to-static-local-variables)
- [Code removal](https://github.com/dynup/kpatch/blob/master/doc/patch-author-guide.md#code-removal)

### Toolchain pinning

The **livepatch** generation process needs a toolchain matching the one used to build the original kernel. Depending on the number of system to serve, this will require a large collection of gcc.

### Security concerns

Automated live patching creation needs to trust the **diff** provided. It is advised to keep the process containerized.
For this purpose [Docker](https://wiki.gentoo.org/wiki/Docker) images are being worked on.

## Background

This project is part of GSoC 2017 and the code is written by [User:Alicef](https://wiki.gentoo.org/wiki/User:Alicef) mentored by [User:Gokturk](https://wiki.gentoo.org/wiki/User:Gokturk)

Written code:

- [kpatch ebuild merged in the Gentoo official repository](https://github.com/aliceinwire/elivepatch)
- [elivepatch client](https://github.com/aliceinwire/elivepatch-client)
- [elivepatch server](https://github.com/aliceinwire/elivepatch-server)
- [Official Gentoo repository elivepatch merge pull-request](https://github.com/gentoo/gentoo/pull/5608)

Reports:
