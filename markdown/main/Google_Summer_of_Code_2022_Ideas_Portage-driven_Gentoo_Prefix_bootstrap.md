<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2022/Ideas/Portage-driven_Gentoo_Prefix_bootstrap | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2022/Ideas/Portage-driven Gentoo Prefix bootstrap -->
---
title: Google Summer of Code/2022/Ideas/Portage-driven Gentoo Prefix bootstrap
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2022/Ideas/Portage-driven_Gentoo_Prefix_bootstrap
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-03-31"
fingerprint: "395ad701bb02b3c"
license: CC BY-SA 4.0
---

# Google Summer of Code/2022/Ideas/Portage-driven Gentoo Prefix bootstrap

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Bootstrap of Gentoo Prefix on a new environment is fragile and often need manual intervention. It is because Gentoo Prefix runs on a vast number of hosts, and the ::gentoo repository is fast moving. The reliability of bootstrap is crucial for new users to adopt Prefix and is the foundation for more use scenarios.

Stacked Prefix (also known as prefix-stack) was introduced to manage cygwin/win32 setups. It matured with EAPI 7, when BROOT variable are used to express the directory prefix of BDEPEND. This project envision using prefix-stack to bootstrap Prefix, achieving 3 goals:

- Unify the bootstrap logic of Prefix standalone (aka RAP) and guest (aka prefix-rpath);
- Simplify the logic of Prefix bootstrap;
- Substantially speed up the bootstrap if portage is available on the host.

You will need to test out various toolchain bootstrap setups and find out a universal routine for a diverse number of host environments. You are expected to decipher and debug subtle toolchain induced errors.




| Contacts | Required Skills | 
|---|---|
| [Benda Xu](mailto:heroxbd@gentoo.org) |  | 
| Expected Project Size | Expected Outcomes | 
| 350 hours | A revised Gentoo Prefix bootstrap script that leverages prefix-stack and directly use host portage if available. | 
| Project Difficulty |  | 
| hard |  |
