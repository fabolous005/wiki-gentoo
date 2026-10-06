<!-- source: https://wiki.gentoo.org/wiki/Git/Local_bare_repo | group: Gentoo Wiki (Main) | wiki-title: Git/Local bare repo -->
---
title: Git/Local bare repo
url: https://wiki.gentoo.org/wiki/Git/Local_bare_repo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-22"
fingerprint: b5f9d85e4ca1fa40
license: CC BY-SA 4.0
---

# Git/Local bare repo

[Git](https://wiki.gentoo.org/wiki/Git)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

In some situations it may be advantageous to have a private git repository hosted on a computer attached to the local network. Use cases for this setup include an open-source package developer/maintainer who needs to test the code on local machines running different operating systems and/or different architectures before pushing to the *public* repository. It may also be adequate for a small software development shop that does not require all the bells and whistles of a full-blown locally-installed GitHub Enterprise or Gitea solution.

The shared repositories do not even need to be code-related. It could be used to track and share any files between machines on a LAN.

This section will detail creating and configuring such a setup. It requires some one-time setup on a machine that will host the bare repositories (the "server") and minor configuration for each client that will connect to it.

### Server configuration

The only prerequisites for the server machine are that it is running [**sshd**](https://wiki.gentoo.org/wiki/SSH#Server_configuration) and that it has **git** installed.

First, create an unprivileged system user to own the bare repositories.

`root #````
mkdir -p /srv/git
```
`root #``useradd --system --home-dir /srv/git --shell /usr/bin/git-shell git`
This example uses *git* as the username, but any new username can be used. Just be sure to carefully substitute the username in the following commands where applicable. Likewise, while `/srv/git` is the conventional location for the bare repo(s), it is not a requirement. Unless circumstances require a different location, it is best to stick with the convention.

`git-shell` is used as the default shell so that interactive logins to the git account are refused. Now set up ownership and permissions, and create a file to hold the public ssh keys of all the allowed clients.

`root #````
mkdir -p /srv/git/.ssh
```
`root #````
touch /srv/git/.ssh/authorized_keys
```
`root #````
chown -R git:git /srv/git
```
`root #````
chmod 700 /srv/git/.ssh
```
`root #``chmod 600 /srv/git/.ssh/authorized_keys`
Add the public keys for all the clients to `/srv/git/.ssh/authorized_keys`

`root #``nano /srv/git/.ssh/authorized_keys`
**`/srv/git/.ssh/authorized_keys`**

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAGHghgGGUYdtdttyUYTduytdTtDuytduytDUytduyDtdu larry@clientone
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAguUGIUyugGYIUUHtfpkJopjIOojPIJIUUHOIUHOuihou sally@clienttwo
```
Now initialize the base repo(s). It is necessary to run the following commands as the *git* user. Since this user's shell does not allow interactive logins, it is necessary to pass the `--shell` option to `su` which temporarily overrides the default shell.

`root #``su git --shell=/bin/bash`
'

`git $````
git init --bare /srv/git/my_new_project.git
```
`git $``git init --bare /srv/git/my_existing_project.git`
Note the `--bare` flag passed to the `git init` command. It is necessary to initialize a bare repo for all projects in this same way. Run the same command whether it is a brand new repo, or a project that is already started and currently stored elsewhere. While all other commands were a one-time setup, `git init --bare project.git` will need to be run as the *git* user every time a new project repo is added.

### Client setup

There is not much to do on the client side other than to clone new repos into a local working tree, and to add existing repos as a new remote. To clone a new repo:

`user $``git clone` [ssh://git@gitserver/srv/git/my_new_project.git](ssh://git@gitserver/srv/git/my_new_project.git)
This will automatically add the bare repo on *gitserver* as the default remote. To push an existing repo onto the bare repo and set it as a new remote:

`user $``git remote add lan` [ssh://git@gitserver/srv/git/my_existing_project.git](ssh://git@gitserver/srv/git/my_existing_project.git)
`user $``git remote -v`
lan	ssh://git@gitserver/srv/git/my\_existing\_project.git (fetch)
lan	ssh://git@gitserver/srv/git/my\_existing\_project.git (push)
origin	https://github.com/MyGithubUser/my\_existing\_project.git (fetch)
origin	https://github.com/MyGithubUser/my\_existing\_project.git (push)

`user $``git push lan main`
Note that *lan* is just a name for the remote. It is possible to name the remote anything. Optionally, you can make the bare repo the default remote, which allows for running unqualified `git pull` and `git push`es from the client.

`user $``git branch --set-upstream-to=lan/main main`
