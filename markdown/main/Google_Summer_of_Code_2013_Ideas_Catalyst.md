<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas/Catalyst | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2013/Ideas/Catalyst -->
---
title: Google Summer of Code/2013/Ideas/Catalyst
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2013/Ideas/Catalyst
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-11-09"
fingerprint: a07e52e0c3839f0
license: CC BY-SA 4.0
---

# Google Summer of Code/2013/Ideas/Catalyst

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Catalyst is Gentoo's stage building software which automates the building of Gentoo's release media.  Unfortunately it's  [software base](http://ln-s.net/+vQU) is in need of some major updating.  A rewrite has been started a couple of times.  The most recent [catalyst rewrite branch](http://dev.gentoo.org/~dolsen/catalyst/) will be the starting point for future work.  Catalyst is written in a combination of python and bash, much like it's package manager portage.  Problems with the original code include, not being installed to python's site-packages, poor code organization (growing pains), hard coded paths making changes difficult, not using what are now built-in python features and modules, etc..  An initial restructure has been done with some code split into smaller logical files/modules.  Also some of the hard coded paths have been made configurable.

#### Completion of code re-write

1. Migrate more code to use python built-in's rather than it's own code. ie: ConfigParser, argsparse,...
2. Make more variables configurable, remove all remaining hard coded paths.
3. Modularize the code further where it makes sense and reduce code duplication.
4. Update or create docstrings for all functions and classes.
5. Add more inline code comments to explain why something is done the way it is.
6. Update portions of the code to match new features in portage, compression utilities, etc.
7. Make catalyst package manager agnostic, allowing it to use any PMS compliant package manager.
8. Add more options for image types produced.
9. Possibly add more target types
10. Separate out the command line code from the main operational api. This allows for the creation of alternate front-ends or embedding catalyst's operation into other tools.
11. debug!, debug!, debug!

#### Documentation

1. Produce user instruction documentation about how to create/use the different target types
2. Instructions for image generation options.
3. Instructions for user configurable variables and how they might affect what is produced.
4. Document anything else that may come up.



| Contacts | Required Skills | 
|---|---|
|  |  |
