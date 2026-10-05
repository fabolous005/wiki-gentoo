<!-- source: https://wiki.gentoo.org/wiki/Darcs | group: Gentoo Wiki (Main) | wiki-title: Darcs -->
---
title: darcs
url: https://wiki.gentoo.org/wiki/Darcs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-08"
fingerprint: fb1cb77e8f8faeed
license: CC BY-SA 4.0
---

# darcs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Darcs** (**darcs** is a distributed version control system created by David Roundy. Contrary to other systems like git that are snapshot-based, Darcs is patched-based. As such, a repository can be seen as a set of patches, where each patch is not necessarily ordered with respect to other patches.



## Installation

### Emerge

`root #``emerge --ask dev-vcs/darcs`
## Configuration

### Files

## Usage

### Invocation

## Tips

## Troubleshooting

Darcs is case-insentive by default so it will refuse to add a file that has it case changed. This can be solved with

`user $``darcs add --case-ok`
Beware that it can lead to strange behaviours on case-insentivie filesystems (like [Windows](https://learn.microsoft.com/en-us/windows/wsl/case-sensitivity)).

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose dev-vcs/darcs`
## See Also

- [Git](https://wiki.gentoo.org/wiki/Git) — widely used, open source, distributed [version control system](https://wiki.gentoo.org/wiki/Version_control_systems)
- [Subversion](https://wiki.gentoo.org/index.php?title=Subversion&action=edit&redlink=1)
