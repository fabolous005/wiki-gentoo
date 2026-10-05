<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2022/Ideas/Portage_constructed_build_environments_and_circular_dependency_solver | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2022/Ideas/Portage constructed build environments and circular dependency solver -->
---
title: Google Summer of Code/2022/Ideas/Portage constructed build environments and circular dependency solver
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2022/Ideas/Portage_constructed_build_environments_and_circular_dependency_solver
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-02-26"
fingerprint: "6c98f6394d77c521"
license: CC BY-SA 4.0
---

# Google Summer of Code/2022/Ideas/Portage constructed build environments and circular dependency solver

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Improving on the new binpkgs code, we could construct consistent and clean build environments for packages. By generating binpkgs as SquashFS, portage could construct build environments in a mount namespace, with constricted available packages, to detect build system bugs like automatic dependency detection, hard-coded paths, direct calls to gcc/clang, missing (or even extraneous) dependencies. This decoupling of the build and host environment also allows us to break apart build and install steps in the build graph, so that circular dependencies could be solved by building temporary packages with cycle-breaking flags (for instance, `harfbuzz[-truetype]`, `freetype[harfbuzz]`, `harfbuzz[truetype]`, install both).


| Contacts | Required Skills | 
|---|---|
|  |  | 
| Expected Project Size | Expected Outcomes | 
| 175 hours |  | 
| Project Difficulty |  | 
| Hard, mainly because handling Portage codebase |  |
