<!-- source: https://wiki.gentoo.org/wiki/GKernelCI | group: Gentoo Wiki (Main) | wiki-title: GKernelCI -->
---
title: GKernelCI
url: https://wiki.gentoo.org/wiki/GKernelCI
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-21"
fingerprint: b792204b0caa0786
license: CC BY-SA 4.0
---

# GKernelCI

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gentoo kernel CI is an autonomous kernel building and testing system based on buildbot.

[https://gkernelci.gentoo.org](https://gkernelci.gentoo.org)  

username: "demo"  

password: "demo"

Contributors desiring a user account should reach out to [Alicef](https://wiki.gentoo.org/wiki/User:Alicef).

## Code

Contributions are welcome!



## Using Github

With Github changes are immediate, there is no need to wait. The result is also display near the commit on GitHub.

#### Testing before committing

Send a pull request toward a branch with the needed changes to [https://github.com/gentoo/linux-patches](https://github.com/gentoo/linux-patches)

### Using Git

With git we are using a time check done every 10 minutes. So the build can take some time before starting.

#### Testing before committing

Making a branch like 4.11\_001 will test the new commit in the new branch:

`root #````
git checkout 4.11
```
`root #``git branch 4.11_001`
Change files:

`root #``git push -u origin 4.11_001`
Check [buildbot](http://kernel1.amd64.dev.gentoo.org:8010/#/console) that is building the pushed branch.

For removing the branch:

`root #``git push origin --delete 4.11_001`
For removing a branch locally:

`root #``git branch -D 4.11_001`
### Reproducing the environment

### Adding a worker

### Stabilization

pushing files to git.gentoo.org/repo/gentoo.git will start to run some test for stabilizing the package.

enabled only for:

sys-kernel/gentoo-sources-\*

If you want to add other packages please ask alicef.

## Todo (if you have any ideas please add them here)

- Testing kernel (more testing, more fun. Using more than one testing suite would be better)
- Adding more Qemu architecture for testing the kernel on more architecture (qemu-system-arm for example)
- Secure the failure trigger on failure (as now there are some false positive)
- Mailing system (in progress)
- Name the steps (in progress)
- ~~auto cleaning feature~~
- Indicate which keywords have been tested and ready to stabilize

## Changelog

### Changelog as been moved to github

2017-12

- auto cleaning feature

2017-08

- Create qemu kernel testing image.
- Build kernel upon github pull request and git push.
- Updated buildbot to 0.9.7.
- Added 4.12 4.11 4.10.
- Added password file for keeping password safe.
