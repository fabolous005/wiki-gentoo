<!-- source: https://wiki.gentoo.org/wiki/Hardened/Grsecurity2_Quickstart | group: Gentoo Wiki (Main) | wiki-title: Hardened/Grsecurity2 Quickstart -->
---
title: Hardened/Grsecurity2 Quickstart
url: https://wiki.gentoo.org/wiki/Hardened/Grsecurity2_Quickstart
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-11-17"
fingerprint: "3517d97e7da2bbd8"
license: CC BY-SA 4.0
---

# Hardened/Grsecurity2 Quickstart

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This document features the grsecurity 2.x security patches, supported kernel configuration options, and tools provided by the grsecurity project in order to raise system security to higher standards.

## About Grsecurity

### The Grsecurity project

The grsecurity project, hosted on [http://grsecurity.net](https://grsecurity.net), provides various patches to the Linux kernel which enhance a system's overall security. The various features brought by grsecurity are discussed in the next chapter; a comprehensive list is maintained on the [grsecurity features page](https://en.wikibooks.org/wiki/Grsecurity/Appendix/Grsecurity_and_PaX_Configuration_Options) itself.

As grsecurity's features are mostly kernel-based, the majority of this document explains the various kernel features and their respective sysctl operands (if applicable).

### Gentoo Hardened integration

The [Gentoo Hardened project](https://wiki.gentoo.org/wiki/Project:Hardened) maintains security-enhancement features for Gentoo, including but not limited to grsecurity.

### Kernel configuration

Throughout this document kernel configuration using the kernel variables like `CONFIG_GRKERNSEC_PAX_NO_ACL_FLAGS` will be discussed. These are the variables that the kernel build process uses to determine if a certain feature needs to be compiled.

When configuring the kernel with make menuconfig or a similar make front-end, a graphical interface can be used to select the various kernel options. If the Help button is pressed on a certain kernel feature it is possible to see the kernel variable (and its dependencies) responsible for controlling the kernel feature.

With a bit of thinking and some time it is easy to configure the kernel as needed. And if a certain feature (variable) cannot be found, there's always the possibility to edit the /usr/src/linux/.config file by hand! :)

Of course, to be able to select the various grsecurity kernel options, grsecurity must be enabled in the kernel:

**Activating grsecurity**

## PaX

### Fighting the exploitation of software bugs

PaX introduces a couple of security mechanisms that make it harder for attackers to exploit software bugs that involve memory corruption (so do not treat PaX as if it protects against all possible software bugs). The [PaX introduction document](https://pax.grsecurity.net/docs/pax.txt) talks about three possible exploit techniques:

1. Introduce/execute arbitrary code;
2. Execute existing code out of original program order;
3. Execute existing code in original program order with arbitrary data

One prevention method disallows executable code to be stored in writable memory. When we look at a process, it requires five memory regions:

1. A *data section* which contains the statically allocated and global data;
2. A *BSS region* (Block Started by Symbol) which contains information about the zero-initialized data of the process;
3. A *code region* , also called the *text segment* , which contains the executable instructions;
4. A *heap* which contains the dynamically allocated memory;
5. A *stack* which contains the local variables

The first PaX prevention method, called *NOEXEC*, is meant to give control over the run-time code generation. It marks memory pages that do not contain executable code as non-executable. This means that the heap and the stack, which only contain variable data and should not contain executable code, are marked as non-executable. Exploits that place code in these areas with the intention of running it will fail.

NOEXEC does more than this actually, interested readers should focus their attention to the [PaX NOEXEC documentation](https://pax.grsecurity.net/docs/noexec.txt).

The second PaX prevention method, called **ASLR** (Address Space Layout Randomization), randomize the addresses given to memory requests. Where previously memory was assigned contiguously (which means exploits know where the tasks' memory regions are situated) ASLR randomizes this allocation, rendering techniques that rely on this information useless.

More information about ASLR can be found [online](https://pax.grsecurity.net/docs/aslr.txt).

### Enabling PaX

The recommended kernel setting for PaX is:

**Recommended PaX kernel configuration**

When running a non-**x86** system it is possible to observe that there `CONFIG_PAX_NOEXEC` does not exist in the kernel configuration. In this scenario select `CONFIG_PAX_PAGEEXEC` instead since it is the only non-exec implementation around.

### Controlling PaX

Not all Linux applications are happy with the PaX security restrictions. These tools include xorg-x11, java, mplayer, xmms and others. When planning to use them be sure to elevate the protections for these applications using the paxctl command.

`root #``emerge --ask sys-apps/paxctl`
[app-misc/pax-utils](https://packages.gentoo.org/packages/app-misc/pax-utils) is a small toolbox which contains useful applications to administrate a PaX aware server.

`root #``emerge --ask app-misc/pax-utils`
Interesting tools include scanelf and pspax:

- scanelf can be used to scan over library and binary directories and list the various permissions and ELF types that pertain to running an ideal pax/grsec setup.
- pspax can display PaX flags/capabilities/xattr from the kernel's perspective.

### Verifying the PaX settings

Peter Busser has written a regression test suite called paxtest. This tool will check various cases of possible attack vectors and provide information on the results. After it is ran it will leave a log file called paxtest.log in the current working directory.

`root #``emerge --ask app-admin/paxtest`
Running paxtest looks something like this:

`root #``paxtest blackhat`
Executable anonymous mapping             : Killed
Executable bss                           : Killed
Executable data                          : Killed
Executable heap                          : Killed
Executable stack                         : Killed
Executable shared library bss            : Killed
Executable shared library data           : Killed
Executable anonymous mapping (mprotect)  : Killed
Executable bss (mprotect)                : Killed
Executable data (mprotect)               : Killed
Executable heap (mprotect)               : Killed
Executable stack (mprotect)              : Killed
Executable shared library bss (mprotect) : Killed
Executable shared library data (mprotect): Killed
Writable text segments                   : Killed
Anonymous mapping randomisation test     : 16 bits (guessed)
Heap randomisation test (ET\_EXEC)        : 13 bits (guessed)
Heap randomisation test (PIE)            : 25 bits (guessed)
Main executable randomisation (ET\_EXEC)  : 16 bits (guessed)
Main executable randomisation (PIE)      : 17 bits (guessed)
Shared library randomisation test        : 16 bits (guessed)
Stack randomisation test (SEGMEXEC)      : 23 bits (guessed)
Stack randomisation test (PAGEEXEC)      : No randomisation
Return to function (strcpy)              : Vulnerable
Return to function (memcpy)              : Vulnerable
Return to function (strcpy, RANDEXEC)    : Killed
Return to function (memcpy, RANDEXEC)    : Killed

In the above example run you notice that:

- strcpy and memcpy are listed as *Vulnerable*. This is expected and normal - it is simply showing the need for a technology such as ProPolice/SSP
- There is no randomization for PAGEEXEC. This is normal since the recommended **x86** kernel configuration did not activate the PAGEEXEC setting. However, on arches that support a true NX (non-executable) bit (most of them do, including x86\_64), PAGEEXEC is the only method available for NOEXEC and has no performance hit.

## RBAC

### Role based access control

There are two basic types of access control mechanisms used to prevent unauthorized access to files (or information in general): DAC (Discretionary Access Control) and MAC (Mandatory Access Control). By default, Linux uses a DAC mechanism: the creator of the file can define who has access to the file. A MAC system however forces everyone to follow rules set by the administrator.

The MAC implementation grsecurity supports is called Role Based Access Control. RBAC associates *roles* with each user. Each role defines what operations can be performed on certain objects. Given a well-written collection of roles and operations your users will be restricted to perform only those tasks that you tell them they can do. The default "deny-all" ensures that a user cannot perform an action that has not been considered by a system administrator.

### Configuring the kernel

The recommended kernel setting for RBAC is:

**Recommended RBAC kernel configuration**

### Working with gradm

gradm is a tool which allows a policy to be administered and maintained for the system. With it, the RBAC system can be enabled or disabled, RBAC roles reloaded, roles changed for users, passwords set for admin mode, etc.

When installing gradm a default policy will be installed in /etc/grsec/policy:

`root #``emerge --ask sys-apps/gradm`
By default, the RBAC policies are not activated. It is the system administrator's job to determine when the system should have an RBAC policy enforced and not Gentoo's. Before activating the RBAC system an administrator password should be set.

`root #``gradm -P`
Setting up grsecurity RBAC password
Password: ## (Enter a well-chosen password)
Re-enter Password: ## (Enter the same password for confirmation)
Password written in /etc/grsec/pw

`root #``gradm -E`
To disable the RBAC system, run `gradm -D`. If permission is denied, switch to the admin role, and try again:

`root #``gradm -a admin`
Password: ## (Enter your admin role password)

`root #``gradm -D`
To leave the admin role, run `gradm -u admin`:

`root #``gradm -u admin`
### Generating a policy

The RBAC system comes with a great feature called "learning mode". The learning mode can generate an anticipatory least privilege policy for the system. This allows for time and money savings by being able to rapidly deploy multiple secure servers.

To use the learning mode, activate it using gradm:

`root #``gradm -F -L /etc/grsec/learning.log`
Now use the system, do the things that would normally be done. Try to avoid rsyncing, running the locate command, or any other heavy file I/O operation since these can really slow down the processing time.

After sufficiently using the them to obtain a good policy, let gradm process and propose roles under /etc/grsec/learning.roles:

`root #``gradm -F -L /etc/grsec/learning.log -O /etc/grsec/learning.roles`
Audit the /etc/grsec/learning.roles as needed and save it as /etc/grsec/policy (mode 0600) when finished.

`root #````
mv /etc/grsec/learning.roles /etc/grsec/policy
```
`root #````
chmod 0600 /etc/grsec/policy
```
You will now be able to enable the RBAC system with the new learned policy.

### Tweaking the policy

An interesting feature of grsecurity 2.x is *Set Operation Support* for the configuration file. Currently it supports unions, intersections and differences of sets (of objects in this case).

Here is an example of its use, and the resulting objects that will be added to the subject:

The above would expand to:

This is the result of the `&` operator which takes both sets and returns the files that exist in both sets and the permission for those files that exist in both sets.

This example would expand to:

This is the result of the `|` operator which takes both sets and returns the files that exist in either set. If a file exists in both sets, it is returned as well and the mode contains the flags that exist in either set.

This example would expand to:

This is the result of the `-` operator which takes both sets and returns the files that exist in the set on the left but not in the match of the file in set on the right. If a file exists on the left and a match is found on the right (either the filenames are the same, or a parent directory exists in the right set), the file is returned and the mode of the second set is removed from the first set, and that file is returned.

In some obscure pseudo-language you could see this as:

As for order of precedence (from highest to lowest): "`-`, `&`, `|`".

If you do not want to bother remembering precedence, parenthesis support is also included, so you can do things like:

## Filesystem protection

### Fighting chroot and filesystem abuse

Grsecurity2 includes many patches that prohibits users from gaining unnecessary knowledge about the system. This includes restrictions on /proc usage, chrooting, linking, etc.

### Kernel configuration

We recommend the following grsecurity kernel configuration for filesystem protection:

**Activating filesystem protection**

### Triggering the security mechanism

When using a kernel compiled with the above (or similar) settings, the option is available to enable/disable many of the features through the /proc filesystem or via sysctl.

The example below shows an excerpt of a typical /etc/sysctl.conf:

**`/etc/sysctl.conf`**

**Example settings**

Enable or disable settings at will using the sysctl command:

Toggling the exec\_logging feature ON:

`root #``sysctl -w kernel.grsecurity.exec_logging = 1`
Toggling the exec\_logging feature OFF:

`root #``sysctl -w kernel.grsecurity.exec_logging = 0``root #``sysctl -w kernel.grsecurity.grsec_lock = 1`
## Kernel auditing

### Extend the system's logging facilities

grsecurity adds extra functionality to the kernel pertaining the logging. With grsecurity's *Kernel Auditing* the kernel informs when applications are started, devices (un)mounted, etc.

### The various kernel audit settings

The following kernel configuration section can be used to enable grsecurity's kernel audit settings:

**Activating kernel auditing**

## Process restrictions

### Executable protection

With grsecurity executables can be restricted. Since most exploits work through one or more running processes this protection can save the system's health.

### Network protection

Linux' TCP/IP stack is vulnerable to prediction-based attacks. grsecurity includes randomization patches to counter these attacks. Apart from these socket restrictions can be enabled, disallowing certain groups network access alltogether.

### Kernel settings

The following kernel settings enable various executable and network protections:

**Kernel setting**

## The hardened toolchain

Although it is outside the scope of this document the use of the [hardened toolchain](https://wiki.gentoo.org/wiki/Project:Hardened/FAQ#How_do_I_switch_to_the_hardened_profile.3F) should be mentioned which completes the grsec/PaX model from userspace.

## See also

- [Capability Names and Descriptions](https://wiki.gentoo.org/wiki/Project:Hardened/Overview_of_POSIX_capabilities)
- [Using PaX with Gentoo QuickStart](https://wiki.gentoo.org/wiki/Project:Hardened/PaX_Quickstart) (NEW)
- [Grsecurity with Gentoo 1.9.x MAC system](https://wiki.gentoo.org/wiki/Project:Hardened/Grsecurity2_Quickstart) (OLD)

## External resources
