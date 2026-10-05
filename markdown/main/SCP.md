<!-- source: https://wiki.gentoo.org/wiki/SCP | group: Gentoo Wiki (Main) | wiki-title: SCP -->
---
title: SCP
url: https://wiki.gentoo.org/wiki/SCP
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-03"
fingerprint: ba29d5d8b5b9bf60
license: CC BY-SA 4.0
---

# SCP

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**SCP** (**S**ecure **C**opy **P**rogram) is an interactive file transfer program, similar to the copy command, that copies files over an encrypted SSH transport. It uses many features of [SSH](https://wiki.gentoo.org/wiki/SSH), such as public key authentication and compression.[\[1\]](https://wiki.gentoo.org#cite_note-1)

## Installation

The scp command is part of the [net-misc/openssh](https://packages.gentoo.org/packages/net-misc/openssh) package, and deployments of Gentoo Linux should already have OpenSSH installed, as the package is part of the [system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>). The presence and proper functioning of OpenSSH can be checked by running the scp command, which should output a usage statement:

`user $``scp````
usage: scp [-346ABCpqrTv] [-c cipher] [-F ssh_config] [-i identity_file]
            [-J destination] [-l limit] [-o ssh_option] [-P port]
            [-S program] source ... target
```
If no usage statement is printed, OpenSSH may be corrupt, or not installed. Try re-installation by following the [Installation section from the SSH article](https://wiki.gentoo.org/wiki/SSH#Installation).

## Configuration

For configuration see [the configuration section in the SSH article](https://wiki.gentoo.org/wiki/SSH#Configuration).

## Usage examples

Copy a file from a remote computer locally:

`user $``scp user@somecomputer:/path/file /path/file`
Where

- 'user' is a username on the remote computer. This defaults to the username on the local computer
- 'somecomputer' is the hostname or the IP address of the remote computer


Copy a local file to a remote computer:

`user $``scp /path/file user@somecomputer:/path/file`
## See also

- [SFTP](https://wiki.gentoo.org/wiki/SFTP) — an interactive file transfer program, similar to [FTP](https://wiki.gentoo.org/wiki/FTP), which performs all operations over an encrypted [SSH](https://wiki.gentoo.org/wiki/SSH) transport.
- [SSHFS](https://wiki.gentoo.org/wiki/SSHFS) — a secure shell client used to mount remote filesystems to local machines.
- [SSH](https://wiki.gentoo.org/wiki/SSH) — the ubiquitous tool for logging into and working on remote machines securely.
- [Rsync](https://wiki.gentoo.org/wiki/Rsync) — a powerful file sync program capable of efficient file transfers and directory synchronization.
- [CurlFtpFS](https://wiki.gentoo.org/wiki/CurlFtpFS) — allows for [mounting](https://wiki.gentoo.org/wiki/Mount) an FTP folder as a regular directory to the local directory tree.

## External resources

- The SCP man page locally (man scp) or online at [openbsd.org](http://www.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man1/scp.1?query=scp&sec=1)
