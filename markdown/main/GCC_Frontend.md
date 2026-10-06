<!-- source: https://wiki.gentoo.org/wiki/GCC/Frontend | group: Gentoo Wiki (Main) | wiki-title: GCC/Frontend -->
---
title: GCC/Frontend
url: https://wiki.gentoo.org/wiki/GCC/Frontend
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-21"
fingerprint: "863a132d6a7f7a79"
license: CC BY-SA 4.0
---

# GCC/Frontend

From Gentoo Wiki

\< [GCC](https://wiki.gentoo.org/wiki/GCC)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes how to write a [frontend](https://en.wikipedia.org/wiki/Compiler#Front_end) for [GCC](https://wiki.gentoo.org/wiki/GCC) using [JIT](https://gcc.gnu.org/onlinedocs/jit/).

GCC uses 3 main intermediate representation: [GENERIC](https://gcc.gnu.org/onlinedocs/gccint/GENERIC.html), [GIMPLE](https://gcc.gnu.org/onlinedocs/gccint/GIMPLE.html), and [RTL](https://gcc.gnu.org/onlinedocs/gccint/RTL.html). Traditionally, one could write a frontend [using GENERIC](https://thinkingeek.com/2016/01/05/tiny-gcc-front-part-1/). However, this is not a trivial task due to lack of documentation and no C API for GENERIC, only C++. With JIT, it is much easier to write a GCC frontend.

## Prerequisites

GCC must have JIT flag:

FILE **`/etc/portage/package.use/gcc`**

```
sys-devel/gcc jit
```
JIT provides both C API and [C++ API](https://gcc.gnu.org/onlinedocs/jit/cp/). Only the C API will be used for this page.

## Tutorial

Create a C file:

FILE **`usingjit.c`**

```
#include <libgccjit.h>
#include <stdio.h>
#include <stdlib.h>
int main(int argc, char **argv){
    if(argc < 2){
        fprintf(stderr, "Expected file argument\n");
        exit(1);
    }
    // GCC uses the context to store functions, global variables, types and more
    gcc_jit_context *ctxt = gcc_jit_context_acquire ();
    // gcc_jit_type represents a type within the library. These are required for main() and it's return value.
    gcc_jit_type *type_int = gcc_jit_context_get_type(ctxt, GCC_JIT_TYPE_INT);
    gcc_jit_type *type_char_ptr = gcc_jit_type_get_pointer(gcc_jit_context_get_type(ctxt, GCC_JIT_TYPE_CHAR));
    gcc_jit_type *type_char_ptr_ptr = gcc_jit_type_get_pointer(type_char_ptr);
    // gcc_jit_param are function parameters and is use to contruct a function.
    gcc_jit_param *params[2];
    params[0] = gcc_jit_context_new_param(ctxt, NULL, type_int, "argc");
    params[1] = gcc_jit_context_new_param(ctxt, NULL, type_char_ptr_ptr, "argv");
    // GCC_JIT_FUNCTION_EXPORTED makes the function visible, required for main().
    gcc_jit_function *func_main = gcc_jit_context_new_function(ctxt, NULL, GCC_JIT_FUNCTION_EXPORTED, type_int, "main", 2, params, 0);
    // gcc_jit_block is a sequence of statements. The first block is where the function starts. All blocks must be terminated.
    // Blocks can link to other blocks, acting like a linked list.
    gcc_jit_block *block_main = gcc_jit_function_new_block(func_main, NULL);
    // Only required statement in main(). Equal to return 0;
    gcc_jit_block_end_with_return(block_main, NULL, gcc_jit_context_new_rvalue_from_int(ctxt, type_int, 0));
    // Equal to -O2
    gcc_jit_context_set_int_option(ctxt, GCC_JIT_INT_OPTION_OPTIMIZATION_LEVEL, 2);
    // Compile the context to a file
    gcc_jit_context_compile_to_file(ctxt, GCC_JIT_OUTPUT_KIND_EXECUTABLE, argv[1]);
}
```
Compile the file:

`user $``gcc usingjit.c -lgccjit -O2 -o usingjit.exe`
Run the executable to create executable from JIT:

`user $``./usingjit.exe jitoutput.exe`
If everything went well, the executable returns 0:

`user $``./jitoutput.exe``user $``echo $?`
0
