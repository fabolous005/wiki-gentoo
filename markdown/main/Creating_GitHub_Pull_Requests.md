<!-- source: https://wiki.gentoo.org/wiki/Creating_GitHub_Pull_Requests | group: Gentoo Wiki (Main) | wiki-title: Creating GitHub Pull Requests -->
---
title: Creating GitHub Pull Requests
url: https://wiki.gentoo.org/wiki/Creating_GitHub_Pull_Requests
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-27"
fingerprint: ae8da4c34c92cbfa
license: CC BY-SA 4.0
---

# Creating GitHub Pull Requests

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article explains how to contribute to Gentoo by creating [pull requests on GitHub](https://github.com/gentoo/gentoo/pulls).

![](https://wiki.gentoo.org/images/thumb/6/6c/Cycle_of_usercontributions.png/300px-Cycle_of_usercontributions.png)

### How to make a pull request

Making a pull request is explained at length in the various how-tos put together by the GitHub folks, here are three must-read how-tos:



## Configuration

### Repository's remote configuration

#### Variant a: User configures a local repository

Clone the github mirror of the Gentoo repository under the remote name "origin" into the path/to/save directory (substitute path/to/save with the directory of choice):

`user $``git clone -o origin https://github.com/gentoo/gentoo.git path/to/save`
Step into the new directory, then view the 'remote' settings

`user $``cd $_``user $``git remote -v`
origin        https://github.com/gentoo/gentoo.git (fetch)
origin        https://github.com/gentoo/gentoo.git (push)

![Github fork.png](https://wiki.gentoo.org/images/2/20/Github_fork.png)

Fork the [Gentoo repository on GitHub](https://github.com/gentoo/gentoo)

Add it under the remote name "github" to your local repository.

`user $``git remote add github <UrlOfYourFork.git>`
Verify the repository's remote settings

`user $``git remote -v`
github  https://github.com/\<github user>/gentoo (fetch)
github  https://github.com/\<github user>/gentoo (push)
origin        https://github.com/gentoo/gentoo.git (fetch)
origin        https://github.com/gentoo/gentoo.git (push)

#### Variant b: Using the git repository as the ::gentoo repository

`root #``mkdir /etc/portage/repos.conf/`
Add the following to the file:

**`/etc/portage/repos.conf/gentoo.conf`**

```
[gentoo]
location = /var/db/repos/gentoo
sync-type = git
sync-uri = https://github.com/gentoo/gentoo.git
auto-sync = yes
sync-user = portage:portage
```
Add the following, substitute \<dev\_user\_name> with your username as appropriate in the script below.

**`/etc/portage/postsync.d/99-user-dev-perms`**

```
#!/bin/bash
DEV_USER_NAME=<dev_user_name>
find /var/db/repos/gentoo/ -type d -exec setfacl -m u:$DEV_USER_NAME:rwx {} \;
find /var/db/repos/gentoo/ -type f -exec setfacl -m u:$DEV_USER_NAME:rw {} \;
find /var/db/repos/gentoo/.git -type d -exec setfacl -m u:$DEV_USER_NAME:rwx {} \;
find /var/db/repos/gentoo/.git -type f -exec setfacl -m u:$DEV_USER_NAME:rw {} \;
```
`root #``chmod +x /etc/portage/postsync.d/99-user-dev-perms`
Then sync the repository.

`root #``emerge --sync`
![Github fork.png](https://wiki.gentoo.org/images/2/20/Github_fork.png)

Fork the [Gentoo repository on GitHub](https://github.com/gentoo/gentoo)

Add it under the remote name "github" to your local repository.

`user $````
cd /var/db/repos/gentoo
```
`user $````
git remote add github <UrlOfYourFork.git>
```
### Repository's user configuration

[Configure git to use the target key](https://docs.github.com/en/github/authenticating-to-github/telling-git-about-your-signing-key) for code signing and to **properly sign-off all your commits** (a more detailed description of how to create the signingkey is given in the [next section](https://wiki.gentoo.org#GPG_configuration)):

`user $````
git config --local user.name "Your Full Name"
```
`user $````
git config --local user.email "example@domain.tld"
```
`user $````
git config --local user.signingkey KEY-FINGERPRINT
```
`user $````
git config --local commit.gpgsign 1
```
Add helpful optional settings:

`user $````
git config --local pull.ff only
```
`user $````
git config --local pull.rebase merges
```
`user $````
git config --local push.default simple
```
`user $````
git config --local push.gpgsign 0
```
Verify the settings:

`user $``git config --local --list`
### GPG configuration

Add the following to gpg.conf:

**`~/.gnupg/gpg.conf`**

**General GPG Setup**

Now to generate the key:

`user $``gpg --full-generate-key`
- Select the algorithm.
- Set the key size.
- Specify how long the key should be valid (No more than 5 years).
- Confirm the information is correct.
- Set your name.
- Set your passphrase, and confirm it.
- It might take some time if you've chosen a high bit length key.

Retrieve your GPG public keyID via the following command:

`user $``gpg --list-public-keys --keyid-format 0xlong example@domain.tld`
The key id is the portion after the \<algorithm>/ on the line beginning with pub as shown in bold below. If you have more than one key with the specified UID you will need to select the correct key yourself from the list of returned keys.

pub   rsa4096/**KEY-FINGERPRINT**

Upload your key to the keyserver:

`user $``gpg --keyserver keys.openpgp.org --send-key KEY-FINGERPRINT`
## Usage

### Step 1: User updates the local repository

`user $``git checkout master``user $``git pull origin master`
### Step 2: User changes a package

![Circular workflow step1.png](https://wiki.gentoo.org/images/thumb/5/5c/Circular_workflow_step1.png/200px-Circular_workflow_step1.png)

Say you are making changes to package app-foo/bar. Create a local branch with the changes:

`user $``git checkout -b <branch name> master`
Make the changes and make sure to run [pkgcheck](https://wiki.gentoo.org/wiki/Pkgcheck) to check for basic errors:

`user $````
git add .
```
`user $````
pkgdev manifest
```
`user $````
pkgcheck scan --net --commits
```
Then commit the changes, preferably using [pkgdev](https://wiki.gentoo.org/wiki/Pkgdev).

`user $``pkgdev commit --signoff`
pkgdev can also be configured to always put [signoff automatically](https://wiki.gentoo.org/wiki/Pkgdev#Pkgdev_configuration).



If the commit is to close or comment bugs on Bugzilla, these should be [mentioned in the commit message](https://wiki.gentoo.org#Links_to_bug_report.28s.29) as suggested by [GLEP66](https://www.gentoo.org/glep/glep-0066.html#commit-messages). pkgdev supports this through the `--bug` and `--closes` options.

`user $``pkgdev commit --closes NNNNNN --signoff`
### Step 3: User makes a pull request

![Circular workflow step2.png](https://wiki.gentoo.org/images/thumb/a/a9/Circular_workflow_step2.png/200px-Circular_workflow_step2.png)

Now that the changes have been made and the local branch updated, it is time to send it off to GitHub and make a PR (Pull Request) for the Gentoo Developers.

Start by pushing the branch with the changes to your GitHub repository:

`user $``git push github <branch name>`
Then [create a pull request](https://help.github.com/articles/creating-a-pull-request/) from your GitHub repository's local branch to the Gentoo repository's master branch. When the changes have been merged, you may delete the local repository's branch with:

`user $````
git checkout master
```
`user $````
git branch -d <branch name>
```
`user $````
git push github :<branch name>
```
## Links to bug report(s)

A bot automatically picks up bug reports if the link(s) to the bug in question appear(s) in the body of the commit message. The bot then writes a message in the bug report and/or closes it. That way, other users quickly get to know a patch or a fix has landed in the tree.

**This feature helps Gentoo developers save time as they don't have to switch back and forth between Bugzilla, GitHub, and their terminal to figure out which bug reports may be closed.**

### How does it work?

The bot can parse two types of header:

will automatically write a message in the bug report **without** closing it.

Closes: [https://bugs.gentoo.org/123456](https://bugs.gentoo.org/123456)

will automatically write a message in the bug report, change the status of the bug report to RESOLVED and the resolution to FIXED.

If the commit involves several bug reports, they can be mentioned and stack the links. The bot will write a message in each bug report:

Bug: [https://bugs.gentoo.org/123456](https://bugs.gentoo.org/123456)
Bug: [https://bugs.gentoo.org/456789](https://bugs.gentoo.org/456789)
Bug: [https://bugs.gentoo.org/101010](https://bugs.gentoo.org/101010)

It also works with the `Closes:` header:

Closes: [https://bugs.gentoo.org/123456](https://bugs.gentoo.org/123456)
Closes: [https://bugs.gentoo.org/456789](https://bugs.gentoo.org/456789)
Closes: [https://bugs.gentoo.org/101010](https://bugs.gentoo.org/101010)

Feel free to add as much information as possible: upstream forums, mailing lists, discussions, links to changelogs, etc. The bot won't post messages all over the place though, but these useful information will appear in the ChangeLog file of the ebuild.

## QA checks

### gentoo-repo-qa-bot is engaging in a conversation with me in the PR. I don't understand.

![](https://wiki.gentoo.org/images/1/10/Gentoo-repo-qa-bot_All_checks_have_failed.png)

We have set up an automated CI system which performs various QA checks when a PR is filed. These checks may result in two possible outcomes displayed next to your PR:

- a green check-mark meaning everything's fine and your PR isn't offending the tree.
- a red cross meaning something is up and your PR needs fixing.

Our QA bot is chatty when a red cross shows up. At this point, it might point out two types of error:

Issues persisted from underlying repository state:


This error means that unfortunately, you forked the tree whilst it was in a very unstable state (meaning broken). Indeed, every now and then developers break the tree and, tough luck, it turns out you forked the tree into your GitHub profile or synced it up at this very moment. But no big deal as there's nothing for you to do.

New issues:


This error is a bit more serious and means your PR isn't complying with our QA standards. Usually, one or more link(s) are displayed right below for you to visualize in a browser what it's all about. Go ahead, take a look and fix those errors. Push again and wait for the bot to go over your PR again (every 30 minutes) until a green check-mark appears.

## Troubleshooting

### Possible issues with signing

It's possible that a message comes up like

`user $````
error: gpg failed to sign the data
```
fatal: failed to write commit object

!!! Exiting on git (shell) error code: 128
In such a case the `GPG_TTY` environment variable is to be set. To make sure it is always set, it can be added to the \~/.bashrc or to /etc/portage/make.conf:

**`~/.bashrc`**

```
export GPG_TTY=$(tty)
```
Setting pinentry to a terminal-friendly variant, like pinentry-ncurses or pinentry-tty might also help with this issue.

The available pinentry variants can be listed with [eselect](https://wiki.gentoo.org/wiki/Eselect):

`user $``eselect pinentry list`
Available pinentry binary implementations:
  \[1\]   pinentry-qt
  \[2\]   pinentry-qt4
  \[3\]   pinentry-curses
  \[4\]   pinentry-tty \*

Set the desired handler with

`root #``eselect pinentry set [number]`
For further information about gpg, please visit to the [GnuPG](https://wiki.gentoo.org/wiki/GnuPG) article.

### repo 'gentoo' is undefined

By default emerge uses /usr/share/portage/config/repos.conf however, pkgdev reads from /etc/portage/repos.conf/gentoo.conf.

This can be corrected by following [Pkgdev#Gentoo\_ebuild\_repository](https://wiki.gentoo.org/wiki/Pkgdev#Gentoo_ebuild_repository)

## See also

- [Gentoo git workflow](https://wiki.gentoo.org/wiki/Gentoo_git_workflow) — outlines some rules and best-practices regarding the typical workflow for ebuild developers using git.
- [Project:GitHub/Pull requests](https://wiki.gentoo.org/wiki/Project:GitHub/Pull_requests)
- [Standard git workflow](https://wiki.gentoo.org/wiki/Standard_git_workflow) — describing a **modern git workflow** for contributing to Gentoo, with [pkgcheck](https://wiki.gentoo.org/wiki/Pkgcheck) and [pkgdev](https://wiki.gentoo.org/wiki/Pkgdev)
- [Submitting\_ebuilds](https://wiki.gentoo.org/wiki/Submitting_ebuilds) — explains how to submit ebuilds for inclusion in the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository)
- [Incus/Gentoo\_Github\_pullrequest\_testing](https://wiki.gentoo.org/wiki/Incus/Gentoo_Github_pullrequest_testing) — easy and automated way for testing ebuild contributions via [Gentoo's Github mirror](https://github.com/gentoo/gentoo) that's based on [Incus](https://wiki.gentoo.org/wiki/Incus)
