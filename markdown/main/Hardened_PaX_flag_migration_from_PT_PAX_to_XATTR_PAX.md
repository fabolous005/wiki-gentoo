<!-- source: https://wiki.gentoo.org/wiki/Hardened/PaX_flag_migration_from_PT_PAX_to_XATTR_PAX | group: Gentoo Wiki (Main) | wiki-title: Hardened/PaX flag migration from PT PAX to XATTR PAX -->
---
title: Hardened/PaX flag migration from PT PAX to XATTR PAX
url: https://wiki.gentoo.org/wiki/Hardened/PaX_flag_migration_from_PT_PAX_to_XATTR_PAX
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-31"
fingerprint: aff310b86f2c3fb9
license: CC BY-SA 4.0
---

# Hardened/PaX flag migration from PT PAX to XATTR PAX

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A quick guide on migrating PaX flags from PT\_PAX to XATTR\_PAX.

## Before you start reading!

This page is a quick howto on migrating from PT\_PAX to XATTR\_PAX flags. It presupposes the reader knows what PaX is all about. See our [Pax Quickstart](https://wiki.gentoo.org/wiki/Hardened/PaX_Quickstart) for a broad coverage. For an in-depth explanation of how PaX works, see the [Homepage of The PaX Team](https://pax.grsecurity.net).

## The three ways of marking PaX flags: EI\_PAX, PT\_PAX and XATTR\_PAX

PaX provides various protections against abuses of memory. Some of these protections can only be enabled or disabled by (re)configuring the kernel and recompiling/rebooting. However a few important ones (PAGEEXEC, EMUTRAMP, MPROTECT, RANDMMAP and SEGMEXEC) can be tweaked when the system is up and running by marking the PaX flags on the ELF objects of the program you want to run. Since some programs need to use memory in a way normally forbidden by PaX, we may have to relax some restrictions on a per program basis.

Historically, the PaX flags have been stored in one of three different locations. At first, they were housed in the ELF header of the objects (EI\_PAX), but this broke with updates to glibc. They were next moved to an ELF program header (PT\_PAX) and this was mostly satisfactory, except for those occassion programs where adding such a header was problematic for one reason or another. The next generation places the flags in the extended attributes of the filesystem(s). As long as all the filesystems and the utilities working with those files/filesystems respects the extended attributes, this is the best solution because it essentially does not touch the ELF binaries themselves.

## Migrating from PT\_PAX to XATTR\_PAX

Marking the ELF header (EI\_PAX) has been completely deprecated in Gentoo. If you have an ancient system which still uses EI\_PAX, then its probably so broken by this point that no migration is possible anyhow. Start over.

Marking the ELF program header (PT\_PAX) is still supported, but that support will **slowly** disappear. As of glibc-2.16, the elf.h header file will no longer contain the definitions of the PT\_PAX\_FLAGS program header, nor of the values of the PaX flags that live there ([bug #440018](https://bugs.gentoo.org/show_bug.cgi?id=440018)). At some point in the future, the patch against binutils which includes the PT\_PAX\_FLAGS program header will also go. At that point only XATTR\_FLAGS will remain. You might want to get ahead of the game and migrate now. PT\_PAX is not supported in the 17.0 and up profile.

The process of migration is safe because at no point will we actually dump the PT\_PAX flags, so we can always revert if something goes wrong. The essential caveat to remember is that the kernel ultimately decides whether to use PT\_PAX or XATTR\_PAX (or both). So you can have both markings on an ELF object and have the kernel read one and ignore the other. Note that if you enable **both** PT\_PAX and XATTR\_PAX in the kernel, **and** you create an XATTR\_PAX field, then the kernel expects both fields to be identical, otherwise neither is respected. It **is** okay to enable both fields provided you don't create XATTR\_PAX; then, the PT\_PAX field is fully respected. We can use this as part of the migration; but in general, we do not recommend setting both unless there is a good reason. Just switch from one to the other, i.e. PT\_PAX xor XATTR\_PAX.

So, starting from a stock PT\_PAX system, you can migrate to XATTR\_PAX as follows:

**1. Userland preliminaries:**

First, let's make sure that your userland utilities can handle extended attributes in general and XATTR\_PAX in particular. If they cannot, then either the XATTR\_PAX markings will fail or they'll get lost as we pack/unpack our ELF objects, for example, when using tar. So,

- **a.** make sure you set USE=xattr in your global USE flags, and
- **b.** emerge >=sys-apps/elfix-0.8.1 without disabling either ptpax or xtpax USE flags.
- **c.** add PAX\_MARKINGS="XT" in the make.conf file.

**2. Kernel preliminaries:**

As you do the migration, you must make sure your filesystem can accomodate extended attributes, including tmpfs! If your kernel hasn't been already so configured, do so now and reboot into it. Choosing PAX\_XATTR\_PAX\_FLAGS under the PaX kernel menu will automatically set extended attributes on as many filesystems as can support them. Remember, you can enable both PT\_PAX and XATTR\_PAX in the kernel at this point, and PT\_PAX will be respected until you create XATTR\_PAX fields on the target binaries. We'll tolerate this as a transition, but we recommend using only XATTR\_PAX afterward.

**3. Migrate the flags:**

The elfix package comes with migrate-pax. Running it with the -m flag will copy the PT\_PAX flags to XATTR\_PAX for every ELF object that portage knows about, **except** for those object which have the **default** flags. Since a kernel configured to use only XATTR\_PAX will fall back on the default flags when no XATTR\_PAX field is found, there is no reason to create such a field when the default flags are desired. Running `migrate-pax -m` is very safe and you can easily undo it by running `migrate-pax -d`.

**4. Boot into an XATTR\_PAX only kernel:**

You can now boot into a pure XATTR\_PAX kernel. Make sure PT\_PAX is off. Even though the flags should be the same in both fields, or XATTR\_PAX absent in the case of default flags, we will be on the side of caution and keep control over the effective flags by using only XATTR\_PAX.

**5. Profit!**

If you really want to make sure it worked, the elfix package comes with some test suites. These are tricky to use correctly because if you have the wrong combination of PT\_PAX versus XATTR\_PAX userland/kernel configurations, you'll get a lot of false failures. The next section shows you how to test.

## Testing whether the migration worked and XATTR\_PAX flags are respected

So did the migration work? And is the kernel recognizing XATTR\_PAX markings? You can verify that the migration worked by spot checking with paxctl-ng. Try something like the following:

`root #``paxctl-ng -v /bin/*`
....
/bin/uncompress:
	ELF ERROR: elf\_kind() fail: this is not an elf file.   \<--- This is not really an ELF object, expect failure
	PT\_PAX   : not found
	XATTR\_PAX: not found
  
/bin/vdir:                                                     \<--- Good! No XATTR\_PAX flags means we get the default
	PT\_PAX   : -e---                                       \<--- Only 'e' (don't emulate trampolines) is the default setting.
	XATTR\_PAX: not found
  
/bin/ypdomainname:                                             \<--- Good!  Both PT\_PAX and XATTR\_PAX are identical
	PT\_PAX   : -em--
	XATTR\_PAX: -em--

`root #``paxctl-ng -v /lib/*````
      ....
/lib/libz.so.1.2.7:                                            <--- Even libraries should have matching flags (here its the default)
	PT_PAX   : -e---
	XATTR_PAX: not found
  
/lib/modules:
	open(O_RDWR) failed: cannot change PT_PAX flags        <--- This is a directory, expect failure
	ELF ERROR: elf_begin() fail: (null)
	PT_PAX   : not found
	XATTR_PAX: not found
      ....
```
To check if the kernel is recognizing XATTR\_PAX markings, we'll use a test suite from the [sys-apps/elfix](https://packages.gentoo.org/packages/sys-apps/elfix) package. We'll have to checkout the git repo since the ebuild doesn't run tests. They are tricky and can give lots of false negatives if you don't have the right combination of PT\_PAX versus XATTR\_PAX in both userland and kernel. You can proceed as follows:

`root #````
cd elfix
```
`root #````
./autogen.sh
```
`root #````
./configure --disable-ptpax --enable-xtpax --enable-tests
```
`root #````
make
```
`root #````
cd tests/pxtpax/
```
`root #``./daemontest.sh````
================================================================================
  
 RUNNING DAEMON TEST
  
 NOTE:
   1) This test is only for amd64 and i686
   2) This test will fail on amd64 unless the following are enabled in the kernel:
        CONFIG_PAX_PAGEEXEC
        CONFIG_PAX_EMUTRAMP
        CONFIG_PAX_MPROTECT
        CONFIG_PAX_RANDMMAP
   3) This test will fail on i686 unless the following are enabled in the kernel:
        CONFIG_PAX_EMUTRAMP
        CONFIG_PAX_MPROTECT
        CONFIG_PAX_RANDMMAP
        CONFIG_PAX_SEGMEXEC
  
................................................................................
................................................................................
................................................................................
...
  
 Mismatches = 0
  
================================================================================
```
No mismatches! It worked. What the test is doing is marking a daemon with every possible combination of PaX flags, starting it, checking if its running with the expected flags, and reporting. If you want to have fun with this, try enabling PT\_PAX and disabling XATTR\_PAX when you configure elfix, but keep only XATTR\_PAX support in the kernel. It will fail miserably as no PaX flags are respected.
