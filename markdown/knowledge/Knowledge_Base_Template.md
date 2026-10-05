<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Template | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Template -->
---
title: Knowledge Base:Template
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Template
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2015-03-20"
fingerprint: "76463a341abf97ce"
license: CC BY-SA 4.0
---

# Knowledge Base:Template

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

This section contains information about the issue or topic. It is wise to contain some verbatim information, such as error messages:

`root #``emerge ...`
!!! SELinux module not found. Please verify that it was installed.

## Environment

Describe to the user when the issue described in the "Synopsis" above is indeed applicable in this context.

Be clear in this matter, and use verifiable methods, such as checking output with `grep` or so...

`root #``eselect profile show`
Current /etc/make.profile symlink:
  hardened/linux/amd64/selinux

## Analysis

Provide a detailed analysis of why this issue occurs. If possible, do not simply describe the fix; try to provide insight into *how* and *why* the issue happened so the user can learn from it.

## Resolution

Give, in short sets of commands, instructions that will fix or resolve the issue:

`root #``rlpkg portage`
## See also

(This is an optional part.)

If there are related topics the user can read to gather knowledge located ***inside** this wiki*, then list them each with a bullet point here under the a "See also" section:

- [Knowledge Base](https://wiki.gentoo.org/wiki/Knowledge_Base:Main_Page) - The front page to the Knowledge Base

## External resources

(This is an optional part.)

If there are additional resources that can be view ***outside** this wiki*, list them each with a bullet point here under an "External resources" section:

- Visit the [Gentoo Forums](http://forums.gentoo.org/) to acquire even more knowledge!
