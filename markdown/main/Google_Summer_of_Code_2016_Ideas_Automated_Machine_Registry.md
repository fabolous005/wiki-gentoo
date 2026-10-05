<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2016/Ideas/Automated_Machine_Registry | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2016/Ideas/Automated Machine Registry -->
---
title: Google Summer of Code/2016/Ideas/Automated Machine Registry
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2016/Ideas/Automated_Machine_Registry
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: "6ff7dd444868c4eb"
license: CC BY-SA 4.0
---

# Google Summer of Code/2016/Ideas/Automated Machine Registry

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Build an automated machine registry for automated server installations. This idea involves creating a Registry Server (no Web UI for the initial project) that would receive requests from clients embedded in Gentoo minimal install media (whatever the format).

The idea is when a server boots up, it registers automatically to the Registry Server and then, according to some of the data the server registers with and profiles associated with it (Type of machine, capabilities, model), it can either be automatically assigned a role (then tie it up with Puppet/Chef), or just be accessed for further configuration.

The project would involve writing:

- Registry
- Client code
- Client and Registry ebuild
- Scripts to add the client to the install media
- Tools to query the Registry (machines avail, add profile, add/update/del machine, etc).

The registry would maintain information about the state of machines, maybe even add handlers for monitoring systems etc.



| Contacts | Required Skills | 
|---|---|
|  |  |
