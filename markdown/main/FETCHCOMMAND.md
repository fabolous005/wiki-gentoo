<!-- source: https://wiki.gentoo.org/wiki/FETCHCOMMAND | group: Gentoo Wiki (Main) | wiki-title: FETCHCOMMAND -->
---
title: FETCHCOMMAND
url: https://wiki.gentoo.org/wiki/FETCHCOMMAND
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-27"
fingerprint: a7b97a1f073bf9ad
license: CC BY-SA 4.0
---

# FETCHCOMMAND

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

`FETCHCOMMAND` is a variable that defines the command used for fetching package sources from the Internet. It can be set in the [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) configuration file.

## Syntax

The following placeholders can be used:

| Placeholder | Meaning | Example | Required | 
|---|---|---|---|
| `\${DISTDIR}` | Location of your local source file repository | /var/cache/distfiles | Yes | 
| `\${FILE}` | Destination file name | example-1.0.tar.xz | Yes | 
| `\${URI}` | Download URL | [https://example.com/download/example-1.0.tar.xz](https://example.com/download/example-1.0.tar.xz) | Yes | 
| `\${DIGESTS}` | Space separated list of file digests | blake2b:\<hexdigest> sha512:\<hexdigest> |  | 

## Variants

Specific variants of `FETCHCOMMAND` supported by Portage<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> are listed below:

| Placeholder | Meaning | Example | 
|---|---|---|
| `FETCHCOMMAND` | Used for HTTP/HTTPS/FTP downloads | `FETCHCOMMAND="wget -t 3 -T 60 --passive-ftp -O \"\${DISTDIR}/\${FILE}\" \"\${URI}\""` | 
| `RESUMECOMMAND` | Used to resume a HTTP/HTTPS/FTP download | `RESUMECOMMAND="wget -c -t 3 -T 60 --passive-ftp -O \"\${DISTDIR}/\${FILE}\" \"\${URI}\""` | 
| `FETCHCOMMAND_FTP` | A more specific variant used exclusively for FTP |  | 
| `RESUMECOMMAND_FTP` | A more specific variant used exclusively for FTP |  | 
| `FETCHCOMMAND_RSYNC` | Used for downloading from an rsync mirror | `FETCHCOMMAND_RSYNC="rsync -LtvP \"\${URI}\" \"\${DISTDIR}/\${FILE}\""` | 
| `RESUMECOMMAND_RSYNC` | As above |  | 
| `FETCHCOMMAND_SSH` | Used for downloading over SSH |  | 
| `RESUMECOMMAND_SSH` | As above |  | 
| `FETCHCOMMAND_SFTP` | Used for downloading from a SFTP mirror |  | 

## Speeding up downloads with Wget2

[net-misc/wget2](https://packages.gentoo.org/packages/net-misc/wget2) is the successor to GNU Wget.

In many cases Wget2 downloads much faster than Wget1.x due to HTTP2, HTTP compression,
parallel connections and use of If-Modified-Since HTTP header.[\[2\]](https://wiki.gentoo.org#cite_note-2)

Add the following line to your [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) file:

**`/etc/portage/make.conf`**

```
FETCHCOMMAND="wget2 -v --metalink=off --progress=none -t 3 -T 60 -O \"\${DISTDIR}/\${FILE}\" \"\${URI}\""
RESUMECOMMAND="wget2 -v --metalink=off --progress=none -c -t 3 -T 60 -O \"\${DISTDIR}/\${FILE}\" \"\${URI}\""
# Wget2 has no support for FTP
FETCHCOMMAND_FTP="wget -t 3 -T 60 --passive-ftp -O \"\${DISTDIR}/\${FILE}\" \"\${URI}\""
RESUMECOMMAND_FTP="wget -c -t 3 -T 60 --passive-ftp -O \"\${DISTDIR}/\${FILE}\" \"\${URI}\""
```
## Speeding up downloads with aria2

fetchcommandwrapper combines download tool [net-misc/aria2](https://packages.gentoo.org/packages/net-misc/aria2) with variable `[GENTOO_MIRRORS](https://wiki.gentoo.org/wiki/GENTOO_MIRRORS)` to speed up distfile downloads of Portage by downloading from multiple mirrors at the same time.

### Install

`root #``emerge --ask app-portage/fetchcommandwrapper`
Then add the following line to your [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) file:

**`/etc/portage/make.conf`**

```
source /usr/share/fetchcommandwrapper/make.conf
```
