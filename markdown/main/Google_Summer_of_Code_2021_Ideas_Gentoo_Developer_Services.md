<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2021/Ideas/Gentoo_Developer_Services | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2021/Ideas/Gentoo Developer Services -->
---
title: Google Summer of Code/2021/Ideas/Gentoo Developer Services
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2021/Ideas/Gentoo_Developer_Services
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-03-22"
fingerprint: "738ab30dd142178"
license: CC BY-SA 4.0
---

# Google Summer of Code/2021/Ideas/Gentoo Developer Services

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Gentoo is running a number of core services for both users and developers, many of which are bespoke for the community. Gentoo developers are constantly working on improving these services to deliver an enhanced experience for all Gentoo users and developers. This year, you have the chance to be part of a major overhaul of these services. That is, we have been rewriting a lot of these services and you can help with finally getting these rewrites into production to modernize and improve our infrastructure. The goal at end of the project is to have the old services fully replaced in production, to enhances the services we offer for all Gentoo users and developers.

In detail, this involves:

- **Gentoo mail archives ([https://archives.gentoo.org](https://archives.gentoo.org)):** Last year, we've created a complete rewrite of our mail archive ([source code](https://gitweb.gentoo.org/sites/archives.git/)). Mostly notably, this rewrite   provides the possibility of searching for mails using full text search. This rewrite is nearly ready for production. Your job would be to finish the work on the ingress of new mails as well as getting the new solution into production - meaning you will understand and fixing bugs that arise, once the rewrite is in production.
- **Gentoo GLSAMaker ([https://glsamaker.gentoo.org](https://glsamaker.gentoo.org), internal use only):** Last year, we've also created a complete rewrite of our GLSAMaker ([source code](https://gitweb.gentoo.org/proj/glsamaker.git/)). The GLSAMaker is a tool for Gentoo's Security Team to create and publish Gentoo Linux Security Advisories (GLSAs). The new rewrite is still missing some features compared to the old GLSAMaker. Part of your responsibility here is implementing these missing features. Also you will be working with the security team to clarify which features might be useful to enhance their workflow. Finally, the goal here is also to get the new rewrite into production as well. Again, this means understanding and fixing bugs that arise, once the rewrite is in production.
- **Gentoo Packages Database ([https://packages.gentoo.org](https://packages.gentoo.org)):** Finally, we have also created a rewrite of our packages database last year ([source code](https://gitweb.gentoo.org/sites/soko.git/)). This rewrite is already running live in production. Recently, we've been starting to add more functionality to let the user customize the information based on their preferences. These changes are, however, not working perfectly well currently. You've got the chance to enhance the user experience by reworking the customization features. Part of this will also include communicating with the packages.gentoo.org userbase to see which features are needed here.
- **Gentoo Recruiting Platform:** In former times we have been using our own recruiting platform ([source code](https://gitweb.gentoo.org/proj/recruiting-webapp.git/)) for recruiting new Gentoo developers. Actually this is an old Google Summer of Code project. This project is currently not used in production anymore. Reviving this project will help recruiting more developers in future, which in turn will help the whole community. So the goal here is to get the recruiting platform working in production again.
- **Containerization of our existing services:** We've already been starting to containerize a lot of our services. However, there is still work needed to create gentoo-based container images for some of our services. Part of this effort is to create container images for services that are not fully containerized yet, e.g. the mail archive or improve existing container images which are not using Gentoo-based images yet.


After the end of the project, you've got the chance to continue helping to maintain the services as an external maintainer in case you are interested. 
Alternatively, you can take even more responsibility by becoming an official Gentoo developer and take care of the services this way. 
Of course, the existing base of developers will help you with any questions that arise throughout.

All in all, you've got the chance to become a major part of the modernization and enhancement of our core services for users and developers. These projects touch many of the core services that Gentoo developers and users interact with to both keep the distribution running and use it daily.

Last but not least, most of our services are nowadays written in Go. So ideally speaking you should be either familiar with the language or don't mind to dig into it while working on the services. Don't get scared by this though, if you are motivated and ready to learn something new, you are good to go. Please, don't hesitate to reach out in case of any questions.



| Contacts | Required Skills | 
|---|---|
|  |  |
