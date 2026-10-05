<!-- source: https://wiki.gentoo.org/wiki/Scribus | group: Gentoo Wiki (Main) | wiki-title: Scribus -->
---
title: Scribus
url: https://wiki.gentoo.org/wiki/Scribus
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-12-27"
categories: ['Blog']
fingerprint: "9092a3591cce2ce4"
license: CC BY-SA 4.0
---

# Scribus

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Scribus** is a full-featured [Qt](https://wiki.gentoo.org/wiki/Qt)-based desktop publishing package available for Linux, Windows, MacOS, and other operating systems. Scribus uses its own XML-based file format .sla to to store documents. It is Unicode native and supports layouts with complex scripts such as kanji and right-to-left scripts such as Arabic and Hebrew. In Scribus the document is just a canvas with various frames. Text must be placed inside of a text frame. Images tables have their own frame types and, much like commonly used graphics programs such as [GIMP](https://wiki.gentoo.org/wiki/GIMP), frames can be layered to achive a specific effect.

Scribus can import text from a variety of common open formats with various limitations. OpenDocument Text .odt documents have the highest degree of support, Scribus' import filter can import document text along with paragraph style information. Additionally, HTML and PUB documents have a high degree of support as well. Microsoft's Office Open XML .docx can be read by Scribus with reasonable fidelity. Indirect support for legacy Microsoft Word .doc files is possible indirectly via [antiword](https://wiki.gentoo.org/wiki/Antiword). Lowest common denominator file formats such as .rtf, MarkDown, and plain text will work but by their nature contain no useful paragraph formatting information.

## Support for Desktop Publishing File Formats

Generally, Scribus cannot read or write the closed source undocumented file formats of commercial desktop publishing programs. The rare exception being Scribus support for the Microsoft Publisher 2000 documents by means of the [libmspub library](https://wiki.documentfoundation.org/DLP/Libraries/libmspub) produced by the OpenDocument foundation. This library has partial import capability for Publisher 95–98 documents but functionality remains spotty.

Lightweight desktop publishing interchange formats such as QuarkXPress Tag, InDesign's IDML, and InCopy's ICML can all be used with Scribus as they're publicly documented.

## Installation

### USE flags


| [+boost](https://packages.gentoo.org/useflags/+boost) | Enable support for Boost based enhancement | 
| [+jpegxl](https://packages.gentoo.org/useflags/+jpegxl) | Add JPEG XL image support | 
| [+minimal](https://packages.gentoo.org/useflags/+minimal) | Don't install headers (only required for e.g. plug-in developers) | 
| [+pdf](https://packages.gentoo.org/useflags/+pdf) | Add general support for PDF (Portable Document Format), this replaces the pdflib and cpdflib flags | 
| [+templates](https://packages.gentoo.org/useflags/+templates) | Document templates | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [examples](https://packages.gentoo.org/useflags/examples) | Install examples, usually source code | 
| [graphicsmagick](https://packages.gentoo.org/useflags/graphicsmagick) | Build and link against GraphicsMagick instead of ImageMagick (requires USE=imagemagick if optional) | 
| [osg](https://packages.gentoo.org/useflags/osg) | 3D rendering via dev-games/openscenegraph | 
| [scripts](https://packages.gentoo.org/useflags/scripts) | Install the scripts | 
| [tk](https://packages.gentoo.org/useflags/tk) | Install tk based scripts e.g. FontSample.py | 

### Emerge

`root #``emerge --ask app-office/scribus`
### Additional software

Legacy Microsoft Word .doc document file support via [antiword](https://wiki.gentoo.org/wiki/Antiword).

### Invocation

`user $``scribus --help````
Scribus, Open Source Desktop Publishing
---------------------------------------
Homepage:       http://www.scribus.net
Documentation:  http://docs.scribus.net
Wiki:           http://wiki.scribus.net
Issues:         http://bugs.scribus.net
Usage: scribus [options] [files]
Options:
     -fi,  --font-info                      Show information on the console when fonts are being loaded
     -h,   --help                           Print help (this message) and exit
     -l,   --lang                           Uses xx as shortcut for a language, eg `en' or `de'
     -la,  --langs-available                List the currently installed interface languages
     -ns,  --no-splash                      Do not show the splashscreen on startup
     -nns, --never-splash                   Stop showing the splashscreen on startup. Writes an empty file called .neversplash in ~/.config/scribus
     -pr,  --prefs <path>                   Use path for user given preferences location
     -pi,  --profile-info                   Show location of ICC profile information on console while starting
     -u,   --upgradecheck                   Download a file from the Scribus website and show the latest available version
     -v,   --version                        Output version information and exit
     -py,  --python-script <script> [arguments ...]  Run script in Python [with optional arguments]. This option must be last option used
     -g,   --no-gui                         Do not start GUI
     --                                     Explicit end of command line options
```
## Troubleshooting

### Scribus can import .doc files on Windows but not on Linux

Scribus uses [antiword](https://wiki.gentoo.org/wiki/Antiword) as an import filter for legacy Microsoft Office .doc files. Upstream builds of Scribus for Windows bundle *antiword* with with Scribus. Upstream builds for Linux do not contain *antiword* by default but will use it if it happens to be present on the system. Gentoo mimics this behavior. This issue can be addressed by installing antiword:

`root #``emerge --ask app-text/antiword`
Once Scribus is restarted the .doc will become a valid import option. Alternatively, if [LibreOffice](https://wiki.gentoo.org/wiki/LibreOffice) is installed the document can be converted to OpenDocument Text format directly:

`user $``libreoffice --headless --convert-to odt *.doc`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-office/scribus`
## See also

- [TeX Live](https://wiki.gentoo.org/wiki/TeX_Live) — a complete [TeX](https://en.wikipedia.org/wiki/TeX) distribution with several programs to create professional documents.
- [AsciiDoc](https://wiki.gentoo.org/index.php?title=AsciiDoc&action=edit&redlink=1)
- [LibreOffice](https://wiki.gentoo.org/wiki/LibreOffice) — a full office productivity suite.
- [pandoc](https://wiki.gentoo.org/wiki/Pandoc) — a command line tool for document format and markup language conversion written in [Haskell](https://wiki.gentoo.org/wiki/Haskell)
- [antiword](https://wiki.gentoo.org/wiki/Antiword) — a program for displaying legacy Microsoft Word .doc documents in common use from MS Word 97 – 2007 as plain text.
