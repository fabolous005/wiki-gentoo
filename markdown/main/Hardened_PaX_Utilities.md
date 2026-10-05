<!-- source: https://wiki.gentoo.org/wiki/Hardened/PaX_Utilities | group: Gentoo Wiki (Main) | wiki-title: Hardened/PaX Utilities -->
---
title: Hardened/PaX Utilities
url: https://wiki.gentoo.org/wiki/Hardened/PaX_Utilities
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2016-11-13"
fingerprint: adc6187f7553738c
license: CC BY-SA 4.0
---

# Hardened/PaX Utilities

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide provides instruction on securing systems by using the [app-misc/pax-utils](https://packages.gentoo.org/packages/app-misc/pax-utils) package to find and identify problematic binaries.

## What is this guide about?

### Introduction

The security of a system goes beyond setting up a decent firewall and good service configurations. The binaries being run and the libraries that are loaded might also be vulnerable against attacks. Although the exact vulnerabilities are not known until they are discovered, there are methods used to help prevent attacks from happening.

One possible attack vector is to make advantage of writable *and* executable segments in a program or library, allowing malicious users to run their own code using the vulnerable application or library.

This guide will inform the reader on how to use the [app-misc/pax-utils](https://packages.gentoo.org/packages/app-misc/pax-utils) package to find and identify problematic binaries. The use of pspax (a tool to view PaX-specific capabilities) and dumpelf (a tool that prints out a C structure containing a workable copy of a given object) will also be covered.

But before we start with that, some information on *objects* is in place. Users familiar with segments and dynamic linking will not learn anything from this and can immediately continue with [Extracting ELF Information from Binaries](https://wiki.gentoo.org#Extracting_ELF_Information_from_Binaries).

### ELF objects

Every executable binary on the system is structured in a specific way, allowing the Linux kernel to load and execute the file. Actually, this goes beyond plain executable binaries: this also holds for shared objects; more about those later.

The structure of such a binary is defined in the ELF standard. ELF stands for *Executable and Linkable Format*. For readers who are really interested in the gory details, check out the [Generic ELF spec](http://refspecs.linux-foundation.org/LSB_4.0.0/LSB-Core-generic/LSB-Core-generic/elf-generic.html) or the elf(5) man page.

An executable ELF file has the following parts:

- The *ELF header* contains information on the *type* of file (is it an executable, a shared library, ...), the target architecture, the location of the Program Header, Section Header and String Header in the file and the location of the first executable instruction
- The *Program Header* informs the system how to create a process from the binary file. It is actually a table consisting of entries for each segment in the program. Each entry contains the type, addresses (physical and virtual), size, access rights, ...
- The *Section Header* is a table consisting of entries for each section in the program. Each entry contains the name, type, size, ... and *what* information the section holds.
- Data, containing the sections and segments mentioned previously.

A *section* is a small unit consisting of specific data: instructions, variable data, symbol table, relocation information, and so on. A *segment* is a collection of sections; segments are the units that are actually transferred to memory.

Way back when, every application binary contained *everything* it needed to operate correctly. Such binaries are called *statically linked* binaries. They are, however, space consuming since different applications use the same functions over and over again.

A *shared object* contains the definition and instructions for such functions. Every application that wants can *dynamically* link against such a shared object so that it can benefit from the already existing functionality.

An application that is dynamically linked to a shared object contains *symbols*, references for the real functionality. When such an application is loaded in memory, it will first ask the runtime linker to resolve each and every symbol it has. The runtime linker will load the appropriate shared objects in memory and resolve the symbolic references between them.

### Segments and sections

How the ELF file is looked upon depends on the view we have: when we are dealing with a binary file in Execution View, the ELF file contains segments. When the file is seen in Linking View, the ELF file contains sections. One segment spans just one or more (continuous) sections.

## Extracting ELF information from binaries

### The scanelf application

The scanelf application is part of the [app-misc/pax-utils](https://packages.gentoo.org/packages/app-misc/pax-utils) package. With this application it is possible to print out information specific to the ELF structure of a binary. The following table sums up the various options.

| Option | Long option | Description | 
|---|---|---|
| `-p` | `--path` | Scan all directories in PATH environment | 
| `-l` | `--ldpath` | Scan all directories in /etc/ld.so.conf | 
| `-R` | `--recursive` | Scan directories recursively | 
| `-m` | `--mount` | Don't recursively cross mount points | 
| `-y` | `--symlink` | Don't scan symlinks | 
| `-A` | `--archives` | Scan archives (.a files) | 
| `-L` | `--ldcache` | Utilize ld.so.cache information (use with `-r` or `-e`). | 
| `-X` | `--fix` | Try and 'fix' bad things (use with `-r` or `-e`). | 
| `-z <arg>` | `--setpax <arg>` | Sets EI\_PAX/PT\_PAX\_FLAGS to \[arg\] (use with -Xx) | 
| Option | Long option | Description | 
| `-x` | `--pax` | Print PaX markings | 
| `-e` | `--header` | Print GNU\_STACK/PT\_LOAD markings | 
| `-t` | `--textrel` | Print TEXTREL information | 
| `-r` | `--rpath` | Print RPATH information | 
| `-n` | `--needed` | Print NEEDED information | 
| `-i` | `--interp` | Print INTERP information | 
| `-b` | `--bind` | Print BIND information | 
| `-S` | `--soname` | Print SONAME information | 
| `-s <arg>` | `--symbol <arg>` | Find a specified symbol | 
| `-k <arg>` | `--section <arg>` | Find a specified section | 
| `-N [arg]` | `--lib [arg]` | Find a specified library | 
| `-g` | `--gmatch` | Use strncmp to match libraries (use with `-N`). | 
| `-T` | `--textrels` | Locate cause of TEXTREL | 
| `-E <arg>` | `--etype <arg>` | Print only ELF files matching etype ET\_DYN,ET\_EXEC ... | 
| `-M <arg>` | `--bits <arg>` | Print only ELF files matching numeric bits | 
| `-a` | `--all` | Print all scanned info (-x -e -t -r -b) | 
| Option | Long option | Description | 
| `-q` | `--quiet` | Only output 'bad' things | 
| `-v` | `--verbose` | Be verbose (can be specified more than once) | 
| `-F <arg>` | `--format <arg>` | Use specified format for output | 
| `-f <arg>` | `--from <arg>` | Read input stream from a filename | 
| `-o <arg>` | `--file <arg>` | Write output stream to a filename | 
| `-B` | `--nobanner` | Don't display the header | 
| `-h` | `--help` | Print this help and exit | 
| `-V` | `--version` | Print version and exit | 

The format specifiers for the `-F` option are given in the following table. Prefix each specifier with `%` (verbose) or `#` (silent) accordingly.

| Specifier | Full name | Specifier | Full name | 
|---|---|---|---|
| F | Filename | x | PaX Flags | 
| e | STACK/RELRO | t | TEXTREL | 
| r | RPATH | n | NEEDED | 
| i | INTERP | b | BIND | 
| s | Symbol | N | Library | 
| o | Type | p | File name | 
| f | Base file name | k | Section | 
| a | ARCH/e\_machine |  |  | 

### Using scanelf for text relocations

As an example, we will use scanelf to find binaries containing text relocations.

A relocation is an operation that rewrites an address in a loaded segment. Such an address rewrite can happen when a segment has references to a shared object and that shared object is loaded in memory. In this case, the references are substituted with the real address values. Similar events can occur inside the shared object itself.

A text relocation is a relocation in the text segment. Since text segments contain executable code, system administrators might prefer not to have these segments writable. This is perfectly possible, but since text relocations actually write in the text segment, it is not always feasible.

When desiring to eliminate text relocations, make sure that the application and shared object is built with *Position Independent Code* (PIC), making references obsolete. This not only increases security, but also increases the performance in case of shared objects (allowing writes in the text segment requires a swap space reservation and a private copy of the shared object for each application that uses it).

The following example will search library paths recursively, without leaving the mounted file system and ignoring symbolic links, for any ELF binary containing a text relocation:

`root #``scanelf -lqtmyR`
To scan the entire system for *any* file containing text relocations:

`root #``scanelf -qtmyR /`
### Using scanelf for specific header

The scanelf utility can be used to quickly identify files that contain a given section header using the `-k` .section option.

In this example we are looking for all files in /usr/lib/debug recursively using a format modifier with quiet mode enabled that have been stripped. A stripped ELF will lack a .symtab entry, so we use the '!' to invert the matching logic.

`root #``scanelf -k '!.symtab' /usr/lib/debug -Rq -F%F#k`


### Using scanelf for specific segment markings

Each segment has specific flags assigned to it in the Program Header of the binary. One of those flags is the type of the segment. Interesting values are PT\_LOAD (the segment must be loaded in memory from file), PT\_DYNAMIC (the segment contains dynamic linking information), PT\_INTERP (the segment contains the name of the program interpreter), PT\_GNU\_STACK (a GNU extension for the ELF format, used by some stack protection mechanisms), and PT\_PAX\_FLAGS (a PaX extension for the ELF format, used by the security-minded [PaX Project](https://pax.grsecurity.net/).

If we want to scan all executables in the current working directory, PATH environment and library paths and report those who have a writable and executable PT\_LOAD or PT\_GNU\_STACK marking, use the following command can be used:

`root #``scanelf -lpqe .`
### Using scanelf's format modifier handler

A useful feature of the scanelf utility is the format modifier handler. With this option the output of scanelf can be controlled, thereby simplifying parsing the output with scripts.

As an example scanelf will be used to print the file names that contain text relocations:

`root #``scanelf -l -p -R -q -F "%F #t"`
## Listing PaX flags and capabilities

### About PaX

[PaX](https://pax.grsecurity.net) is a project hosted by the [grsecurity](https://www.grsecurity.net) project. Quoting the [PaX documentation](https://pax.grsecurity.net/docs/pax.txt), its main goal is "to research various defense mechanisms against the exploitation of software bugs that give an attacker arbitrary read/write access to the attacked task's address space. This class of bugs contains among others various forms of buffer overflow bugs (be they stack or heap based), user supplied format string bugs, etc."

To be able to benefit from these defense mechanisms a Linux kernel patched with the latest PaX code is required. The [Hardened Gentoo](https://wiki.gentoo.org/wiki/Project:Hardened) project supports PaX and its parent project grsecurity. The supported kernel package is [sys-kernel/hardened-sources](https://packages.gentoo.org/packages/sys-kernel/hardened-sources).

The Gentoo Hardened project has a [Gentoo PaX Quickstart Guide](https://wiki.gentoo.org/wiki/Project:Hardened/PaX_Quickstart) for the reader's pleasure.

### Flags and capabilities

If the system toolchain supports it binaries can have additional PaX flags in their program header. The following flags are supported:

| Flag | Name | Description | 
|---|---|---|
| `P` | PAGEEXEC | Refuse code execution on writable pages based on the NX bit (or emulated NX bit) | 
| `S` | SEGMEXEC | Refuse code execution on writable pages based on the segmentation logic of IA-32 | 
| `E` | EMUTRAMP | Allow known code execution sequences on writable pages that should not cause any harm | 
| `M` | MPROTECT | Prevent the creation of new executable code to the process address space | 
| `R` | RANDMMAP | Randomize the stack base to prevent certain stack overflow attacks from being successful | 
| `X` | RANDEXEC | Randomize the address where the application maps to prevent certain attacks from being exploitable | 

The default Linux kernel also supports certain capabilities, grouped in the so-called *POSIX.1e capabilities*. A listing of those capabilities can be found in the [POSIX Capabilities](https://wiki.gentoo.org/wiki/Hardened/Overview_of_POSIX_capabilities) article.

### Using pspax

The pspax application, which is part of the [app-misc/pax-utils](https://packages.gentoo.org/packages/app-misc/pax-utils) package, displays the run-time capabilities of all programs the user has permission to run. On Linux kernels with additional support for extended attributes (such as SELinux) those attributes are shown as well.

When ran, pspax shows the following information:

| Column | Description | 
|---|---|
| USER | Owner of the process | 
| PID | Process id | 
| PAX | Run-time PaX flags (if applicable) | 
| MAPS | Write/eXecute markings for the process map | 
| ELF\_TYPE | Process executable type: ET\_DYN or ET\_EXEC | 
| NAME | Name of the process | 
| CAPS | POSIX.1e capabilities (see note) | 
| ATTR | Extended attributes (if applicable) | 

By default, pspax does not show any kernel processes. For those to be taken as well use the `-a` option.

## Programming with ELF files

### The dumpelf utility

The dumpelf utility can convert a ELF file into human readable C code that defines a structure with the same image as the original ELF file.

`user $``dumpelf /bin/hostname````
#include <elf.h>
  
## /*
 * ELF dump of '/bin/hostname'
 *     10276 (0x2824) bytes
 */
  
struct {
        Elf32_Ehdr ehdr;
        Elf32_Phdr phdrs[8];
        Elf32_Shdr shdrs[26];
} dumpedelf_0 = {
  
.ehdr = {
## (... Output stripped ...)
```
