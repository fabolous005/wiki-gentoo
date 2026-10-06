<!-- source: https://wiki.gentoo.org/wiki/Git/Tweaks | group: Gentoo Wiki (Main) | wiki-title: Git/Tweaks -->
---
title: Git/Tweaks
url: https://wiki.gentoo.org/wiki/Git/Tweaks
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-29"
fingerprint: fe83bb501da72bd8
license: CC BY-SA 4.0
---

# Git/Tweaks

[Git](https://wiki.gentoo.org/wiki/Git)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The [devmanual](https://devmanual.gentoo.org/ebuild-maintenance/git/index.html) has some good introductory material on gentoo.git settings and best practices but this page aims to document some neat bonus features and config options available in git.

## Small files

Tell git to optimize for many small files ([https://github.blog/2019-11-03-highlights-from-git-2-24/](https://github.blog/2019-11-03-highlights-from-git-2-24/)).

This enables the 'many files' macro which can change meaning depending on the version of git: it's a metasetting. So, if there are future improvements/improved advice on what's best for repositories with many files, you'll automatically pick it up without modifying individual "real" settings.

Run this within a git repository (need to run this, like all the commands here, once per repository):

`user $````
git config feature.manyFiles true 
```
## watchman

[dev-util/watchman](https://packages.gentoo.org/packages/dev-util/watchman) is a tool by Facebook much like [app-admin/entr](https://packages.gentoo.org/packages/app-admin/entr). git ships with hooks that integrate with watchman to [speed up](https://github.blog/2018-04-05-git-217-released/) commands like git status!

To enable it, first install it:

`root #``emerge --ask dev-util/watchman`
Then run the following within a git repository:

`user $````
cp .git/hooks/fsmonitor-watchman.sample .git/hooks/query-watchman
```
`user $````
chmod +x .git/hooks/query-watchman
```
`user $````
git config core.fsmonitor .git/hooks/query-watchman
```
## Maintenance subcommand

git [since v2.31](https://github.blog/2021-03-15-highlights-from-git-2-31/) supports the git maintenance subcommand. No longer must users run several commands to keep their repositories in good health.

It has support for [cron](https://wiki.gentoo.org/wiki/Cron) and since 2.34, it can make use of [systemd user services](https://wiki.gentoo.org/wiki/Systemd#User_services), so there's no excuse not to use it.

`user $````
git maintenance start # sets the needed config options *and* schedules it to run automatically
```
`user $````
git maintenance run # run it manually as a one-off to get the benefits immediately
```
Note that it doesn't, as of 2.38.1, add git gc to any scheduled runs. To run it weekly:

`user $``git config --global maintenance.gc.schedule weekly`
For systemd, it may be needed to explicitly run:

`user $``git maintenance start --scheduler=systemd-timer`
## Optimizing

There are a few additional options that can be set which *might* help. You should research them first though.

### Parallel checkout

This is off by default (for now?) because it doesn't help on rotational media or other slow disks. It is however worth it on SSDs and such.

Read more in the [github's git v2.33 roundup](https://github.blog/2021-08-16-highlights-from-git-2-33/) or another [writeup](https://matheustavares.gitlab.io/posts/parallel-checkout).

Run the following:

`user $````
git config --global checkout.workers 0
```
Where the '0' denotes that the number of cores should be used.

### Reverse index

This is enabled by default since git 2.41.0. See again [github's git v2.31 roundup](https://github.blog/2021-03-15-highlights-from-git-2-31/).

`user $````
git config pack.writeReverseIndex true
```
`user $````
git repack -Ad
```
### Additional index

See the [discussion](https://lore.kernel.org/git/robbat2-20240507T053331-859497691Z@orbis-terrarum.net/) on the git ML.

`user $````
git commit-graph write --changed-paths --reachable
```
