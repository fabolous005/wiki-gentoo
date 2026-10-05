<!-- source: https://wiki.gentoo.org/wiki/Eclass_testing | group: Gentoo Wiki (Main) | wiki-title: Eclass testing -->
---
title: Eclass testing
url: https://wiki.gentoo.org/wiki/Eclass_testing
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-22"
fingerprint: fa8f9ad06d254a7e
license: CC BY-SA 4.0
---

# Eclass testing

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a basic guide to write tests for Gentoo Eclasses.

## Testing framework

In order to test eclasses, you need to source the eclass/tests/tests-common.sh file in the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Gentoo_ebuild_repository). It sets up a testing environment, provides replacements for commonly used functions and adds helpers for running tests.

Tests are bash scripts that can be located in any directory, although in this guide eclass/tests will be assumed.

### Sourcing

In the ::gentoo repo, it's as simple as:

In other repositories, use [portageq](https://wiki.gentoo.org/wiki/Portageq) to locate the file:

Note the `TESTS_ECLASS_SEARCH_PATHS` variable. Without it you won't be able to inherit Gentoo eclasses.

### Overview of helpers

| Function | Description | Example use | 
|---|---|---|
| tbegin `message` | ebegin wrapper for a test case. |  | 
| t `cmd` | Run `cmd` and set the suite's exit status to non-zero if it failed. |  | 
| tend `status` | eend wrapper for a test case. |  | 
| texit | Clean up and set the suite's exit status. Call it at the end of your test suite. |  |
