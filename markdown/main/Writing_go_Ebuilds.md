<!-- source: https://wiki.gentoo.org/wiki/Writing_go_Ebuilds | group: Gentoo Wiki (Main) | wiki-title: Writing go Ebuilds -->
---
title: Writing go Ebuilds
url: https://wiki.gentoo.org/wiki/Writing_go_Ebuilds
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-02"
fingerprint: "8e28865b0f6f01ea"
license: CC BY-SA 4.0
---

# Writing go Ebuilds

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a short reference, intended to be read alongside [Basic guide to write Gentoo Ebuilds](https://wiki.gentoo.org/wiki/Basic_guide_to_write_Gentoo_Ebuilds) and the [go-module.eclass documentation](https://devmanual.gentoo.org/eclass-reference/go-module.eclass/index.html).

## go-module eclass

If the software to be packaged has a file named go.mod in its top level directory, it uses modules and the ebuild should inherit this eclass.

### Packaging the dependencies

Ebuilds using the [go-module](https://devmanual.gentoo.org/eclass-reference/go-module.eclass/index.html) eclass are different from most other ebuilds, because they have to include every go dependency. With some luck, the project will have a vendor/ directory and no special action will be needed. If it doesn't, in the short-term, the dependencies will have to be packaged manually. But longer term, try to convince upstream to [generate a tarball](https://github.com/noborus/ov/pull/196/files) in CI.

Previously, go-module packages often used `EGO_SUM`, but [that is deprecated](https://gitweb.gentoo.org/repo/gentoo.git/commit/?id=62fb29b23e3bc0eec89b6cf33d33c6cc939c1990) and will not be covered here. The currently favored method is creating a vendor or dependency-tarball.

Write the ebuild like normal, `inherit go-module` and add the upstream tarball to `SRC_URI`. Then unpack the package and cd to the build directory:

`user $````
cd "$(portageq get_repo_path / <repo name>)"/app-misc/foo
```
`user $````
nano foo-1.ebuild
```
`user $````
ebuild foo-1.ebuild unpack
```
`user $````
cd /path/to/the/unpacked/source/foo-1
```
#### Automatically creating tarballs with gentoo-golang-dist

Vendor tarballs can be automatically created using [gentoo-golang-dist](https://github.com/gentoo-golang-dist), which uses GitHub actions to create tarballs for each tag of a given repository. To add packages to the official gentoo-golang-dist, users can submit a bug [here](https://bugs.gentoo.org/enter_bug.cgi?product=Gentoo%20Infrastructure&component=gentoo-golang-dist&short_desc=Addition%20of%20%3CPACKAGE%3E&comment=Repository%20URL%3A%0AInitial%20tags%20needed%3A%0ASpecial%Go%20arguments%20needed%20to%20get%20the%20tarball%3A%20N/A), which should follow the format:

Repository URL: The repository URL for the desired package. Official locations should be preferred over mirrors.
Initial tags needed: The first tag a tarball is needed for, usually the latest.
Special Go arguments needed to get the tarball: Sometimes additional directories are needed. Leave as N/A if unsure.

Alternatively, users can create their own version of gentoo-golang-dist using [projg2/golang-dist-mirror-action](https://github.com/projg2/golang-dist-mirror-action).

Once a tarball has been created, its URL can be added to `SRC_URI`. Please prefer the smaller vendor tarball if it works.

#### Manual Vendor tarball

This method produces far smaller tarballs, but might not include everything needed to compile the package, especially if the dependencies compile C or C++ code:

`user $````
go mod vendor
```
`user $````
cd ..
```
`user $````
tar --create --owner root --group root --auto-compress --file foo-1-vendor.tar.xz foo-1/vendor
```
Upload the tarball somewhere and add it to `SRC_URI`.

#### Manual Dependency tarball

If the package won't compile because [the vendor tarball has not all dependencies](https://archives.gentoo.org/gentoo-dev/message/c29e1c6a355101fd00eb0cbc49b1c540), a dependency tarball is needed:

`user $````
GOMODCACHE="${PWD}"/go-mod go mod download -modcacherw -x
```
`user $````
tar --create --owner root --group root --auto-compress --file foo-1-deps.tar.xz go-mod
```
Upload the tarball somewhere and add it to `SRC_URI`.

#### Uploading the tarball

Gentoo developers can put the generated tarballs into their devspace, but what about others?

- For those having access to a web or FTP server, or a file hosting service, put it there. Make sure the file can be downloaded without having to solve a captcha or having a cookie set. The easiest way to test this is by downloading it with wget.
- For those who have access to a git forge such as GitLab, Gitea, GitHub, … create an empty repository, add a new tag for each new version (named `${P}`) and upload the tarballs to these ”releases“. Make sure that the host of the forge allows this usage.
- An alternative approach when having access to a git forge is using a CI pipeline to automatically create and publish dependency tarballs. An example for automating this with the GitLab CI can be found here: [https://gitlab.fem-net.de/gentoo/fem-overlay-vendored](https://gitlab.fem-net.de/gentoo/fem-overlay-vendored).

### Compiling and installing

Since the go-module eclass doesn't implement `src_compile()` and `src_install()`, this must be done manually:

**`foo-1.ebuild`**

**Simple implementation of the compile and install phases**

```
() {
    ego build
}
src_install() {
    dobin foo
    default
}
…
```
### Writing live ebuilds

First create the skeleton ebuild as described above. Then change the version in the ebuild's file name to 9999.

In the ebuild itself:

- Remove the unnecessary `SRC_URI` variable; sources are typically fetched via git-r3.eclass or similar. git-r3 should come after go-module on the inherit line.

- Create a `src_unpack()` function:

```
 src_unpack() {
     git-r3_src_unpack
     go-module_live_vendor # This is needed most of the time except when the source includes the vendor files too, like the lazygit project
 }
```
This will fetch the source from git and then the needed modules using go.

### Unbundling C/C++ libraries

One way to detect bundled C or C++ libraries is to create a vendor tarball instead of a dependency tarball, because the former does not include the C/C++ source code. If the build fails, there is a good chance one has been found. Determine the go package that caused the failure by studying the build log and look into its source code. Go uses `#cgo` statements in go files to set compiler commands. With luck, the author included a define to switch from compiling the library to using the system one:

**`foo-1-unbundle-webp.patch`**

**Example patch for unbundling libwebp**

```
--- a/vendor/github.com/bep/gowebp/internal/libwebp/a__cgo.go
+++ b/vendor/github.com/bep/gowebp/internal/libwebp/a__cgo.go
@@ -2,5 +2,6 @@
 package libwebp
-// #cgo linux LDFLAGS: -lm
+// #cgo linux LDFLAGS: -lm -lwebp
+// #cgo CFLAGS: -DLIBWEBP_NO_SRC
 import "C"
--
2.35.1
```
## Licenses

Since Go programs are statically linked, it is important that the ebuild's `LICENSE` setting includes the licenses of all statically linked dependencies. So please make sure it is accurate. Use a utility like [dev-go/go-licenses](https://packages.gentoo.org/packages/dev-go/go-licenses) to extract this information.

To run `go-licenses`, cd into the source directory, then run go-licenses report ./.... The full list of properly formatted Gentoo-compatible licenses can be obtained by running:

```
$ printf 'LICENSE+=" '; go-licenses report ./... 2>/dev/null |
awk -F ',' '{ print $NF }' |
sort --unique |
sed -e "$(sed -n -E 's|^(\S+)\s*=\s*(\S+)$|s/\1/\2/;|p' /var/db/repos/gentoo/metadata/license-mapping.conf)" |
tr '[:space:]' ' '; printf '"\n'
```
This gets the licenses, parses and formats them, uses a [sed(1)](https://man.archlinux.org/man/sed.1.en) [command to parse the license mapping in license-mapping.conf to generate a sed command, runs that for the previously formatted go licenses](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

There is [bug #967017](https://bugs.gentoo.org/show_bug.cgi?id=967017), that talks about a wrapper, and until that gets resolved, there's also [User:Ingenarel](https://wiki.gentoo.org/wiki/User:Ingenarel)['s wrapper](https://codeberg.org/ingenarel-NeoJesus/gentoo-dev-scripts/src/branch/master/bins/gentoo-go-license)

## See also

- [Basic guide to write Gentoo Ebuilds](https://wiki.gentoo.org/wiki/Basic_guide_to_write_Gentoo_Ebuilds) — getting started writing **[ebuilds](https://wiki.gentoo.org/wiki/Ebuild)**, to harness the power of [Portage](https://wiki.gentoo.org/wiki/Portage), to install and manage even more software.
- [Go ebuild tricks](https://wiki.gentoo.org/wiki/Go_ebuild_tricks) — a collection of more niche Go packaging issues & how to tackle them, in lieu of a full-blown Python guide analogue.
