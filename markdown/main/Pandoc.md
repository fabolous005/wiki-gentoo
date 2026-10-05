<!-- source: https://wiki.gentoo.org/wiki/Pandoc | group: Gentoo Wiki (Main) | wiki-title: Pandoc -->
---
title: pandoc
url: https://wiki.gentoo.org/wiki/Pandoc
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-15"
fingerprint: f6430a5e6a771cc0
license: CC BY-SA 4.0
---

# pandoc

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**pandoc** is a command line tool for document format and markup language conversion written in [Haskell](https://wiki.gentoo.org/wiki/Haskell). Much like a compiler, pandoc parses documents with a recursive grammar, converts the input to an intermediate representation, stores that intermediate representation in an abstract syntax tree (AST), and then walks the AST to reproduce the document in the desired output format. However, pandoc has its own API for scripted document conversion and custom inport/export filters can be written in [Lua](https://wiki.gentoo.org/wiki/Lua).

pandoc supports a vast number of input and output formats but the list of supported formats is not orthogonal, some formats are export only. Additionally, pandoc has its own flavor of markdown that serves as its native document format.

## Installation

### USE flags


| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [embed-data-files](https://packages.gentoo.org/useflags/embed-data-files) | Embed data files in binary for relocatable executable. | 
| [hscolour](https://packages.gentoo.org/useflags/hscolour) | Include coloured haskell sources to generated documentation (dev-haskell/hscolour) | 
| [profile](https://packages.gentoo.org/useflags/profile) | Add support for software performance analysis (will likely vary from ebuild to ebuild) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [trypandoc](https://packages.gentoo.org/useflags/trypandoc) | Build trypandoc cgi executable. | 

### Emerge

For the **amd64** and **arm64** architectures the binary package [app-text/pandoc-bin](https://packages.gentoo.org/packages/app-text/pandoc-bin) is available. To install this precompiled version, replace pandoc with pandoc-bin in the following installation command:

`root #``emerge --ask app-text/pandoc`
## Configuration

### Files

- $HOME/.local/share or as specified in `$XDG_DATA_HOME` - Local (per user) configuration file.

## Troubleshooting

### Converting between document types causes loss of some formatting information

To some extent this is expected behavior. Not all document formats are equally robust. Further, the intermediate representation used by pandoc does not preserve every possible formatting option.

### Inability to convert MS Word .doc files

This is expected behavior, modern MS Office .docx files are supported but legacy .doc files are not. There are two possible workarounds:

The most basic option is to use [antiword](https://wiki.gentoo.org/wiki/Antiword) to convert the .doc to plain text.

`user $``antiword legacy_document.doc > legacy_document.txt`
This is a valid for many use cases but a lot of formatting information can be lost this way.

A more robust solution is to leverage a little-known feature of antiword, DocBook XML output support, in order to ensure a well formatted document conversion:

`user $``antiword -x db infile.doc | pandoc -f docbook`
The last option is to import the .doc document into [LibreOffice](https://wiki.gentoo.org/wiki/LibreOffice) and export it as either LibreOffice's native .odt or MS Office's modern .docx format. Once you have the .docx version of the file you can leverage pandoc as normal.

This can even be accomplished from the command line as follows:

`user $``libreoffice --convert-to odt legacy_document.doc`
Your mileage may vary as to whether the antiword DocBook XML or LibreOffice conversion methods results in a better document conversion, but there should be few if any differences between the two methods in the resulting output.

### "File \`lmodern.sty' not found" when converting from markdown to pdf

By default, Pandoc will attempt to create the PDF using [LaTeX](https://wiki.gentoo.org/wiki/LaTeX), which requires the lm LaTeX package provided by [dev-texlive/texlive-fontsrecommended](https://packages.gentoo.org/packages/dev-texlive/texlive-fontsrecommended) to be installed:

`root #``emerge --ask dev-texlive/texlive-fontsrecommended`
### "File \`xcolor.sty' not found" when converting from markdown to pdf

Similarly, Pandoc also requires the LaTeX package xcolor for PDF creation, which is supplied by [dev-texlive/texlive-latexrecommended](https://packages.gentoo.org/packages/dev-texlive/texlive-latexrecommended):

`root #``emerge --ask dev-texlive/texlive-latexrecommended`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-text/pandoc`
## See also

- [antiword](https://wiki.gentoo.org/wiki/Antiword) — a program for displaying legacy Microsoft Word .doc documents in common use from MS Word 97 – 2007 as plain text.
- [unrtf](https://wiki.gentoo.org/wiki/Unrtf) — a program for displaying legacy Rich Text Format .rtf documents as HTML or plain text.
- [app-text/lowdown](https://packages.gentoo.org/packages/app-text/lowdown) a program converting from markdown to html, latex, man, odt and other
