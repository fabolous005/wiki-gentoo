<!-- source: https://wiki.gentoo.org/wiki/Git-lfs | group: Gentoo Wiki (Main) | wiki-title: Git-lfs -->
---
title: git-lfs
url: https://wiki.gentoo.org/wiki/Git-lfs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-12-10"
fingerprint: e547b898b58b0ed6
license: CC BY-SA 4.0
---

# git-lfs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


Git **L**arge **F**ile **S**torage (LFS) is an open source plugin created by GitHub that enables the [git](https://wiki.gentoo.org/wiki/Git) version control system to better track binary blobs. It does so by creating a text-based reference to the blob, then tracking and storing the blob in a location external to the git repository itself; typically on a content server.

## Installation

### Emerge

`root #``emerge --ask dev-vcs/git-lfs`
### Configuration

In order to use git-lfs, your \~/.gitconfig file must be setup with the appropriate filters. Run the following command to do this automatically.

`user $``git lfs install --skip-repo`
## Usage

Binary files must be tracked by file extension. This enables git LFS to make proper distinctions between binary and non-binary files.

GitHub has released a video on YouTube explaining how to utilize git LFS: [https://www.youtube.com/watch?v=uLR1RNqJ1Mw](https://www.youtube.com/watch?v=uLR1RNqJ1Mw)

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose dev-vcs/git-lfs`
You will want to manually remove everything under \[filter "lfs"\] in \~/.gitconfig.

## See also

- [Git](https://wiki.gentoo.org/wiki/Git) — widely used, open source, distributed [version control system](https://wiki.gentoo.org/wiki/Version_control_systems)
