<!-- source: https://wiki.gentoo.org/wiki/Etc-update | group: Gentoo Wiki (Main) | wiki-title: Etc-update -->
---
title: Etc-update
url: https://wiki.gentoo.org/wiki/Etc-update
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-15"
fingerprint: "94127e69c6a79ad1"
license: CC BY-SA 4.0
---

# Etc-update

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**etc-update** is a tool to merge configuration files with an interactive merging setup and can also auto-merge trivial changes.

`root #``etc-update --help````
etc-update: Handle configuration file updates
Usage: etc-update [options] [paths to scan]
If no paths are specified, then ${CONFIG_PROTECT} will be used.
Options:
  -d, --debug    Enable shell debugging
  -h, --help     Show help and run away
  -p, --preen    Automerge trivial changes only and quit
  -q, --quiet    Show only essential output
  -v, --verbose  Show settings and such along the way
  -V, --version  Show version and trundle away
  --automode <mode>
             -3 to auto merge all files
             -5 to auto-merge AND not use 'mv -i'
             -7 to discard all updates
             -9 to discard all updates AND not use 'rm -i'
```
After merging the straightforward changes, a list of protected files will be provided that have an update waiting. At the bottom the possible options are shown:

When entering `-1`, etc-update will exit and discontinue any other changes. With `-3` or `-5`, all listed configuration files will be overwritten with the newer versions. It is therefore very important to first select the configuration files that should not be automatically updated. This is simply a matter of entering the number listed to the left of that configuration file.

As an example, we select the configuration file /etc/pear.conf:

The differences between the two files are shown. If the updated configuration file can be used without problems, enter `1`. If the updated configuration file isn't necessary, or doesn't provide any new or useful information, enter `2`. If the current configuration file has to be interactively updated, enter `3`.

There is no point in further elaborating the interactive merging here. For completeness sake, we will list the possible commands that can be used while interactively merging the two files. Users are greeted with two lines (the original one, and the proposed new one) and a prompt at which the user can enter one of the following commands:

After having finished updating the important configuration files, users can then automatically update all the other configuration files. etc-update will exit if it doesn't find any more updateable configuration files.

## See also

- [cfg-update](https://wiki.gentoo.org/wiki/Cfg-update) — a utility used on Gentoo to manage configuration file updates.
- [dispatch-conf](https://wiki.gentoo.org/wiki/Dispatch-conf) — a utility included with [Portage](https://wiki.gentoo.org/wiki/Portage), used to safely and conveniently manage configuration files after package updates.
