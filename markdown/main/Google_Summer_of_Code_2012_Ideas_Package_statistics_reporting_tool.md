<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Package_statistics_reporting_tool | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2012/Ideas/Package statistics reporting tool -->
---
title: Google Summer of Code/2012/Ideas/Package statistics reporting tool
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2012/Ideas/Package_statistics_reporting_tool
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-04-02"
fingerprint: a7e92e28a935431a
license: CC BY-SA 4.0
---

# Google Summer of Code/2012/Ideas/Package statistics reporting tool

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A user end program to upload anonymous information about installed packages on a users machine to a database that package maintainers and developers have access to. Last year's effort is called [Gentoostats](https://wiki.gentoo.org/wiki/Gentoostats).

This post from planet Gentoo titled, 'Gentoo: A critical look at the QA process' suggests that it is difficult for maintainers to decide whether or not to mark a package as stable. The main issue being: if it's not marked as stable then users wont use it, and it's very difficult for maintainers to test the package properly on their own. This creates a blurring between \~arch and arch. If stable packages are left in \~arch for long periods of time, users will begin to use \~arch as if it were stable more often, which defeats the purpose of \~arch in the first place. If maintainers push packages into arch because it works on their machines but breaks on users due to the high number of different system setups this makes gentoo look unreliable, further blurring \~arch and arch.

Here are some reasons why this project would help Gentoo:

- Now developers can't see when users are happy with a package, only when they are not.
- Finding incompatibilities between specific package versions
- General user interest in specific packages to help trim down unused packages from portage.



| Contacts | Required Skills | 
|---|---|
|  |  |
