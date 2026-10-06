<!-- source: https://wiki.gentoo.org/wiki/Collectd | group: Gentoo Wiki (Main) | wiki-title: Collectd -->
---
title: collectd
url: https://wiki.gentoo.org/wiki/Collectd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: a090136ecb9ebf08
license: CC BY-SA 4.0
---

# collectd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**collectd** is a daemon that regularly collects, transfers, and stores system information. It is typically used for collecting performance information and other system data like disk, memory, and network usage, and from many other sources using a number of plugins. [Other software](https://collectd.org/wiki/index.php/List_of_front-ends) can then be used to read databases of collected information to plot it and analyze it.

## Installation

### USE Flags


### USE flags for
            [app-metrics/collectd](https://packages.gentoo.org/packages/app-metrics/collectd)
            
            Collects system statistics and provides mechanisms to store the values

| [+filecaps](https://packages.gentoo.org/useflags/+filecaps) | When set collectd daemon will have set required capabilities to run most plugins even if run as unprivileged user | 
| [contrib](https://packages.gentoo.org/useflags/contrib) | Install user-contributed files in the doc directory | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [java](https://packages.gentoo.org/useflags/java) | Must be set (workaround for java-pkg-opt-2 eclass limitation) when you want java or genericjmx plugin | 
| [perl](https://packages.gentoo.org/useflags/perl) | Add optional support/bindings for the Perl language | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [udev](https://packages.gentoo.org/useflags/udev) | Enable optional udev usage in disk plugin; Required for smart plugin | 
| [xfs](https://packages.gentoo.org/useflags/xfs) | Enable optional capability to filter on XFS file system in df plugin; Requires XFS headers from sys-fs/xfsprogs | 

### Emerge

First, determine which plugins you want on your system. The collectd wiki contains a [list of plugins](https://collectd.org/wiki/index.php/Table_of_Plugins), or check the *collectd* [man page](https://wiki.gentoo.org/wiki/Man_page). These plugins are specified as USE flags with a `collectd_plugins_` prefix, though these can also be specified with a `COLLECTD_PLUGINS` variable in /etc/portage/make.conf:

**`/etc/portage/package.use/collectd`**

**Define some plugins with COLLECTD\_PLUGINS**

```
*/* COLLECTD_PLUGINS: load memory syslog
```
collectd requires a plugin to enable any sort of logging. Make sure you enable the [syslog plugin](https://collectd.org/wiki/index.php/Plugin:SysLog) and/or the [logfile plugin](https://collectd.org/wiki/index.php/Plugin:LogFile).

To write data, you need to enable write plugins. The [rrdtool plugin](https://collectd.org/wiki/index.php/Plugin:RRDtool) is a common plugin. Data can be written to other collectd daemons using the [network plugin](https://collectd.org/wiki/index.php/Plugin:Network).

To install:

`root #``emerge --ask app-metrics/collectd`
## Configuration

With collectd and some plugins installed, enable them in the configuration file by uncommenting them, for instance, the syslog plugin:

**`/etc/collectd.conf`**

**Enabling syslog**

```
LoadPlugin syslog
<Plugin syslog>
        LogLevel info  
</Plugin>
```
### rrdtool plugin

Default location for saving the rrd files by collectd is BaseDir but that will mix the rrdtool's files with other plugins. So redefine it.

**`/etc/collectd.conf`**

```
<Plugin rrdtool>
        DataDir "/var/lib/collectd/rrd"
</Plugin>
```
### Service

To start collectd on start-up:

`root #``rc-update add collectd default`
## Troubleshooting

Check the logs for errors after starting up to make sure everything is going smoothly.

## Plotting front-end packages

Also see the [full list of front-ends known by collectd](https://collectd.org/wiki/index.php/List_of_front-ends).
