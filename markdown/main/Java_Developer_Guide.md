<!-- source: https://wiki.gentoo.org/wiki/Java_Developer_Guide | group: Gentoo Wiki (Main) | wiki-title: Java Developer Guide -->
---
title: Java Developer Guide
url: https://wiki.gentoo.org/wiki/Java_Developer_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-18"
fingerprint: af093a1b0eb18f00
license: CC BY-SA 4.0
---

# Java Developer Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide covers specific details on Gentoo Java ebuilds.

## Java on Gentoo

This section will give you more insight into how Gentoo handles Java. You should be familiar with the [Java User Guide](https://wiki.gentoo.org/wiki/Java) before proceeding.

### Virtual machines (VMs)

As discussed in the User Guide, there are several VMs available from portage.

Testing all packages to ensure they build and run with every VM is a huge undertaking, and there simply are not enough resources to guarantee every package will build and run with every VM.

We now maintain a list of "supported virtual machines" for each architecture. These are the VMs we will test the packages against before committing changes to portage. When you emerge a package, it will by default try to use the best "supported virtual machine."

Of course, Gentoo and Linux in general are about choice, so if you prefer a different VM over the "supported virtual machines", you can easily use that VM instead. However, you should also know that bugs reported with one of the non-"supported virtual machine" will get a low priority if it isn't present using a "supported virtual machine".

### Configuring which VM to use

You can choose which VM to use on a per-user basis, and which VM to use for the system (ie when running things as root). Both of these are configured using java-config.

A user's current VM is represented by a symlink located at \~/.gentoo/java-config-2/current-user-vm. This symlink points to the `JAVA_HOME` of the chosen VM. Similarly, the system VM is represented by a symlink at /etc/java-config-2/current-system-vm.

The current VM can also be changed on the fly. This can be accomplished setting the environment `GENTOO_VM` to contain the name of a VM that java-config knows about.

### Java tools

The traditional Java tools, ie, java, javac, javadoc, etc, are all located in /usr/bin. They are actually all symlinks to the run-java-tool script. This script will call the appropriate tool, depending on how the script is invoked, from the current VM. The contents of the `GENTOO_VM` variable is checked first, then the user VM, and lastly the system VM.

### Build-time VM switching

As outlined in the User Guide, and mentioned briefly earlier, the VM will switch at build time to accommodate the needs of the package. The VM to use is first determined by `JAVA_PKG_FORCE_VM`, then /etc/java-config-2/build/jdk.conf, and lastly the system VM.

### Bytecode compatibility

The default behavior of javac is to compile bytecode that will be compatible with the current VM version and higher (IE forward compatible). It is possible to specify which VM to compile for to provide the best compatibility.

At build time, the `DEPEND` and `RDEPEND` variables will determine what VM to compile for based on [virtual/jdk](https://packages.gentoo.org/packages/virtual/jdk) and [virtual/jre](https://packages.gentoo.org/packages/virtual/jre). Additionally, this can be controlled by the environment variables [`JAVA_PKG_WANT_SOURCE`](https://devmanual.gentoo.org/eclass-reference/java-utils-2.eclass/#lbAF) and `JAVA_PKG_WANT_TARGET`.

There is a wrapper for javac called ejavac, which will use the appropriate VM's javac, and then specify the appropriate `-target` and `-source`. For projects that use ant as build system, any eventually hardcoded `-target` and `-source` attributes must be patched out from any build.xml files which are used.

## Filesystem layout

In general, the directory policies are handled by the helper functions in the java-utils-2 eclass.

These functions adhere to the following path name conventions:

- /usr/share/${PN}-${SLOT}/package.env contains information about the package.
- .jar files created from source code are installed to /usr/share/${PN}-${SLOT}/lib/
- .jar pre-built files not compiled by the ebuild are installed to /opt/${PN}-${SLOT}/lib
- Javadoc documentation is installed to /usr/share/doc/${PF}/html/api/
- Source archives are installed to /usr/share/${PN}-${SLOT}/source/
- User-executable scripts are installed to /usr/bin
- System-wide environment files are installed to /usr/env.d/java
- User-specific environment files can be put into ${HOME}/.gentoo/env.d/

### Moving or renaming a Java package

When changing the name of a Java package, the process of doing it is different from what's written in [devmanual](https://devmanual.gentoo.org/ebuild-maintenance/package-moves/#moving-or-renaming-a-package). Because the system thinks after the package move that the package is already installed, but with just the package name change and without the package files being moved to a new directory, the Java eclasses don't work, because the location of the jar files is searched in the new directory but the files are still untouched in the old directory.

## Prepare Enviroment

Before getting started there are a few things to prepare in your development environment, such as environment variables to set, and/or packages to merge.

### Variables

In addition to `ECLASS_DEBUG_OUTPUT=on`, there are a couple Java specific portage environment variables to be set. They are optional, but the first is recommend to be set by anyone doing Java ebuild development.

- `JAVA_PKG_STRICT`=true
- Set this variable in make.conf to enable use of [dev-java/java-dep-check](https://packages.gentoo.org/packages/dev-java/java-dep-check). Which will look for and report any unneeded dependencies and/or any bundled dependencies found not part of the package dependencies.

- `JAVA_PKG_DEBUG`=true
- Set this variable to enable debugging in Java eclasses

### Packages

There are some packages that can help you with Java ebuild development and bumping to new versions.

#### Java API check

Each time you are bumping a Java library, you should check whether the API of the library did not change significantly. If the change is significant, you should decide whether new `[SLOT](https://wiki.gentoo.org/wiki/SLOT)` should be introduced to prevent breaking of existing applications depending on this package and the current `SLOT`.

There are several tools that could help you to get fast overview of the changes.

- [dev-util/japi-compliance-checker](https://packages.gentoo.org/packages/dev-util/japi-compliance-checker)
- This package checks for changes in Java API of two jar files. It is fast and also works with latest Java bytecode.

- [dev-util/pkgdiff](https://packages.gentoo.org/packages/dev-util/pkgdiff)
- This is a more general tool that can also check for Java API changes. The output is more detailed and also includes information about other files included. It takes more time to generate the report though.

## General guidelines

In addition to standard Gentoo ebuild guidelines, there are some specific guidelines for Java packages:

- Avoid using [bundled](https://wiki.gentoo.org/wiki/Why_not_bundle_dependencies) .jar files at all costs for source-based packages. Instead, they should use system installed versions with the help of our eclass functions.
- If you only need the path to installed libraries, you can use `java-pkg_getjar(s)`. Don't call `java-config` directly - it will not record the dependency in the package env.
- Always provide an easily understandable reason after `die`, so that end-users will provide the maintainers with sensible feedback if the build should fail.
- Avoid cluttering up the environment by adding environment files to /etc/env.d/. Instead, store your env file in /etc/env.d/java/, and then have user scripts source their environment file when it launches. Otherwise, developers, who regularly override `CLASSPATH`, `CATALINA_HOME` and other environment variables, will have problems running regular apps. If you use the launcher it will also automatically source the appropriate env file.
- Make sure you always compile with a correct source/target version. This is important to ensure future and backwards compatibility. Also, as the lowest supported version of JDK in Gentoo is 1.8, use at least that version if possible to avoid future problems when lower source/target versions will become unsupported by JDKs, which would cause an unnecessary extra maintenance of the packages.
- Do no install jars which contain versions in their filename, ie castor-0.9.7.jar. Use `java-pkg_newjar` which renames and installs jars without version in the file name.

### USE flags

Gentoo Java ebuilds unlike most others start with a USE flag variable, a Java specific one `JAVA_PKG_IUSE`. The only thing that can come before this variable in an ebuild is the `EAPI` variable. You still set the other USE flag for various general USE flags, but there are Java specific USE flags that go in this variable and DO NOT go in the other normal USE flag.



#### About USE flags

For more information regarding USE flags, refer to the [USE flags](https://devmanual.gentoo.org/general-concepts/use-flags) chapter from the Gentoo Development Guide.

#### Java specific USE flags

There are a few specific common USE flags for Java ebuilds as follows. These use flags do not go in the normal `USE` variable but go in `JAVA_PKG_IUSE` instead. Any use flag other than the following would go in the normal `USE` variable. The `JAVA_PKG_IUSE` must precede the [`inherit`](https://devmanual.gentoo.org/ebuild-writing/using-eclasses/) line in an ebuild.

The USE flags that go in `JAVA_PKG_IUSE`

- If USE FLAG [binary](https://packages.gentoo.org/useflags/binary)
- The `doc` flag will build API documentation using javadoc.
- The `source` flag installs a zip of the source code of a package. This is traditionally used for IDEs to 'attach' source to the libraries that are being use;
- The `test` Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently)

#### Masked gentoo-vm

The USE flag [gentoo-vm](https://packages.gentoo.org/useflags/gentoo-vm)[bug #805008](https://bugs.gentoo.org/show_bug.cgi?id=805008) for details on [how to unmask](https://wiki.gentoo.org/wiki/User:Sam/Portage_help/Java_unmasking) this USE flag.



#### Java USE flag usage

The `JAVA_PKG_IUSE` USE flag/variable MUST reside in the ebuild before the inherit line.

If a manual or other extensive documentation is available, it should be installed using the `doc` USE flag. If the build system can, you should build javadocs also using the `doc` USE flag. If it does not, you should consider patching it to do so, and provide the patch upstream. HTML documentation should be installed using `dohtml` and javadocs using `java-pkg_dojavadoc`.

If you want to go all the way, add the `source` USE flag that installs the complete source code as a .zip file. Use `java-pkg_dosrc` for this purpose. This allows IDEs such as Eclipse and NetBeans to do complete source-level debugging. The `source` USE flag will add [app-arch/zip](https://packages.gentoo.org/packages/app-arch/zip) to DEPEND automatically, you do not need to add this dependency.

### Dependencies

Gentoo Java ebuild dependencies for the most part are just like in any other ebuild. There are two common dependencies that will always exist for any Java ebuild, no matter if Java is optional or not. Though if Java is optional, the dependencies would be conditional based on a USE flag, likely `java` USE flag. There are some packages which may require a JDK at runtime, otherwise most should have a JRE for runtime. When depending on a package, take care that you depend on a sufficiently recent version, and explicitly ensure at building time that the providing package gives you the correct interface, i.e. the correct `SLOT[USE]`.

#### JDK/JRE

For any package that builds from source, `DEPEND` should be set to `>=virtual/jdk-[minimum-version]:*`. If the package does not build from source and is a Java binary, you can use a JRE instead of a JDK in `DEPEND`.

All java packages will require a JRE for `RDEPEND`, and should be set to `>=virtual/jre-[minimum-version]:*`. Unless the package requires a JDK at runtime.

Some packages do not compile with different JDK versions. If you come across such a package, you have to restrict the compilation to the specific JDK version. Running such a package with newer JDK/JRE is usually safe so you can restrict only the JDK version for compilation.

While it is not required, you will commonly see a dependency variable that is used in both `DEPEND` and `RDEPEND`. The variable [`CP_DEPEND`](https://github.com/gentoo/gentoo/blob/b8d4c6c6f7715711e68ea83cffaa208cc70dd7ed/eclass/java-utils-2.eclass#L2936-L2960)  is used for anything that will get added to the classpath and go in both `DEPEND` and `RDEPEND`. This is very common in most if not all Java ebuilds.

### Slots

For Gentoo Java packages, [slots](https://wiki.gentoo.org/wiki/SLOT) are obligatory for every dependency, which also includes packages with [`SLOT="0"`](https://devmanual.gentoo.org/ebuild-writing/variables#slot). These must end in `:0`. Failure to adhere to this can cause issues if a package is later slotted. Portage will pull in the latest version, likely the slotted one, removing the un-slotted version, which will break usage of the dependency in the Java ebuild.

#### Changing Ebuild SLOT

When changing ebuild `SLOT`, the process of doing it is different for Java packages, because `SLOT` change affects the installation path of the package and also content of package.env. For that reason, the correct steps to change an ebuild `SLOT` is to do a revision bump with the new `SLOT` and then update all the affected packages that should depend on the new `SLOT`. `slotmove` (in profiles/updates/) must not be used in this case because it is used only for packages that don't change their installation content during the `SLOT` change.

### Versions

For most Gentoo Java packages you will exclude versions from dependencies, unless you need a specific version of a dependency or a newer version than what might be available. For large Java Applications that are split into multiple packages on Gentoo, it is recommended but not required, to add the version of the package to the dependency, `~category/package-${PV}:${SLOT}`. This ensures other packages for the same application will all be built using the same version, in case there could be potential issues mixing pieces of an application with a different version. By having all packages depend on the version and slot, it ensures this cannot happen. Upstreams will likely not address or respond to bugs or issues that arise from using pieces with other pieces from different versions. Nor is it something Gentoo developers or users should be wasting time on.

Take the following example using [dev-java/bcmail](https://packages.gentoo.org/packages/dev-java/bcmail) which depends on [dev-java/bcpkix](https://packages.gentoo.org/packages/dev-java/bcpkix). Both come from the same sources, thus they need to have [dev-java/bcmail](https://packages.gentoo.org/packages/dev-java/bcmail) depend on a specific version of [dev-java/bcpkix](https://packages.gentoo.org/packages/dev-java/bcpkix) as follows. This would be the depend for [dev-java/bcmail](https://packages.gentoo.org/packages/dev-java/bcmail). The version does not matter, nor does the slot. What matters is that we are passing both from the parent package to its dependency, which comes from the same sources.

## Preparing sources

Gentoo Java ebuilds use the normal `src_unpack()` and `src_prepare()`. Within `src_prepare()`, along with doing the normal patching and other things you might need to do in order to prepare the sources, it is policy and practice to always remove any bundled class and/or jar files that came with the package sources, which is one of the first things you should do for any Java package, regardless of optional Java support or not.

### Removing bundled classes and jars

To remove bundled compiled stuff selectively, use `java-pkg_clean` or `JAVA_RM_FILES`[java-utils-2.eclass](https://devmanual.gentoo.org/eclass-reference/java-utils-2.eclass) . These can also be used together. The `JAVA_PKG_NO_CLEAN``java-pkg_clean`.

### Replacing removed bundled jars

There are some [build systems](https://wiki.gentoo.org/wiki/Build_automation#Available_software) that require jars to be in certain locations. Per Gentoo Java Packaging Policy and the previous section those jars have now been removed. They need to be replaced using symlinks to system jars to let the build system proceed without further modification. This is mostly a legacy way of doing things, as most build systems will look for jars on the classpath.

### Modify build system

There are times when you need to modify the build system. If a project uses a build system, it can happen it calls javac directly instead of calling ejavac or even calls javac with wrong values for the `-source` and `-target` options. If not using `ejavac`, it must be ensured to have them `javac -source "$(java-pkg_get-source)" -target "$(java-pkg_get-target)"`.

## Writing the ebuild using eant

See [Using\_eant](https://wiki.gentoo.org/wiki/Java_Developer_Guide/Using_eant)

## Writing the ebuild using java-pkg-simple.eclass

See [Using java-pkg-simple.eclass](https://wiki.gentoo.org/wiki/Java_Developer_Guide/Using_java-pkg-simple.eclass)

## Typical examples

### Using java-pkg-opt-2.eclass

**`bar-1.0.ebuild`**

**Example of a an ebuild with optional Java support**

```
EAPI=9
inherit java-pkg-opt-2
DESCRIPTION="Fictional example ebuild"
HOMEPAGE="https://www.gentoo.org/"
SRC_URI="mirror://gentoo/${P}.tar.gz"
LICENSE="LGPL-2.1"
SLOT="0"
KEYWORDS="~amd64 ~ia64 ~ppc ~ppc64 ~sparc ~x86"
IUSE="doc nls"
DEPEND="java? ( >=virtual/jdk-1.8:* )"
RDEPEND="java? ( >=virtual/jre-1.8:* )"
PATCHES=( "${FILESDIR}/${P}.patch" )
src_prepare() {
        # Remove bundled jars and .class files
        java-pkg_clean
        java-pkg-opt-2_src_prepare
}
src_compile() {
        local myconf="$(use_enable java)"
        if use java; then
                myconf="${myconf} --with-javac-args=\"$(java-pkg_javac-args)\""
        fi
        econf $(use_enable nls) ${myconf} || die
        emake || die
}
src_install() {
        make install DESTDIR=${D} || die
        if use java; then
                java-pkg_newjar build/java/${P}.jar ${PN}.jar
                if use doc; then
                        java-pkg_dohtml -r doc/java
                fi
        fi
}
```
## Warnings

There are some common warnings that are produced by the java compiler. A few are Gentoo specific and others are general. Some are of concern, and others are not.

### Bootstrap class path

Anytime the source version is lower than the current java version you will see the following warning.

While this is a generic warning, it comes from a Gentoo specific issue. Anytime the source version is lower than the current java version, a rt.jar from that older version needs to be set as the bootstrap classpath. This is presently not done on Gentoo, thus the warning.

It is safe to ignore, providing you do not try to run the resulting java binaries on a version of java older than that which it was compiled on. That does somewhat defeat the purpose of using a lower source but sometimes that is necessary for a variety of other reasons.
