<!-- source: https://wiki.gentoo.org/wiki/Overlay:Stuff | group: Gentoo Overlay | wiki-title: Overlay:Stuff -->
---
title: Overlay:stuff
url: https://wiki.gentoo.org/wiki/Overlay:Stuff
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-11-09"
fingerprint: "6339c05cc81773aa"
license: CC BY-SA 4.0
---

# Overlay:stuff

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**stuff** is a third-party Gentoo ebuild repository with GPU and NPU computing
tools, local AI software, scientific applications, TeX Live, DeaDBeeF plugins,
kernel packages, and selected compatibility packages.

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
The repository uses thin manifests and masters = gentoo, so the main Gentoo repository must remain enabled. Most packages are keyworded \~arch.

## Mirrors

- [github.com/istitov/stuff](https://github.com/istitov/stuff) — primary repository
- [gitlab.com/istitov/stuff](https://gitlab.com/istitov/stuff) — mirror
- [codeberg.org/istitov/stuff](https://codeberg.org/istitov/stuff) — mirror

## Package areas

### GPU compute and local AI

#### AMD ROCm

::gentoo carries the core ROCm 10.1 stack. The overlay adds 10.1 components it
lacks, such as sci-libs/hipTensor, sci-libs/rocALUTION,
sys-cluster/rocSHMEM, media-libs/rocDecode, app-admin/rdc and
dev-util/rocm-smi 10.1 (::gentoo stops at 7.2), plus modified revisions of
sci-libs/composable-kernel, sci-libs/hipSOLVER and sci-libs/rocSPARSE.
The 10.0 line, with AMD's LLVM fork llvm-core/rocm-llvm, and the 7.2.4 line remain
as rollbacks. dev-util/therock-bin installs AMD's prebuilt TheRock SDK under
/opt, beside the source-built /usr stack. See [ROCm](https://wiki.gentoo.org/wiki/ROCm) for general setup.

#### AMD Ryzen AI NPU

sci-ml/fastflowlm is an LLM runtime for AMD XDNA 2 NPUs. sci-ml/openflowlm is its live-only community fork, which builds the open NPU kernels from source. sci-ml/flm-q4nx-converter converts GGUF fine-tunes of supported models to FastFlowLM's format. The stack also includes the XDNA driver and runtime (dev-libs/xdna-driver, dev-libs/xrt-xdna, dev-util/xrt), the sci-ml/lemonade local inference server and the sci-ml/amd-gaia agent framework.

#### NVIDIA CUDA

dev-python/vllm, dev-python/cupy, dev-python/pycuda, the dev-python/cuda-bindings / dev-python/cuda-python / dev-python/cuda-pathfinder chain, and dev-util/nvidia-cuda-toolkit.

#### Local LLM tooling

sci-misc/llama-cpp runs and serves models; sci-misc/llama-swap routes requests between inference backends. www-apps/hollama and dev-util/aichat are clients, and dev-util/rtk filters command output to cut the tokens sent to coding assistants.

#### Speech and audio

Speech recognition, diarization and text-to-speech: app-accessibility/whisper-cpp, sci-ml/sherpa-onnx, sci-ml/pyannote-audio with its sci-ml/pyannote-\* packages, and augmentation blocks such as sci-ml/torch-audiomentations.

### Scientific software

- **Electron microscopy:** dev-python/hyperspy with its I/O layer and extensions (dev-python/exspy, dev-python/atomap, dev-python/pyxem),dev-python/py4dstem for 4D-STEM, and sci-physics/prismatic for STEM simulation.
- **Scattering and X-ray absorption:** sci-physics/mantid, sci-physics/sasview,sci-libs/ausaxs, sci-physics/xraylarch and sci-physics/demeter.
- **Micromagnetism, diffraction and scanning probe:** sci-physics/mumax, sci-physics/oommf, sci-physics/vampire, sci-physics/bgmn with sci-physics/profex, and sci-visualization/gwyddion.

### TeX Live

::gentoo ships TeX Live 2024 as stable and 2026 as \~arch. The overlay keeps TeX Live 2025, tracks newer snapshots of 2026, and carries newer dev-tex/biber, dev-tex/biblatex and dev-tex/pgf, keyworded \~amd64 and \~arm64.

### DeaDBeeF plugins

Third-party plugins for the DeaDBeeF audio player under media-plugins/deadbeef-\*: formats, output, sound processing, visualization, interface widgets and desktop integration.

### Compatibility and legacy packages

::gentoo removed its Qt5 packages in June 2026. The overlay carries selected dev-qt/\*:5 modules at 5.15.19 LTS with the KDE Qt5 Patch Collection (QtWebEngine is not included), dev-python/pyqt5, and the Qt5-capable x11-libs/qscintilla 2.14.1-r1. Consumers that can still use them include games-roguelike/dwarftherapist and sci-physics/prismatic with USE=qt5, and sci-physics/mantid 6.16.1.1.

A small Python 2 layer (vendored \*\_py2 eclasses and dev-python/\*-python2 forks) supports legacy scientific scripts, notably sci-visualization/gwyddion's pygwy bindings.

### Kernel and system packages

sys-kernel/pf-sources revisions add security backports to the pf-kernel releases that ::gentoo carries, and keep older series back to 6.1. sys-kernel/pf-sources-extended covers the LTS branches 6.1, 6.6, 6.12 and 6.18. Also sys-apps/dkms-gentoo and sys-kernel/kernel-cleaner.

### Additional packages

Smaller sets of desktop and developer packages, such as app-office/beancount and app-office/fava, net-im/profanity, app-editors/gobby, dev-vcs/fossil and dev-lang/tcc.

## Caveats

stuff is a third-party overlay. Its ROCm, NPU, CUDA, machine-learning, Python 2 and Qt5 packages move quickly and may need local configuration. Review ebuilds, USE flags, package masks and news items before installing on production systems.

## Contributing

Bug reports, patches and pull requests are welcome, especially for packages that
contributors use and can test. See the repository's
[CONTRIBUTING.md](https://github.com/istitov/stuff/blob/master/CONTRIBUTING.md); changes
should pass [dev-util/pkgcheck](https://packages.gentoo.org/packages/dev-util/pkgcheck) and [dev-util/pkgdev](https://packages.gentoo.org/packages/dev-util/pkgdev) and stay
focused. Useful bug reports include the exact package atom, full emerge --info
output, and the build log or failing command output.

## History

**stuff** was created on 6 December 2010 by [megabaks](https://github.com/megabaks/),
who did most of the early work through the mid-2010s. istitov
([GitHub](https://github.com/istitov/), [Gentoo](https://wiki.gentoo.org/wiki/User:Istitov)) took it over in January
2017 and has maintained it since. [Vasily Lebedev](https://github.com/LebedevV/) contributed many of
the scientific packages. In 2026 the overlay was cleaned up and refocused on GPU, NPU and
AI tooling, and gained the Qt5 packages and TeX Live 2025/2026. The
[contributors graph](https://github.com/istitov/stuff/graphs/contributors) has the full
list.

## See also

- [ROCm](https://wiki.gentoo.org/wiki/ROCm) — AMD ROCm GPU compute on Gentoo
- [Ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository) — using and creating ebuild repositories
- [Project:GURU](https://wiki.gentoo.org/wiki/Project:GURU) — the community overlay
- [Project:Science](https://wiki.gentoo.org/wiki/Project:Science) — the scientific project and overlay

## External resources

- [Overlay documentation](https://istitov.github.io/stuff/) — setup, benchmarks and guides for the [Ryzen AI NPU](https://istitov.github.io/stuff/guides/ryzen-ai-npu/), [vLLM](https://istitov.github.io/stuff/guides/vllm/),[llama.cpp](https://istitov.github.io/stuff/guides/llama-setup/),[Qt5 LTS](https://istitov.github.io/stuff/guides/qt5-lts/) and [TeX Live](https://istitov.github.io/stuff/guides/texlive/)
