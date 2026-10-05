<!-- source: https://wiki.gentoo.org/wiki/Hardened/Introduction_to_Hardened_Gentoo | group: Gentoo Wiki (Main) | wiki-title: Hardened/Introduction to Hardened Gentoo -->
---
title: Hardened/Introduction to Hardened Gentoo
url: https://wiki.gentoo.org/wiki/Hardened/Introduction_to_Hardened_Gentoo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-07"
fingerprint: "14fbdd1345a77aec"
license: CC BY-SA 4.0
---

# Hardened/Introduction to Hardened Gentoo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide is a Hardened Gentoo primer.

## Introduction

This guide is meant for anyone unsure about the offerings of the Hardened Gentoo project, how to use them together, and what their respective roles in the project.

The basic security principle that we emphasize is layers of security. Layers are fundamental in ensuring a users machine is not compromised, and if it is, minimizing the damage(s) done. By combining a series of dissimilar, though security related technologies, we make an attacker jump through additional hoops before a compromise may occur. For this reason we always recommended that you decide what your specific needs are and combine those solutions to protect your system. We will try to explain the options and how they can be used together in this document.

Hardened Gentoo is not a product or solution in itself, it is merely a project with a group of developers all working toward the same goal of very proactive security. The sub-projects contained in Hardened Gentoo are not related in any more way than they are hosted within the same project. You might think of it as the same way KDE and GNOME are both part of the desktop project, and both have a common goal, but are otherwise unrelated to each other.

## Technologies offered

### PaX

At the heart of the project is PaX. PaX is a kernel patch that allows systems to be protected against buffer and heap overflows and similar attacks. PaX is the first line of defense in hardening a system.

Because of poorly written software, systems are frequently at risk of compromise because of buffer and heap overflows. Buffer and heap overflows are the result of unchecked bounds in user input in applications. When an attacker has the ability to give input to an application that is inserted into memory but not checked, there exists the possibility of an overflow. Lower level programming languages like C, Object-C and C++ do not automatically protect from overruns. The end result of a buffer overflow is an overrun of adjacent executable code allowing arbitrary modifications to memory. This would normally cause the application to crash if the user input is not understood by the machine. This generally manifests itself as a page fault because the characters that overrun the buffer into the executable area will be treated as an address which probably will not exist. This is the most benign result of an overrun.

If the attacker knows about an overrun in a particular software set, they will have the opportunity to add shellcode to the input and rather than causing the application to crash it will instead execute the instructions they have provided. This is because shellcode is how instructions are stored in memory for execution by the processor. Basically shellcode consists of 'opcodes' which translate to assembly routines. An attacker knows these opcodes very well and can create shellcode which allows them to do anything they desire, such as run a root shell and bind it to a port. When this happens the user will not be aware that any kind of malicious activity has occurred because the application does not crash, instead it starts executing the attackers arbitrary code allowing them to do anything they please.

PaX mitigates the buffer overflow problem by randomly placing each function and buffer in an application in memory. This is called ASLR (Address Space Layout Randomization) and is the cornerstone of PaX. By having random offsets for functions and buffers, the attacker is unable to craft an input which will guarantee that the shellcode will be executed (and makes it very difficult since the application will probably crash and be restarted with new random offsets). ASLR is most useful when used with PIE (position independent executable) code but also works with standard executable code, at the cost of overhead.

PaX also offers the ability for executable segments to be executable and not writable, and likewise writable segments to be writable and not executable. This is called pageexec. On **x86** based processors there is no ability to do this on a hardware level since the **x86** designers collapsed the read and execute memory flags into one in order to save space. Since a page can either be writable or readable and executable, it is not useful to set buffers as non-executable since they would no longer be readable. So on **x86** PaX emulates this behavior at a software level, which introduces overhead but is very helpful for system security.

For more information, please see the [PaX Quickstart](https://wiki.gentoo.org/wiki/Hardened/PaX_Quickstart) document.

### PIE and SSP

In the interest of clarity, while PIE and SSP are generally grouped in discussion because they are both part of the hardened toolchain, they are indeed *different* technologies with different purposes. PIE by itself provides no additional security, but when combined with PaX in the kernel it provides a powerful tool against overflows. SSP is entirely implemented in userland and protects against stack smashing attacks without the assistance of the kernel. Since these are implemented separately and do different things, they are indeed two different layers of protection. For example, one attack that might get past PaX, called ret2libc, would be blocked by SSP.

We have gone through great lengths to provide users with an easy way to build the entire userland with PIE code to take advantage of ASLR with very little overhead. In addition to PIE, the 'hardened' toolchain also provides SSP (stack smashing protection). SSP protects against stack smashing by allocating an area outside of buffers and putting a random, cryptographic canary (or marker) in it. This allows SSP to check whether the canary was overwritten after any write to the buffer and allows it to kill the app if it was overwritten. The hardened toolchain gives users of PIE/SSP userland the easiest possible way. Stages marked with 'hardened' are standard stages built with PIE and SSP, they do not include SELinux/grsecurity access controls.

### grsecurity kernel patches

Although PaX is part of the grsecurity patch set, the [grsecurity project](https://grsecurity.net/) has a lot more kernel improvements to offer than just PaX. Its [features](https://grsecurity.net/features.php) are broad, and focus on a large set of risk mitigations to prevent memory-oriented attacks from being effective. It includes more sanity checks on memory copies, stack overflow preventions, hardened memory permissions, memory address randomization, detection and reaction against bruteforce attacks, enhanced chroot protections, trusted path execution and much, much more.

The [sys-kernel/hardened-sources](https://packages.gentoo.org/packages/sys-kernel/hardened-sources) package is the kernel package of choice if grsecurity based protections are desired on the system. Be sure to read other documentation available on this Wiki as well as on the [grsecurity project site](https://grsecurity.net/) for the instructions on how to enable and work with those protection measures.

### Mandatory access control

While PaX is the first layer of protection, perhaps even the second or third if using firewalls and/or network intrusion detection, it is also recommended that access control be implemented to further secure the system. It is very important to realize access control is the *last* layer of protection. Access control is very helpful to contain attackers that have obtained access to the system, or local user accounts. Currently Hardened Gentoo supports only the [SELinux](https://wiki.gentoo.org/wiki/SELinux) access control mechanism.

For more information about Gentoo's SELinux support, please see the [SELinux](https://wiki.gentoo.org/wiki/SELinux) portal page.

### Integrity Measurement Architecture

The [Integrity Measurement Architecture](https://wiki.gentoo.org/wiki/Integrity_Measurement_Architecture) is a mainly Linux kernel driven security subsystem which focuses on the validation of file integrity before these files are loaded (and perhaps executed). It does so by comparing the hash of the file with a stored hash and only allowing the loading of that file to memory if the hashes match. The technology is specifically designed for preventing offline tampering of data: either the hash is cryptographically secure, ensuring that no-one can generate the same hash without access to a private key, or the validation is done remotely (called *remote attestation*) where the remote side checks the measured hashes against its own list. The latter situation could be used to only allow clients to access a VPN network if they can prove they have not been tampered with.

Alongside IMA is the [Extended Verification Module](https://wiki.gentoo.org/wiki/Extended_Verification_Module) (EVM) subsystem, which provides protection against tampering the hashes themselves, as well as tampering other extended attributes of files, such as SELinux' extended attributes). Together, these technologies play an important role in Linux' adoption of the *Trusted Computing Base*, interacting with specially designed security devices on systems (such as the *Trusted Platform Module* or TPM chip).

## Conclusion

The various technologies and security layers that the Hardened project has to offer have been covered; the project has intentions to offer more and will hopefully do so in the future. Examples of additional layers are intrusion detection/prevention, which would be first even before PaX. Covering encrypted disks and swap space can offer protection from 'physical' security breaches. Auditing allows system administrators to see and act upon risks before they become a compromise. Encrypted network traffic and strong authentication are also layers which are very supported in mainline Linux installations, but will probably not be focused on in this project.

## External resources

| Project | Homepage | Gentoo project page | 
|---|---|---|
| PaX | [http://pax.grsecurity.net](https://pax.grsecurity.net) | [Project:Hardened/PaX Quickstart](https://wiki.gentoo.org/wiki/Project:Hardened/PaX_Quickstart) | 
| PIE | Not Available | Not Available | 
| SSP | [http://www.trl.ibm.com/projects/security/ssp/](http://www.trl.ibm.com/projects/security/ssp/) | Not available | 
| SELinux | [http://selinuxproject.org/page/Main\_Page](http://selinuxproject.org/page/Main_Page) | [Project:SELinux](https://wiki.gentoo.org/wiki/Project:SELinux) | 
| grsecurity | [http://www.grsecurity.net](https://www.grsecurity.net) | [Project:Hardened/Grsecurity2 Quickstart](https://wiki.gentoo.org/wiki/Project:Hardened/Grsecurity2_Quickstart) |
