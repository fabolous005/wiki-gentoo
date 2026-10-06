<!-- source: https://wiki.gentoo.org/wiki/Git | group: Gentoo Wiki (Main) | wiki-title: Git -->
---
title: git
url: https://wiki.gentoo.org/wiki/Git
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-22"
categories: ['dev-vcs']
fingerprint: ba1d915fd1a0fb8c
license: CC BY-SA 4.0
---

# git

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Git** is a widely used, open source, distributed [version control system](https://wiki.gentoo.org/wiki/Version_control_systems), mainly written in [C](https://wiki.gentoo.org/wiki/C).

Git was created by Linus Torvalds for use developing the [Linux kernel](https://wiki.gentoo.org/wiki/Kernel) and other open source projects, with the stated goals of being distributed, fast, and to guarantee that output exactly matches input. It was first released in 2005, and has since become the most widely used distributed version control system.

This article will cover getting started with Git, and general usage.

## Installation

### USE flags


### USE flags for
            [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git)
            
            Stupid content tracker: distributed VCS designed for speed and efficiency

| [+curl](https://packages.gentoo.org/useflags/+curl) | Support fetching and pushing (requires webdav too) over http:// and https:// protocols | 
| [+gpg](https://packages.gentoo.org/useflags/+gpg) | Pull in gnupg for signing -- without gnupg, attempts at signing will fail at runtime! | 
| [+iconv](https://packages.gentoo.org/useflags/+iconv) | Enable support for the iconv character set conversion library | 
| [+nls](https://packages.gentoo.org/useflags/+nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [+pcre](https://packages.gentoo.org/useflags/+pcre) | Add support for Perl Compatible Regular Expressions | 
| [+perl](https://packages.gentoo.org/useflags/+perl) | Adds Perl bindings and tools such as git-send-email | 
| [+rust](https://packages.gentoo.org/useflags/+rust) | Build components using Rust, starting with 2.52 with varint. This will become mandatory upstream with Git 3.0. | 
| [+safe-directory](https://packages.gentoo.org/useflags/+safe-directory) | Respect the safe.directory setting | 
| [+webdav](https://packages.gentoo.org/useflags/+webdav) | Adds support for push'ing to HTTP/HTTPS repositories via DAV | 
| [cgi](https://packages.gentoo.org/useflags/cgi) | Install gitweb too | 
| [cvs](https://packages.gentoo.org/useflags/cvs) | Enable CVS (Concurrent Versions System) integration | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [highlight](https://packages.gentoo.org/useflags/highlight) | GitWeb support for app-text/highlight | 
| [keyring](https://packages.gentoo.org/useflags/keyring) | Enable support for freedesktop.org Secret Service API password store | 
| [perforce](https://packages.gentoo.org/useflags/perforce) | Add support for Perforce version control system (requires manual installation of Perforce client) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [subversion](https://packages.gentoo.org/useflags/subversion) | Include git-svn for dev-vcs/subversion support | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [tk](https://packages.gentoo.org/useflags/tk) | Include the 'gitk' and 'git gui' tools | 
| [xinetd](https://packages.gentoo.org/useflags/xinetd) | Add support for the xinetd super-server | 

### Emerge

Install [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git):

`root #``emerge --ask dev-vcs/git`
### Additional software

There are a number of additional applications that are associated with Git and are of note:

| Package | Description | Notes | 
|---|---|---|
| [app-vim/fugitive](https://packages.gentoo.org/packages/app-vim/fugitive) | Git wrapper plugin for vim. |  | 
| [dev-vcs/git-cola](https://packages.gentoo.org/packages/dev-vcs/git-cola) | "Sleek and powerful" graphical user interface for Git. | As of v4.0.1, git-cola cannot open a fully history clone of [gentoo.git](https://gitweb.gentoo.org/repo/gentoo.git) without serious application freezes. Try a shallow clone if experiencing freezing. | 
| [dev-vcs/git-flow](https://packages.gentoo.org/packages/dev-vcs/git-flow) | Git extensions to provide high-level repository operations. |  | 
| [dev-vcs/gitg](https://packages.gentoo.org/packages/dev-vcs/gitg) | Git repository viewer for GNOME. |  | 
| [dev-vcs/qgit](https://packages.gentoo.org/packages/dev-vcs/qgit) | Qt-based GUI for git repositories. | As of v2.10, qgit has no issues opening a full history clone of [gentoo.git](https://gitweb.gentoo.org/repo/gentoo.git). | 

This is a partial selection of packages available in the Gentoo repository, see the [dev-vcs](https://packages.gentoo.org/categories/dev-vcs) category on the Gentoo Packages site, or use [eix](https://wiki.gentoo.org/wiki/Eix) (eix --category dev-vcs), to see packages from the *dev-vcs* category that may be of interest.

There is additional software in the [GURU](https://wiki.gentoo.org/wiki/GURU) repository, such as the *gitui* and *lazygit* GUIs.

## Configuration

Before contributing to a project, it is imperative to establish a user name and email for each user. Substitute the bracketed "Larry" references (brackets and everything in-between, but leave the quotes) in the next example for a personal user name and e-mail address:

`user $````
git config --global user.email "<larry@gentoo.org>"
```
`user $````
git config --global user.name "<Larry the cow>"
```
### Local

If there will be just one user of the project, or when creating something which will be shared in a distributed way, start on the local workstation.

If the intent is to have a central server which everyone uses as the "official" server (e.g. GitHub) then it might be easier to create an empty repository there.

The next list of commands will describe how to create a repository on a workstation:

`user $````
cd ~/src
```
`user $````
mkdir hello
```
`user $````
cd hello
```
`user $````
touch readme.txt
```
`user $````
git init
```
The local repository has now been created.

Let's make some edits:

`user $````
echo "Hello, world!" >> readme.txt
```
The new readme.txt file must be added (staged) before it can be included in the git repository. Use the next commands to stage the file and to make the commit:

`user $````
git add readme.txt
```
`user $````
git commit -m "Added text to readme.txt"
```
One of many nice features of git - on commit message writing screen (for example in Vim) [you can see the diff](https://stackoverflow.com/a/46160765/1879101)

**`~/.gitconfig`**

```
[commit]
    verbose = true
```
### Bash completion

To setup [bash completion](https://wiki.gentoo.org/wiki/Bash#Tab_completion) ([see here](https://git-scm.com/book/en/v2/Appendix-A%3A-Git-in-Other-Environments-Git-in-Bash) for more info):

**`~/.config/bashrc`**

```
.  /usr/share/bash-completion/completions/git
```
### Zsh completion

[Zsh](https://wiki.gentoo.org/wiki/Zsh) will prefer completions that come later on `fpath`. Since both [app-shells/zsh](https://packages.gentoo.org/packages/app-shells/zsh) and [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git) install a completion script \_git, and Git's comes later on `fpath`, default installations of both will use Git's packaged Zsh completion.

If you prefer to use Zsh's packaged completion script, adjust `fpath` so that the containing directory is earlier on `fpath`:

**`~/.zshrc`**

```
# Assuming normal options
fpath=(/usr/share/zsh/5.9/functions/Completion/Unix/_git $fpath)
```
### Status in bash prompt

It is possible to configure the [Bash](https://wiki.gentoo.org/wiki/Bash) prompt to show information such as the name of the current branch, flag of uncommited changes, number of commits that were not pushed, etc., all without additional software<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>:

**`~/.config/bashrc`**

```
source /usr/share/git/git-prompt.sh
export PS1='\[\033[01;32m\]\u@\h\[\033[01;34m\] \w\[\033[01;33m\]$(__git_ps1)\[\033[01;34m\] \$\[\033[00m\] '
export GIT_PS1_SHOWDIRTYSTATE=1
```
Similar code works for [Zsh](https://wiki.gentoo.org/wiki/Zsh), also.

To see numbers like `[master ↑·2|●1✚ 1]`, and with time: use [https://github.com/magicmonty/bash-git-prompt](https://github.com/magicmonty/bash-git-prompt).

### Server

This section will cover setting up a Git server for remote project management through SSH.

#### Initial setup

Start by creating the required group, user, and home directory. The user uses the `git-shell` to prevent normal shell access.

`root #````
groupadd git
```
`root #``useradd -m -g git -d /var/git -s /usr/bin/git-shell git`
Edit /etc/conf.d/git-daemon to change user from *"nobody"* to *"git"* and start the daemon:

**`/etc/conf.d/git-daemon`**

```
GIT_USER="git"
GIT_GROUP="git"
```
If desired to accept git push and allow access all direct, it needs two options --enable=receive-pack and --export-all in GITDAEMON\_OPTS , e.g.:

**`/etc/conf.d/git-daemon`**

```
GITDAEMON_OPTS="--syslog --export-all --enable=receive-pack --base-path=/var/git"
```
Start the daemon:

`root #``/etc/init.d/git-daemon start`
### SSH keys

[SSH](https://wiki.gentoo.org/wiki/SSH) is the preferred method to handle the secure communications between client and server.

## Usage

### Creating a patch

See [Creating a patch](https://wiki.gentoo.org/wiki/Creating_a_patch).

### Bisecting with live ebuilds

See [Bisecting with live ebuilds](https://wiki.gentoo.org/wiki/Bisecting_with_live_ebuilds).

### Create a repository and make the initial commit

On the server:

Become the *git* user to make sure all objects are owned by this user:

`root #``su git`
Create a bare repository:

`git $````
cd /var/git
```
`git $````
mkdir /var/git/newproject.git
```
`git $````
cd /var/git/newproject.git
```
`git $````
git init --bare
```
On a client station:

`git $````
mkdir ~/newproject
```
`git $````
cd ~/newproject
```
`git $````
git init
```
`git $````
touch test
```
`git $````
git add test
```
`git $````
git config --global user.email "larry@gentoo.org"
```
`git $````
git config --global user.name "larry_the_cow"
```
`git $````
git commit -m 'initial commit'
```
`git $````
git remote add origin git@example.com:/var/git/newproject.git
```
`git $````
git push origin master
```
Writing to config this way will create \~/.gitconfig, but it can be moved to \~/.config/git/config, to house the git config under git.

### Common commands

Clone a repository:

`user $``git clone git@example.com:/newproject.git``user $``git clone git://example.com:/newproject.git`
### Repository management via GUI

If [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git) was built with [tk](https://packages.gentoo.org/useflags/tk)[, Git will provide a tk GUI. Launch it from a directory containing a Git repository, using:](https://wiki.gentoo.org/wiki/USE_flag)

`user ~/repository.git $````
gitk
```
### Serving and managing repositories via builtin web interface

Git comes with a built-in web interface called [gitweb](http://git-scm.com/docs/gitweb). It can run on a variety of web servers:

- [lighttpd](https://wiki.gentoo.org/wiki/Lighttpd) - No configuration necessary.
- [Apache](https://wiki.gentoo.org/wiki/Apache) - Some configuration necessary.
- [nginx](https://wiki.gentoo.org/wiki/Nginx) - A small, robust, and high-performance HTTP server and reverse proxy.

In order to use gitweb, be sure one of the three web servers has been installed and git has been built with the `cgi` USE flag.

There is a simple setup script that will create a working default configuration, start a web server (the default configuration is for lighttpd) and open the URL in a browser. Navigate to the repositories directory. Once inside, type:

`user ~/repository.git $````
git instaweb
```
If git instaweb opens a 404 error, enable the `cgi` USE flag and rebuild git.

#### Help

Find out more about the options using the the built-in help output:

`user ~/repository.git $````
git help instaweb
```
For additional help, consider reading the contextual man page:

`user ~/repository.git $````
man git instaweb
```
#### Configuration

Per-project configuration can be set in the repositories .git/config file:

`user ~/repository.git $````
vim .git/config
```
Values in this file should be in an [ini-style format](https://en.wikipedia.org/wiki/INI_file):

**`.git/config`**

**Setting lighttpd values for instaweb in a repository's configuration**

```
[instaweb]
        ; local = true
        httpd = lighttpd
        port = 8080
        browser = elinks
        modulePath = /usr/lib64/lighttpd/
```
Adjust the values as needed. If the `local = true` line is uncommented (remove the `;`), instaweb will only be reachable from the localhost.

## See also

- [Cgit](https://wiki.gentoo.org/wiki/Cgit) — a hyperfast web frontend for git repositories written in C
- [Git/Tweaks](https://wiki.gentoo.org/wiki/Git/Tweaks) — aims to document some neat bonus features and config options available in git.
- [Tracking changes to "/etc" with git for backup](https://wiki.gentoo.org/wiki/Tracking_changes_to_%22/etc%22_with_git_for_backup) — Tracking the /etc directory with git
- [CVS](https://wiki.gentoo.org/wiki/CVS)
- [Kernel git-bisect](https://wiki.gentoo.org/wiki/Kernel_git-bisect) — a [Git] tool to find the commit that caused problems between versions.
- [Portage with Git](https://wiki.gentoo.org/wiki/Portage_with_Git) — use [Git] to synchronize the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository)
- [Git/Local bare repo](https://wiki.gentoo.org/wiki/Git/Local_bare_repo) — Configuring a shared LAN git repository

## External resources

- [git flow documentation](https://jeffkreeftmeijer.com/2010/why-arent-you-using-git-flow/) — Client side scripts to make git repository management a snap.
- [The Official Git Handbook](https://git-scm.com/book/) — Hosted at the official git website.
- [Git – The simple guide](https://rogerdudler.github.io/git-guide/)
- [Git from the inside out](https://codewords.recurse.com/issues/two/git-from-the-inside-out) — A well written publication from the Recurse Center. This article addresses what happens beneath the surface when using git.
- [Lesser known git commands](https://hackernoon.com/lesser-known-git-commands-151a1918a60) — A blog entry on helpful commands that go unnoticed.
- [https://git-send-email.io](https://git-send-email.io) — git collaboration over email.
