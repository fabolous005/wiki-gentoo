<!-- source: https://wiki.gentoo.org/wiki/Rocprofiler | group: Gentoo Wiki (Main) | wiki-title: Rocprofiler -->
---
title: Rocprofiler
url: https://wiki.gentoo.org/wiki/Rocprofiler
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-07"
fingerprint: b7eb91cb0ec33f92
license: CC BY-SA 4.0
---

# Rocprofiler

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**rocprofiler** is the profiler utility for GPGPU programs written in [HIP](https://wiki.gentoo.org/wiki/HIP).

## Installation

[dev-util/rocprofiler](https://packages.gentoo.org/packages/dev-util/rocprofiler) pulls [HIP](https://wiki.gentoo.org/wiki/HIP) toolchain (provided by [dev-util/hip](https://packages.gentoo.org/packages/dev-util/hip)) along with profiling/tracing libraries, headers, and scripts.

### Emerge

Install [dev-util/rocprofiler](https://packages.gentoo.org/packages/dev-util/rocprofiler):

`root #``emerge --ask dev-util/rocprofiler`
## Usage

Detailed usage can be found in [rocprof document](https://docs.amd.com/bundle/AMD-ROCProfiler-User-Guide-v5.1/page/rocprof_Command_Line_Tool.html).

### Profiling

To simply profile GPU kernels in a program, run the following command:

`user $``rocprof <program-to-be-profiled>`
This will generate a `results.csv` containing execution duration of kernels.

To simply trace a program, collecting HIP/HSA API calls and kernel execution details, run the following command:

`user $``rocprof --sys-trace <program-to-be-profiled>`
This will further generates `results.json`， which is a standard trace file.

Full command line arguments can be viewed by running `rocprof --help`.

### Viewing results

There are various viewers, including [Chromium](https://wiki.gentoo.org/wiki/Chromium) and [perfetto](https://ui.perfetto.dev/).

Using `chrome://tracing` to load and view trace files should be the simplest method, and does not require network.

Note that Debian has stripped the tracing <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> of its chromium package.

### Performance Counters

Accessing performance counters requires the use of the proprietary AQL profiler library, which has been stripped from Gentoo. Thus, rocprofiler will crash when one attempts to list or record performance counters.

`user $````
rocprof --list-basic
```
RPL: on '230607\_034945' from '/usr' in '/root':
Basic HW counters:
/usr/bin/rocprof: line 389:   574 Segmentation fault      (core dumped) /usr/bin/rocprof-ctrl

To restore performance counters, edit `dev-libs/rocr-runtime` and remove `"${FILESDIR}/${PN}-4.3.0_no-aqlprofiler.patch"`, then edit `dev-util/rocprofiler` and remove `"${FILESDIR}/${PN}-4.3.0-no-aqlprofile.patch"` and `"${FILESDIR}/${PN}-5.3.3-remove-aql-in-cmake.patch`. Finally, obtain a copy of `libhsa-amd-aqlprofile64.so` (as of writing, it's `libhsa-amd-aqlprofile64.so.1.0.50500`) from [https://repo.radeon.com/rocm/apt/debian/pool/main/h/hsa-amd-aqlprofile/](https://repo.radeon.com/rocm/apt/debian/pool/main/h/hsa-amd-aqlprofile/) by extracting the apt package. Finally, copy `libhsa-amd-aqlprofile64.so.1.0.50500` to `/usr/lib64/` and create a symlink `libhsa-amd-aqlprofile64.so` to the versioned library.

According to Gentoo developer Marek Szuba:

For the record, this profiler has long since been deprecated in favour of RCP ([https://github.com/GPUOpen-Tools/radeon\_compute\_profiler](https://github.com/GPUOpen-Tools/radeon_compute_profiler)). Between that and it being proprietary, I would very much advise against adding it to the tree. And yes, candrews and I *will* eventually get to packaging RCP for Gentoo :-)


While it's generally true, if raw performance counters are needed, there's no alternative way than using `rocprofile` and `hsa-amd-aqlprofile`. Thus, the writer of this Gentoo Wiki article believes that the long-term solution is making proprietary library an opt-in option by a USE flag.

#### Legacy GPUs

Furthermore, older graphics cards are not supported by `hsa-amd-aqlprofile`. Due to its proprietary nature, it's not possible to patch the source to re-enable support (unlike how pre-VEGA hardware is re-enabled in `dev-libs/rocm-opencl-runtime`). But it's possible to install older versions of the library.

`user $````
rocprof --list-basic
```
RPL: on '230607\_065133' from '/usr' in '/usr/local/lib'
Basic HW counters:
ERROR: rocprofiler\_iterate\_info(), Translate(), ImportMetrics: bad block name 'GRBM', GFXIP is not supported(gfx803)

## External resources

- [AMD ROCProfiler User Guide v5.1](https://docs.amd.com/bundle/AMD-ROCProfiler-User-Guide-v5.1/page/rocprof_Command_Line_Tool.html) – The official user guide
