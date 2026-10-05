<!-- source: https://wiki.gentoo.org/wiki/Overlay:Stuff | group: Gentoo Overlay | wiki-title: Overlay:Stuff -->
---
title: Overlay:stuff
url: https://wiki.gentoo.org/wiki/Overlay:Stuff
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-08"
fingerprint: a311505c8ad7f7aa
license: CC BY-SA 4.0
---

# Overlay:stuff

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**stuff** is a third-party Gentoo ebuild overlay for curated packages that are not
in the main Gentoo repository, need local maintenance, or are intentionally kept
ahead of ::gentoo for selected use cases.

Its primary emphasis is **GPU compute and local AI**: AMD ROCm, AMD Ryzen-AI / NPU
tooling, NVIDIA CUDA, local LLM software, and speech/audio machine learning. The
overlay also contains scientific physics and microscopy software, SAXS/SANS/XAFS
analysis tools, a full TeX Live packaging, the DeaDBeeF plugin ecosystem, selected kernel packages, Qt5
compatibility packages, and a small Python 2 preservation layer for legacy scientific
scripts.

## Usage

The overlay can be enabled with [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository):

`root #````
emerge --ask app-eselect/eselect-repository
```
`root #````
eselect repository enable stuff
```
`root #``emerge --sync stuff`
After syncing, read repository news items when present:

`root #``eselect news read`
The repository uses thin manifests and masters = gentoo, so the main Gentoo repository must remain enabled. Packages may require package-specific \~arch keywording.

## Mirrors

- [github.com/istitov/stuff](https://github.com/istitov/stuff) — primary repository
- [gitlab.com/istitov/stuff](https://gitlab.com/istitov/stuff) — mirror
- [codeberg.org/istitov/stuff](https://codeberg.org/istitov/stuff) — mirror

## Package areas

The [repository README](https://github.com/istitov/stuff/blob/master/README.md)
contains a more complete and frequently updated package overview.

### GPU compute and local AI

The overlay's central focus: GPU-compute stacks for both AMD and NVIDIA hardware, the AMD NPU (XDNA/XDNA2) software layer, and backend-agnostic local LLM tooling.

#### AMD ROCm

Local updates of the ROCm stack, often tracking newer releases than ::gentoo:

- **HIP toolchain** — dev-util/hip, dev-util/hipcc, dev-util/hipify-clang
- **Runtime and core libraries** — dev-libs/rocm-core, dev-libs/rocm-comgr, dev-libs/rocm-device-libs, dev-libs/rocm-opencl-runtime, dev-libs/rccl
- **HIP / ROC math libraries** — sci-libs/hipBLAS, sci-libs/hipFFT, sci-libs/hipRAND, sci-libs/hipSOLVER, sci-libs/hipSPARSE, sci-libs/rocBLAS, sci-libs/rocFFT, sci-libs/rocSOLVER, sci-libs/composable-kernel, sci-libs/miopen
- **Management tools** — dev-util/rocm-smi, dev-util/rocminfo
- **SDK** — dev-util/therock-bin, an /opt-installed ROCm SDK package based on AMD's TheRock builds that coexists with the /usr stack

See [ROCm](https://wiki.gentoo.org/wiki/ROCm) for general ROCm setup.

#### AMD Ryzen-AI / NPU

NPU-focused LLM tooling for AMD Ryzen AI systems, including XDNA/XDNA2 driver and runtime components: sci-ml/fastflowlm (NPU-first LLM runtime), sci-ml/lemonade (AMD Lemonade SDK), sci-ml/amd-gaia (AMD GAIA stack), and dev-libs/xdna-driver, dev-libs/xrt-xdna, dev-util/xrt (NPU driver and XDNA-extended Xilinx Runtime).

#### NVIDIA CUDA

LLM inference and GPU Python bindings, with the CUDA toolkit: dev-python/vllm, dev-python/cupy, dev-python/pycuda, the dev-python/cuda-bindings / dev-python/cuda-python / dev-python/cuda-pathfinder chain, and dev-util/nvidia-cuda-toolkit.

#### Local LLM tooling

Backend-agnostic — pairs with the AMD and NVIDIA stacks above and any OpenAI-compatible endpoint: sci-misc/llama-cpp, sci-misc/llama-swap (model-swap proxy), www-apps/hollama (chat UI), dev-util/aichat (LLM CLI), dev-util/rtk.

#### Speech and audio machine learning

Speech recognition, diarization, text-to-speech, and audio DSP: app-accessibility/whisper-cpp, sci-ml/sherpa-onnx, sci-ml/pyannote-audio and its companion sci-ml/pyannote-\* packages, and DSP/augmentation blocks such as sci-ml/torch-audiomentations.

### Scientific software

A second focus area, much of it not packaged in ::gentoo: condensed-matter and materials-science tooling for microscopy, scattering, crystallography, and micromagnetism.

#### Electron microscopy (HyperSpy / 4D-STEM)

A HyperSpy-centered ecosystem — dev-python/hyperspy and its GUI backends, the I/O layer (dev-python/rosettasciio, dev-python/ncempy, dev-python/emdfile), and per-domain extensions: dev-python/exspy (EELS/EDS), dev-python/atomap (atomic-column analysis), dev-python/pyxem and dev-python/py4dstem (4D-STEM), with sci-physics/prismatic for STEM image simulation.

#### SANS / SAXS / XAFS

Small-angle scattering and X-ray absorption analysis: sci-physics/mantid (SANS reduction), sci-physics/sasview with dev-python/sasmodels, sci-libs/ausaxs with dev-python/pyausaxs, and the XAFS tools sci-physics/xraylarch and sci-physics/demeter.

#### Micromagnetism and crystallography

Micromagnetic and spin-dynamics solvers (sci-physics/mumax, sci-physics/oommf, sci-physics/vampire), Rietveld / powder-diffraction tools (sci-physics/bgmn, sci-physics/profex), and sci-visualization/gwyddion for scanning-probe data.

#### TeX Live

A complete TeX Live 2025/2026 packaging (honorable mention in scientific tools as used predominantly in scientific publishing) (app-text/texlive-core, app-text/dvipsk, app-text/dvisvgm, app-text/ttf2pk2), the full dev-texlive/\* collection set (basic, fontsrecommended, latexextra, lang\*, etc), and TeX tools such as dev-tex/biber, dev-tex/latexmk, dev-tex/minted, dev-tex/pgf and dev-tex/biblatex.

### DeaDBeeF plugins

Many third-party DeaDBeeF audio-player plugins under media-plugins/deadbeef-\* — additional audio formats, archive support, visualizers, playback control, desktop integration, ratings, and other playback or interface features.

### Compatibility and legacy packages

Selected Qt 5 packages, mostly in the dev-qt/\*:5 slot at the 5.15 LTS line with the KDE Qt5 Patch Collection, plus dev-python/pyqt5, for consumers such as sci-physics/mantid that have not yet moved to Qt6, and a small Python 2 preservation layer (vendored \*\_py2 eclasses and dev-python/\*-python2 forks) for legacy scientific scripts — notably sci-visualization/gwyddion's pygwy bindings.

### Kernel and system packages

sys-kernel/pf-sources and sys-kernel/pf-sources-extended, with local curation of selected security patches, plus sys-apps/dkms-gentoo and sys-kernel/kernel-cleaner.

### Additional packages

The overlay also contains smaller sets of desktop and developer packages, including finance tools such as app-office/beancount and app-office/fava, XMPP software such as net-im/profanity, collaborative editing tools such as app-editors/gobby, and developer tools such as dev-vcs/fossil and dev-lang/tcc.

## Caveats

stuff is a third-party overlay. Some packages are experimental, fast-moving, or special-purpose. Review ebuilds, USE flags, package masks, and news items before enabling packages from the overlay on production systems.

The ROCm, NPU, CUDA, machine-learning, Python 2, and Qt5 packages in particular may need additional local configuration and can change more quickly than packages in ::gentoo.

## Contributing

Bug reports, patches, and pull requests are welcome, especially for packages that
contributors use and can test. Before submitting changes, read the repository's
[CONTRIBUTING.md](https://github.com/istitov/stuff/blob/master/CONTRIBUTING.md); in
general, changes should be checked with [dev-util/pkgcheck](https://packages.gentoo.org/packages/dev-util/pkgcheck) and
[app-portage/pkgdev](https://packages.gentoo.org/packages/app-portage/pkgdev), kept focused, and accompanied by enough information
for review.

Useful bug reports include the exact package atom, full emerge --info output, and the relevant build log or failing command output.

## History

**stuff** has been around since 6 December 2010, when [megabaks](https://github.com/megabaks/) created it as a personal overlay and did most of the early work through the mid-2010s. The overlay was transferred to istitov ([github](https://github.com/istitov/), [gentoo](https://wiki.gentoo.org/wiki/User:Istitov)
) in January 2017 and has been primarily maintained by him ever since. Activity stayed steady through the rest of the decade, with [Vasily Lebedev](https://github.com/LebedevV/) among the main contributors who added a lot of scientific packages to the overlay stack. In 2026 the overlay was significantly cleaned up and given a new tilt towards GPU/NPU/AI tooling; the Qt5 revival (after ::gentoo dropped the slot) and a full TeX Live 2025/2026 set were added as well. The [contributors graph](https://github.com/istitov/stuff/graphs/contributors) has the full list.



## See also

- [ROCm](https://wiki.gentoo.org/wiki/ROCm) — AMD ROCm GPU compute on Gentoo
- [Ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) — using and creating ebuild repositories
- [Project:GURU](https://wiki.gentoo.org/wiki/Project:GURU) — the community overlay
- [Project:Science](https://wiki.gentoo.org/wiki/Project:Science) — the scientific project and overlay
