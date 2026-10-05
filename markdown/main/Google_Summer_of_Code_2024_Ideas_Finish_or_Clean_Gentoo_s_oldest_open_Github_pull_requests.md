<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2024/Ideas/Finish_or_Clean_Gentoo%27s_oldest_open_Github_pull_requests | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2024/Ideas/Finish or Clean Gentoo's oldest open Github pull requests -->
---
title: Google Summer of Code/2024/Ideas/Finish or Clean Gentoo's oldest open Github pull requests
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2024/Ideas/Finish_or_Clean_Gentoo%27s_oldest_open_Github_pull_requests
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-02-08"
fingerprint: "751df3e62a0d4739"
license: CC BY-SA 4.0
---

# Google Summer of Code/2024/Ideas/Finish or Clean Gentoo's oldest open Github pull requests

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The idea is simple: Go through our long-standing open pull requests on [https://github.com/gentoo/gentoo/pulls](https://github.com/gentoo/gentoo/pulls) and figure whether these old contributions are still

1\) relevant,
2) working,
3) mergeable.

Summary:

The GSOC participant should begin by making a judgement call whether the PR they're looking is still relevant: Has someone else merged it in tree, is a linked bug closed other way, is the package removed, is the enhancement required anymore, etc. If the PR turns out to be relevant and appliable, the GSOC participant should then test whether it still works as-is, or does it need (preferably minor) follow-up commits to fulfill the quality requirements.

Longer description:

The GSOC participant should review the contribution: Do they spot any obvious errors, is the code outdated, etc. The GSOC participant should make a judgement call how much work is required to get the contribution merged. The GSOC participant should first and foremost try to communicate with the original author, in hopes that the original author is still around. They should work together in making the contribution mergeable in 2024.

If the contribution is appliable with minor tweaks, but the original author doesn't respond in say \~2-4 weeks, the GSOC participant should work on fixing the reviewed issues by themselves. The GSOC participant needs to know how to grab an existing contribution, retain the authorship, and add their own fix commits on top of the original contribution. They need to be familiar with Gentoo's copyright GLEP 76, git tool and Github in general. The GSOC participant should have a Gentoo bugzilla account and knowledge about general Gentoo development: devmanual, ebuilds, eclasses, QA, current Gentoo maintainers and little bit about how Gentoo's infra operates. The GSOC participant is ideally a long-time active contributor themselves. It helps if they've followed Gentoo-s dev mailing list, and are familiar with the more recent EAPI changes, for spotting old eclass/EAPI functions easily when reviewing code.

The GSOC participant needs an active Gentoo-testing-environment. In other words, they need Gentoo installed. Ideally they'd use Gentoo as their main operating system, but a development environment through containers, VMs and alike should be sufficient. Every contribution they push through needs to be tested - ideally for build-time and runtime. The GSOC participant needs knowledge to debug a) ebuild-related problems, b) upstream-related problems.

While most of this is independent work, the GSOC participant obviously needs good social skills to work together with original authors, and Gentoo developers. Lastly, the GSOC participant needs a Github account, patience and enthusiasm towards contributing to Gentoo!


| Contacts | Required Skills | 
|---|---|
| juippis@gentoo.org | \* bugzilla \* git \* github \* Gentoo GLEPs (66, 76) \* social skills \* Gentoo development (devmanual, EAPI, ebuilds, eclasses, QA) | 
| Expected Project Size | Expected Outcomes | 
| 175+ hours, it's a neverending task. | \* Gentoo's Github pull request count drastically reduced, ignored/forgotten PRs merged | 
| Project Difficulty |  | 
| Will find anything from easy to hard - depends on student's knowledge and confidence. |  |
