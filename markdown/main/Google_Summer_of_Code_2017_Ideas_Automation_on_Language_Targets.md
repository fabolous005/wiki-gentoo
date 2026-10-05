<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2017/Ideas/Automation_on_Language_Targets | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2017/Ideas/Automation on Language Targets -->
---
title: Google Summer of Code/2017/Ideas/Automation on Language Targets
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2017/Ideas/Automation_on_Language_Targets
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: "376cbaec4f548a9b"
license: CC BY-SA 4.0
---

# Google Summer of Code/2017/Ideas/Automation on Language Targets

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

It often takes a while for Gentoo to move to the latest versions of Python, Ruby, etc. This is often due to the fact, that our eclasses are build in a way, that they require PYTHON\_COMPAT or RUBY\_TARGETS to be set to the latest version. The goal of this idea is to build an interface for developers, that provides them information (probably a graph) on which packages require the language target to be enabled first.

The basic idea does probably not have enough content for 3 months full-time work, it needs to be extended, e.g. by automating the test process, building a frontend, thinking about solutions for cyclic graphs (test dependencies depend on the to be tested package themselves) or verifying with upstream information (e.g. if this python version supported is mentioned in packages' setup.py).



| Contacts | Required Skills | 
|---|---|
|  |  |
