<!-- source: https://wiki.gentoo.org/wiki/SFTP | group: Gentoo Wiki (Main) | wiki-title: SFTP -->
---
title: sftp
url: https://wiki.gentoo.org/wiki/SFTP
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-03"
fingerprint: "3e79f0dcf591fd70"
license: CC BY-SA 4.0
---

# sftp

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**SFTP** (**S**ecure **F**ile **T**ransfer **P**rogram) is an interactive file transfer program, similar to [FTP](https://wiki.gentoo.org/wiki/FTP), which performs all operations over an encrypted [SSH](https://wiki.gentoo.org/wiki/SSH) transport. It uses many features of SSH, such as public key authentication and compression.[\[1\]](https://wiki.gentoo.org#cite_note-1)

## Installation

The sftp command is part of the [net-misc/openssh](https://packages.gentoo.org/packages/net-misc/openssh) package, and deployments of Gentoo Linux should already have OpenSSH installed, as the package is part of the [system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>). The presence and proper functioning of OpenSSH can be checked by running the sftp command, which should output a usage statement:

`user $``sftp````
usage: sftp [-46AaCfNpqrv] [-B buffer_size] [-b batchfile] [-c cipher]
          [-D sftp_server_path] [-F ssh_config] [-i identity_file]
          [-J destination] [-l limit] [-o ssh_option] [-P port]
          [-R num_requests] [-S program] [-s subsystem | sftp_server]
          destination
```
If no usage statement is printed, OpenSSH may be corrupt, or not installed. Try re-installation by following the [Installation section from the SSH article](https://wiki.gentoo.org/wiki/SSH#Installation).

## Configuration

For having tab completion the  [`libedit`](https://packages.gentoo.org/useflags/libedit) USE flag needs to be enabled.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>  For further configuration see [the configuration section in the SSH article](https://wiki.gentoo.org/wiki/SSH#Configuration).

## See also

- [CurlFtpFS](https://wiki.gentoo.org/wiki/CurlFtpFS) — allows for [mounting](https://wiki.gentoo.org/wiki/Mount) an FTP folder as a regular directory to the local directory tree.
- [SCP](https://wiki.gentoo.org/wiki/SCP) — an interactive file transfer program, similar to the copy command, that copies files over an encrypted SSH transport.
- [SSH](https://wiki.gentoo.org/wiki/SSH) — the ubiquitous tool for logging into and working on remote machines securely.
- [SSHFS](https://wiki.gentoo.org/wiki/SSHFS) — a secure shell client used to mount remote filesystems to local machines.

## External resources

- The official sftp man page locally via man sftp or online at [openbsd.org](http://www.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man1/sftp.1?query=sftp&sec=1)

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [http://www.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man1/sftp.1?query=sftp&sec=1](http://www.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man1/sftp.1?query=sftp&sec=1)
2. [↑](https://wiki.gentoo.org#cite_ref-2) [Gentoo Forums :: View topic - (SOLVED) sftp autocompletion](https://forums.gentoo.org/viewtopic.php?p=7828294#7828294), [Welcome – Gentoo Linux](https://gentoo.org/), October 15th, 2015. Retrieved on October 15th, 2015.
