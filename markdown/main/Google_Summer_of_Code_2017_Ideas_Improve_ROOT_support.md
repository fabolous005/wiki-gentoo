<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2017/Ideas/Improve_ROOT_support | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2017/Ideas/Improve ROOT support -->
---
title: Google Summer of Code/2017/Ideas/Improve ROOT support
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2017/Ideas/Improve_ROOT_support
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: f46bdb4450e621fc
license: CC BY-SA 4.0
---

# Google Summer of Code/2017/Ideas/Improve ROOT support

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[ROOT](https://root.cern.ch/) is a modular scientific software framework, a standard de-facto analysis tool in HEP, but used in other scientific and industrial areas too. We support ROOT in Gentoo and have a long list of improvements awaiting implementation:

- move ROOT from configure to cmake;
- add new USE flags and dependencies;
- move documentation generation from THtml to doxygen;
- improve PyROOT:
  - dependency cleanup (split ROOT.py and put Pythonize.cxx in its own .so);
  - make the core of PyROOT standalone, so that it as "PyCling" when it's packaged with Cling and libCore/libCling can be used standalone;
  - get clingcwrapper.cxx from PyPy into ROOT as a standalone .so, so that PyROOT can run on PyPy w/o further ado;
- make Cling installable as a separate package.



| Contacts | Required Skills | 
|---|---|
|  |  |
