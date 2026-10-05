<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Porthole_plug-ins_and_extensions | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2012/Ideas/Porthole plug-ins and extensions -->
---
title: Google Summer of Code/2012/Ideas/Porthole plug-ins and extensions
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Porthole_plug-ins_and_extensions
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-01-10"
fingerprint: "676de3cc9a2bdde2"
license: CC BY-SA 4.0
---

# Google Summer of Code/2012/Ideas/Porthole plug-ins and extensions

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Porthole is a GTK+-based frontend to Portage. This project would enable Porthole to improve its ability to manage remote computers, gather and report package statistics, and more. The work would encompass creating:

- A basic python based control interface API for linking to remote computers and groups of computers to gather information about installed packages, and install/update those using portage and/or pkgcore as remote backends.  One possible means of making those connections is by using [dev-python/pyro](https://packages.gentoo.org/packages/dev-python/pyro).
- Extending the work started in the public\_api branch of portage to include running emerge via the public api.
- Create a simple cli for gathering/reporting update info for #1
- Create a porthole plug-in for connecting to the control interface, displaying the info in portholes views and and dispatching desired actions to the remotes via the control interface.



| Contacts | Required Skills | 
|---|---|
|  |  |
