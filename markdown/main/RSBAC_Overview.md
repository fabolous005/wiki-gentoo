<!-- source: https://wiki.gentoo.org/wiki/RSBAC/Overview | group: Gentoo Wiki (Main) | wiki-title: RSBAC/Overview -->
---
title: RSBAC/Overview
url: https://wiki.gentoo.org/wiki/RSBAC/Overview
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-07"
fingerprint: bc389818ccac0688
license: CC BY-SA 4.0
---

# RSBAC/Overview

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This document provides an overview of RSBAC access control system.

## Key features

- Free Open Source (GPL) Linux kernel security extension
- Independent of governments and big companies
- Several well-known and new security models, including MAC, ACL and RC
- Control over individual users and program network accesses
- Any combination of models is possible
- Easily extensible: write your own model for runtime registration
- Supports all the current kernels
- Stable for production use

## What is RSBAC?

RSBAC is a flexible, powerful and fast open source access control framework for current Linux kernels, which has been in stable production use since January 2000 (version 1.0.9a). The full development has been done independently, and no existing access control code has been reused.

The standard package includes a range of access control models like MAC, RC, ACL (see below). Furthermore, the runtime registration facility (REG) makes it easy to implement your own access control model as a kernel module and get it registered at runtime.

The RSBAC framework is based on the [Generalized Framework for Access Control (GFAC)](http://www.acsac.org/secshelf/book001/09.pdf) by Abrams and LaPadula. All security relevant system calls are extended by security enforcement code. This code calls the central decision component, which in turn calls all active decision modules and generates a combined decision. This decision is then enforced by the system call extensions.

Decisions are based on the type of access (request type), the access target and on the values of attributes attached to the subject calling and to the target to be accessed. Additional independent attributes can be used by individual modules, e.g. the privacy module. All attributes are stored in fully protected directories, one on each mounted device. Thus changes to attributes require special system calls.

All types of network accesses can be controlled individually for all users and programs. This gives you full control over their network behaviour and makes unintended network accesses easier to prevent and detect.

As all types of access decisions are based on general decision requests, many different security policies can be implemented as a decision module. Apart from the builtin models shown below, the optional Module Registration (REG) allows for registration of additional, individual decision modules at runtime.

## Implemented models

In the RSBAC version 1.2.5, the following modules are included. Please note that all modules are optional.

### MAC

Bell-LaPadula Mandatory Access Control

### UM

The User Management in RSBAC is kernel based and complements or totally replace Linux’s subsystem. Administration of users is enforced with granularity and flexibility.

### PM

Privacy Model. [Simone Fischer-Huebner](http://www.cs.kau.se/~simone/) 's Privacy Model in its first implementation. See RSBAC [paper on PM implementation](http://rsbac.org/doc/media/niss98.php) for the National Information Systems Security Conference (NISSC 98)

### Dazuko

This is not really an access control model, but rather a system protection module against malware. Execution and reading of malware infected files can be prevented.

### FF

File Flags. Provide and use flags for dirs and files, currently execute\_only (files), read\_only (files and dirs), search\_only (dirs), secure\_delete (files), no\_execute (files), add\_inherited (files and dirs), no\_rename\_or\_delete (files and dirs, no inheritance) and append\_only(files and dirs). Only FF security officers may modify these flags.

### RC

Role Compatibility. Defines roles and types for each target type (file, dir, dev, ipc, scd, process). For each role, compatibility to all types and to other roles can be set individually and with request granularity. For administration there is a fine grained separation-of-duty. Granted rights can have a time limit. Please also refer to the [Nordsec 2002 RC Paper](http://rsbac.org/doc/media/rc-nordsec2002/index.html) for the detailed model design and specification.

### AUTH

Authorization enforcement. Controls all CHANGE\_OWNER requests for process targets, only programs/processes with general setuid allowance and those with a capability for the target user ID may setuid. Capabilities can be controlled by other programs/processes, e.g. authentication daemons.

### ACL

Access Control Lists. For every object there is an Access Control List, defining which subjects may access this object with which request types. Subjects can be of type user, RC role and ACL group. Objects are grouped by their target type, but have individual ACLs. If there is no ACL entry for a subject at an object, rights are inherited from parent objects, restricted by an inheritance mask. Direct (user) and indirect (role, group) rights are accumulated. For each object type there is a default ACL on top of the normal hierarchy. Group management has been added in version 1.0.9a. Granted rights and group memberships can have a time limit.

### CAP

Linux Capabilities. For all users and programs you can define a minimum and a maximum Linux capability set ("set of root special rights"). This lets you e.g. run server programs as normal user, or restrict rights of root programs in the standard Linux way.

### JAIL

Process Jails. This module adds a new system call rsbac\_jail, which is basically a super set of the FreeBSD jail system call. It encapsulates the calling process and all subprocesses in a chroot environment with a fixed IP address and a lot of further restrictions.

### RES

Linux Resources. For all users and programs you can define a minimum and a maximum Linux process resource set (e.g. memory size, number of open files, number of processes per user). Internally, these sets are applied to the standard Linux resource flags.

All decision modules are described in detail on the module description page.

A general goal of RSBAC design has been to some day reach the (obsolete) Orange Book (TCSEC) B1 level.
