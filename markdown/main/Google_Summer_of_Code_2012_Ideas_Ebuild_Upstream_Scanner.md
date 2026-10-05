<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Ebuild_Upstream_Scanner | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2012/Ideas/Ebuild Upstream Scanner -->
---
title: Google Summer of Code/2012/Ideas/Ebuild Upstream Scanner
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Ebuild_Upstream_Scanner
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-04-02"
fingerprint: "7bfbcf49097b53be"
license: CC BY-SA 4.0
---

# Google Summer of Code/2012/Ideas/Ebuild Upstream Scanner

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

euscan (Ebuild Upstream Scanner) is an utility to check if an ebuild
have new upstream version. It was designed to provide the same
features as debian's uscan and DEHS ([http://dehs.alioth.debian.org/](http://dehs.alioth.debian.org/)).
euscan is also a web interface that aggregates the result of euscan
ran on all gentoo ebuilds (currently hosted at
[http://euscan.iksaif.net](http://euscan.iksaif.net) but will move to gentoo.org someday). There
are a lot of things to do on euscan, here are some examples:

#### Celery port

Currently euscan use shell scripts and GNU parallel to scan the portage tree. Using celery (a python task queue that integrates well with django) would allow a more flexible scan process. Scanning the tree or a package could be done with a simple django command starting a celery task, the web interface could rescan old packages on-demand too. While adding celery commands, it would be great to add simple commands to: create the local filesytem needed by euscan, add/sync/remove an overlay, etc... Celery is not really packaged for gentoo, but I got some ebuilds in my overlay, so another side of the project could be to push them to the portage tree.

#### Version Detection Enhancements

euscan currently use multiple heuristics to determine upstream version, but there is a lot to do to enhance that to remove false positive and be able to scan more package:

- create a script to gather statistics on euscan success and failure and use methods.
- use metadata.xml's \<upstream>\<remote-id> tag with the appropriate euscan site-handler (pypi, pearl, cpan, etc...) and make sure that all packages have this tag correctly set in the portage tree
- create a new metadata tag \<upstream>\<watch> that works like debian/watch and make euscan use it. Create a script to import informations from the associated debian/watch file.
- steal ideas from other tools (uscan, portscout) and use other source of data (youri, distrowatch, distromatch, whoas; Equivalent-Packages)

#### Euscan Web enhancements

Add an account system, and allow maintainer to register to subscribe
to automatic weekly notifications. Integrate euscan with other webapps
like [http://portage.gentoo.org](http://portage.gentoo.org), [http://znurt.org/](http://znurt.org/),
[http://gpo.zugaina.org/](http://gpo.zugaina.org/), [Gentoostats](https://wiki.gentoo.org/wiki/Gentoostats) and try
to create a single killer webapp . Add better overlay support and
tweak django administration.



| Contacts | Required Skills | 
|---|---|
|  |  |
