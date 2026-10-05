<!-- source: https://wiki.gentoo.org/wiki/C-Vise | group: Gentoo Wiki (Main) | wiki-title: C-Vise -->
---
title: C-Vise
url: https://wiki.gentoo.org/wiki/C-Vise
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-07-19"
fingerprint: f266514eee9f5fe4
license: CC BY-SA 4.0
---

# C-Vise

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**C-Vise** is a super-parallel Python port of the C-Reduce. The port is fully compatible to the C-Reduce and uses the same efficient LLVM-based C/C++ reduction tool named clang\_delta.

## Installation

### USE flags


### Emerge

To install [dev-util/cvise](https://packages.gentoo.org/packages/dev-util/cvise):

`root #``emerge --ask dev-util/cvise`
## Usage

### Reducing the official example

**C-vise'**s official example is:

**`pr94534.c`**

```
template<typename T>
class Demo
{
  struct
  {
    Demo* p;
  } payload{this};
  friend decltype(payload);
};
int main()
{
  Demo<int> d;
}
```
The issue with this example is it compiles on Clang but not GCC, so to figure out why, a reduction script has to be made.

To make a script to reduce, a simple shell file should suffice:

**`reduce.sh`**

```
#!/bin/sh
g++ pr94534.C -c 2>&1 | grep 'instantiate_class_template_1' && clang++ -c pr94534.C
```
And finally the reduction may be run:

`user $``cvise reduce.sh pr94534.c````
INFO ===< 30356 >===
INFO running 16 interestingness tests in parallel
INFO INITIAL PASSES
INFO ===< IncludesPass >===
...
template <typename> class a {
  int b;
  friend decltype(b);
};
void c() { a<int> d; }
```
## See also

- [GCC\_ICE\_reporting\_guide](https://wiki.gentoo.org/wiki/GCC_ICE_reporting_guide) — guide to debugging **GCC Internal Compiler Errors** (ICEs)
