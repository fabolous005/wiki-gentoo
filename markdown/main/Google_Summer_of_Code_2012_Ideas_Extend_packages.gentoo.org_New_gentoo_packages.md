<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Extend_packages.gentoo.org_New_gentoo_packages | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2012/Ideas/Extend packages.gentoo.org New gentoo packages -->
---
title: Google Summer of Code/2012/Ideas/Extend packages.gentoo.org New gentoo packages
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Extend_packages.gentoo.org_New_gentoo_packages
hostname: gentoo.org
sitename: Google Summer of Code/2012/Ideas/Extend packages.gentoo.org New gentoo packages
date: "2022-04-02"
fingerprint: "452b7d2009e1251b"
license: CC BY-SA 4.0
---

# Google Summer of Code/2012/Ideas/Extend packages.gentoo.org New gentoo packages

From Gentoo Wiki

\< [Google Summer of Code](https://wiki.gentoo.org/wiki/Google_Summer_of_Code) | [2012](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012) | [Ideas](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## [Extend packages.gentoo.org New gentoo packages]

Creating new packages.gentoo.org with rich web interface and advanced features. Port it to django.

gentoo-packages is new version of packages.gentoo.org site written on Python using Django framework.

You could find sources on GitHub or here.  [https://github.com/bacher09/gentoo-packages](https://github.com/bacher09/gentoo-packages)

Summary:

- implemented package scanning utility, that collect info about herds, maintainers, packages, use flags, licenses. It could collect info about overlays. Scanning tool support collection packages data through portage or pkgcore. For that was developed backends mechanism. It also could validate some data on database.
- implemented web interface with many server pages: Ebuilds view for fresh ebuilds, packages views for searching packages, repositories view for show all available repositories, repository view for displaying repository info and stats, licenses and license view for showing info about license, maintainers view for displaying info about maintainers, categories view for showing categories list, package view for displaying info about package (changelog, use flags, keywords, depends, herds, maintainers, metadata, etc), ebuild view for showing info just about that ebuild. In packages view they could be sorted by update or created time, that allows show newest packages. User could choice info about what arches would be showed to him. Also user could search packages by many params like herd, use flag, license, category, overlay. For ebuild also available rss and atom feeds. For performance boost some data are cached.

Plans for the future:

- Extending package search, allow search by arch.
- Use solr for searching.
- Use celery for scanning tasks.
- Add the ability to show packages screenshots (some thing similar to screenshots.debian.net).
- Extend rss and atom feeds.
- Deploy server to packages.gentoo.org site.
- Some other minor plans, I will add TODO list on next week.

I want thank my mentor Matthew Summers, Brian Dolbec and all other who helped me working on my project.




| Contacts | Required Skills | 
|---|---|
|  |  | 



#### Mailing List Archives

 [Extend packages.gentoo.org New gentoo packages - Mailing List Archives](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Extend_packages.gentoo.org_New_gentoo_packages/MailingListArchives)
