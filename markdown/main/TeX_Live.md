<!-- source: https://wiki.gentoo.org/wiki/TeX_Live | group: Gentoo Wiki (Main) | wiki-title: TeX Live -->
---
title: TeX Live
url: https://wiki.gentoo.org/wiki/TeX_Live
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-04"
categories: ['dev-texlive']
fingerprint: "3ea3027a03a67902"
license: CC BY-SA 4.0
---

# TeX Live

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**TeX Live** is a complete [TeX](https://en.wikipedia.org/wiki/TeX) distribution with several programs to create professional documents.

## Installation

### USE flags


| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [cjk](https://packages.gentoo.org/useflags/cjk) | Add support for Multi-byte character languages (Chinese, Japanese, Korean) | 
| [context](https://packages.gentoo.org/useflags/context) | Add support for the ConTeXt format (dev-texlive/texlive-context) | 
| [extra](https://packages.gentoo.org/useflags/extra) | Add support for extra TeXLive packages | 
| [games](https://packages.gentoo.org/useflags/games) | Add typesetting support for games (chess, etc.) (dev-texlive/texlive-games) | 
| [graphics](https://packages.gentoo.org/useflags/graphics) | Add support for several graphics packages (pgf, tikz,...) | 
| [humanities](https://packages.gentoo.org/useflags/humanities) | Add LaTeX support for the humanities (dev-texlive/texlive-humanities) | 
| [luatex](https://packages.gentoo.org/useflags/luatex) | Install TeX Live packages that require luatex | 
| [metapost](https://packages.gentoo.org/useflags/metapost) | Add support for metapost: A tool for creating graphics in scalable PostScript | 
| [music](https://packages.gentoo.org/useflags/music) | Add support for music typesetting (dev-texlive/texlive-music) | 
| [pdfannotextractor](https://packages.gentoo.org/useflags/pdfannotextractor) | Add dev-tex/pdfannotextractor support, for extracting annotations from PDF files | 
| [png](https://packages.gentoo.org/useflags/png) | Add support for libpng (PNG images) | 
| [pstricks](https://packages.gentoo.org/useflags/pstricks) | Add pstricks packages (dev-texlive/texlive-pstricks) | 
| [publishers](https://packages.gentoo.org/useflags/publishers) | Add support for publishers (dev-texlive/texlive-publishers) | 
| [science](https://packages.gentoo.org/useflags/science) | Add typesetting support for natural and computer sciences (dev-texlive/texlive-mathscience) | 
| [tex4ht](https://packages.gentoo.org/useflags/tex4ht) | Add support for dev-tex/tex4ht (for converting (La)TeX to (X)HTML, XML and OO.org) | 
| [texi2html](https://packages.gentoo.org/useflags/texi2html) | Add support for app-text/texi2html which converts texi files to HTML | 
| [truetype](https://packages.gentoo.org/useflags/truetype) | Add support for FreeType and/or FreeType2 fonts | 
| [xetex](https://packages.gentoo.org/useflags/xetex) | Add support for XeTeX macros (dev-texlive/texlive-xetex) | 
| [xml](https://packages.gentoo.org/useflags/xml) | Add support for XML files | 

### Emerge

Install [app-text/texlive](https://packages.gentoo.org/packages/app-text/texlive):

`root #``emerge --ask app-text/texlive`
## Upgrading

When users try to upgrade TeXLive, they might meet a failure due to "hard blocking". Here are a solution to overcome it and the reasons behind it.

### Solution

First, unmerge some packages in [dev-texlive](https://packages.gentoo.org/categories/dev-texlive):

`root #``qlist -I dev-texlive/texlive-lang > texlive-lang-packages.txt``root #``# Or, to use 'eix' instead of qlist, do instead:``root #``eix '-I#' dev-texlive/texlive-lang  > texlive-lang-packages.txt``root #``# Then``root #``emerge --deselect=n --unmerge texlive-latex texlive-basic $(cat texlive-lang-packages.txt)`
Now the upgrade can be done:

`root #``emerge --ask --oneshot --update app-text/texlive $(cat texlive-lang-packages.txt)`
Or simply pull in "world":

`root #````
emerge -uDN1a @world
```
### Why this workaround?

Some dev-texlive/texlive-\* packages ([see below](https://wiki.gentoo.org#hardBlockers)) do "hard blocking". For example, texlive-basic-2016 hard-blocks \<texlive-basic-2016, i.e., older versions of itself. Hard-blocked packages can't be installed simultaneously, even temporarily. But as of 2017, portage can't resolve hard blocks automatically ([bug #250286](https://bugs.gentoo.org/show_bug.cgi?id=250286)), and users must handle them manually. They are hard instead of normal blocks because of some file moves. ([bug #606730](https://bugs.gentoo.org/show_bug.cgi?id=606730#c3))

Thus, to upgrade [dev-texlive/texlive-basic](https://packages.gentoo.org/packages/dev-texlive/texlive-basic), users must first unmerge it manually using the above procedure.

### Some more details

Although some language packs have only 2012 versions (e.g. texlive-langcroatian-2012), even for them the above solution (re-emerging them afterwards) works, too. It's because blocking in an ebuild file is in fact *one-sided*, unlike normal dependency, which is checked at "emerge --depclean": Assume virtual/src-0.ebuild contains DEPEND="!!cat/dest", and cat/dest-0.ebuild has no dependency. Then:

- if you already have cat/dest, then emerge virtual/src will fail. However,
- even if you already have virtual/src, emerge cat/dest is allowed.


!!. As of 2017, the only ebuilds using the !! operator (in our context) are [dev-texlive/texlive-basic](https://packages.gentoo.org/packages/dev-texlive/texlive-basic), [dev-texlive/texlive-latex](https://packages.gentoo.org/packages/dev-texlive/texlive-latex), and [dev-texlive/texlive-langcjk](https://packages.gentoo.org/packages/dev-texlive/texlive-langcjk). Packages blocked by them are these three themselves plus texlive-lang\*.

## Usage

Most of the time users will implement TeX Live through the editor of choice. The following is a list of compilers capable of translating a .tex document into .pdf file:

LaTeX:

`user $``pdflatex mydocument.tex`
XeTeX:

`user $``xelatex mydocument.tex`
LuaTeX:

`user $``luatex mydocument.tex`
### Editors

There are several editors users can choose from in Gentoo. To list a few pure TeX editors:

| Editor | Package | Homepage | Description | 
|---|---|---|---|
| kile | [app-editors/kile](https://packages.gentoo.org/packages/app-editors/kile) | [https://kile.sourceforge.io/](https://kile.sourceforge.io/) | A [KDE](https://wiki.gentoo.org/wiki/KDE) based editor, which lets users write in LaTeX code and then compile the document. | 
| texmaker | [app-office/texmaker](https://packages.gentoo.org/packages/app-office/texmaker) | [http://www.xm1math.net/texmaker/](http://www.xm1math.net/texmaker/) | A [Qt](https://wiki.gentoo.org/wiki/Qt) based editor, which lets user write in LaTeX code and then compile the document | 
| texstudio | [app-office/texstudio](https://packages.gentoo.org/packages/app-office/texstudio) | [http://texstudio.sourceforge.net/](http://texstudio.sourceforge.net/) | Free cross-platform LaTeX editor (former texmakerX) | 
| gummi | [app-editors/gummi](https://packages.gentoo.org/packages/app-editors/gummi) | [https://github.com/alexandervdm/gummi](https://github.com/alexandervdm/gummi) | GTK based editor, which lets users write in LaTeX code and then compile the document | 
| GNOME LaTeX | [app-editors/gnome-latex](https://packages.gentoo.org/packages/app-editors/gnome-latex) | [https://wiki.gnome.org/Apps/GNOME-LaTeX](https://wiki.gnome.org/Apps/GNOME-LaTeX) | [GNOME](https://wiki.gentoo.org/wiki/GNOME) based editor, which lets users write in LaTeX code and then compile the document | 
| lyx | [app-office/lyx](https://packages.gentoo.org/packages/app-office/lyx) | [https://www.lyx.org/](https://www.lyx.org/) | Qt based [WYSIWYM](https://en.wikipedia.org/wiki/WYSIWYM) editor, users type directly into the LaTeX document and can edit LaTeX code afterwards | 
| texworks | [app-editors/texworks](https://packages.gentoo.org/packages/app-editors/texworks) | [https://www.tug.org/texworks/](https://www.tug.org/texworks/) | Qt based editor, suggested editor by the UK-TUG which lets users write in LaTeX code and then compile the document | 

Most editors support LaTeX syntax highlighting like [Vim](https://wiki.gentoo.org/wiki/Vim), [emacs](https://wiki.gentoo.org/wiki/Emacs) (which supports compilation and previewing), [gedit](https://wiki.gentoo.org/wiki/Gedit) or [gleany](https://wiki.gentoo.org/index.php?title=Gleany&action=edit&redlink=1) and most development IDE's provide some plugins like [dev-util/netbeans](https://packages.gentoo.org/packages/dev-util/netbeans).

### Programs with LaTeX support

- [net-im/pidgin](https://packages.gentoo.org/packages/net-im/pidgin) - has a LaTeX plugin to display mathematical formulae.

### Find the Gentoo package that contains a tex package

[app-portage/pfl](https://packages.gentoo.org/packages/app-portage/pfl) contains a program to search in an online database for a Gentoo package containing a particular file.

`root #``emerge --ask app-portage/pfl``user $``pfl``user $``e-file yhmath.sty`
\*  dev-texlive/texlive-mathextra
	Available Versions:	2015 2014 
	Description:		TeXLive Mathematics packages
	Matched Files:		/usr/share/texmf-dist/tex/latex/yhmath/yhmath.sty;
\[I\] dev-texlive/texlive-mathscience
	Available Versions:	2017 2016 
	Last Installed Ver:	2016 (Thu Jul 20 17:41:05 2017)
	Description:		TeXLive Mathematics and science packages
	Matched Files:		/usr/share/texmf-dist/tex/latex/yhmath/yhmath.sty;

### ConTeXT

After setting the `context` USE flag for [app-text/texlive](https://packages.gentoo.org/packages/app-text/texlive), generate the cache with

`user $``mtxrun --generate`
To compile documents, set the TEXMF variable, otherwise context will fail with "unknown script context.lua" (see [this bug report](https://bugs.gentoo.org/710154)).

`user $``export TEXMF=/usr/share/texmf-dist`
Then compile the source file with

`user $``context main.tex`
### texdoc

Texdoc is included in the [dev-texlive/texlive-binextra](https://packages.gentoo.org/packages/dev-texlive/texlive-binextra) package. It allowes the user to search the original TeX documentation by keyword. For example, to view all documentation about *article*:

`user $``texdoc article`
This should open the documentation with the default PDF editor.

To specify the PDF viewer, edit the config file at /usr/share/texmf-dist/texdoc/texdoc.cnf.

Note that this will run the specified command with a potentially compressed PDF. A good solution is to create a wrapper script and use [app-text/pdftk](https://packages.gentoo.org/packages/app-text/pdftk) to decompress the pdf:

**`pdf_wrapper.sh`**

```
#set your pdf viewer
PDF_VIEWER="your command"
# extract the filename and omit the extension
FILE_NAME="$(basename "$1")"
FILE_NAME_NOEXT="${FILE_NAME%.*}"
if $(file $1 | grep -q encoded); then
	pdftk "$1" output /tmp/"$FILE_NAME_NOEXT"_unzip.pdf uncompress
	${PDF_VIEWER} /tmp/"$FILE_NAME_NOEXT"_unzip.pdf "${@:2}"
	rm /tmp/"$FILE_NAME_NOEXT"_unzip.pdf
else
	${PDF_VIEWER} "$1" "${@:2}"
fi
```
In order to make the texdoc command as useful as possible, the `doc` USE flag for TeX Live and any other module for which users would like documentation available can be enabled.

To enable the `doc` USE flag for all TeX Live packages, use a wildcard:

`root #``echo "dev-texlive/* doc" >> /etc/portage/package.use`
A full list of installed TeX modules can be shown with the following command:

`root #``qlist -C -I texlive`
To enable the `doc` USE flag only for certain modules, add them to /etc/portage/package.use. A quick way of adding them all is by running this command:

`root #``qlist -C -I texlive | sed 's/$/ doc/' >> /etc/portage/package.use`
## Troubleshooting

### Increase main memory

The error message "TeX capacity exceeded, sorry main memory size = xxxxxxxxx" can be prevented by

1\. Change the value of main\_memory in file /etc/texmf/texmf.d/20sizes.cnf. This should persist through all future texliveupgrades.

2\. Regenerate the system-wide texmf.cnf

`root #``texmf-update`
3\. Regenerate `.fmt` files

`root #``fmtutil-sys --all`
