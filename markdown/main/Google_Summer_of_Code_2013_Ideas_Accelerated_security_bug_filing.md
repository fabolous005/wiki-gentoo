<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas/Accelerated_security_bug_filing | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2013/Ideas/Accelerated security bug filing -->
---
title: Google Summer of Code/2013/Ideas/Accelerated security bug filing
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas/Accelerated_security_bug_filing
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: d44b85e457b357a8
license: CC BY-SA 4.0
---

# Google Summer of Code/2013/Ideas/Accelerated security bug filing

[Google Summer of Code](https://wiki.gentoo.org/wiki/Google_Summer_of_Code) |

[2013](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013) |

[Ideas](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## [Accelerated security bug filing]

Security is one of the main things Gentoo must do well to be a viable distribution. Usually we file security bugs based on Secunia's advisories. We currently have a web-based tool called glsamaker, which files bug only after MITRE has published the vulnerability (CVE), but this tool is such that it can only be accessible to our internal security team. The limitations of glsamaker mean we are not able to file the bug when the advisory comes out; the application cannot file bugs until much later, creating a potential vulnerability window between the Secunia and MITRE CVE dates.

This would be a local client application, not a webapp, so that any Gentoo user could easily run it without Gentoo needing to provide a web service. It would likely use [pybugz](https://github.com/williamh/pybugz) to handle the bug filing but other options are also possible if you don't think that is the best solution.



| Contacts | Required Skills | 
|---|---|
|  |  |
