<!-- source: https://wiki.gentoo.org/wiki/Erlang | group: Gentoo Wiki (Main) | wiki-title: Erlang -->
---
title: Erlang
url: https://wiki.gentoo.org/wiki/Erlang
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-15"
fingerprint: f743997c8a82b9cc
license: CC BY-SA 4.0
---

# Erlang

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Erlang** is a concurrent, functional language, adapted to writing distributed, fault tolerant systems.

## Installation

### USE flags


### USE flags for
            [dev-lang/erlang](https://packages.gentoo.org/packages/dev-lang/erlang)
            
            Erlang programming language, runtime environment and libraries (OTP)

| [+kpoll](https://packages.gentoo.org/useflags/+kpoll) | Enable kernel polling support | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [emacs](https://packages.gentoo.org/useflags/emacs) | Add support for GNU Emacs | 
| [java](https://packages.gentoo.org/useflags/java) | Add support for Java | 
| [odbc](https://packages.gentoo.org/useflags/odbc) | Add ODBC Support (Open DataBase Connectivity) | 
| [sctp](https://packages.gentoo.org/useflags/sctp) | Support for Stream Control Transmission Protocol | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [tk](https://packages.gentoo.org/useflags/tk) | Add support for Tk GUI toolkit | 
| [wxwidgets](https://packages.gentoo.org/useflags/wxwidgets) | Add support for wxWidgets/wxGTK GUI toolkit | 

### Emerge

Install:

`root #``emerge --ask dev-lang/erlang`
## Usage

### Invocation

`user $``erl`
Erlang/OTP 24 \[erts-12.0.2\] \[source\] \[64-bit\] \[smp:12:12\] \[ds:12:12:10\] \[async-threads:1\] \[jit\]
Eshell V12.0.2  (abort with ^G)
1>

To exit the shell: `Ctrl`+`g` then `q` and `Return`.
