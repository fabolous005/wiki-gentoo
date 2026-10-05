<!-- source: https://wiki.gentoo.org/wiki/RadeonProRenderSDK | group: Gentoo Wiki (Main) | wiki-title: RadeonProRenderSDK -->
---
title: RadeonProRenderSDK
url: https://wiki.gentoo.org/wiki/RadeonProRenderSDK
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-16"
fingerprint: "9bc6921e1c8f9bc6"
license: CC BY-SA 4.0
---

# RadeonProRenderSDK

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

AMD [Radeon ProRender](https://github.com/GPUOpen-LibrariesAndSDKs/RadeonProRenderSDK) is a powerful physically-based rendering engine that enables creative professionals to produce stunningly photorealistic images. [https://www.amd.com/en/technologies/radeon-prorender](https://www.amd.com/en/technologies/radeon-prorender)

## ProRender SDK

### Repository preparation

Ebuild for SDK installation not in official gentoo repository, but can be found in [feniksa](https://github.com/feniksa/gentoo-overlay/tree/master/dev-util/radeon-pro-render-sdk) overlay. Use eselect repository to configure the additional repository.

#### eselect repository

Install [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository):

`root #``emerge --ask --noreplace app-eselect/eselect-repository`
Enable and sync the feniksa repository:

`root #``eselect repository enable feniksa``root #``emerge --sync feniksa`
### SDK installation

To install Radeon Pro Render SDK:

`root #``emerge --ask dev-libs/radeon-pro-render-sdk`
### Tutorials

#### Build

Install the package with the examples USE flag:

`root #``emerge --ask dev-libs/radeon-pro-render-sdk[examples]`
#### Run

All tutorial binaries may be found at /usr/share/RadeonProRender/tutorials/Bin directory. First, let's check ProRender initialize

`root #````
cd /usr/share/RadeonProRender/tutorials/Bin
```
`root #``./00_context_creation64D`
Radeon ProRender SDK simple context creation tutorial.
RPR Context creation succeeded.
GPU0 name : NVIDIA GeForce GTX 1080

If everything is fine, last line should show GPU name. It means, that RadeonProRender correctly initialize OpenCL, found available GPU and initialized resources for PBR rendering . Source code of this tutorial may be found at /usr/share/RadeonProRender/tutorials/00\_context\_creation/main.cpp

Let's run some PBR

`root #````
cd /usr/share/RadeonProRender/tutorials/Bin
```
`root #``./22_material_uber64D`
RPR Context creation succeeded.
Rendering 22\_material\_uber\_00.png ...
Rendering 22\_material\_uber\_01.png ...
Rendering 22\_material\_uber\_02.png ...
Rendering 22\_material\_uber\_03.png ...
Rendering 22\_material\_uber\_04.png ...
Rendering 22\_material\_uber\_05.png ...
Rendering 22\_material\_uber\_06.png ...
Rendering 22\_material\_uber\_07.png ...
Rendering 22\_material\_uber\_08.png ...
Rendering 22\_material\_uber\_final.png ...

In folder /usr/share/RadeonProRender/tutorials/Bin should be png files with result of rendering. Source code of material creation may be found at /usr/share/RadeonProRender/tutorials/22\_material\_uber/main.cpp

## Image Processing Library

To install Radeon Pro Render [Image Filtering library](https://github.com/GPUOpen-LibrariesAndSDKs/RadeonImageFilter)

`root #``emerge --ask dev-libs/radeon-pro-image-filter`
### Tutorials

#### Build

Install the package with the examples USE flag:

`root #``emerge --ask dev-libs/radeon-pro-image-filter[examples]`
#### Run

All tutorial binaries may be found at /usr/share/RadeonImageFilter/samples/Bin directory. First, let's check ProRender initialize

`root #````
cd /usr/share/RadeonImageFilter
```
`root #````
 mkdir images
```
`root #``./samples/Bin/AIDenoiser/AIDenoiser`
\[MSG  @RIF\]: Loaded RIF API version: 1.7.3.0x1c7b817e
INFO: MIOpen version: 2.0.5
\[MSG  @RIF\]: Loading model: ./models/denoise\_c9\_ldr.pb
INFO: Memory: required: 0, single node max: 0, requested sum: 0
RIF inference time: 57133 us. Resolution is : 600x800
outputPath=./images/output\_0.hdr


In folder /usr/share/RadeonImageFilter/images should be png/hdr files with sample run results
