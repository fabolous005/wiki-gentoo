<!-- source: https://wiki.gentoo.org/wiki/Future_EAPI/DEPENDENCIES_variable | group: Gentoo Wiki (Main) | wiki-title: Future EAPI/DEPENDENCIES variable -->
---
title: Future EAPI/DEPENDENCIES variable
url: https://wiki.gentoo.org/wiki/Future_EAPI/DEPENDENCIES_variable
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-14"
fingerprint: "3e18ef754fa58e95"
license: CC BY-SA 4.0
---

# Future EAPI/DEPENDENCIES variable

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Since we are considering adding new dependency types, some developers are suggesting we should adopt exheres-0 single *DEPENDENCIES* variable.

## Example

A copy-paste from [Exheres for smarties](https://www.exherbo.org/docs/eapi/exheres-for-smarties.html#dependencies):

```
   DEPENDENCIES="
       build+run:
           foo/bar
       run:
           foo/baz
       post:
           foo/monkey
   "
```
And:

```
   DEPENDENCIES="
       blah? ( cat/build-and-run )
       foo? (
           cat/build-and-run
           build:
               cat/build-only
           run:
               cat/run-only
               bar? (
                   cat/run-only
                   post:
                       cat/post-only
               )
               cat/run-only-again
       )
       cat/build-and-run"
```
## Details

Shortly saying:

- *foo:* starts a block of dependencies of type *foo*,
- *foo+bar+baz:* means that the listed package belong to *foo*, *bar* and *baz*-type dependencies at the same time,
- the default block is *build+run*,
- blocks are scoped to the enclosing braces.

Advantages:

- easier way of specifying common dependencies in really weird cases,
- ability to replace one-letter-DEPEND variables with a long, human-readable labels.

Disadvantages:

- necessity of rewriting whole *\*DEPEND* when migrating to the new EAPI,
- necessity of handling two different dependency syntaxes in eclasses,
- added complexity to the dependency syntax parser (and it is spooky already),
- necessity of converting the new block to old-style dependencies -- for compatibility with external tools,
- reinventing the wheel -- introducing a new syntax to avoid proper use of bash variables.

## Feedback

### Default dependency type in a block

The exheres-0 spec assumes that the default dependency type is *run+build*, and so that dependencies before the first label have that type. It may be better to always require an explicit label in the dependency block to make it more readable.

### Use existing (or a similar syntax) for labels

Instead of introducing a completely new syntax for the parser, use one similar to USEdeps.

DEPENDENCIES="
dep\_run? ( cat/monkeys )
dep\_run+build? ( cat/foo )"

or:

DEPENDENCIES="
dep:run? ( cat/monkeys )
dep:run,build? ( cat/foo )"

### Eclass compatibility hack

Instead of requiring all eclasses to support two different dependency syntax per EAPI, we allow them to rot with the old syntax if they like and introduce a compatibility layer translating them in the package manager.

### Less optimal getting of dependencies of given type

The new syntax is likely to introduce additional complexity when trying to obtain the variables of a single given type. With the current type, it is enough to query the particular group. With the new type, it is necessary to either grep the complete list, or keep split up version.

Harrings response: This isn't a valid criticism, to get the deps now requires API access to the PM in some fashion; there's no reason in supporting this, a PM can't provide views of the deps (for example, here are is the tree applicable during build; the manager already will have to be able to do this rendering after all).

## Alternatives

### More meaningful separate variable names

An alternative is to replace existing variables with ones having more meaningful names, or just to introduce the new ones with more meaningful names, e.g.:

- `BADEPEND` → `BUILD_AGAINST_DEPEND`
- `BDEPEND` → `BUILD_DEPEND`
- `DEPEND` → `HOST_DEPEND` (`HOST_BUILD_DEPEND`?)
- `FDEPEND` → `FETCH_DEPEND`
- `IDEPEND` → `INSTALL_DEPEND` (`PKG_DEPEND`?)
- `LDEPEND` → `LIB_DEPEND` (`LINK_DEPEND`?)
- `PDEPEND` → `POST_DEPEND`
- `RDEPEND` → `RUN_DEPEND` (`RUNTIME_DEPEND`?)
- `SDEPEND` → `SUGGESTED_DEPEND`
- …
