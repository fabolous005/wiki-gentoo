<!-- source: https://wiki.gentoo.org/wiki/Hardened/Debugging | group: Gentoo Wiki (Main) | wiki-title: Hardened/Debugging -->
---
title: Hardened/Debugging
url: https://wiki.gentoo.org/wiki/Hardened/Debugging
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2015-05-30"
fingerprint: b3475c9765293da0
license: CC BY-SA 4.0
---

# Hardened/Debugging

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

In this document we study the ways to do proper binary debugging when using a hardened kernel and toolchain with PaX/Grsec, PIE and SSP.

## Solving the '??' issue.

When debugging you'll probably have found that `GDB` may not show the addresses showing instead a stream of lines with **??** where the symbol should be. This can be caused by two different things.

The first cause is that your `GDB` version is too old and is unable to realize that the addresses are relative. This should be fixed in the current stable versions of `GDB` so you should try to upgrade it. Other workaround is applying solution 3.

The second reason is that your hardened kernel may be hiding the mappings. This is a known problem and [has been fixed upstream](https://forums.grsecurity.net/viewtopic.php?f=1&t=2467) so it will be fixed on further releases of `hardened-sources` . Anyway, until the fix reaches the tree and is stabilized, you can apply any of the solutions.

### Solution 1: Disabling RANDMMAP on the binary

One solution is disabling the RANDMMAP feature with `paxctl` for that particular binary. Doing this will make Grsec disable the mapping protection for that binary as it makes no sense protecting it then. This means a more secure environment but also getting away from the way the binary would be executed on the real environment.

`root #``paxctl -r binary`
### Solution 2: Disabling the option to hide mappings

Other fix is disabling the option that hides the addresses on the PaX protected executables to avoid attacks based on that information. This option may make the things easier for an attacker until it is enabled again although also means that the environment will be the most similar possible to the real execution environment.

**Disabling the mapping hiding.**

### Solution 3: Linking a non PIE binary

A last solution is disabling the last pie linking stage while compiling using `-nopie` . All previous compilation can still use `-fPIE` as normal (which is also the default with the hardened compiler) so that your executable is as close as possible to the real thing as long as the final link must create a regular executable.Try adding `-nopie` to LDFLAGS if you're building with emerge.

## Using breakpoints

You may find that PaX may prevent `GDB` from setting software breakpoints, depending on how the kernel is configured. This includes the breakpoint at main which you need to get started. There are two workarounds with different effects and constraints to to solve this.

### Solution 1: Removing the RANDEXEC and MPROTECT flags

The first solution is making PaX disable the RANDEXEC and MPROTECT features for the binary to be debugged. To do this you have to set with `paxctl` the `m` and `x` flags on the executable. The `x` flag is set by default, so it should suffice to do:

`root #``/sbin/paxctl -m binary`
After that `GDB` should be able to add software breakpoints on the binary, if it still can't try disabling the SEGMEXEC and PAGEEXEC features (flags `s` and `p` respectively).

`root #``/sbin/paxctl -ps binary`
Below we'll expose what's happening on a lower level when you add a software breakpoint, and why PaX disallows this. You need to know a bit about how processors work in order to understand it. This is not needed to solve your problem so feel free to ignore it.

When the debugger adds a soft breakpoint it changes the instruction on the executable memory image so it is a breakpoint instruction (on x86 and amd64 they are the `bp` and `bu` instructions). This instruction halts the processor and gives the control back to the debugger and has the advantage that it can be set in an unlimited number of points on the program. As PaX disallows writes in executable memory for security reasons it is impossible for the debugger to modify the code and add the breakpoint.

### Solution 2: Using hardware breakpoints

Another solution is using hardware breakpoints, they don't require any changes on PaX behavior, but they are usually limited (for example to a maximum of 4 on x86 and amd64 including address watchpoints) and also have the problem that they require the program to be already running in order to be added (although there is some WIP to fix this in `GDB` ).

To use them just use the `hbreak` instead of the `break` command.

Below we'll expose what's happening on a lower level when you add a hardware breakpoint. You need to know a bit about how processors work in order to understand it. This is not needed to solve your problem so feel free to ignore it.

When the debugger adds a hardware breakpoint it changes some of the processor registers (on x86 and amd64 they are the Dr registers) so the processor halts when a certain address is accessed (either for reading, writing or execution). As a result this implies that no data has to be written in memory solving the soft breakpoints problem, but also limits the number of available breakpoints.

## Restoring the file after debugging

After debugging you may want to restore the system to its normal state, if you used `paxctl` you can reset the flags to default using the `-z` flag. Since the -z flags will zero all the flags also want to keep trampoline emulation disabled. This is done with the `-e` flag.

`root #``paxctl -ze binary`
