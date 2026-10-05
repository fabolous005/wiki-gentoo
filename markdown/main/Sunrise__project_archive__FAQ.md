<!-- source: https://wiki.gentoo.org/wiki/Sunrise_(project_archive)/FAQ | group: Gentoo Wiki (Main) | wiki-title: Sunrise (project archive)/FAQ -->
---
title: Sunrise (project archive)/FAQ
url: https://wiki.gentoo.org/wiki/Sunrise_(project_archive)/FAQ
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-09-20"
categories: ['http://overlays.gentoo.org/svn/proj/sunrise/reviewed/category/package']
fingerprint: "7f395c524d3477ff"
license: CC BY-SA 4.0
---

# Sunrise (project archive)/FAQ

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**


This FAQ page should answer any questions you have about the Sunrise Project.

We are trying to answer as much as possible from the many comments we got on the mailing lists. If you feel something is missing or wrong, please tell us and we will add or change it.

## User FAQ

### How am I supposed to report a bug for this overlay?

You are not supposed to report one, you should fix it yourself. It is not allowed and not useful to file new bugs about issues with ebuilds in the Sunrise Overlay on bugs.gentoo.org. If you really want to report something you can find out the author with git log and chat with him directly. If you cannot find the author or he does not respond and you cannot fix the bug yourself, please contact the Sunrise Team (see next question).

For usage questions you should use the "Unsupported Software Forum" on [forums.gentoo.org](http://forums.gentoo.org).

### How do I contact the Sunrise Team?

IRC is preferred, you can find us on irc.gentoo.org (freenode) in channel [#gentoo-sunrise](irc://irc.gentoo.org/#gentoo-sunrise).

IRC quickstart: emerge -va irssi; irssi -c irc.gentoo.org -n yournick. After it has connected you type "/j #gentoo-sunrise"

In case you need some admin-stuff done, just ask one of the ops in the channel. You can also contact us with nick@… (see [Sunrise Page](https://wiki.gentoo.org/wiki/Sunrise)) if we are not available on IRC. If you don't know who is the right contact person, you can also send us a mail at sunrise@….

### How can I get a patch of my changes?

Just chdir into the directory and do a

`user $``app-misc/myapp $ git diff . > ~/my_diff.patch`
Then you can submit that file, it incorporates all the changes you made after checkout

More information is available in the [git manual](http://git-scm.com/doc/).

### What can I read to learn more about ebuilds?

ebuilds are explained in great detail in the [Gentoo Development Guide](http://devmanual.gentoo.org/) as well as the [Gentoo Developer Handbook](http://www.gentoo.org/proj/en/devrel/handbook/handbook.xml) and additionally [Gentoo Package Manager Specification](http://www.gentoo.org/proj/en/qa/pms.xml) which is also available as [app-doc/pms](https://packages.gentoo.org/packages/app-doc/pms).

## Commit FAQ

### Can I have access to the overlay?

Yes, you need to contact us and we'll check your submission. You need to have one ebuild reviewed by at least one gentoo dev. Your initial access is bundled with reviewing and committing your first ebuild. When we see that your ebuild style is improving you can take the ebuild quiz and we can allow you to commit on your own, you are a trusted committer then.

### Can I commit everything I like to the overlay?

Every ebuild needs a bug on  [http://bugs.gentoo.org](http://bugs.gentoo.org) that needs to be mentioned in the initial commit. And the overlay addition needs to be mentioned in the bug. If there exists no bug, you need to open one.

Two types of bugs are allowed:

- bugs assigned to maintainer-wanted@…: those are new ebuilds, that are not yet in the portage tree. They can be easily added to the overlay, the bug needs to be updated and the herds in CC have the power to get it removed from the overlay if they want to take care of adding it to the tree themselves. Herds could also have a better official overlay for herd related packages. For example you should not add packages from the PHP overlay or concerning PHP to the Sunrise overlay, rather ask for access to the PHP or Webapps overlay and talk to those herds first, depending on where you feel your package should go.

- bugs assigned to maintainer-needed@…: those are ebuilds in the portage tree, which lack a maintainer. You can commit changes for those to portage-review/ and ask a developer to review and commit to the tree.

For other assigners or version bump bugs that have been long ignored you need the agreement of the assigner. You can also contact us and we can see what we can do.

Of course all ebuilds need to be \~keyworded, no stable keywords are allowed, you need to follow repoman, gentoo QA standards and discuss your first few ebuilds (see previous question).

### How to commit to the overlay?

You can find a step-by-step HOWTO at [How to commit](https://wiki.gentoo.org/wiki/Sunrise/How_to_commit).

### How can I become a Gentoo developer now?

Usually you don't become a gentoo developer instantly - you first have to prove that you are capable of it. We see the way as follows:

- you start by helping out with the Sunrise overlay, let your ebuilds reviewed and listen to comments
- after some time when you are confident with your ebuild skills you can take the [ebuild quiz](http://www.gentoo.org/proj/en/devrel/quiz/ebuild-quiz.txt)  
- when the Sunrise leads have reviewed the answers and found them to be ok we can make you a trusted committer - that means you can now commit without prior review
- now you need to find a mentor to open a new developer bug for you - as you already have the ebuild quiz this should not be that hard
- the mentor will give you additional instruction and expose you to the [end quiz](http://www.gentoo.org/proj/en/devrel/quiz/end-quiz.txt)
- you will have to send both quizzes to the recruiters then and they will decide if they are happy with your answers

Find additional information on the [recruiters page](http://www.gentoo.org/proj/en/devrel/recruiters/index.xml).

## Concerns by other Developers FAQ

### Is this not a ricer overlay? Should the name not be sunrice?

No, this is not a ricer overlay at all, because we are just holding ebuilds, that are not in the tree. Ricing is done by using experimental core packages to build your software which cannot be in the overlay, because they are already in the tree.

### But there is access control? Why is there access control?

We just want to avoid to have random people commiting random stuff to it. You can stop by in #gentoo-sunrise and ask for a password. Also we want to make sure that all ebuilds committed to the overlay are discussed prior to commit.

### But Bugzilla is actually easier!

We do think that Sunrise is easier. Sunrise:

Bugzilla:

`user $````
wget http://bugs.gentoo.org/attachment.cgi?id=24199 -O haha.ebuild
```
`user $````
mkdir files
```
`user $````
wget http://bugs.gentoo.org/attachment.cgi?id=24199 -O files/mysuperpatch.diff
```
`user $````
ebuild haha.ebuild digest
```
and that is just one ebuild with one patch.

But in contrast to that it requires more knowledge and tools to get something into sunrise - more work for contributors. Also contributors have to get their ebuilds reviewed before committing - bugzilla is easier here.

Also note that the Bugzilla way is a bit easier using [Pybugz](https://wiki.gentoo.org/wiki/Pybugz).

### Do I have to use the Sunrise now?

No, you are by no means forced to use the Sunrise overlay. It is designed to be free and open and not forced. We will not force any developer into checking out or using the overlay and we will not force any user into using it.

### Why should this be on official Gentoo hardware?

We are aiming to have a unique place for users and developers to look for contributed ebuilds. For example you can find ebuild-exchange and BreakMyGentoo? and many other small overlays containing only a few packages. It is much more convenient to have this in one single place. This needs to be on \*.gentoo.org because it is a good way to establish this as unique and outstanding among the many projects and that this project gets the community support it needs in order to be successful in creating a unique place to look for contributed ebuilds that do not belong to herd overlays.

Another reason is that we do not have access to all these overlays sprawled over the web. For a gentoo-hosted overlay we can easily fix security problems and make sure that people who want to contribute actually get access. I have personally tried to contact the maintainer of a widely used overlay when I added the packages to the tree and had some conflicts with his, he never answered. It gives gentoo a better reputation if we have control even over packages that are not maintained, but used anyway.

Finally you can also find forums.gentoo.org and bugs.gentoo.org on official gentoo hardware, where users can create content themselves - Sunrise is just a complement of those for posting ebuilds.

### Why was this announced prior to discussing it?

It is current policy to create a project page and announce a new project. When we announced it we had only a very rough outline of what we are doing. Now with the experience from running it a few days we have added a lot of FAQ, have changed a few bits on the project page, have covered many of the concerns by our fellow developers. These experienced changes would not have been possible without testing it out and seeing how it behaves with users committing to it. It is hard to talk discuss something that does not exist.

### Do I need to support Sunrise users and their questions now?

You can treat this as you like on a case-by-case basis:

- Ebuild development questions should for example be discussed in [#gentoo-dev-help](irc://chat.freenode.net/#gentoo-dev-help) and I have seen threads about it on forums.gentoo.org and even helped there. There is no reason why questions about ebuild writing for the Sunrise overlay should not be treated equally.
- For Questions that do not show any development interest like "The overlay-app crashes on first-run, please help0r" you can just say "Fix it yourself or complain upstream". Alternatively you are of course free to help him figure out the problem and fix it in the overlay :)
- For usage questions the "Unsupported Software forum" on forums.gentoo.org is the right place

### How are you ensuring that there is no b0rken/malicious code getting into the overlay?

We have set up rules to forbid adding code to eclass/. We also make sure that only ebuilds get committed to ebuild directories. Only ebuilds for software not in portage are allowed, it is not possible that Sunrise upgrades your gcc or other core system packages.

All commits are publically viewable on the timeline page. Everyone can help out with reviewing by checking the page and having a look at the linked commits. Also we have an IRC-Bot announcing commits in #gentoo-sunrise. Every commit is checked by more than one pair of eyes.

As an additional safety measure we have implemented developer review with two branches:

- there is the general sunrise/ dir where every ebuild should go first. Every contributor has commit-access there.

- the second branch is reviewed/ which is only available to developers. We have a simple script that allows developers to mark a certain revision as reviewed. reviewed/ is what is checked out by layman.

### brix suggestion instead of sunrise: proxy-maintainership?

While we think this is a good idea and promised to help, we do not think it can be a replacement for Sunrise, rather an additional resource. The reason is that proxy-maintaining needs a lot more time - you get bugs in bugzilla, the user asks you to update it, you need to commit yourself .. and it is much less rewarding than working on a package in IRC and have it improved by various people until the author can commit it himself when it is perfect.

### What has been said on the Gentoo Council meeting?

Full conversation log can be [found here](https://projects.gentoo.org/council/meeting-logs/20060615.txt).
