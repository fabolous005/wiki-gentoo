<!-- source: https://wiki.gentoo.org/wiki/Hyperfine | group: Gentoo Wiki (Main) | wiki-title: Hyperfine -->
---
title: Hyperfine
url: https://wiki.gentoo.org/wiki/Hyperfine
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-05"
fingerprint: db5af8d12ea17cc0
license: CC BY-SA 4.0
---

# Hyperfine

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Hyperfine** is a command-line benchmarking tool. It runs an analysis across multiple runs, has support for arbitrary shell commands, and can export results to various formats.

## Installation

### USE flags


### Emerge

`root #``emerge --ask app-benchmarks/hyperfine`
## Usage

### Basic Benchmarks

The simplest benchmark you can run is likely the `sleep` command. For example:

`user $``hyperfine 'sleep 0.3'`
### Warmup runs

Warmup runs will likely come in handy if you are running a program that is I/O intensive, like [grep](https://wiki.gentoo.org/wiki/Grep). This is because benchmarks can be heavily influenced by whether the disk caches are warm or cold.

If you wish to benchmark for a warm cache, you can run

`user $``hyperfine --warmup 3 'grep -R TODO *'`
For cold caches, you can use the `-p`/`--prepare` option to run a special command before each timing run.

`user $``hyperfine --prepare 'sync; echo 3 | sudo tee /proc/sys/vm/drop_caches' 'grep -R TODO *'`
### Exporting results

You can export hyperfine results to a various amount of formats, for markdown you would use `--export-markdown <file>` to create a table of the results.

The JSON output is more detailed and can be used in plotting charts.
