<!-- source: https://wiki.gentoo.org/wiki/Elixir | group: Gentoo Wiki (Main) | wiki-title: Elixir -->
---
title: Elixir
url: https://wiki.gentoo.org/wiki/Elixir
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-06-15"
fingerprint: f6d1fdd892b83b57
license: CC BY-SA 4.0
---

# Elixir

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Elixir** is a dynamic programming language, with a functional paradigm to easily write scalable and maintainable projects. Elixir runs on [Erlang's](https://wiki.gentoo.org/wiki/Erlang) BEAM virtual machine.

## Installation

### USE flags


### USE flags for
            [dev-lang/elixir](https://packages.gentoo.org/packages/dev-lang/elixir)
            
            Elixir programming language

| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask dev-lang/elixir`
## Usage

### Invocation

`user $``iex --help````
Usage: iex [options] [.exs file] [data]
The following options are exclusive to IEx:
  --dot-iex "PATH"    Overrides default .iex.exs file and uses path instead;
                      path can be empty, then no file will be loaded
  --remsh NAME        Connects to a node using a remote shell
It accepts all other options listed by "elixir --help".
```
