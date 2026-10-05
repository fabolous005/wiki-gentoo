<!-- source: https://wiki.gentoo.org/wiki/FTP | group: Gentoo Wiki (Main) | wiki-title: FTP -->
---
title: FTP
url: https://wiki.gentoo.org/wiki/FTP
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-07"
fingerprint: "3619ed5942830ad5"
license: CC BY-SA 4.0
---

# FTP

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**File Transfer Protocol (FTP)** is an legacy protocol for transferring files between computers. Because FTP traffic is not encrypted in transit and can easily be spoofed, it is considered insecure in modern networked computing. It has therefore been displaced by secure alternatives such as [sftp](https://wiki.gentoo.org/wiki/Sftp), and [scp](https://wiki.gentoo.org/wiki/Scp) for most use-cases. It is occasionally still used in a retrocomputing context or as a means to update firmware on embedded devices along side alternatives such as [TFTP](https://wiki.gentoo.org/index.php?title=TFTP&action=edit&redlink=1).

While the FTP protocol itself is insecure, as native support for TLS over FTP is relatively rare among FTP servers, it is technically possible to encrypt the traffic by using a TLS wrapper such as [net-misc/stunnel](https://packages.gentoo.org/packages/net-misc/stunnel). Such a configuration known as FTPS.

## Installation

### USE flags


### Emerge

Historically, the standard Linux FTP client has been [net-ftp/ftp](https://packages.gentoo.org/packages/net-ftp/ftp). There is an SSL USE flag for the package but SSL support remains incomplete. Installing it is is simple enough:

`root #``emerge --ask net-ftp/ftp`
### Alternative FTP Clients

| Name | Package | Description | 
|---|---|---|
| Graphical FTP Clients |  |  | 
| filezilla | [net-ftp/filezilla](https://packages.gentoo.org/packages/net-ftp/filezilla) | A graphical FTP client with lots of useful features and an intuitive file manager style interface. | 
| Console-based FTP Clients |  |  | 
| cmdftp | [net-ftp/cmdftp](https://packages.gentoo.org/packages/net-ftp/cmdftp) | A light weight, yet robust command line FTP client with shell-like functionality. | 
| gftp | [net-ftp/gftp](https://packages.gentoo.org/packages/net-ftp/gftp) | A free multithreaded file transfer client. | 
| lftp | [net-ftp/lftp](https://packages.gentoo.org/packages/net-ftp/lftp) | A sophisticated file transfer program with support for FTP, SFTP, HTTP, HTTPS, and Torrent protocols. | 
| tnftp | [net-ftp/tnftp](https://packages.gentoo.org/packages/net-ftp/tnftp) | NetBSD's native feature rich FTP client. | 
| yafc | [net-ftp/yafc](https://packages.gentoo.org/packages/net-ftp/yafc) | A console-based FTP client with advanced features. | 

## Configuration

The .netrc file is used to configure FTP auto-login behavior. The "default" token in the .netrc file matches any machine name except for explicitly specified machine tokens. There can be only one "default" token, and it should be placed after all machine tokens. It is commonly used in the following format:

**`~/.netrc`**

**Default .netrc Syntax**

This configuration allows the user to automatically log in as an anonymous FTP user to machines not specified in the .netrc file. The auto-login behavior can be disabled by using the "-n" flag.

The following tokens are used in the .netrc file:

**login name**
Identifies a user on the remote machine. If this token is present, the auto-login process will initiate a login using the specified name.

**password string**
Provides a password for the auto-login process. If this token is present, the specified string will be supplied when the remote server requires a password during the login process. Note that if this token is present in the .netrc file for any user other than "anonymous", the auto-login process will be aborted if the .netrc file is readable by anyone besides the user.

**account string**
Supplies an additional account password. If this token is present, the auto-login process will supply the specified string when the remote server requires an additional account password. Alternatively, the auto-login process will initiate an ACCT command if no password is specified.

**macdef name**
Defines a macro. This token functions similarly to the "ftp macdef" command. A macro is defined with the specified name, and its contents start from the next line in the .netrc file and continue until a null line (consecutive new-line characters) is encountered. If a macro named "init" is defined, it is automatically executed as the last step in the auto-login process.

### Service

Historically, the default FTP daemon for Linux was ftpd. Currently, Gentoo supports three different FTP daemons:

- [ProFTPD](https://wiki.gentoo.org/wiki/ProFTPD) ([net-ftp/proftpd](https://packages.gentoo.org/packages/net-ftp/proftpd))
- The [Very secure FTPd](https://wiki.gentoo.org/wiki/Vsftpd) ([net-ftp/vsftpd](https://packages.gentoo.org/packages/net-ftp/vsftpd))
- Pure-FTPd ([net-ftp/pure-ftpd](https://packages.gentoo.org/packages/net-ftp/pure-ftpd))

#### Firewall Configuration for FTP

The following firewall rules are typical of an FTP server providing FTP in the clear:

The following firewall rules are more typical of an FTP server providing FTP over TLS:

## Usage

### Invocation

`user $``ftp`
ftp> help
Commands may be abbreviated.  Commands are:
!		dir		mdelete		qc		site
$		disconnect	mdir		sendport	size
account		exit		mget		put		status
append		form		mkdir		pwd		struct
ascii		get		mls		quit		system
bell		glob		mode		quote		sunique
binary		hash		modtime		recv		tenex
bye		help		mput		reget		tick
case		idle		newer		rstatus		trace
cd		image		nmap		rhelp		type
cdup		ipany		nlist		rename		user
chmod		ipv4		ntrans		reset		umask
close		ipv6		open		restart		verbose
cr		lcd		prompt		rmdir		?
delete		ls		passive		runique
debug		macdef		proxy		send

Basic usage of the ftp client is simple enough. Downloading a file works like this:

`user $``ftp gentoo.org`
ftp> get \<desired\_file>
ftp> exit

The file will be deposited in the current working (local) directory.

Uploading a file works similarly:

`user $``ftp gentoo.org`
ftp> cd \<remote\_directory>
ftp> send \<local\_path\_and\_filename>
ftp> exit

## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose net-ftp/ftp`
## See also

- [vsftpd](https://wiki.gentoo.org/wiki/Vsftpd) — an FTP server for UNIX-like systems.
- [ProFTPD](https://wiki.gentoo.org/wiki/ProFTPD) — highly configurable FTP server.
- [SFTP](https://wiki.gentoo.org/wiki/SFTP) — an interactive file transfer program, similar to [FTP], which performs all operations over an encrypted [SSH](https://wiki.gentoo.org/wiki/SSH) transport.
- [TFTP](https://wiki.gentoo.org/index.php?title=TFTP&action=edit&redlink=1)
