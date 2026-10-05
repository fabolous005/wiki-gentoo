<!-- source: https://wiki.gentoo.org/wiki/Sunrise_(project_archive)/Coding_standards | group: Gentoo Wiki (Main) | wiki-title: Sunrise (project archive)/Coding standards -->
---
title: Sunrise (project archive)/Coding standards
url: https://wiki.gentoo.org/wiki/Sunrise_(project_archive)/Coding_standards
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-09-20"
fingerprint: "8641f887aaa02370"
license: CC BY-SA 4.0
---

# Sunrise (project archive)/Coding standards

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**

This page exists as a guide for writing new - and hopefully - better ebuilds. It isn't meant to be a a collection of rules, but rather a collection of ideas and best practices.

## Know what to document

If you think something you wrote might not be clear to someone else reading your ebuild document it. Document freely, document "TODOs", document non-standard configure scripts and Makefiles.

## Review and review

Review ebuilds and let others review your code.

## Listen to repoman

The repoman command is your friend. Listen what it says and change your ebuild accordingly.

## Inherit directly

Don't count on other eclasses to inherit from the eclass you're using functions of. This can change and will break your ebuild.

This might or might not work, depending on whether kde.eclass inherits from eutils and how the package manager handles multiple inheritance. But as soon as kde.eclass stops inheriting from eutils.eclass, your ebuild will stop working.

## Don't pollute global scope

Keep the variables as private as possible. Declare them local when used in only one function. Declare variables as late as possible. This makes it easier to see where it has been used.

## Prefer readability

Writing ebuilds isn't about writing cool "oneliners". Therefore: know when to use them and when not. Write for clarity and readability if not sure. It also helps others to review your code.

## Use eclasses correctly

Try to use the eclasses correctly. Read them and ebuilds using them if not sure.

## Leave the build-system no choice

Many build-systems are broken and it's hard to handle them. Nevertheless, try to leave them no choice but follow our environment variables and USE-flags. Write a patch, if necessary. Check if they really depend only on what you tell them to depend on.

## Prefer using provided functions

There are eclasses which provide really usefull functions. For example: eutils, autotools, toolchain-funcs and versionator. Read them and use the functions provided there. Those functions are proven and known to work.

`user $``ls /usr/lib/portage/bin/ebuild-helpers`
gives you some more tools to work with.

## Keep a consistent variable naming scheme

ALLUPPERCASE variables seem to be interpreted as variables belonging to the global scope by many people. Prefer to use lowercase or camelCase variables for local variables. Use the 'local' keyword pro-actively.

## Keep variables ordered

Variables should be kept in default order so that one get things quickly.

## Quote variables where spaces are unwanted

Basic rule: Don't quote in assignments and quote when used with commands

## If necessary, redistribute sources

Sometimes upstream doesn't provide persistent location for sources so it's impossible to put anything in SRC\_URI. In such case (and only in such case) if license permits distribute sources at any public web-site which provides you with such location. Please avoid using private webspace where longevity of the data is not guaranteed.

## Additional guidelines for ebuilds

- If your package installs additional documentation (API docs etc.), make sure their installation is optional via USE="doc"
- If your package installs examples, make sure their installation is optional via USE="examples" and treat them also as documentation
- If you need to patch a makefile in a package that uses autotools, patch the Makefile.am and run eautoreconf (from 'inherit autotools') instead of patching autotools-generated files.
- More will go here as we come up with them

## Some projects host their own guidelines for proper eclass usage or ebuild writing

## Some ebuilds require additional review by corresponding teams

- Java ebuilds require review by Java team members on [#gentoo-java](irc://chat.freenode.net/#gentoo-java) or java@…
- KDE ebuilds require review by KDE team members on [#gentoo-kde](irc://chat.freenode.net/#gentoo-kde) or kde@…
