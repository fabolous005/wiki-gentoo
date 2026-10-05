<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas/Log_Collector | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2013/Ideas/Log Collector -->
---
title: Google Summer of Code/2013/Ideas/Log Collector
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas/Log_Collector
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-04-12"
fingerprint: e52d4de72a354190
license: CC BY-SA 4.0
---

# Google Summer of Code/2013/Ideas/Log Collector

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

#### Log collector/analyzer for tinderbox build logs

Flameeyes's tinderbox has been using a custom, spit-and-chewingum log collector and analyzer written in Ruby for a while. A replacement is dearly needed.

The ideal software would be a daemon service that receives build logs from Portage, both for storage in full and for metadata extraction (package name, maintainers, type of failure if any, emerge --info output, ...), and then provide a (password-protected) web interface that allows to read the failing logs, and open bugs at Gentoo's Bugzilla.

Bonus points for:

- being completely standalone (current setup depends on Amazon's S3);
- attaching logs instead of just linking them (size limits could make it hard to achieve);
- searching for the list of already open bugs for the failing package;
- proper integration with Portage so that no extra software is required on the tinderbox side to push the logs;
- standard, HTTP-based protocol between the tinderbox and the collector, which can be proxied through Squid;
- being written in Python to reduce the language fragmentation (current setup is written in Shell and Ruby).

Further information on [Flameeyes's Weblog](https://flameeyes.blog/2013/02/28/gsoc-proposal-a-better-log-aggregator-analyzer/).



| Contacts | Required Skills | 
|---|---|
|  |  |
