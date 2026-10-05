<!-- source: https://wiki.gentoo.org/wiki/GNU_Octave | group: Gentoo Wiki (Main) | wiki-title: GNU Octave -->
---
title: GNU Octave
url: https://wiki.gentoo.org/wiki/GNU_Octave
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-14"
fingerprint: "6e834958d1827bc4"
license: CC BY-SA 4.0
---

# GNU Octave

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**GNU Octave** is a free and open-source computing environment and high-level interactive programming language, that is primarily intended for numerical computations.

## Installation

### USE flags


| [+glpk](https://packages.gentoo.org/useflags/+glpk) | Add support for sci-mathematics/glpk for linear programming | 
| [+qhull](https://packages.gentoo.org/useflags/+qhull) | Add support for media-libs/qhull, to allow \`delaunay', \`convhull', and related functions | 
| [+qrupdate](https://packages.gentoo.org/useflags/+qrupdate) | Add support for sci-libs/qrupdatefor QR and Cholesky update functions | 
| [+sparse](https://packages.gentoo.org/useflags/+sparse) | Add enhanced support for sparse matrix algebra with SuiteSparse | 
| [curl](https://packages.gentoo.org/useflags/curl) | Add support for client-side URL transfer library | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [fftw](https://packages.gentoo.org/useflags/fftw) | Use FFTW library for computing Fourier transforms | 
| [gnuplot](https://packages.gentoo.org/useflags/gnuplot) | Use sci-visualization/gnuplot to render plots if OpenGL is unavailable | 
| [gui](https://packages.gentoo.org/useflags/gui) | Enable support for a graphical user interface | 
| [hdf5](https://packages.gentoo.org/useflags/hdf5) | Add support for the Hierarchical Data Format v5 | 
| [imagemagick](https://packages.gentoo.org/useflags/imagemagick) | Use media-gfx/graphicsmagick to read and write images | 
| [java](https://packages.gentoo.org/useflags/java) | Add support for Java | 
| [json](https://packages.gentoo.org/useflags/json) | Allow using jsonencode and jsondecode commands via dev-libs/rapidjson | 
| [klu](https://packages.gentoo.org/useflags/klu) | Add support for KLU (sci-libs/klu) | 
| [portaudio](https://packages.gentoo.org/useflags/portaudio) | Add support for the crossplatform portaudio audio API | 
| [postscript](https://packages.gentoo.org/useflags/postscript) | Enable support for the PostScript language (often with ghostscript-gpl or libspectre) | 
| [readline](https://packages.gentoo.org/useflags/readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [sndfile](https://packages.gentoo.org/useflags/sndfile) | Add support for libsndfile | 
| [spqr](https://packages.gentoo.org/useflags/spqr) | Add support for SPQR (sci-libs/spqr) | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [sundials](https://packages.gentoo.org/useflags/sundials) | Enable the ode15i and ode15s ODE solvers using sci-libs/sundials | 
| [zlib](https://packages.gentoo.org/useflags/zlib) | Add support for zlib compression | 

### Emerge

`root #``emerge --ask sci-mathematics/octave`
### Octave packages

Octave's functionality (i.e. selection of functions available to the user in octave) is extended via octave-packages<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, usually provided by octave-forge<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>. There are two ways to install octave packages:

- Use Octave's own pkg command to install missing packages (requires the `curl` USE flag)
- Use [app-portage/g-octave](https://packages.gentoo.org/packages/app-portage/g-octave) to generate ebuilds for octave-packages from Octave-Forge and install them via Portage

There is conflicting information about which method to prefer [\[3\]](https://wiki.gentoo.org#cite_note-3)<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>, so no recommendation can be given at this point.

## See also

- [Matlab](https://wiki.gentoo.org/wiki/Matlab) — explains how to install and run MathWorks Matlab on Gentoo.
