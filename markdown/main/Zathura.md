<!-- source: https://wiki.gentoo.org/wiki/Zathura | group: Gentoo Wiki (Main) | wiki-title: Zathura -->
---
title: Zathura
url: https://wiki.gentoo.org/wiki/Zathura
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-12"
fingerprint: ee50093c5dae79e4
license: CC BY-SA 4.0
---

# Zathura

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Zathura** is a free, plugin-based document viewer. Plugins are available for PDF (via poppler or MuPDF), PostScript, DjVu, and EPUB. It was written to be lightweight and controlled with vi-like keybindings.

## Installation

### USE flags


| [+man](https://packages.gentoo.org/useflags/+man) | Build and install man pages | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [landlock](https://packages.gentoo.org/useflags/landlock) | Build the sandboxed version using the Landlock (a Linux Security Module) | 
| [seccomp](https://packages.gentoo.org/useflags/seccomp) | Enable seccomp (secure computing mode) to perform system call filtering at runtime to increase security of programs | 
| [synctex](https://packages.gentoo.org/useflags/synctex) | Use libsynctex to get latex codeline from pdf | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 


| [+pdf](https://packages.gentoo.org/useflags/+pdf) | Add general support for PDF (Portable Document Format), this replaces the pdflib and cpdflib flags | 
| [cb](https://packages.gentoo.org/useflags/cb) | Install plug-in for ComicBook support | 
| [djvu](https://packages.gentoo.org/useflags/djvu) | Support DjVu, a PDF-like document format esp. suited for scanned documents | 
| [epub](https://packages.gentoo.org/useflags/epub) | Install plug-in E-Book support | 
| [postscript](https://packages.gentoo.org/useflags/postscript) | Enable support for the PostScript language (often with ghostscript-gpl or libspectre) | 

### Emerge

Install [app-text/zathura](https://packages.gentoo.org/packages/app-text/zathura) and [app-text/zathura-meta](https://packages.gentoo.org/packages/app-text/zathura-meta):

`root #``emerge --ask app-text/zathura app-text/zathura-meta`
## Usage

If you use xdg-open, you might like to set it to be your default PDF application. First, ensure a desktop entry for zathura exists at /usr/share/applications/org.pwmt.zathura.desktop. Then, set zathura as default using xdg-mime:

`user $``xdg-mime default org.pwmt.zathura.desktop application/pdf`
