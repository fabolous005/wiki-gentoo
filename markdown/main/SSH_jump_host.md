<!-- source: https://wiki.gentoo.org/wiki/SSH_jump_host | group: Gentoo Wiki (Main) | wiki-title: SSH jump host -->
---
title: SSH jump host
url: https://wiki.gentoo.org/wiki/SSH_jump_host
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-10"
fingerprint: "84426a130d94dbae"
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

### Single jump

`ProxyJump` hosts can be defined inside each user's SSH config file.

**`~/.ssh/config`**

**ProxyJump Example**

```
### First jump host. Directly reachable
Host betajump
  HostName jumphost1.example.org
 
### Host to jump to via jumphost1.example.org
Host behindbeta
  HostName behindbeta.example.org
  ProxyJump  betajump
```
See the corresponding single jump section under Usage below.

### Multiple jump

The same syntax can be used to make jumps over multiple machines:

**`~/.ssh/config`**

**Add this text**

```
### First jump host. Directly reachable
Host alphajump
  HostName jumphost1.example.org
 
### Second jumphost. Only reachable via jumphost1.example.org
Host betajump
  HostName jumphost2.example.org
  ProxyJump alphajump
 
### Host only reachable via alphajump and betajump
Host behindalphabeta
  HostName behindalphabeta.example.org
  ProxyJump betajump
```
See the corresponding multiple jump section under Usage below.

`user $``ssh behindalphabeta`
### Static jump host list

Static jump host list means, that the jump host(s) are known and can be defined before initiating the first ssh connection. Therefore a static jump host 'routing' can be defined in the user's \~/.ssh/config file. The advantage in comparison to the dynamic jump host option is, that you don't have to provide the .ssh config on jump hosts between your machine and all the other jump hosts between you and the final host you want to jump to.

## Usage

### Single jump

`user $``ssh behindalpha`
If usernames on machines differ, specify them by modifying the correspondent `ProxyJump` line:

**`~/.ssh/config`**

**Modify correspondent ProxyCommand**

```
ProxyJump  otheruser@behindalpha
```
It works with the scp command, too:

`user $``scp filename behindalphabeta:~/`
### Multiple jump

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

- [SSH\_tunneling](https://wiki.gentoo.org/wiki/SSH_tunneling) — a method of connecting to machines on the other side of a gateway machine.
- [SSH](https://wiki.gentoo.org/wiki/SSH) — the ubiquitous tool for logging into and working on remote machines securely.
