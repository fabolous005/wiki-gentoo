<!-- source: https://wiki.gentoo.org/wiki/Calibre | group: Gentoo Wiki (Main) | wiki-title: Calibre -->
---
title: Calibre
url: https://wiki.gentoo.org/wiki/Calibre
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-16"
fingerprint: "5a111b1c4d86b9cc"
license: CC BY-SA 4.0
---

# Calibre

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Calibre** is an electronic book management tool.

## Installation

### USE flags


### USE flags for
            [app-text/calibre](https://packages.gentoo.org/packages/app-text/calibre)
            
            Ebook management application

| [+font-subsetting](https://packages.gentoo.org/useflags/+font-subsetting) | Enable font subsetting support | 
| [+system-mathjax](https://packages.gentoo.org/useflags/+system-mathjax) | Use a system copy of mathjax | 
| [+udisks](https://packages.gentoo.org/useflags/+udisks) | Enable storage management support (automounting, volume monitoring, etc) | 
| [ios](https://packages.gentoo.org/useflags/ios) | Enable support for Apple's iDevice with iOS operating system (iPad, iPhone, iPod, etc) | 
| [speech](https://packages.gentoo.org/useflags/speech) | Enable text-to-speech support | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [unrar](https://packages.gentoo.org/useflags/unrar) | Enable support for comic books compressed with the non-free Rar format | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

`root #``emerge --ask app-text/calibre`
## Troubleshooting

### Unable to obtain/generate cover art

Calibre requires [dev-qt/qtgui](https://packages.gentoo.org/packages/dev-qt/qtgui) to have jpeg support for cover art.  See [bug #676664](https://bugs.gentoo.org/show_bug.cgi?id=676664)

FILE **`/etc/portage/package.use/calibre`**

```
dev-qt/qtgui jpeg
```
`root #``emerge --ask --oneshot dev-qt/qtgui`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose app-text/calibre`
## See also

- [Gentoo Wiki:Suggestions/Archive](https://wiki.gentoo.org/wiki/Gentoo_Wiki:Suggestions/Archive#Print_to_PDF_and.2For_Export_to_EBook_File_Format.3F)
- [QT Desktop Applications](https://wiki.gentoo.org/wiki/Qt_Desktop_applications) — a list of recommendations for a light-weight, non-[KDE](https://wiki.gentoo.org/wiki/KDE), [Qt](https://wiki.gentoo.org/wiki/Qt)-only desktop environment.
- [Recommended Applications](https://wiki.gentoo.org/wiki/Recommended_applications) — applications recommended for use in a graphical environment ([X11](https://wiki.gentoo.org/wiki/Xorg), [Wayland](https://wiki.gentoo.org/wiki/Wayland))
