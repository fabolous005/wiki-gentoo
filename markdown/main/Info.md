<!-- source: https://wiki.gentoo.org/wiki/Info | group: Gentoo Wiki (Main) | wiki-title: Info -->
---
title: Info
url: https://wiki.gentoo.org/wiki/Info
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-28"
fingerprint: "8c08bb212aa57994"
license: CC BY-SA 4.0
---

# Info

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

- Rework to be less verbose.
- etc.

![](https://wiki.gentoo.org/images/thumb/a/ab/Pinfo.png/300px-Pinfo.png)

The **info** command is used to view and navigate info pages that contain computer program documentation. It is part of the [Texinfo](https://www.gnu.org/software/texinfo/) documentation system.

Most users will be familiar with the **[man](https://wiki.gentoo.org/wiki/Man_page)** documentation system. While man is good for quickly looking up items, it lacks structure in linking man pages together. Info pages can link with other pages, create menus and ease navigation in general. The contents of the man pages is sometimes complimentary to the info system, sometimes they will be different, sometimes only one system will contain anything at all.

Info pages are available even when a system is not connected to the Internet. The files are usually stored in /usr/share/info but are viewed with a dedicated program, such as the info command.

It is a real advantage to have documentation present on a system in a standardized and accessible way. Getting into the habit of looking for answers in the info and man pages is very good practice, they often contain the most complete documentation available.

## Installation


### USE flags


| [+standalone](https://packages.gentoo.org/useflags/+standalone) | Build standalone version that survives all Portage bugs | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [static](https://packages.gentoo.org/useflags/static) | !!do not set this during bootstrap!! Causes binaries to be statically linked instead of dynamically | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

Info may already be present on some systems, in which case this section can be skipped. Type whereis info (belongs to [sys-apps/util-linux](https://packages.gentoo.org/packages/sys-apps/util-linux), which is usually part of the [system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>)) to determine if info is already installed.

### Emerge

Install [sys-apps/texinfo](https://packages.gentoo.org/packages/sys-apps/texinfo) package:

`root #``emerge --ask sys-apps/texinfo`
### Pinfo


pinfo ([app-text/pinfo](https://packages.gentoo.org/packages/app-text/pinfo)) is a colorized alternative to the info viewer, with enhanced browsing facilities. If desired, this could be installed instead of or in parallel to [sys-apps/texinfo](https://packages.gentoo.org/packages/sys-apps/texinfo) (in which case substitute pinfo for info when following the rest of this document):

`root #``emerge --ask sys-apps/pinfo`
See the pinfo [documentation](http://pinfo.sourceforge.net/doc/pinfo.html), [website](http://pinfo.sourceforge.net/), and [github](https://github.com/baszoetekouw/pinfo) for more information.

## Usage

### Invocation

To begin viewing - and navigating through - the info pages, invoke info with no arguments. The user will be presented with an overview of the documentation stored on their system:

`user $``info`
Options:

`user $``info --help````
Usage: info [OPTION]... [MENU-ITEM...]
Read documentation in Info format.
Frequently-used options:
  -a, --all                    use all matching manuals
  -k, --apropos=STRING         look up STRING in all indices of all manuals
  -d, --directory=DIR          add DIR to INFOPATH
  -f, --file=MANUAL            specify Info manual to visit
  -h, --help                   display this help and exit
      --index-search=STRING    go to node pointed by index entry STRING
  -n, --node=NODENAME          specify nodes in first visited Info file
  -o, --output=FILE            output selected nodes to FILE
  -O, --show-options, --usage  go to command-line options node
      --subnodes               recursively output menu items
  -v, --variable VAR=VALUE     assign VALUE to Info variable VAR
      --version                display version information and exit
  -w, --where, --location      print physical location of Info file
The first non-option argument, if present, is the menu entry to start from;
it is searched for in all 'dir' files along INFOPATH.
If it is not present, info merges all 'dir' files and shows the result.
Any remaining arguments are treated as the names of menu
items relative to the initial node visited.
For a summary of key bindings, type H within Info.
Examples:
  info                         show top-level dir menu
  info info-stnd               show the manual for this Info program
  info emacs                   start at emacs node from top-level dir
  info emacs buffers           select buffers menu entry in emacs manual
  info emacs -n Files          start at Files node within emacs manual
  info '(emacs)Files'          alternative way to start at Files node
  info --show-options emacs    start at node with emacs' command line options
  info --subnodes -o out.txt emacs
                               dump entire emacs manual to out.txt
  info -f ./foo.info           show file ./foo.info, not searching dir
Email bug reports to bug-texinfo@gnu.org,
general questions and discussion to help-texinfo@gnu.org.
Texinfo home page: http://www.gnu.org/software/texinfo/
```
### Browsing info pages

Now that info is started, the screen will be similar to this:

Right now there are a bunch of entries with an asterisk before them. These are menu items for navigating through different node levels.

There are two ways of selecting menus, either with arrows or by number. In order to look at the wget info page, navigating with the arrow keys, use the `↓` key until reaching the line for wget:

Once on this line, hit the `Enter` key to select the menu item. This will bring up the info page for wget:

In terms of nodes, this is considered the `Top` node for the wget page. Consider the `Top` node to be the same as the table of contents for that particular info page.

To navigate the page itself, users have a couple of different methods. First off is the standard info method. This is using the `Space` key to move forward a page and the `Backspace`/`Delete` keys to move back a page. This is the recommended method as it automatically advances/retreats to the appropriate node in the document. In order to skip entire nodes without using `Space`/`Backspace`/`Delete`, users can also use the `[` (advance backwards) and `]` (advance forwards) keys.

Another way to navigate is through the `Page up`/`Page down` keys. These work, but they will not advance/retreat like `Space`/`Backspace`/`Delete` will.

As mentioned earlier, there are two ways of selecting menus. The second way will now be described here. The numbers `1-9` can be used to reference to the first-ninth menu entries in a document. This can be used to quickly peruse through documents. For example, users can press `3` to reach the `Recursive Download` menu entry. So press `3` and it will bring up the `Recursive Download` screen:

Here is a good time to note a few things. First off the top header section. This header shows the navigation capable from this particular screen. The page indicated by `Next:` can be accessed by pressing the `n` key, and the page indicated by `Prev:` can be accessed by pressing the `p` key. Please note that this will only work for the same level. If overused users could round up in totally unrelated content. It's better to use `Space`/`Backspace`/`Delete`/`[`/`]` to navigate in a linear fashion.

If for some reason users get lost, there are a few ways to get out. First is the `t` key. This will take the user straight to the toplevel (table of contents) for the particular info page being browsed. If users want to return to the last page looked out, they can do so with the `l` key. If users want to go to the above level, they can do so with the `u` key. The next chapter will look at searching for content.

Now that users can navigate an individual info page, it's important to look at accessing other info pages. The first obvious way is to go to the info page through the dir index listing of info pages. To get to the dir index from deep within a document, simply press the `d` key. From there users can search for the appropriate page they want. However, if they know the actual page, there is an easier way through the `Goto node (` command. To go to an info page by name, type `g` key)`g` to bring up the prompt and enter the name of the page in parentheses:

This will bring up the libc page as shown here:

Now that users know how to go to info pages by name, the next section will look at searching for pieces of information using the info page's index.

### Searching through info

#### Searching using an index

The following example will describe how to lookup the `printf` function of the C library using the libc info page's index. Users should still be at the libc info page from the last section, and if not, they can use the Goto node command to do so. To utilize the index search, hit the `i` key to bring up the prompt, then enter the search term:

After pressing `Enter` upon completion of our query, users are brought to the libc definition for `printf`:

Users have successfully performed a search using the `libc` info page index. However, sometimes what users want is in the page itself. The next section will look at performing searches within the page.

#### Searching using the search command

Starting from the previous location at the `Formatted Output Functions` node, users will look at searching for the `sprintf` variation of the `printf` function. To perform a search, press the `s` key to bring up the search prompt, and then enter the query (sprintf in this case):

Hit `Enter` and it will show the result of the query:

This is the needed function.

## Info pages stored on disk

The main info pages are held in /usr/share/info. Unlike the man style directory layout, /usr/share/info contains what is largely a rather extensive collection of files. These files have the following format:

`pagename` is the actual name of the page (example: `wget`). `[-node]` is an optional construct that designates another node level (generally these are referenced to by the toplevel of the info document in question).

In order to save space these info pages are compressed using the gzip compression scheme by default. Configure the `PORTAGE_COMPRESS` variable in /etc/portage/make.conf to choose different compression algorithms.

Additional info pages can be listed with the `INFOPATH` environment variable (usually set through the various /etc/env.d/ files).

The /usr/share/info/dir file is used when info is run with no parameters. It contains a listing of all info pages available for users to browse.

## Additional tools

In order to make things easier for those that wish to browse info pages through a more friendly graphical interface, the following tools are available:

- [app-text/info2html](https://packages.gentoo.org/packages/app-text/info2html) - Convert info pages to a browse-able HTML format
- [app-text/pinfo](https://packages.gentoo.org/packages/app-text/pinfo) - ncurses based info viewer
- [app-text/tkinfo](https://packages.gentoo.org/packages/app-text/tkinfo) - A tcl/tk based info browser
- [app-vim/info](https://packages.gentoo.org/packages/app-vim/info) - A vim based info browser

The KDE browser Konqueror also allows users to browse info pages through the `info:` URI.

## Additional documentation

- The info command can be used to view its own documentation:

`user $``info info`
- There is also documentation available in the man pages:

`user $``man info`
## See also

- [Man page](https://wiki.gentoo.org/wiki/Man_page) — contains system reference documentation. It is found on most Unix-like systems.
- [tldr pages](https://wiki.gentoo.org/wiki/Tldr_pages) — a project to provide brief documentation for [CLI](https://wiki.gentoo.org/wiki/Shell) commands.
