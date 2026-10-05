<!-- source: https://wiki.gentoo.org/wiki/SSH_jump_host | group: Gentoo Wiki (Main) | wiki-title: SSH jump host -->
---
title: SSH jump host
url: https://wiki.gentoo.org/wiki/SSH_jump_host
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-06"
fingerprint: b4727b132db4dba4
license: CC BY-SA 4.0
---

# SSH jump host

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

SSH jump hosts are employed as an alternative to [SSH tunneling](https://wiki.gentoo.org/wiki/SSH_tunneling) to access internal machines through a gateway.

The idea is to use ProxyCommand to automatically execute the ssh command on remote host to jump to the next host and forward all traffic through.

## Prerequisites

- SSH access to the gateway machine and the internal one.
- Gateway machine has Netcat installed.

## Configuration

`ProxyJump` hosts can be defined inside each user's SSH config file.

**`~/.ssh/config`**

**ProxyJump Example**

See the corresponding single jump section under Usage below.

The same syntax can be used to make jumps over multiple machines:

**`~/.ssh/config`**

**Add this text**

See the corresponding multiple jump section under Usage below.

`user $``ssh behindalphabeta`
### Static jump host list

Static jump host list means, that the jump host(s) are known and can be defined before initiating the first ssh connection. Therefore a static jump host 'routing' can be defined in the user's \~/.ssh/config file. The advantage in comparison to the dynamic jump host option is, that you don't have to provide the .ssh config on jump hosts between your machine and all the other jump hosts between you and the final host you want to jump to.

## Usage

`user $``ssh behindalpha`
If usernames on machines differ, specify them by modifying the correspondent `ProxyJump` line:

**`~/.ssh/config`**

**Modify correspondent ProxyCommand**

It works with the scp command, too:

`user $``scp filename behindalphabeta:~/`
The same syntax can be used to make jumps over multiple machines:

`user $``ssh -J user1@host1:port1,user2@host2:port2 user3@host3`
### Dynamic jump host list

The `-J` option can be used to jump through a host:

`user $``ssh -J host1 host2`
If usernames or ports on machines differ, specify them:

`user $``ssh -J user1@host1:port1 user2@host2 -p port2`
### Tips

To ease the connecting even further:

- Set these commands as [shell aliases](<https://en.wikipedia.org/wiki/Alias_(command)>)
- To avoid typing passwords use [OpenSSH keys](https://wiki.gentoo.org/wiki/SSH#Passwordless_authentication)

## See also

- [SSH](https://wiki.gentoo.org/wiki/SSH) — the ubiquitous tool for logging into and working on remote machines securely.
