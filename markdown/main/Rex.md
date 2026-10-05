<!-- source: https://wiki.gentoo.org/wiki/Rex | group: Gentoo Wiki (Main) | wiki-title: Rex -->
---
title: rex
url: https://wiki.gentoo.org/wiki/Rex
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-27"
fingerprint: b41cdd7526a39aeb
license: CC BY-SA 4.0
---

# rex

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**rex** sometimes stylized as `**(R)?ex**` is a configuration automation framework written in [Perl](https://wiki.gentoo.org/wiki/Perl) known for its shallow learning curve and ease of extensibility. Widely regarded as a user-friendly automation framework, unlike better known alternatives such as [Ansible](https://wiki.gentoo.org/wiki/Ansible), rex does not use YAML to describe tasks by default. Rather, it uses small easily understood snippets of Perl to describe tasks. Indeed, the entire system can readily be extended with custom Perl modules if needed.

Like many other configuration management tools rex is agentless, only requiring valid SSH keys and a working Perl interpreter on the managed system to function. Further, development currently targets Perl 5.10.1 (2009) which makes it possible to easily support legacy systems that would be difficult or impossible for other tools to manage.

## Installation

### USE flags


### Emerge

`root #``emerge --ask app-admin/rex`
## Usage

### Invocation

`user $``rex -h`
usage:
  rex \[\<options>\] \[-H \<host>\] \[-G \<group>\] \<task> \[\<task-options>\]
  rex -T\[m|y|v\] \[\<string>\]
  -b     Run batch
  -e     Run the given code fragment
  -E     Execute a task on the given environment
  -G|-g  Execute a task on the given server groups
  -H     Execute a task on the given hosts (space delimited)
  -z     Execute a task on hosts from this command's output
  -K     Public key file for the ssh connection
  -P     Private key file for the ssh connection
  -p     Password for the ssh connection
  -u     Username for the ssh connection
  -d     Show debug output
  -ddd   Show more debug output (includes profiling output)
  -m     Monochrome output: no colors
  -o     Output format
  -q     Quiet mode: no log output
  -qw    Quiet mode: only output warnings and errors
  -Q     Really quiet: output nothing
  -T     List tasks
  -Ta    List all tasks, including hidden
  -Tm    List tasks in machine-readable format
  -Tv    List tasks verbosely
  -Ty    List tasks in YAML format
  -c     Turn cache ON
  -C     Turn cache OFF
  -f     Use this file instead of Rexfile
  -F     Force: disregard lock file
  -h     Display this help message
  -M     Load this module instead of Rexfile
  -O     Pass additional options, like CMDB path
  -s     Use sudo for every command
  -S     Password for sudo
  -t     Number of threads to use (aka 'parallelism' param)
  -v     Display (R)?ex version

### Unmerge

`root #``emerge --ask --depclean --verbose app-admin/rex`
