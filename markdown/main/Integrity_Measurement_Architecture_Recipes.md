<!-- source: https://wiki.gentoo.org/wiki/Integrity_Measurement_Architecture/Recipes | group: Gentoo Wiki (Main) | wiki-title: Integrity Measurement Architecture/Recipes -->
---
title: Integrity Measurement Architecture/Recipes
url: https://wiki.gentoo.org/wiki/Integrity_Measurement_Architecture/Recipes
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-03-12"
fingerprint: faa1b1ba0d607b6
license: CC BY-SA 4.0
---

# Integrity Measurement Architecture/Recipes

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article introduces Integrity Measurement Architecture recipes for the Linux kernel.

The two default policies included the in kernel, tcb and tcb\_appraise are not very useful on a general-purpose machine, it is recommended to create custom rules.

The format of the rules is in the Linux kernel documentation is located in Documentation/ABI/testing/ima\_policy. To obtain the magic numbers for the "fsmagic" condition see /usr/include/linux/magic.h or include/uapi/linux/magic.h in the kernel sources.

## Built-in policies

The built-in policies are current as of Linux 4.19. Comments have been added to the policies to make it easier to understand.

### tcb

The policy excludes some "pseduo" filesystem from measurement, and measures every file mapped for execution, directly executes, read by root, all modules loaded and all firmware loaded

### tcb\_appraise

As above, some "pseduo" filesystem are excluded, and anything owned by root is appraised

### secure\_boot

This policy requires all modules, firmware, kexec kernels and IMA policies to have an IMA signature.

### Kernel options affecting IMA policies

The kernel has a few option append to add IMA policies, if IMA build time configured policy rules or Enable multiple writes to the IMA policy is selected

**Additional IMA policy options**

If any of these are set to "Y" they already all default and custom policies by adding the following rules:

## Custom policies

### Excluding log files

Measurement and appraisal of log files is not useful and generate kernel spam every time one is opened. It would be useful to exclude known log files, and with the help of SELinux, it is possible to so. List the log file types SELinux knows about:

`user $``seinfo -alogfile -x`
Type Attributes: 1
   attribute logfile;
	auth\_cache\_t
	cron\_log\_t
	dirmngr\_log\_t
	dracut\_var\_log\_t
	faillog\_t
	fsadm\_log\_t
	getty\_log\_t
	initrc\_var\_log\_t
	lastlog\_t
	nscd\_log\_t
	portage\_log\_t
	rsync\_log\_t
	user\_cron\_spool\_log\_t
	var\_log\_t
	wtmp\_t

With this in hand, known log files can be excluded from appraisal and measurement by including this snippet before any "appraise" or "measure" rules
