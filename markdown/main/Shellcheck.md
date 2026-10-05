<!-- source: https://wiki.gentoo.org/wiki/Shellcheck | group: Gentoo Wiki (Main) | wiki-title: Shellcheck -->
---
title: ShellCheck
url: https://wiki.gentoo.org/wiki/Shellcheck
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-02-08"
fingerprint: "15c2492e7fef2ac1"
license: CC BY-SA 4.0
---

# ShellCheck

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**ShellCheck** is a [shell](https://wiki.gentoo.org/wiki/Shell) script static analysis tool written in [Haskell](https://wiki.gentoo.org/wiki/Haskell). It gives various warnings about shell scripts — mainly syntax and semantic problems related to the typical beginner issues, counter-intuitive behavior, or various corner cases.

As of version 0.9.0, ShellCheck supports sh, [bash](https://wiki.gentoo.org/wiki/Bash), [dash](https://wiki.gentoo.org/wiki/Dash), and ksh shells.[\[1\]](https://wiki.gentoo.org#cite_note-1)

It can be integrated in a build suite or [CI/CD](https://en.wikipedia.org/wiki/CI/CD) pipelines such as [Jenkins](https://wiki.gentoo.org/wiki/Jenkins) or [GitLab](https://wiki.gentoo.org/wiki/GitLab).

## Installation

### USE flags


| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [hscolour](https://packages.gentoo.org/useflags/hscolour) | Include coloured haskell sources to generated documentation (dev-haskell/hscolour) | 
| [profile](https://packages.gentoo.org/useflags/profile) | Add support for software performance analysis (will likely vary from ebuild to ebuild) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

#### Binary package (shellcheck-bin)

`root #``emerge --ask dev-util/shellcheck-bin`
#### Source package

`root #``emerge --ask dev-util/shellcheck`
## Configuration

### Files

ShellCheck's behavior can be modified using directives (like `disable` or `enable`) directly in the validated script or using configuration files:

- \~/.shellcheckrc - Local (per user) configuration file.
- /home/larry/project/.shellcheckrc - Per project configuration file.

## Usage

### Invocation

Script validation with an example error output:

`user $``shellcheck validated-script.sh````
In validated-script.sh line 5:
echo $DATE
     ^---^ SC2086 (info): Double quote to prevent globbing and word splitting.
Did you mean: 
echo "$DATE"
For more information:
  https://www.shellcheck.net/wiki/SC2086 -- Double quote to prevent globbing ...
```
## See also

- [Shell](https://wiki.gentoo.org/wiki/Shell) — command-line interpreter that provides a text-based interface to users
