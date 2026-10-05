<!-- source: https://wiki.gentoo.org/wiki/Blender | group: Gentoo Wiki (Main) | wiki-title: Blender -->
---
title: Blender
url: https://wiki.gentoo.org/wiki/Blender
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-02"
fingerprint: ee83595ccdb6b8cc
license: CC BY-SA 4.0
---

# Blender

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Blender** is a free and open-source 3D creation suite. It can perform a variety of tasks, including modeling, rigging, animation, simulation, rendering, compositing and motion tracking, video editing, game creation and even 2D animation<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>. Blender's functionality can also be extended using add-ons written in [Python](https://wiki.gentoo.org/wiki/Python). Blender is a community-driven project, but is supported by the Blender Foundation which funds core development<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>.

## Installation

### USE flags

Blender has a lot of optional features that can be enabled for specific hardware or workflows. See [Configuration](https://wiki.gentoo.org/wiki/Blender#Configuration) for more information.


| [+bullet](https://packages.gentoo.org/useflags/+bullet) | Enable Bullet (Physics Engine). | 
| [+color-management](https://packages.gentoo.org/useflags/+color-management) | Enable color management via media-libs/opencolorio. | 
| [+cycles](https://packages.gentoo.org/useflags/+cycles) | Enable the Cycles raytracing render engine. | 
| [+cycles-bin-kernels](https://packages.gentoo.org/useflags/+cycles-bin-kernels) | Precompile the cycles render kernels for the CUDA/HIP/OneAPI backends, if they are enabled, at compile time. This makes it so that the user doesn't have to wait for the kernels to compile when they are used for the first time in Blender. If this option is not on, they will be built as needed at runtime. | 
| [+embree](https://packages.gentoo.org/useflags/+embree) | Use embree to accelerate certain areas of the Cycles render engine. | 
| [+ffmpeg](https://packages.gentoo.org/useflags/+ffmpeg) | Enable ffmpeg/libav-based audio/video codec support | 
| [+fftw](https://packages.gentoo.org/useflags/+fftw) | Use FFTW library for computing Fourier transforms | 
| [+fluid](https://packages.gentoo.org/useflags/+fluid) | Adds fluid simulation support via the built-in Mantaflow library. | 
| [+gmp](https://packages.gentoo.org/useflags/+gmp) | Add support for dev-libs/gmp (GNU MP library) | 
| [+manifold](https://packages.gentoo.org/useflags/+manifold) | Enable Manifold render backend via sci-mathematics/manifold | 
| [+nanovdb](https://packages.gentoo.org/useflags/+nanovdb) | Enable nanoVDB support in Cycles. Uses less memory than regular openVDB when rendering. | 
| [+oidn](https://packages.gentoo.org/useflags/+oidn) | Enable OpenImageDenoiser Support | 
| [+openexr](https://packages.gentoo.org/useflags/+openexr) | Support for the OpenEXR graphics file format | 
| [+opengl](https://packages.gentoo.org/useflags/+opengl) | Add support for OpenGL (3D graphics) | 
| [+openmp](https://packages.gentoo.org/useflags/+openmp) | Build support for the OpenMP (support parallel computing), requires >=sys-devel/gcc-4.2 built with USE="openmp" | 
| [+openpgl](https://packages.gentoo.org/useflags/+openpgl) | Enable path guiding support in Cycles | 
| [+opensubdiv](https://packages.gentoo.org/useflags/+opensubdiv) | Add rendering support form OpenSubdiv from Dreamworks Animation through media-libs/opensubdiv. | 
| [+openvdb](https://packages.gentoo.org/useflags/+openvdb) | Enable openvdb for volumetric processing, like the voxel remesher. Also enables volumetric GPU preview rendering for Nvidia cards. | 
| [+pdf](https://packages.gentoo.org/useflags/+pdf) | Add general support for PDF (Portable Document Format), this replaces the pdflib and cpdflib flags | 
| [+potrace](https://packages.gentoo.org/useflags/+potrace) | Add support for converting bitmaps into Grease pencil line using the potrace library. | 
| [+pugixml](https://packages.gentoo.org/useflags/+pugixml) | Enable PugiXML support (Used for OpenImageIO, Grease Pencil SVG export) | 
| [+rubberband](https://packages.gentoo.org/useflags/+rubberband) | Build with Rubber Band for audio time-stretching and pitch-scaling (used by Audaspace) via media-libs/rubberband | 
| [+sndfile](https://packages.gentoo.org/useflags/+sndfile) | Add support for libsndfile | 
| [+tbb](https://packages.gentoo.org/useflags/+tbb) | Use threading building blocks library from dev-cpp/tbb. | 
| [+tiff](https://packages.gentoo.org/useflags/+tiff) | Add support for the TIFF image format | 
| [+truetype](https://packages.gentoo.org/useflags/+truetype) | Add support for FreeType and/or FreeType2 fonts | 
| [+webp](https://packages.gentoo.org/useflags/+webp) | Add support for the WebP image format | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [alembic](https://packages.gentoo.org/useflags/alembic) | Add support for Alembic through media-gfx/alembic. | 
| [collada](https://packages.gentoo.org/useflags/collada) | Add support for Collada interchange format through media-libs/opencollada. | 
| [cuda](https://packages.gentoo.org/useflags/cuda) | Build cycles renderer with nVidia CUDA support. | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [experimental](https://packages.gentoo.org/useflags/experimental) | Enable experimental features | 
| [gnome](https://packages.gentoo.org/useflags/gnome) | Add GNOME support | 
| [hip](https://packages.gentoo.org/useflags/hip) | Build cycles renderer with AMD HIP support. | 
| [hiprt](https://packages.gentoo.org/useflags/hiprt) | Enable AMD HIP GPU ray tracing acceleration via dev-libs/hiprt. | 
| [jack](https://packages.gentoo.org/useflags/jack) | Add support for the JACK Audio Connection Kit | 
| [jemalloc](https://packages.gentoo.org/useflags/jemalloc) | Use dev-libs/jemalloc for memory management | 
| [jpeg2k](https://packages.gentoo.org/useflags/jpeg2k) | Support for JPEG 2000, a wavelet-based image compression format | 
| [man](https://packages.gentoo.org/useflags/man) | Build and install man pages | 
| [ndof](https://packages.gentoo.org/useflags/ndof) | Enable NDOF input devices (SpaceNavigator and friends). | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [openal](https://packages.gentoo.org/useflags/openal) | Add support for the Open Audio Library | 
| [optix](https://packages.gentoo.org/useflags/optix) | Add support for NVIDIA's OptiX Raytracing Engine. | 
| [osl](https://packages.gentoo.org/useflags/osl) | Add support for OpenShadingLanguage scripting. | 
| [pipewire](https://packages.gentoo.org/useflags/pipewire) | Enable Pipewire for audio support on Linux | 
| [pulseaudio](https://packages.gentoo.org/useflags/pulseaudio) | Add sound server support via media-libs/libpulse (may be PulseAudio or PipeWire) | 
| [renderdoc](https://packages.gentoo.org/useflags/renderdoc) | Build Blender with renderdoc support | 
| [sdl](https://packages.gentoo.org/useflags/sdl) | Add support for Simple Direct Layer (media library) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [valgrind](https://packages.gentoo.org/useflags/valgrind) | Enable annotations for accuracy. May slow down runtime slightly. Safe to use even if not currently using dev-debug/valgrind | 
| [vulkan](https://packages.gentoo.org/useflags/vulkan) | Add support for the Vulkan viewport backend | 
| [wayland](https://packages.gentoo.org/useflags/wayland) | Enable dev-libs/wayland backend | 

### Emerge

`root #``emerge --ask media-gfx/blender`
## Configuration

Since Blender supports so many different hardware configurations, platforms, and use cases there are a lot of optional `USE` flags that can be enabled.

### Audio device support

Support for [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio),  [JACK](https://wiki.gentoo.org/wiki/JACK), [OpenAL](https://wiki.gentoo.org/index.php?title=OpenAL&action=edit&redlink=1), and [SDL](https://wiki.gentoo.org/index.php?title=SDL&action=edit&redlink=1) audio can optionally be enabled through their respective `USE` flags.

To choose the preferred audio backend, go to the Edit->Preferences->System tab and set the Audio Dev to the desired setting.

### CUDA support

Cycles renderer can work on GPUs, for example Nvidia GTX 970 is about twice as fast as an i5 4690k on traditional BMW benchmark.

To enable graphics card rendering with Nvidia graphics cards, install Cuda:

`root #``emerge --ask --verbose dev-util/nvidia-cuda-toolkit`
Inside Blender, go to the Edit->Preferences->System tab and set Compute Device to CUDA and select the graphics card in the box below. If the graphics card is not supported these options will not be visible.

Now set the renderer to Cycles Renderer and in the renderer panel under the Render options set the Device to GPU Compute.

The first time a render is created with a new version of blender, the CUDA kernels will need to be compiled. This may take over 15 minutes.

### oneAPI support

### File format support

Support OpenCOLLADA (.dae), jpeg2k, sndfile, and tiff image file formats can optionally be enabled through USE flags.

The `collada` USE flag adds entries to File->Import/Export for Collada (Default) (.dae) files.
The others can be used with background images in the properties panel of the 3D View or as output formats in the render panel.

Blender should work with either ffmpeg or libav libraries, although only ffmpeg is officially recommended by the Blender developers.

### Headless (server only)

For render farms it is possible to compile blender with the `headless` USE flag. This is *not recommended* for most users.

This feature reduces the Blender file size by around 5 MB, but it **will not be possible to run blender normally** as the GUI is not available.

In headless mode, Blender can still be used to run python scripts from the commmand line:

`user $``blender -b -P script.py [-- [--optionsforscript .. ] ]`
### Memory allocator

It is recommended to enable `jemalloc` to use a more efficient memory allocator. This reduces wasted memory during rendering and allows for larger scenes to be rendered.

### Memory profiling

Support for memory profiling can be enabled using the `valgrind` USE flag. See [Debugging](https://wiki.gentoo.org/wiki/Debugging) for instructions on setting up Valgrind.

### OpenColorio

Open Colorio provides additional options under the Color Management section of the Scene panel.

Inside Blender, select the Render View and Look options, and adjust the exposure, gamma, and curves to obtain the desired look.

### OpenSubdiv

*OpenSubdiv* is a set of libraries that provide high-performance subdivision surface modifier evaluation<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>. This can dramatically improve the frame rate of viewing animations in the viewport when using high levels of subdivision.
Enable the `opensubdiv` `USE` flag to enable support in Blender.

### OpenVDB

OpenVDB provides a data structure for storing and manipulating volumetric information efficiently. It can be compiled into blender using the `openvdb` USE flag, and `openvdb-compression` is also recommended as the data can require upwards of 20MB.

In Blender, set the renderer to Cycles Renderer. Go to the physics panel and enable physics for Smoke. In the smoke section select Domain. Save the file to enable editing of the smoke cache. Change File Format to Openvdb and select Blosc compression if desired. Now create and bake the simulation.

## See also

- [Debugging](https://wiki.gentoo.org/wiki/Debugging)
- [SpaceNavigator](https://wiki.gentoo.org/wiki/SpaceNavigator)
- [OpenCL](https://wiki.gentoo.org/wiki/OpenCL) — a framework for writing programs that execute across heterogeneous computing platforms (CPUs, GPUs, DSPs, FPGAs, ASICs, etc.).
- [Project:Artwork/Artwork](https://wiki.gentoo.org/wiki/Project:Artwork/Artwork): Gentoo artwork, including a .blend file used to create some of it.
