<!-- source: https://wiki.gentoo.org/wiki/Go | group: Gentoo Wiki (Main) | wiki-title: Go -->
---
title: Go
url: https://wiki.gentoo.org/wiki/Go
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-09"
fingerprint: "36233e78ce472d99"
license: CC BY-SA 4.0
---

# Go

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Go** is an open source, statically typed, compiled programming language.

Go can be used to write software, and is used to make some of the packages that are available in the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository).

## Installation

### USE flags


### USE flags for
            [dev-lang/go](https://packages.gentoo.org/packages/dev-lang/go)
            
            A concurrent garbage collected and typesafe programming language

### Emerge

Install Go:

`root #``emerge --ask dev-lang/go`
## Update

Because of Go static linking packages built with an old version of Go will retain vulnerabilities that were fixed in newer versions until rebuilt. Fortunately portage will take care of recompiling vulnerable packages for you with BDEPEND slot-operators.

## Configuration

### Environment variables

Users may customize how Go builds packages by setting these variables, similar to the way the C build system is configured using `CFLAGS` or `LDFLAGS`:

- `GOAMD64` (corresponds to x86\_64 [microarchitecture levels](https://en.wikipedia.org/wiki/X86-64#Microarchitecture_levels))
- `GO386` (for x86)
- `GOARM` (for arm)
- `CGO_CFLAGS`
- `CGO_CXXFLAGS`
- `CGO_FFLAGS`
- `CGO_LDFLAGS`

For example, in /etc/portage/make.conf, one may set:

**`/etc/portage/make.conf`**

```
CGO_CFLAGS="${CFLAGS}"
CGO_CXXFLAGS="${CXXFLAGS}"
CGO_FFLAGS="${FFLAGS}"
CGO_LDFLAGS="${LDFLAGS}"
# https://go.dev/wiki/MinimumRequirements#microarchitecture-support
# Pick carefully based on https://en.wikipedia.org/wiki/X86-64#Microarchitecture_levels!
# For amd64 (v1 (default)/v2/v3/v4):
#GOAMD64="v3"
# For x86 (sse2 (default)/softfloat):
#GO386=sse2
# For arm (5/6 (usually default)/7):
#GOARM=6
```
## Usage

### Invocation

`user $``go --help`
Go is a tool for managing Go source code.
Usage:
	go \<command> \[arguments\]
The commands are:
	bug         start a bug report
	build       compile packages and dependencies
	clean       remove object files and cached files
	doc         show documentation for package or symbol
	env         print Go environment information
	fix         update packages to use new APIs
	fmt         gofmt (reformat) package sources
	generate    generate Go files by processing source
	get         add dependencies to current module and install them
	install     compile and install packages and dependencies
	list        list packages or modules
	mod         module maintenance
	work        workspace maintenance
	run         compile and run Go program
	test        test packages
	tool        run specified go tool
	version     print Go version
	vet         report likely mistakes in packages
Use "go help \<command>" for more information about a command.
Additional help topics:
	buildconstraint build constraints
	buildmode       build modes
	c               calling between Go and C
	cache           build and test caching
	environment     environment variables
	filetype        file types
	go.mod          the go.mod file
	gopath          GOPATH environment variable
	gopath-get      legacy GOPATH go get
	goproxy         module proxy protocol
	importpath      import path syntax
	modules         modules, module versions, and more
	module-get      module-aware go get
	module-auth     module authentication using go.sum
	packages        package lists and patterns
	private         configuration for downloading non-public code
	testflag        testing flags
	testfunc        testing functions
	vcs             controlling version control with GOVCS
Use "go help \<topic>" for more information about that topic.

### LSP

"gopls" (pronounced “Go please”) is the official language server for Go, developed by the Go team. It provides a wide variety of IDE features to any LSP-compatible editor.

To install gopls:

`root #``emerge --ask dev-go/gopls`
## Removal

### Unmerge

To remove Go:

`root #``emerge --ask --depclean --verbose dev-lang/go`
## See also

- [Writing go Ebuilds](https://wiki.gentoo.org/wiki/Writing_go_Ebuilds) — a short reference, intended to be read alongside [Basic guide to write Gentoo Ebuilds](https://wiki.gentoo.org/wiki/Basic_guide_to_write_Gentoo_Ebuilds) and the [go-module.eclass documentation](https://devmanual.gentoo.org/eclass-reference/go-module.eclass/index.html)
- [Application level package management](https://wiki.gentoo.org/wiki/Application_level_package_management) — provides best practice recommendations on managing the coexistence of operating system and **application level package managers** on Gentoo.
