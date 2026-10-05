<!-- source: https://wiki.gentoo.org/wiki/SSH_tunneling | group: Gentoo Wiki (Main) | wiki-title: SSH tunneling -->
---
title: SSH tunneling
url: https://wiki.gentoo.org/wiki/SSH_tunneling
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-02"
fingerprint: b518fd436fa1eed6
license: CC BY-SA 4.0
---

# SSH tunneling

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**SSH tunneling** is a method of connecting to machines on the other side of a gateway machine. The gateway machine will be 'tunneled' through in order to gain access to machines on the other side. This method presumes both these machines are running [SSH](https://wiki.gentoo.org/wiki/SSH) making it possible to set up the [tunnel](https://en.wikipedia.org/wiki/Tunneling_protocol).

## Usage

Begin with creating a ssh tunneling session:

`user $``ssh -f <GATEWAY_USERNAME@>GATEWAY -L localhost:CPORT:SERVER:SPORT -N`
The `-f` option instructs the ssh instance to go into the background, and `-N` instructs it to not launch a shell. Followed by:

`user $``ssh -p CPORT <SUSERNAME@>localhost`
The variables above represent:

- `GATEWAY`
- The hostname/IP address of the gateway machine.
- `GATEWAY_USERNAME`
- The username on the gateway (optional if this username is the same as on the client).
- `SERVER`
- The hostname/IP address of the server you wish to log into.
- `SUSERNAME`
- The username on the server (optional if this username is the same as on the client).
- `SPORT`
- The port number on which the server SSH daemon is listening, by default 22.
- `CPORT`
- The port number of your choosing on which the tunnel will be receiving connections on the client machine (should be greater than 1024 unless you are invoking as root).

You can [scp](https://en.wikipedia.org/wiki/Secure_copy) files from the server as you would normally by specifying the tunnel port:

`user $``scp -P CPORT localhost:REMOTEPATH LOCALPATH`
Similarly for sending files to the server:

`user $``scp -P CPORT LOCALPATH localhost:REMOTEPATH`
### Tips

In order to make this tunneling process less onerous in the future:

- Set these commands as [shell aliases](<https://en.wikipedia.org/wiki/Alias_(command)>) (in [Bash](https://wiki.gentoo.org/wiki/Bash), usually in \~/.bashrc).
- To avoid typing passwords, copy the client key to the gateway, and the client and gateway keys to the server.
- If you rely upon keeping an unattended connection alive which may become dropped due to timeouts, consider altering the various TCP keepalive settings in the client and server configurations. Perhaps the most robust solution is to install a connection watchdog such as [net-misc/autossh](https://packages.gentoo.org/packages/net-misc/autossh) which will babysit an ssh session and restart it if necessary.

## X11 forwarding

To enable X11 forwarding, first the `X11Forwarding` and `ForwardX11` options must be set to `yes` for both the X client and server being connected to respectively. In your SSH client connection, add the `-Y` option to the second invocation above, and optionally the `-C` switch to also enable compression i.e:

`user $``ssh -YC -p CPORT <SUSERNAME@>localhost`
The following is required for the forwarding of X11 connections from the remote server to local client to work:

- The SSH daemon on the gateway machine must have TCP forwarding must be enabled, otherwise X11 connections won't be forwarded:

- FILE**`/etc/ssh/sshd_config`****On the gateway**

- The xauth tool must be present on the local X server. Install [net-misc/openssh](https://packages.gentoo.org/packages/net-misc/openssh) with the `X` USE flag set to pull it in or install [x11-apps/xauth](https://packages.gentoo.org/packages/x11-apps/xauth).

- X11 forwarding must be enabled in the remote server SSH daemon configuration:

- FILE**`/etc/ssh/sshd_config`****On the server**

## See also

- [SSH](https://wiki.gentoo.org/wiki/SSH) — the ubiquitous tool for logging into and working on remote machines securely.
- [SSH reverse tunneling: getting a static IP from a cloud](https://wiki.gentoo.org/wiki/SSH_reverse_tunneling:_getting_a_static_IP_from_a_cloud)
- [SSH jump host](https://wiki.gentoo.org/wiki/SSH_jump_host) — employed as an alternative to [SSH tunneling] to access internal machines through a gateway.
