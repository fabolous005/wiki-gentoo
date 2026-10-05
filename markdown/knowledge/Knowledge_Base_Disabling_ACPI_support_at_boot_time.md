<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Disabling_ACPI_support_at_boot_time | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Disabling_ACPI_support_at_boot_time -->
---
title: Knowledge Base:Disabling ACPI support at boot time
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Disabling_ACPI_support_at_boot_time
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-07-19"
fingerprint: "7e131314d1a63de4"
license: CC BY-SA 4.0
---

# Knowledge Base:Disabling ACPI support at boot time

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

When boot failures occur, it is sometimes recommended to disable [ACPI](https://wiki.gentoo.org/wiki/ACPI) support to verify if the ACPI support is indeed the reason for the malfunction. This will cause the entire system to run without ACPI support so power management functions will not work well.

## Environment

Gentoo Linux systems with ACPI support enabled:

`root #``zgrep 'CONFIG_ACPI=' /proc/config.gz`
CONFIG\_ACPI=y

## Analysis

When ACPI support is built in the Linux kernel, users can pass a boot option (`acpi=off`) to disable ACPI support in the Linux kernel for that boot.

## Resolution

During bootup, edit the boot line in the boot loader and add the `acpi=off` boot option to the kernel line, and then boot this entry. This will cause ACPI support to be disabled for this single boot session. If you want to make this permanent, either edit the boot loader configuration or remove ACPI support from the Linux kernel configuration.

## See also

- [Adjusting GRUB settings for a single boot session](https://wiki.gentoo.org/wiki/Knowledge_Base:Adjusting_GRUB_settings_for_a_single_boot_session) - Gentoo Wiki Knowledge Base.
