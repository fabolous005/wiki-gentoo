<!-- source: https://wiki.gentoo.org/wiki/Gentoo_git_workflow | group: Gentoo Wiki (Main) | wiki-title: Gentoo git workflow -->
---
title: Gentoo git workflow
author: Retaining commit author information
url: https://wiki.gentoo.org/wiki/Gentoo_git_workflow
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-17"
fingerprint: "36b9b9c6fee84999"
license: CC BY-SA 4.0
---

# Gentoo git workflow

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article outlines some rules and best-practices regarding the typical workflow for ebuild developers using git.

Developers are encouraged to use [pkgcheck](https://wiki.gentoo.org/wiki/Pkgcheck) and [pkgdev](https://wiki.gentoo.org/wiki/Pkgdev).

## Commit policy

- Atomic commits (one logical change).
- All commits must be pkgcheck-valid (exceptions only on right-hand branches).
- Commits may span across multiple ebuilds/directories if it's one logical change.
- Every commit on the left-most line of the history (that is, all the commits following the first parent of each commit) must be GPG signed by a Gentoo developer.
- pkgcheck scan must be run from all related ebuild directories (or related category directories or top-level directory) on the tip of the local master branch (as in: right before pushing and also after resolving push-conflicts).

### Atomicity

- Commits in git are cheap and local, so use them often.
- Split version bumps and ebuild cleanup in separate commits (makes reverting ebuild removals much easier).
- One may break atomicity in order to make a commit pkgcheck-valid (or reconsidering the order you commit in, e.g. license first, then the new ebuild).

### Commit message format

- All lines max 75 characters.
- If `CATEGORY/PN` is very long and the 75 character limit is impossible to obey, it is OK to exceed the limit in this case.
- First line brief explanation.
- Second line *always* empty.
- Optional detailed multiline explanation must start at the third line.
- Commits that affect primarily a particular subsystem should prepend the following code to the first line of the commit message:
  - Single package -> `CATEGORY/PN:`
  - Profile directory -> `profiles:`
  - Eclass directory -> `ECLASSNAME.eclass:`
  - Licenses directory -> `licenses:`
  - Metadata directory -> `metadata:`
  - A whole category -> `CATEGORY:`
- If the change affects multiple directories, but is mostly related to a particular subsystem, then prepend the subsystem which best reflects the intention (e.g. you add a new license, but also modify profiles/license\_groups).
- It is also encouraged to use tag formats such as `Acked-by:`, `Suggested-by:`, and so on. Please review the [kernel patch guideline](https://www.kernel.org/doc/Documentation/process/submitting-patches.rst) for additional tag variants.

**Example**

app-misc/foo: version bump to 0.5
Bump to 0.5. This also fixes security
bug 93829 and introduces the new USE flag 'bar'.
Bug: https://bugs.gentoo.org/93829
Acked-by: Hans Wurst \<hans@gentoo.org>
Reported-by: Alice Wonderland \<alice@example.com>
Signed-off-by: A Developer \<example@gentoo.org>

## Branching model

- The primary production-ready branch is [master](https://gitweb.gentoo.org/repo/gentoo.git/log/) (users will pull from here), there are **only fast-forward** pushes allowed.
- There may be developer-specific, task-specific, project-specific branches, etc.

### Branch naming convention

- Developer branches: `dev/<name>`
- Project branches: `project/<name>`
- If in doubt, or if the branch could be useful to others, discuss the naming on-list beforehand.

### About rebasing

- Primary use case: in case of a non-fast-forward push conflict to remote master, try git pull --rebase=preserve first; if that yields complicated conflicts, abort the rebase and continue with a regular merge (if the conflicts are trivial or even expected, e.g. arch teams keywording/stabilizing stuff, then stick to the rebase).
- To preserve merges during a rebase use git rebase --preserve-merges ... (if appropriate, e.g. for user branches).
- Don't use `--preserve-merges` if you do an interactive rebase (see BUGS in git-rebase manpage).
- Commits that are not on the remote master branch yet may be rewritten/squashed/splitted etc via interactive rebase, however the rebase must never span beyond those commits.
- Never rebase on already pushed commits.
- There are no particular rules for rebasing on non-master remote branches, but be aware that others might base their work on them.
- There are no particular rules for rebasing non-remote branches, as long as they don't clutter the history when merged back into master.

### About merging

- **Do not *ever* commit implicit merges done by git pull**. It is possible to set git config --local pull.ff only to avoid git implicitly creating those.
- We essentially never use merge commits in ::gentoo. GLEP 66 does [allow](https://www.gentoo.org/glep/glep-0066.html#id6) for them in some cases but this isn't used in practice.

## Remote model

We have a main developer repository where developers work and commit (every Gentoo ebuild developer has direct push access). For every push into the repository, automated magic things merge stuff into user sync repository and update the metadata cache there.

User sync repository is for power users that want to fetch via git. It's quite fast and efficient for frequent updates, and also saves space by being free of the ChangeLog files.

On top of the user sync repository, rsync is propagated.

## Best practices

- Before starting work on a local master, it's good to pull the latest changeset from the remote master.
- It might be a good idea for projects/developers to accumulate changes either in their own branch or a separate repository and only push to remote master in intervals (that decreases the push rate and potential conflicts).

## Repository settings

### Summary

This is a summary of all the needed commands:

`user $````
git config --local user.signingkey 0xLONG-GPG-KEY
```
`user $````
git config --local commit.gpgsign 1
```
`user $````
git config --local push.gpgsign 1
```
`user $````
git config --local pull.rebase true
```
`user $``git config --local --unset pull.ff`
For details, read on!

### Cloning

Clone the repository. This will make a shallow clone and speed up clone time:

`user $``git clone --depth=50 git+ssh://git@git.gentoo.org/repo/gentoo.git`
Using the git+ssh:// protocol of course requires the user's SSH public key on the server.
To get the full history, just omit `--depth=50` from the above command.

To make contributions via pull-requests, fork the repository (e.g. via GitHub) and then add it as a remote to a local git clone:

`user $``git remote add fork ssh://git@github.com:yourusername/gentoo.git`
### Configuration

All developers should at least have the following configuration settings in their local developer repository. These setting will be written to .git/config and can also be edited manually. Run these from within the repository cloned in the step above:

`user $````
cd gentoo
```
`user $````
git config --local user.name "Your Full Name"
```
`user $````
git config --local user.email "example@gentoo.org"
```
`user $````
git config --local pull.rebase true
```
`user $````
git config --local --unset pull.ff
```
`user $``git config --local push.default simple`
#### Signoff

This is separate from signing commits or pushes. Gentoo requires that contributions comply with [GLEP 76](https://www.gentoo.org/glep/glep-0076.html) and the [Certificate of Origin](https://www.gentoo.org/glep/glep-0076.html#certificate-of-origin) is provided by contributors.

For commits or git operations outside of [pkgdev](https://wiki.gentoo.org/wiki/Pkgdev), some more (optional) work is needed.

Have git enable it for git format-patch too:

`user $````
cd gentoo
```
`user $``git config --local format.signoff yes`
But git does not support forcing it for git commit. Instead, git commit -s must be used. There are workarounds for this on Stack Overflow [here](https://stackoverflow.com/questions/41881722/how-can-i-force-git-commit-s-using-git-commit-command).

#### GPG configuration

##### Creating a key

Technically, it's not required to have a key as a non-developer, but metadata/layout.conf in ::gentoo requires signed commits from people pushing. It's therefore easier with tooling to just use gpg and set it up even if technically as a contributor, commits don't need to be signed (as the developer pushing it will sign their commits).

##### Setting it up

To get the GPG key run this command. It should be the top line (starting with pub). If there is more than one key with the UID it will need to be selected manually (from the list of returned keys).

`user $``gpg --list-public-keys --keyid-format 0xlong example@gentoo.org``user $````
git config --local user.signingkey 0xLONG-GPG-KEY
```
`user $````
git config --local commit.gpgsign 1
```
`user $````
git config --local push.gpgsign 1
```
## Workflow walkthrough

![Cycle of usercontributions.png](https://wiki.gentoo.org/images/thumb/6/6c/Cycle_of_usercontributions.png/300px-Cycle_of_usercontributions.png)

These are just examples and people may choose different local workflows (especially in terms of when to stage/commit) as long as the end result works and is pkgcheck-checked. These examples try to be very safe, but basic.

Before doing anything, make sure the local git tree is in a correct state and the correct branch is checked out.

`user $``git status`
### Common ebuild work

1. Pull in the latest changeset, before starting work: `user $``git pull --rebase origin master`
2. Do the work (including removing files)
3. Confirm the ebuild directory is the current working directory
4. Create the manifest: `user $``pkgdev manifest`
5. Stage files (including removed ones), if any: `user $``git add <new-files> <changed-files> <removed-files>`
6. Check for errors: `user $``pkgcheck scan`
  1. If errors occur, fix them and continue from point 4
7. Commit the files: `user $``pkgdev commit -s` and enter a meaningful commit message
8. Push to the dev repository: `user $``git push --signed origin master`
  1. If updates were rejected because of non-fast-forward push, try: `user $``git pull --rebase=merges -S"$(git config --get user.signingkey)" origin master` first, then run: `user $``pkgcheck scan --commits` and continue from point 8.
    1. If the rebase fails, but the conflicts are trivial and don't contain useful information (such as keyword stabilization), fix the conflicts and finish the rebase via: `user $``git mergetool``user $``git rebase --continue` and continue from point 4
    2. If the rebase fails and the conflicts are complicated or you think the information is useful, continue with a regular merge: `user $``git rebase --abort``user $``git merge remotes/origin/master`
      1. If merge conflicts occur, fix them via: `user $``git mergetool` and continue from point 4
      2. If no merge conflicts occur, run: `user $``pkgcheck scan --commits` and continue from point 8 above.

### Pull requests from developers and users

#### using pram

[app-portage/pram](https://packages.gentoo.org/packages/app-portage/pram) is a tool to help merge pull requests easily.

1. Open the pull request assigned for you, or whatever you want to work on. We use [https://github.com/gentoo/gentoo/pull/12345](https://github.com/gentoo/gentoo/pull/12345) as an example.
2. Make sure that everything is done correctly and that every commit has proper sign-off included.
3. Apply and automatically close the pull request to ::gentoo repo via `user $``git pull``user $``pram 12345``user $``git push`

#### git am method

1. Find a pull request you wish to merge, [https://github.com/gentoo/gentoo/pull/1](https://github.com/gentoo/gentoo/pull/1) as an example.
2. Ensure your checkout is up to date: `user $``git pull`
3. Fetch and apply chosen commit:
4. You can review the changes with: `user $``git log remotes/origin/master..HEAD``user $``git diff remotes/origin/master..HEAD`
5. Make sure chosen commit didn't break things `user $``pkgcheck scan --commits`
6. Push your changes `user $``git push --signed`

#### git cherry-pick method

1. Identify the remote URL and the branch to be merged.
2. Add a new remote: `user $``git remote add <remote-name> <url>`
3. Fetch the changes: `user $``git fetch <remote-name> <branch>`
4. You may review the changes manually first: `user $``git log master..remotes/<remote-name>/<branch>``user $``git diff master..remotes/<remote-name>/<branch>`
5. Checkout the remote branch: `user $``git checkout remotes/<remote-name>/<branch>` (you are now in detached HEAD mode)
6. Test the ebuilds and run pkgcheck: `user $``pkgcheck scan`
7. If everything is fine, switch back to master: `user $``git checkout master`
8. Ensure your master branch is curent: `user $``git pull`
9. If it's just one commit, do: `user $``git cherry-pick <commit-hash>`
10. If there are multiple commits in one consistent range, do: `user $``git cherry-pick <first-new-commit-hash>^..<latest-new-commit-hash>`
11. Push your changes `user $``git push --signed`

## Tips and tricks

### Working with git

Staging files can be tedious, especially on the CLI. To mitigate this, it is possible to use the graphical clients gitk (for browsing history) and git gui (for staging/unstaging changes etc.). This will require enabling the `tk` USE flag for [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git).

### Diffs for revision bumps

Suppose you are making a new revision of the `app-foo/bar` package—you are removing `app-foo/bar-1.0.0` and replacing it with the new revision `app-foo/bar-1.0.0-r1`. In that case, git (by default) sees two changes: removal of the file `bar-1.0.0.ebuild` and addition of the file `bar-1.0.0-r1.ebuild`. If you attempt to view your changes with `git diff` or `git show`, you'll get a useless diff containing the entirety of both files.

Git can be coerced into showing the diff between the old and new revisions even though they live in separate files. The following command-line options specify how hard git should try to find files that have been copied or renamed:

`user $``git diff --find-renames --find-copies --find-copies-harder`
In the common case, searching for renames is enough. To make that behavior the default easily:

`user $``git config --local diff.renames true`
### Using the gentoo git checkout as a local tree

To use a git development checkout as a local tree, two things are required:

1. make sure the directory has the correct access rights
2. generate/get metadata-cache, dtd, glsa, projects.xml, and news yourself

For the latter, there are example hooks for [portage](https://github.com/hasufell/portage-gentoo-git-config) and [paludis](https://github.com/hasufell/paludis-gentoo-git-config). You should probably skip the files *[/etc/portage/repos.conf](https://wiki.gentoo.org/wiki//etc/portage/repos.conf)/gentoo.conf* and */etc/paludis/repositories/gentoo.conf* respectively, so that portage/paludis doesn't mess up your checkout (as in: disable auto-sync).

### Github pull request made easy

cd into your gentoo Git repository and add these lines to the repo's .git/config file:

\[remote "github"\]
  url = git@github.com:gentoo/gentoo.git
  fetch = +refs/heads/\*:refs/remotes/github/\*
  fetch = +refs/pull/\*/head:refs/remotes/github/pr/\*

Now type:

`user $``git fetch github`
git should go and fetch all pull requests, and list them as branches.

`user $``git branch -a`
Each pull request will now be displayed as a branch name, each with a different name such as "remotes/github/pr/123". For instance if you wish to fetch changes brought by [https://github.com/gentoo/gentoo/pull/105](https://github.com/gentoo/gentoo/pull/105), simply type:

`user $``git checkout remotes/github/pr/105`
to checkout the pull request.

### Grafting Gentoo history onto the active repository

To graft the historical Gentoo repository onto your current one simply run:

`user $``git fetch historical``user $``git replace --graft 56bd759df1d0c750a065b8c845e93d5dfa6b549d cvs-final-2015-08-08`
The general syntax of the last command is:

`user $``git replace --graft <first new commit> <last old commit>`
(This may be useful if you're not using the official versions of the repositories.)

Once the history is merged, git will behave as if it is one big repo, and it should still be possible to push from that to Gentoo as the head is untouched. Merging the history will remove the gpg signature on the initial gentoo commit (since it is being modified to do the graft).

### Preventing 'git repack -a' from touching huge packs

Normally git repack -a (invoked by git gc) tries to repack everything into a single pack. If the local repository contains a few huge packs already (commits from initial clone, grafted history pack), the repack can take really long and be truly memory-consuming.

To avoid that, mark the huge pack files for keeping. In this case, git repack -a won't touch those packs and instead focus on repacking everything else. As a result, you can get most of the benefits of repacking with major time saving.

`user $``cd .git/refs/packs``user $``ls -lh`
total 2.2G
-r--r--r-- 1 mgorny mgorny  17M Oct  3 16:14 pack-05c10cc8ef170fd182619ef32ec4007e5f32f46d.idx
-r--r--r-- 1 mgorny mgorny 219M Oct  5 16:12 pack-05c10cc8ef170fd182619ef32ec4007e5f32f46d.pack
-r--r--r-- 1 mgorny mgorny  27K Oct  5 14:09 pack-107a6ca6e4b781d12cc452a651a478daac3b3d95.idx
-r--r--r-- 1 mgorny mgorny 978K Oct  5 16:12 pack-107a6ca6e4b781d12cc452a651a478daac3b3d95.pack
-r--r--r-- 1 mgorny mgorny  25K Oct  6 08:14 pack-10f2b4a18cd1a99dcbc502987eb130680221b558.idx
-r--r--r-- 1 mgorny mgorny 770K Oct  6 08:14 pack-10f2b4a18cd1a99dcbc502987eb130680221b558.pack
-r--r--r-- 1 mgorny mgorny 197M Oct  3 18:06 pack-412c2dda845a79854872afaf5f9cd4ea896aef38.idx
-r--r--r-- 1 mgorny mgorny 1.7G Oct  3 18:05 pack-412c2dda845a79854872afaf5f9cd4ea896aef38.pack
-r--r--r-- 1 mgorny mgorny 5.1K Oct  5 15:38 pack-460c1a0fc0e6fc76d367e2b4010c879ce9b062e2.idx
-r--r--r-- 1 mgorny mgorny 187K Oct  5 16:12 pack-460c1a0fc0e6fc76d367e2b4010c879ce9b062e2.pack
-r--r--r-- 1 mgorny mgorny 1.4M Oct  6 08:49 pack-5a593371028f920159f628208955707a9153962a.idx
-r--r--r-- 1 mgorny mgorny  24M Oct  6 08:49 pack-5a593371028f920159f628208955707a9153962a.pack
-r--r--r-- 1 mgorny mgorny  14K Oct  4 08:41 pack-c90e9378fa114d9a603c3ea40c4ff76e2d4cbac6.idx
-r--r--r-- 1 mgorny mgorny 401K Oct  5 16:12 pack-c90e9378fa114d9a603c3ea40c4ff76e2d4cbac6.pack

Once you identify the huge packs you'd like to keep (`05c10cc8ef170fd182619ef32ec4007e5f32f46d` and `412c2dda845a79854872afaf5f9cd4ea896aef38` in this case), create .keep files for them:

`user $``touch pack-05c10cc8ef170fd182619ef32ec4007e5f32f46d.keep pack-412c2dda845a79854872afaf5f9cd4ea896aef38.keep`
It is important that appropriate credit is given when committing on behalf of others. Git can natively differentiate between who authored a commit and who pushed it ([example](https://gitweb.gentoo.org/repo/gentoo.git/commit/?id=f6485aff0129ae4c8df5a211af1a51d03ecb6de4)), and most methods of merging handle this automatically.

If you have applied someone else's changes manually or made local edits to a PR, check that the author field is accurate:

`user $``git log --format=fuller`
If necessary, amend the commit to give credit where it is due:

`user $``git commit --amend --author="Joe Smith <jsmith@example.org>"`
If it is necessity to credit several authors, add trailer lines like `Co-authored-by: Name Surname <nsurname@example.org>` (as currently supported by major Git hosts like GitHub and GitLab) to the long commit description part:

app-misc/foo: version bump to 0.5
Bump to 0.5. This also fixes security
bug 93829 and introduces the new USE flag 'bar'.
Bug: https://bugs.gentoo.org/93829
Acked-by: Hans Wurst \<hans@gentoo.org>
Co-authored-by: Jane Doe \<jdoe@example.org>
Reported-by: Alice Wonderland \<alice@example.com>
Signed-off-by: Joe Smith \<jsmith@example.org>
Signed-off-by: A Developer \<example@gentoo.org>

### A cli-tool to crawl through pull requests

[gengee](https://github.com/wimmuskee/gengee) is a tool that makes queries through [gentoo/gentoo/ pull requests](https://github.com/gentoo/gentoo/pulls). It can show "outdated" pull requests, ie where a newer version is added to ::gentoo tree, where a linked bug is already closed, or where package in question has been removed from ::gentoo. You can use it to query pull requests assigned to certain maintainers, or opened by certain authors.

`user $``gengee-query -m python@gentoo.org``user $``gengee-query -a juippis``user $``gengee-query -l "need assignment"`
and so much more.

Check the homepage for an ebuild, info on how to set it up and documentation on how to use it.

### Automatic git pull/rebase merge involving conflicts with KEYWORDS

[gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit) above version 0.50 has a tool called merge-driver-ekeyword which can be utilized as a git's merge driver for conflicts with KEYWORDS in ebuilds. This is especially handy for any arch tester. Please see the [original announcement](https://archives.gentoo.org/gentoo-dev/message/b4fb2bf79a7df1933601606a40abe215).

To utilize the tool, browse to your development repository's location, and add these to ./.git/config

**`./.git/config`**

And add the following into ./.git/info/attributes

**`./.git/info/attributes`**

## See also

- [Standard git workflow](https://wiki.gentoo.org/wiki/Standard_git_workflow) — describing a **modern git workflow** for contributing to Gentoo, with [pkgcheck](https://wiki.gentoo.org/wiki/Pkgcheck) and [pkgdev](https://wiki.gentoo.org/wiki/Pkgdev)
- [GitHub Pull Requests](https://wiki.gentoo.org/wiki/GitHub_Pull_Requests) — how to contribute to Gentoo by creating [pull requests on GitHub](https://github.com/gentoo/gentoo/pulls).
- [Git](https://wiki.gentoo.org/wiki/Git) — widely used, open source, distributed [version control system](https://wiki.gentoo.org/wiki/Version_control_systems)
- [pkgcheck](https://wiki.gentoo.org/wiki/Pkgcheck) — a [pkgcore](https://wiki.gentoo.org/wiki/Pkgcore)-based QA utility for ebuild repos.
- [pkgdev](https://wiki.gentoo.org/wiki/Pkgdev) — a collection of tools for Gentoo development.
- [Project:Infrastructure/Git\_migration](https://wiki.gentoo.org/wiki/Project:Infrastructure/Git_migration)
- [Project:Repository\_mirror\_and\_CI](https://wiki.gentoo.org/wiki/Project:Repository_mirror_and_CI)
- [Gentoo on Codeberg](https://www.gentoo.org/news/2026/02/16/codeberg.html)

## External resources

- [Git Tutorial by Lars Vogel](http://www.vogella.com/tutorials/Git/article.html)
- [Tips and Tricks on gitready](http://gitready.com/)
- [kernel patch guideline](https://www.kernel.org/doc/Documentation/process/submitting-patches.rst)
- [gentoo gitweb](https://gitweb.gentoo.org/repo/gentoo.git)
- [gentoo github mirror](https://github.com/gentoo/gentoo)
- [https://github.com/gentoo-mirror/gentoo](https://github.com/gentoo-mirror/gentoo) - Gentoo repository on [Gentoo repository mirrors](https://github.com/gentoo-mirror)
