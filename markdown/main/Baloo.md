<!-- source: https://wiki.gentoo.org/wiki/Baloo | group: Gentoo Wiki (Main) | wiki-title: Baloo -->
---
title: Baloo
url: https://wiki.gentoo.org/wiki/Baloo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-09-08"
fingerprint: fb07f23d4d96abba
license: CC BY-SA 4.0
---

# Baloo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Baloo** is a file-indexing and search framework provided as part of the [KDE](https://wiki.gentoo.org/wiki/KDE)/Plasma software suite.

Baloo includes a [daemon](https://wiki.gentoo.org/wiki/Category:Daemons) for file indexing, a search framework (that powers for example the [Dolphin](https://wiki.gentoo.org/wiki/Dolphin) file manager's search functionality), and a [commnd-line](https://wiki.gentoo.org/wiki/Shell) utility (described below).

## Installation

The [kde-frameworks/baloo](https://packages.gentoo.org/packages/kde-frameworks/baloo) package won't usually be [emerged](https://wiki.gentoo.org/wiki/Emerge) manually, but pulled in by installing a piece of software that depends on it.

### USE flags


### USE flags for
            [kde-frameworks/baloo](https://packages.gentoo.org/packages/kde-frameworks/baloo)
            
            Framework for searching and managing metadata

| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

## Configuration

In KDE/Plasma, some basic Baloo settings can be managed through a [GUI](https://wiki.gentoo.org/wiki/Category:GUI_software) by opening the *Application Launcher*, clicking on the *System* category, then opening the *System Settings* tool. Scroll down the list of settings to select the *Search* entry.

This dialogue contains an option to enable or disable Baloo indexing for the current user's KDE/plasma sessions.

### Files

- \~/.config/baloofilerc - User-specific configuration file. See [upstream documentation](https://community.kde.org/Baloo/Configuration) for details.
- /etc/xdg/autostart/baloo\_file.desktop - XDG autostart file to allow the Baloo daemon to automatically run upon user login (for supported [desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment)). If this behavior is not desired, this file may be modified by the system administrator to change this behavior system-wide as required, for example by removing the auto start line for certain desktop environments.

### Services

#### systemd

On systemd systems, the Baloo utility can be controlled as [user service](https://wiki.gentoo.org/wiki/Systemd#User_services) and configured to automatically run in the background.

To disable the Baloo systemd user service:

`user $``systemctl --user disable --now kde-baloo.service`
#### XDG autostart

When [kde-frameworks/baloo](https://packages.gentoo.org/packages/kde-frameworks/baloo) is installed, it will place a baloo\_file.desktop [.desktop file](https://wiki.gentoo.org/wiki/.desktop_files) in /etc/xdg/autostart. This will allow some [desktop environments](https://wiki.gentoo.org/wiki/Desktop_environment) to start baloo during desktop session startup.

For information on how to enable or disable the autostarting of services provided by through the XDG autostart mechanism, refer to the appropriate desktop environment documentation.[\[1\]](https://wiki.gentoo.org#cite_note-1)

## Usage

### Invocation

Baloo includes a userspace utility to manage its functionality:

`user $``balooctl6 --help````
Usage: balooctl6 [options] command status enable disable purge suspend resume check index clear config monitor indexSize failed
Options:
  -f, --format <format>  Output format <multiline|json|simple>.
                         The default format is "multiline".
                         Only applies to "balooctl status <file>"
  -v, --version          Displays version information.
  -h, --help             Displays help on commandline options.
  --help-all             Displays help including Qt specific options.
Arguments:
  command                The command to execute
  status                 Print the status of the indexer
  enable                 Enable the file indexer
  disable                Disable the file indexer
  purge                  Remove the index database
  suspend                Suspend the file indexer
  resume                 Resume the file indexer
  check                  Check for any unindexed files and index them
  index                  Index the specified files
  clear                  Forget the specified files
  config                 Modify the Baloo configuration
  monitor                Monitor the file indexer
  indexSize              Display the disk space used by index
  failed                 Display files which could not be indexed
```
To avoid any possible confusion, note that this command used to be named balooctl but in Gentoo since plasma 6 the command is balooctl6.[\[2\]](https://wiki.gentoo.org#cite_note-2)

### Check index size

Disk space consumed by index can be displayed using:

`user $``balooctl6 indexSize````
File Size: 6.27 GiB
Used:      3.66 GiB
           PostingDB:     978.16 MiB    26.124 %
          PositionDB:       1.46 GiB    39.929 %
            DocTerms:     875.02 MiB    23.369 %
    DocFilenameTerms:     116.66 MiB     3.116 %
       DocXattrTerms:            0 B     0.000 %
              IdTree:      31.55 MiB     0.843 %
          IdFileName:     133.30 MiB     3.560 %
             DocTime:      81.82 MiB     2.185 %
             DocData:      27.00 MiB     0.721 %
   ContentIndexingDB:            0 B     0.000 %
         FailedIdsDB:       4.00 KiB     0.000 %
             MTimeDB:       5.71 MiB     0.153 %
```
### Purge index

To free up disk space consumed by the index, issue:

`user $``balooctl6 purge`
Stopping the File Indexer .... - done
Deleted the index database
Restarting the File Indexer

### Check indexing status

`user $``balooctl6 status`
...
Baloo File Indexer is running
Indexer state: Indexing file content
...

### Disable indexing

`user $``balooctl6 disable`
## Troubleshooting

### Baloo is using 100% of one CPU core

When Baloo is running it will use 100% of one CPU core. To disable indexing (which will impact the ability to search for files) see the [disable section](https://wiki.gentoo.org#Disable_indexing) above.

## See also

- [Plasma](https://wiki.gentoo.org/wiki/Plasma) — a free software community, producing a wide range of applications including the popular Plasma desktop environment.
