<!-- source: https://wiki.gentoo.org/wiki/Kernel/IKCONFIG_support | group: Gentoo Wiki (Main) | wiki-title: Kernel/IKCONFIG support -->
---
title: Kernel/IKCONFIG support
url: https://wiki.gentoo.org/wiki/Kernel/IKCONFIG_support
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-12"
fingerprint: "56821338c5a7b98d"
license: CC BY-SA 4.0
---

# Kernel/IKCONFIG support

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Enabling **I**n-**k**ernel **C**onfig (**IKCONFIG**) support for the Linux kernel enables the inspection of kernel configuration for running kernels. Optionally the kernel's configuration can be exported and viewed through the /proc/config.gz file.

## Introduction

In-kernel .config support, also known as **IKCONFIG**, allows system administrators to build a copy of the configuration the kernel was built with *into* the kernel itself.  This allows inspecting the the configuration of the kernel while it is running, without having to worry whether they changed or cleaned the kernel source directory after it was compiled.

### Why is it important?

In kernel config aids system administrators with debugging things like if a particular module was compiled when the kernel was built, whether a certain networking option was enabled, or any other configuration value that was set when the kernel was compiled. This information is often asked for when looking for support on the forums or IRC channels.

## Configuration

### Enabling IKCONFIG support

Kernel .config support can be enabled either built in to the kernel, or as a module. If built in to the kernel, there will always be a /proc/config.gz file to read the configuration from. If preferred, it can be built as a module so that the configuration is only available when the user loads the module.

### Optional: Loading the module on boot

If the kernel was built with IKCONFIG support as a module, and module is to be loaded when the system boots, add it to the list of auto-loaded modules for the system.

**`/etc/modules-load.d/configs.conf`**

## Usage

### Accessing IKCONFIG

#### Reading with the kernels helper script

The Linux kernel sources ship with a helper script to decode the kernel configuration from the IKCONFIG kernel symbol (module). Running the following command will print the kernel configuration to stdout:

`root #``/usr/src/linux/scripts/extract-ikconfig /lib/modules/$kernel_version/kernel/kernel/configs.ko`
#### Loading the configs module

If IKCONFIG support has been built as a module, the module will need to be loaded into the kernel before use. This can be done by running the following command:

`root #``modprobe configs`
Administrators should now be able to see the file /proc/config.gz. This is a compressed copy of the entire kernel configuration at build time.

When users finish looking at the configuration, they can unload the module with:

`root #``modprobe -r configs`
The module should now have been unloaded and the /proc/config.gz file removed.

### Reading IKCONFIG

/proc/config.gz is a compressed copy of the kernel configuration, which means that in order to read it, it needs to be decompressed. There are two ways to do so.

#### Quick queries

When checking one or two options, then the easiest way to read the file is to grep it through the decompressor:

`user $``zgrep IKCONFIG /proc/config.gz`
CONFIG\_IKCONFIG=m
CONFIG\_IKCONFIG\_PROC=y

#### Full file review

Those who want a more in-depth look at the configuration of the running kernel can use the less command, which will automatically decompress the file into viewable format:

`user $``less /proc/config.gz`
