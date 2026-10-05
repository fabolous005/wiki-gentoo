<!-- source: https://wiki.gentoo.org/wiki/ImageMagick | group: Gentoo Wiki (Main) | wiki-title: ImageMagick -->
---
title: ImageMagick
url: https://wiki.gentoo.org/wiki/ImageMagick
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-20"
fingerprint: "56024956c486388c"
license: CC BY-SA 4.0
---

# ImageMagick

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**ImageMagick** is a collection of tools and libraries for many image formats (over 200). Supported formats include PNG, JPEG, GIF, WebP, HEIC, SVG, PDF, DPX, EXR and TIFF.[\[1\]](https://wiki.gentoo.org#cite_note-1)

## Installation

### USE flags


| [+cxx](https://packages.gentoo.org/useflags/+cxx) | Build support for C++ (bindings, extra libraries, code generation, ...) | 
| [+png](https://packages.gentoo.org/useflags/+png) | Add support for libpng (PNG images) | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [bzip2](https://packages.gentoo.org/useflags/bzip2) | Enable bzip2 compression support | 
| [corefonts](https://packages.gentoo.org/useflags/corefonts) | Use media-fonts/corefonts which is required by some commands | 
| [djvu](https://packages.gentoo.org/useflags/djvu) | Support DjVu, a PDF-like document format esp. suited for scanned documents | 
| [fftw](https://packages.gentoo.org/useflags/fftw) | Use FFTW library for computing Fourier transforms | 
| [fontconfig](https://packages.gentoo.org/useflags/fontconfig) | Support for configuring and customizing font access via media-libs/fontconfig | 
| [fpx](https://packages.gentoo.org/useflags/fpx) | Enable media-libs/libfpx support | 
| [graphviz](https://packages.gentoo.org/useflags/graphviz) | Add support for the Graphviz library | 
| [hardened](https://packages.gentoo.org/useflags/hardened) | Activate default security enhancements for toolchain (gcc, glibc, binutils) | 
| [hdri](https://packages.gentoo.org/useflags/hdri) | Enable High Dynamic Range Images formats | 
| [heif](https://packages.gentoo.org/useflags/heif) | Enable support for ISO/IEC 23008-12:2017 HEIF/HEIC image format | 
| [jbig](https://packages.gentoo.org/useflags/jbig) | Enable jbig-kit support for tiff, Hylafax, ImageMagick, etc | 
| [jpeg](https://packages.gentoo.org/useflags/jpeg) | Add JPEG image support | 
| [jpeg2k](https://packages.gentoo.org/useflags/jpeg2k) | Support for JPEG 2000, a wavelet-based image compression format | 
| [jpegxl](https://packages.gentoo.org/useflags/jpegxl) | Add JPEG XL image support | 
| [lcms](https://packages.gentoo.org/useflags/lcms) | Add lcms support (color management engine) | 
| [lqr](https://packages.gentoo.org/useflags/lqr) | Enable experimental liquid rescale support using media-libs/liblqr | 
| [lzma](https://packages.gentoo.org/useflags/lzma) | Support for LZMA compression algorithm | 
| [opencl](https://packages.gentoo.org/useflags/opencl) | Enable OpenCL support (computation on GPU) | 
| [openexr](https://packages.gentoo.org/useflags/openexr) | Support for the OpenEXR graphics file format | 
| [openmp](https://packages.gentoo.org/useflags/openmp) | Build support for the OpenMP (support parallel computing), requires >=sys-devel/gcc-4.2 built with USE="openmp" | 
| [pango](https://packages.gentoo.org/useflags/pango) | Enable Pango support using x11-libs/pango | 
| [perl](https://packages.gentoo.org/useflags/perl) | Add optional support/bindings for the Perl language | 
| [postscript](https://packages.gentoo.org/useflags/postscript) | Enable support for the PostScript language (often with ghostscript-gpl or libspectre) | 
| [q32](https://packages.gentoo.org/useflags/q32) | Set quantum depth value to 32 | 
| [q8](https://packages.gentoo.org/useflags/q8) | Set quantum depth value to 8 | 
| [raw](https://packages.gentoo.org/useflags/raw) | Add support for raw image formats | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [svg](https://packages.gentoo.org/useflags/svg) | Add support for SVG (Scalable Vector Graphics) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [tiff](https://packages.gentoo.org/useflags/tiff) | Add support for the TIFF image format | 
| [truetype](https://packages.gentoo.org/useflags/truetype) | Add support for FreeType and/or FreeType2 fonts | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [webp](https://packages.gentoo.org/useflags/webp) | Add support for the WebP image format | 
| [wmf](https://packages.gentoo.org/useflags/wmf) | Add support for the Windows Metafile vector image format | 
| [xml](https://packages.gentoo.org/useflags/xml) | Add support for XML files | 
| [zip](https://packages.gentoo.org/useflags/zip) | Enable support for ZIP archives | 
| [zlib](https://packages.gentoo.org/useflags/zlib) | Add support for zlib compression | 

### AV1/AVIF support

To support the new AVIF image file format, the `heif` USE flag must be enabled.

### Emerge

`root #``emerge --ask media-gfx/imagemagick`
## Configuration

### Files

- /etc/ImageMagick-7 - Global (system wide) configuration files for Imagemagick are found in this directory.

## Troubleshooting

### convert will not run due to security policy

The convert command does not run in the out-of-the-box configuration on certain file formats including PS, PS2, PS3, EPS, PDF, and XPS files. This is by design due to a security vulnerabilities in Ghostscript[\[2\]](https://wiki.gentoo.org#cite_note-2)<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>, which is a library utilized by ImageMagick to manipulate (merge, modify, etc.) PDF files. Gentoo's [Security project team](https://wiki.gentoo.org/wiki/Project:Security) has maintained restrictions to ImageMagick's security policy to mitigate ongoing exploit risks in associated libraries.[\[4\]](https://wiki.gentoo.org#cite_note-4)

The solution is to comment out the lines affecting security policy for the file format(s) in question. This action is performed in the following file by using HTML/XML style comments (`<!--` and `-->`), or simply by removing the offending line(s) altogether from the policy.xml configuration file altogether.

**`/etc/ImageMagick-7/policy.xml`**

**Fix convert command for PDFs**

```
<policymap>
  <policy domain="coder" rights="none" pattern="PS" />
  <policy domain="coder" rights="none" pattern="PS2" />
  <policy domain="coder" rights="none" pattern="PS3" />
  <policy domain="coder" rights="none" pattern="EPS" />
  <policy domain="coder" rights="none" pattern="PDF" />
  <policy domain="coder" rights="none" pattern="XPS" />
</policymap>
```
After commenting out or removing the relevant lines above, running the convert command will work as expected:

`user $``convert input_file_1.pdf input_file_2.pdf output_file.pdf`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose media-gfx/imagemagick`
## External resources

These tools offer some functionality similar to ImageMagick:

- [media-gfx/graphicsmagick](https://packages.gentoo.org/packages/media-gfx/graphicsmagick) - Collection of tools and libraries for many image formats
- [media-libs/vips](https://packages.gentoo.org/packages/media-libs/vips) - VIPS Image Processing Library
