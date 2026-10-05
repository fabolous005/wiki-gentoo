<!-- source: https://wiki.gentoo.org/wiki/Man_page/Navigate | group: Gentoo Wiki (Main) | wiki-title: Man page/Navigate -->
---
title: Man page/Navigate
url: https://wiki.gentoo.org/wiki/Man_page/Navigate
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-10-03"
fingerprint: "8f485a712087fb08"
license: CC BY-SA 4.0
---

# Man page/Navigate

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide shows how to navigate man pages using the man command.

## Introduction

### The man program

Everyone at some point in their Linux life has used it: the man command. However, while the man program itself appears to be rather simplistic in its construct, it has a few extra abilities than just simply scrolling through the page. This document hopes to help shed some light on these capabilities.

### Man layout

Man pages are mainly stored in the /usr/share/man directory. However, as long as a man page is located in the `MANPATH` environment variable, man will be able to pick it up. Gentoo will generally store `MANPATH` variables in /etc/env.d. Within these directories are some folders with the structure manX where X is the section number. For example, a standard man layout might look like so:

`user $``ls /usr/share/man | grep man`
man0p
man1
man1p
man2
man3
man3p
man4
man5
man6
man7
man8
man9
mann

The actual section numbering appears fairly standard. However, notice that there is a mann and some man#p folders. The following table lists the above man page directories and what is contained within them:

| Man directory | Description | 
|---|---|
| man0p | The " **p**" here stands for POSIX, as with the other directories with p in their names. Man pages in this directory describe the functionality of various POSIX header files. | 
| man1 | This section is for standard commands. Most programs will put their man pages here, so this section tends to be the largest. | 
| man1p | This section describes the POSIX versions of the commands described in 1p. Since it only describes basic commands, it is much smaller than man1. | 
| man2 | This section describes Linux kernel system calls. | 
| man3 | This section describes standard C library functions. | 
| man4 | This section describes special devices. These devices are generally kernel oriented, but Xorg-X11 has entries in here as well. | 
| man5 | This section describes both the makeup of certain files and what files a program uses. Some reading this document may be familiar with references to the man 5 portage command for a description of Portage's file structure or the man 5 make.conf command for make.conf's makeup. | 
| man6 | This section introduces games and other special toys. | 
| man7 | This section describes standards and other miscellaneous items. These standards can include things such as charsets, SQL statements, ISO standards and regular expressions. | 
| man8 | This section describes administrative commands (those usually run by the root user). | 
| man9 | This section is somewhat sparse, but is meant to contain documentation for various parts of the kernel. | 
| mann | This section is mainly used by [dev-lang/tcl](https://packages.gentoo.org/packages/dev-lang/tcl) or [dev-lang/tk](https://packages.gentoo.org/packages/dev-lang/tk). The "**n**" stands for new. | 

While this is not an extensive and detailed list, it does cover the man pages that most people will be interested in. However, sometimes you can find out what a section does as easily as looking at this table. The next chapter will look at using man to traverse this layout.

## Working with the man layout

### Browsing the man layout

Now that we understand the man layout, we can begin to look it over for commands. Sometimes we may need to narrow down what man page we want. The first way would be addressing by section. To find out a description of a section, one can use man \<section> intro like so:

`user $``man 3 intro````
## (Output slightly modified to fit the document properly)
INTRO(3)               Linux Programmer's Manual                INTRO(3)
 
 
 
NAME
       intro - Introduction to library functions
 
DESCRIPTION
       This chapter describes all library functions excluding the library
       functions described in chapter 2, which implement system calls.
       There are various function groups which can be identified by a
       letter which is appended to the chapter number:
....
```
Unfortunately, this does not always work! However, luckily for us there is another way to search for commands that may return multiple results (such as a library call and system command having the same name). To do so, we use the `-K` option to the man command like so:

`user $``man -K sleep`
/usr/share/man/man0p/time.h.0p.gz? \[ynq\] n
/usr/share/man/man0p/unistd.h.0p.gz? \[ynq\] n
/usr/share/man/man2/alarm.2.gz? \[ynq\] n
/usr/share/man/man2/pause.2.gz? \[ynq\] n
/usr/share/man/man2/futex.2.gz? \[ynq\] n
/usr/share/man/man2/nanosleep.2.gz? \[ynq\] y
/usr/share/man/man2/semop.2.gz? \[ynq\] q

Sometimes the output may be a lot larger. In this case it might be better to specify more specific keywords. Now that we know where to find the man page, the next section will look at viewing the man page.

### Viewing man pages

Viewing man pages can be done in 2 ways, first is with `man [man page name]`. The second way is `man <section> [man page name]`. Take [sys-devel/bc](https://packages.gentoo.org/packages/sys-devel/bc) for example. I can view either the first man page that comes up on bc (which would be section 1, because it is the lowest section containing a man page on bc):

`user $``man bc````
bc(1)                                            bc(1)
 
 
NAME
       bc - An arbitrary precision calculator language
...
```
However, what if I want the POSIX version? Then I can use the second form:

`user $``man 1p bc````
BC(P)        POSIX Programmer's Manual           BC(P)
 
 
NAME
       bc - arbitrary-precision arithmetic language
...
```
And the man page is displayed. Now that we have the man page up, it is time to work with it. The next section will look at navigation and searching.

Navigating a man page is fairly simple. To move up and down line by line, use the `↑` and `↓` arrow keys. To move up page by page, you can use the `Page up` and `Page down` keys. Do however note that these navigation instructions assume the environmental `PAGER` variable is set to use the default pager, less. Less also has a few other commands for navigation, but the arrow keys usually suffice:

Searching, however, is more interesting. The two most basic searches are `/pattern` and `?pattern`. The first version searches forwards, and the second searches backwards. `pattern` is a regular expression pattern that is described in man 7 regex. Let's take for example searching for the `-D` option to emerge. First, bring up the emerge man page:

`user $``man emerge`
Then, at the screen, press the `/` key to bring up the entry prompt to search forwards and enter in our search pattern:

This will search the man page, and bring the searched item into focus:

If you hit a search result by accident and want to continue searching for the same results, simply press the `/` key again, and press `Enter` (i.e. don't put a pattern it). This will cause the search to default to the last pattern used. Now with some man pages, options are listed, then explained later on. Take the man 5 portage man page. It lists the files used, then explains their usage. Searching forward a few times would return the results, but there's an easier way to handle this, with the second search form, backwards searching. Let's use this to find the description on package.unmask. First, bring up man 5 portage:

`user $``man 5 portage`
Now press `Shift`+`g`. This will bring you to the end of the page:

Now we'll go ahead and enter the pattern to search for with the `?pattern` backwards search option. First press the `?` key to bring up the prompt, and then enter in package.unmask, our query:

Then hit `Enter` to bring up the result:

And the search is complete! Note that just as with `/`, using `?` search with no pattern will use the last pattern to search.

### Conclusion

This concludes the man guide. This will hopefully shed some light on navigating man pages, and maybe even give a few new tips to the more experienced users. For those who prefer alternate means of navigating man pages, the following are also available:

- [sys-apps/man2html](https://packages.gentoo.org/packages/sys-apps/man2html) - A program for converting man pages to HTML.
- [app-text/tkman](https://packages.gentoo.org/packages/app-text/tkman) - A tk based man page browser.

Also the KDE web browser [kde-apps/konqueror](https://packages.gentoo.org/packages/kde-apps/konqueror) can browse man pages using the `man:` syntax in the address bar.
