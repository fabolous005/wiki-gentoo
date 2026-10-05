<!-- source: https://wiki.gentoo.org/wiki/Kotlin/Library_Package_Maintainer_Guide | group: Gentoo Wiki (Main) | wiki-title: Kotlin/Library Package Maintainer Guide -->
---
title: Kotlin/Library Package Maintainer Guide
url: https://wiki.gentoo.org/wiki/Kotlin/Library_Package_Maintainer_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-25"
fingerprint: "2d017a094ef32208"
license: CC BY-SA 4.0
---

# Kotlin/Library Package Maintainer Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page contains useful information for any maintainers of the [Kotlin library packages](https://wiki.gentoo.org/wiki/Kotlin#Available_packages)' ebuilds.

## Eclasses

The following eclasses are provided to facilitate creation of ebuilds for Kotlin libraries:

- [kotlin-libs.eclass](https://github.com/6-6-6/spark-overlay/blob/master/eclass/kotlin-libs.eclass)
- The eclass designed for building general Kotlin library packages, with support for compiling additional Java sources, JAR generation, source archive creation, JUnit 4 tests, installing pre-built binary JAR, and checks based on [dev-util/pkgdiff](https://packages.gentoo.org/packages/dev-util/pkgdiff) and [dev-util/japi-compliance-checker](https://packages.gentoo.org/packages/dev-util/japi-compliance-checker).
- [kotlin-core-deps.eclass](https://github.com/6-6-6/spark-overlay/blob/master/eclass/kotlin-core-deps.eclass)
- An eclass based on kotlin-libs.eclass for building modules under the [core directory](https://github.com/JetBrains/kotlin/tree/master/core) in the upstream project's source tree.  The directory consists of reflection.jvm, which is the major part of kotlin-reflect, and various other modules which are both required by reflection.jvm during build time and parts of kotlin-reflect.  The modules correspond to the `:core:*` subprojects in the main Gradle project.  This is a **specialized** eclass which is **not intended to be used by a general Kotlin library package's ebuild**.

## Obtaining compiler arguments for a library package

The compiler options for a Kotlin library package can be obtained using [the same method as for third-party Kotlin packages](https://wiki.gentoo.org/wiki/Kotlin/Package_Maintainer_Guide#Obtaining_Kotlin_compiler_arguments_for_a_package) with some minor variations.

### Prepare the project's source tree

The [Kotlin programming language project](https://github.com/JetBrains/kotlin)'s source tree can either be downloaded as a Zip archive or a tarball storing it or be cloned using [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git).  If Git is used, the following cloning options are recommended to minimize the download size by pulling objects for only one commit for a single tag (`v1.6.10` in this example):

Before any ./gradlew command is run, a few extra steps are needed to fix some potential issues. Enter the project's source root and perform the following steps.

First, the project needs to be configured to skip probing JDK 6 and 7. Otherwise, the following error might be raised:

```
FAILURE: Build failed with an exception.
* Where:
Build file '/home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/build.gradle' line: 88
* What went wrong:
A problem occurred evaluating project ':kotlin-stdlib'.
> Could not resolve all dependencies for configuration ':kotlin-stdlib:commonSources'.
   > Failed to calculate the value of task ':kotlin-stdlib:compileJava' property 'javaCompiler'.
      > No compatible toolchains found for request filter: {languageVersion=6, vendor=any, implementation=vendor-specific} (auto-detect true, auto-download true)
* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output. Run with --scan to get full insights.
* Get more help at 
```
[https://help.gradle.org](https://help.gradle.org)
Deprecated Gradle features were used in this build, making it incompatible with Gradle 7.0.
Use '--warning-mode all' to show the individual deprecation warnings.
See [https://docs.gradle.org/6.9/userguide/command_line_interface.html#sec:command_line_warnings](https://docs.gradle.org/6.9/userguide/command_line_interface.html#sec:command_line_warnings)
BUILD FAILED in 10s
To avoid this error, add `kotlin.build.isObsoleteJdkOverrideEnabled=true` to local.properties<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>:

`user $``echo 'kotlin.build.isObsoleteJdkOverrideEnabled=true' >> local.properties`
Next, it is recommended that Gradle build cache is disabled, although this is not a requirement. The build cache's purpose is to avoid compiling the same source file twice even if ./gradlew clean has been run, but this behavior is undesired if the compiler arguments to build a set of source files need to be collected after the files have been compiled at least once because it will prevent the compiler arguments from being included in the Gradle debug logs.

To disable Gradle build cache, add `org.gradle.caching=false` to local.properties:

`user $``echo 'org.gradle.caching=false' >> local.properties`
### Find out the Gradle compilation tasks for Kotlin library packages

The list of tasks for building all Kotlin compiler and library modules can be found from the output of ./gradlew assemble --dry-run, just like for third-party Kotlin packages. However, many Kotlin library packages consist of both Kotlin and Java source files, and they are covered by different compilation tasks.

`user $``./gradlew assemble --dry-run`
:assemble SKIPPED
:core:builtins:prepareSources SKIPPED
:core:builtins:serialize SKIPPED
:core:builtins:builtinsJar SKIPPED
:kotlin-annotations-jvm:compileKotlin SKIPPED
:kotlin-annotations-jvm:compileJava SKIPPED
:kotlin-annotations-jvm:processResources SKIPPED
:kotlin-annotations-jvm:classes SKIPPED
:kotlin-annotations-jvm:inspectClassesForKotlinIC SKIPPED
:kotlin-annotations-jvm:jar SKIPPED
:prepare:build.version:writeStdlibVersion SKIPPED
:kotlin-stdlib-common:compileKotlinCommon SKIPPED
:kotlin-stdlib-common:processResources SKIPPED
:kotlin-stdlib-common:classes SKIPPED
:kotlin-stdlib-common:inspectClassesForKotlinIC SKIPPED
:kotlin-stdlib-common:jar SKIPPED
:kotlin-stdlib:compileKotlin SKIPPED
:kotlin-stdlib:compileJava SKIPPED
:kotlin-stdlib:processResources SKIPPED
:kotlin-stdlib:classes SKIPPED
:kotlin-stdlib:compileJava9Kotlin SKIPPED
:kotlin-stdlib:inspectClassesForKotlinIC SKIPPED
:kotlin-stdlib:compileJava9Java SKIPPED
:kotlin-stdlib:processJava9Resources SKIPPED
:kotlin-stdlib:java9Classes SKIPPED
:kotlin-stdlib:jar SKIPPED
:core:util.runtime:compileKotlin SKIPPED
:core:util.runtime:compileJava SKIPPED
:core:util.runtime:processResources SKIPPED
:core:util.runtime:classes SKIPPED
:core:util.runtime:inspectClassesForKotlinIC SKIPPED
:core:util.runtime:jar SKIPPED
...

**Both** the Kotlin source files and the Java source files need to be compiled.  These are covered by a single `:classes` task, which can be run with ./gradlew --debug to get compiler options for both set of source files.

`user $``./gradlew :kotlin-stdlib:classes --debug | tee /tmp/kotlin-stdlib-classes.log``user $``grep -B2 'Kotlin compiler args:' /tmp/kotlin-stdlib-classes.log``user $``grep 'NormalizingJavaCompiler' /tmp/kotlin-stdlib-classes.log`
## Kotlin compiler classes and wrappers

The [standalone Kotlin command-line compiler](https://kotlinlang.org/docs/command-line.html) packaged and distributed by the upstream (kotlin-compiler-1.x.y.zip) contains the following components for the compiler proper:

- kotlin-compiler.jar, the main compiler module
- [Various kotlinc\* compiler wrapper executables](https://github.com/JetBrains/kotlin/tree/v1.5.20/compiler/cli/bin)
- Other libraries used by the compiler

A compiler wrapper may set the `KOTLIN_COMPILER` environment variable to the name of the compiler class to be used<sup>[\[2\]](https://wiki.gentoo.org#cite_note-kotlin-js-2)</sup>, and the environment variable will be read by kotlinc, the main Kotlin compiler wrapper<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>.

Here is a list of classes in kotlin-compiler.jar that are used when Gradle builds the Kotlin libraries:

- org.jetbrains.kotlin.cli.jvm.K2JVMCompiler
- Takes Kotlin sources as input, and produces Java class files as output.  This is the default compiler class if `KOTLIN_COMPILER` is unset<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>, and it is also the compiler class used in building all the JVM libraries.
- org.jetbrains.kotlin.cli.js.K2JSCompiler
- Takes Kotlin sources as input, and produces JavaScript files as output.  This is the compiler class for kotlin-js<sup>[\[2\]](https://wiki.gentoo.org#cite_note-kotlin-js-2)</sup>.
- org.jetbrains.kotlin.cli.js.internal.JSStdlibLinker
- Processes JavaScript sources.  Only used in building kotlin-stdlib-js<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup>.
- org.jetbrains.kotlin.serialization.builtins.RunKt
- The built-in serializer, which takes Kotlin sources as input and produces \*.kotlin\_builtins files as output.  Only used in building `:core:builtins`<sup>[\[6\]](https://wiki.gentoo.org#cite_note-6)</sup>.

### Command-line interfaces of Kotlin compiler classes

Different Kotlin compiler classes recognize different sets of options.  Some recognize the [standard options listed in the official documentation](https://kotlinlang.org/docs/compiler-reference.html); others do not.

The following compiler classes recognize the standard options:

- org.jetbrains.kotlin.cli.jvm.K2JVMCompiler
- org.jetbrains.kotlin.cli.js.K2JSCompiler

The following compiler classes have a command-line interface that is not compatible with the standard options:

- org.jetbrains.kotlin.cli.js.internal.JSStdlibLinker
- org.jetbrains.kotlin.serialization.builtins.RunKt

To see if a compiler class complies with the common Kotlin compiler options, try to set `KOTLIN_COMPILER` to that class and run kotlinc -help, then see what options the compiler class accepts.  If no common compiler option is shown at all in the help message or an exception is even thrown, then the compiler class is likely to not comply with the common compiler options.

`user $``KOTLIN_COMPILER=org.jetbrains.kotlin.cli.js.K2JSCompiler kotlinc -help````
Usage: kotlinc-js <options> <source files>
where possible options include:
  -libraries <path>          Paths to Kotlin libraries with .meta.js and .kjsm files, separated by system path separator
  -main {call|noCall}        Define whether the `main` function should be called upon execution
  -meta-info                 Generate .meta.js and .kjsm files with metadata. Use to create a library
  -module-kind {plain|amd|commonjs|umd}
                             Kind of the JS module generated by the compiler
  -no-stdlib                 Don't automatically include the default Kotlin/JS stdlib into compilation dependencies
  -output <filepath>         Destination *.js file for the compilation result
  -output-postfix <path>     Add the content of the specified file to the end of output file
  -output-prefix <path>      Add the content of the specified file to the beginning of output file
  -source-map                Generate source map
  -source-map-base-dirs <path> Base directories for calculating relative paths to source files in source map
  -source-map-embed-sources {always|never|inlining}
                             Embed source files into source map
  -source-map-prefix         Add the specified prefix to paths in the source map
  -target { v5 }             Generate JS files for specific ECMA version
  -Werror                    Report an error if there are any warnings
  -api-version <version>     Allow using declarations only from the specified version of bundled libraries
  -X                         Print a synopsis of advanced options
  -help (-h)                 Print a synopsis of standard options
  -kotlin-home <path>        Path to the home directory of Kotlin compiler used for discovery of runtime libraries
  -language-version <version> Provide source compatibility with the specified version of Kotlin
  -P plugin:<pluginId>:<optionName>=<value>
                             Pass an option to a plugin
  -progressive               Enable progressive compiler mode.
                             In this mode, deprecations and bug fixes for unstable code take effect immediately,
                             instead of going through a graceful migration cycle.
                             Code written in the progressive mode is backward compatible; however, code written in
                             non-progressive mode may cause compilation errors in the progressive mode.
  -script                    Evaluate the given Kotlin script (*.kts) file
  -nowarn                    Generate no warnings
  -verbose                   Enable verbose logging output
  -version                   Display compiler version
  @<argfile>                 Read compiler arguments and file paths from the given file
For details, see https://kotl.in/cli
```
`user $``KOTLIN_COMPILER=org.jetbrains.kotlin.serialization.builtins.RunKt kotlinc -help`
Kotlin built-ins serializer
Usage: ... \<destination dir> (\<source dir>)+
Analyzes Kotlin sources found in the given source directories and serializes
found top-level declarations to \<destination dir> (\*.kotlin\_builtins files)

`user $``KOTLIN_COMPILER=org.jetbrains.kotlin.cli.js.internal.JSStdlibLinker kotlinc -help`
Exception in thread "main" java.lang.reflect.InvocationTargetException
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.jetbrains.kotlin.preloading.Preloader.run(Preloader.java:87)
	at org.jetbrains.kotlin.preloading.Preloader.main(Preloader.java:44)
Caused by: java.lang.ArrayIndexOutOfBoundsException: 1
	at org.jetbrains.kotlin.cli.js.internal.JSStdlibLinker.main(JSStdlibLinker.kt:25)
	... 6 more

## Creating an ebuild based on kotlin-libs.eclass

kotlin-libs.eclass is based on kotlin-utils.eclass and java-pkg-simple.eclass, so it recognizes and reuses many variables of those eclasses. Yet kotlin-libs.eclass supports a few new variables, which are introduced in this section.

### Allow pre-built binary JAR to be used

The upstream hosts pre-built binary JAR artifacts for many libraries on Maven Central.  To allow the pre-built JAR to be installed for an ebuild, specify the URI to the JAR in `KOTLIN_LIBS_BINJAR_SRC_URI` **before** inheriting kotlin-libs, and set `JAVA_BINJAR_FILENAME` to the base name of the JAR anywhere in the ebuild.

Specifying a non-empty value for `KOTLIN_LIBS_BINJAR_SRC_URI` has the following effects:

- A `binary` `USE` flag will be automatically added for the ebuild.  When this `USE` flag is enabled, the package will not be compiled and installed from source; the pre-built JAR pointed by `KOTLIN_LIBS_BINJAR_SRC_URI` will be installed instead.

- If the `binary` `USE` flag is *disabled*, then during `src_test`, the JAR created by the ebuild will be compared with the pre-built JAR pointed by `KOTLIN_LIBS_BINJAR_SRC_URI` for non-trivial difference in JAR contents and incompatibility in API.  This is equivalent to specifying `KOTLIN_TESTING_FRAMEWORKS+=" pkgdiff"`.

- Another variable, `KOTLIN_LIBS_SRCJAR_SRC_URI`, will be recognized by kotlin-libs.eclass.  If `KOTLIN_LIBS_SRCJAR_SRC_URI` has a non-empty value that is set **before** kotlin-libs is inherited, then the `source` `USE` flag can be set when the `binary` `USE` flag is enabled.  With `USE="binary source"`, the ebuild will pull the file pointed by `KOTLIN_LIBS_SRCJAR_SRC_URI` and install it as the source archive for the package at /usr/share/${PN}-${SLOT}/sources.
  - If `KOTLIN_LIBS_SRCJAR_SRC_URI` has a non-empty value, then another variable, `KOTLIN_LIBS_SRCJAR_FILENAME`, **must** be set to the base name of the file pointed by `KOTLIN_LIBS_SRCJAR_SRC_URI`.

### Find and declare dependencies

kotlin-libs.eclass automatically inserts dependency on the Kotlin compiler for the same Kotlin feature release to `DEPEND`.  If the ebuild supports the `binary` `USE` flag, then the Kotlin compiler will be pulled only if any Kotlin sources need to be compiled.

The library dependencies of a Kotlin library package can be  [found](https://wiki.gentoo.org/wiki/Kotlin/Package_Maintainer_Guide#-classpath) and [trimmed](https://wiki.gentoo.org/wiki/Kotlin/Package_Maintainer_Guide#Pruning_dependency_list) in the same method as for third-party Kotlin packages.

A Kotlin library package may also depend on other Kotlin library packages.  It is recommended to declare dependency on the **same version** of them.

If kotlin-stdlib-common-\*.jar appears in a package's classpath, it can be ignored because it provides merely .kotlin\_metadata files, which are not required during compilation.

### Define Kotlin runtime component

For most Kotlin library packages, the META-INF/MANIFEST.MF file in the JAR built by Gradle contains a line like the following:

**`META-INF/MANIFEST.MF`**

**Definition of Kotlin runtime component**

Possible values for this field include `Main` for most Kotlin library packages and `Test` for kotlin.test library modules (i.e. the dev-java/kotlin-test\* packages).

ebuilds for Kotlin library packages should use the `KOTLIN_LIBS_RUNTIME_COMPONENT` variable to define the proper value of `Kotlin-Runtime-Component`.  kotlin-libs.eclass will automatically insert a line defining it into META-INF/MANIFEST.MF in the JAR produced by `kotlin-libs_src_compile`.

If there is no such entry in META-INF/MANIFEST.MF inside the JAR built by Gradle, or META-INF/MANIFEST.MF does not exist in the JAR at all, then `KOTLIN_LIBS_RUNTIME_COMPONENT` needs not be set.

### Use a non-default Kotlin compiler class

kotlin-libs.eclass does not have a variable for setting `KOTLIN_COMPILER` before kotlinc is invoked, so the default compiler class for kotlinc, which is org.jetbrains.kotlin.cli.jvm.K2JVMCompiler, will be used.  However, ebuilds can still use a non-default compiler class using one of the following methods, depending on which specific compiler class should be used.

If the compiler class [recognizes the standard options](https://wiki.gentoo.org#Command-line_interfaces_of_Kotlin_compiler_classes), ebuilds can directly set the `KOTLIN_COMPILER` environment variable before `kotlin-libs_src_compile` is called.  For example, the following code changes the Kotlin compiler class used by kotlin-libs.eclass to org.jetbrains.kotlin.cli.js.K2JSCompiler:

If the compiler class does not recognize the standard options, Gradle will run it by directly calling java.  This is shown in the output of ./gradlew --debug as well, and log messages containing the arguments to java can be searched via occurrences of string `Starting process`.  `:core:builtins` is an example whose build process involves a java invocation:

`user $``grep "Starting process" /tmp/kotlin-core-builtins-classes.log`
2022-02-06T13:54:23.186-0800 \[INFO\] \[org.gradle.process.internal.DefaultExecHandle\] Starting process 'command '/usr/lib64/openjdk-8/bin/java''. Working directory: /home/leo/Projects/forks/kotlin/core/builtins Command: /usr/lib64/openjdk-8/bin/java -Didea.io.use.nio2=true -Dkotlin.builtins.serializer.log=true -Dfile.encoding=UTF-8 -Duser.country=US -Duser.language=en -Duser.variant -cp /home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-compiler-embeddable/1.6.0-dev-3496/9de84f765b592f1383b9b492dd7cfa331dc06a85/kotlin-compiler-embeddable-1.6.0-dev-3496.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-reflect/1.6.0-dev-3496/ee01d7cc516015d118d130002f947b505e0b8297/kotlin-reflect-1.6.0-dev-3496.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib/1.6.0-dev-3496/4048dff0dad97a166ec61d18d1d9f62ca4ce5d7d/kotlin-stdlib-1.6.0-dev-3496.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-script-runtime/1.6.0-dev-3496/8021eb367c0afaf9cdebd1ed3463e4997a5f62e6/kotlin-script-runtime-1.6.0-dev-3496.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-daemon-embeddable/1.6.0-dev-3496/f54154004eb6d8ee537c54d14097be89cd2e6295/kotlin-daemon-embeddable-1.6.0-dev-3496.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.intellij.deps/trove4j/1.0.20181211/216c2e14b070f334479d800987affe4054cd563f/trove4j-1.0.20181211.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/net.java.dev.jna/jna/5.6.0/330f2244e9030119ab3030fc3fededc86713d9cc/jna-5.6.0.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains/annotations/13.0/919f0dfe192fb4e063e7dacadee7f8bb9a2672a9/annotations-13.0.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib-common/1.6.0-dev-3496/378861a801abe7fa88dc7aafc8892f8dd743570f/kotlin-stdlib-common-1.6.0-dev-3496.jar org.jetbrains.kotlin.serialization.builtins.RunKt build/serialize src native build/src

In this case, in the ebuild, the Kotlin compiler class should be started using java too.  To use the compiler class, kotlin-compiler.jar should be included in the classpath of the JVM started by the java invocation.  The full path to kotlin-compiler.jar is ${KOTLIN\_UTILS\_COMPILER\_HOME}/lib/kotlin-compiler.jar, where `KOTLIN_UTILS_COMPILER_HOME` is an output variable of kotlin-utils.eclass that will contain the path where the Kotlin compiler used to build the package is installed.

## Topological ordering of Kotlin library and compiler packages' dependency graph

The [topological ordering](https://en.wikipedia.org/wiki/Topological_sorting) of Kotlin library and compiler packages' dependency graph is useful when a new upstream release of them is to be added and when an obsolete version of them is to be removed.  When a new release is added, it is recommended that the packages are bumped in the topological ordering.  When an obsolete version is removed, the packages should be cleaned up in the reverse ordering instead.

- dev-java/kotlin-stdlib-bootstrap
- dev-java/kotlin-reflect-bootstrap
- dev-lang/kotlin-bin
- virtual/kotlin
- dev-java/kotlin-core-builtins
- dev-java/kotlin-annotations-jvm
- dev-java/kotlin-stdlib
- dev-java/kotlin-test
- dev-java/kotlin-test-junit
- dev-java/kotlin-core-util-runtime
- dev-java/kotlin-core-compiler-common
- dev-java/kotlin-core-compiler-common-jvm
- dev-java/kotlin-core-descriptors
- dev-java/kotlin-core-metadata
- dev-java/kotlin-core-deserialization-common
- dev-java/kotlin-core-deserialization
- dev-java/kotlin-core-metadata-jvm
- dev-java/kotlin-core-deserialization-common-jvm
- dev-java/kotlin-core-descriptors-jvm
- dev-java/kotlin-core-descriptors-runtime
- dev-java/kotlin-reflect
- dev-java/kotlin-stdlib-jdk7
- dev-java/kotlin-stdlib-jdk8
- dev-java/kotlin-stdlib-js
- dev-java/kotlin-test-js

## Adding a new Kotlin feature release

The following steps should be performed when packages and support for a new Kotlin feature release are being added to [the Spark overlay](https://github.com/6-6-6/spark-overlay), in order:

1. Bump dev-java/kotlin-stdlib-bootstrap and dev-java/kotlin-reflect-bootstrap to the new feature release. These are essentially binary packages whose version bump is trivial.
2. Check the version of dev-java/kotlinx-coroutines-core-bin needed by the Kotlin compiler for the new feature release, which is declared in build.gradle.kts under the source tree of the Kotlin programming language project.  The version is assigned to `extra["versions.kotlinx-coroutines-core"]` in the file<sup>[\[7\]](https://wiki.gentoo.org#cite_note-7)</sup>.
  1. If the version has not changed since the latest version of the previous feature release:
    1. Revision-bump dev-java/kotlinx-coroutines-core-bin.
    2. In `src_install`, register dev-java/kotlin-stdlib for the new feature release as an optional dependency.  An example is provided below.
  2. Otherwise:
    1. Bump dev-java/kotlinx-coroutines-core-bin to the new version.
    2. Create a new slot for it, so any Kotlin compiler packages for older feature releases can still use the older version.
    3. In `src_install`, register dev-java/kotlin-stdlib for **only** the new feature release as an optional dependency.
3. Bump dev-lang/kotlin-bin to the new feature release. If the file hierarchy of the standalone Kotlin command-line compiler's Zip archive changes, the ebuild might need to be updated.
4. Bump virtual/kotlin for the new feature release.
5. Add the identifier for the new feature release to the value of `_KOTLIN_UTILS_ALL_VERSIONS` variable in [kotlin-utils.eclass](https://github.com/6-6-6/spark-overlay/blob/master/eclass/kotlin-utils.eclass).
6. Add description of the `KOTLIN_SINGLE_TARGET USE_EXPAND` flag for the new feature release to [profiles/desc/kotlin\_single\_target.desc](https://github.com/6-6-6/spark-overlay/blob/master/profiles/desc/kotlin_single_target.desc).
7. Bump the Kotlin library packages to the new feature release.  Compiler arguments should be updated to allow the packages to build.
  1. Compiler arguments for dev-java/kotlin-core-\* ebuilds are defined in [kotlin-core-deps.eclass](https://github.com/6-6-6/spark-overlay/blob/master/eclass/kotlin-core-deps.eclass); please remember to update the eclass as well.
8. Update Kotlin ebuild test cases in the Spark overlay under [tests/test-cases](https://github.com/6-6-6/spark-overlay/tree/master/tests/test-cases) for the new feature release, and enable the updated test cases in [.github/workflows/docker.yml](https://github.com/6-6-6/spark-overlay/blob/master/.github/workflows/docker.yml), which defines the GitHub Actions workflow that runs them.

## Retiring a Kotlin feature release from `KOTLIN_COMPAT`

When a Kotlin feature release is getting too old, it could be retired from the set of feature releases whose identifier in `KOTLIN_COMPAT` is recognized.  After this action, third-party Kotlin ebuilds can no longer be built with Kotlin compilers for the feature release in question.

To retire a Kotlin feature release from `KOTLIN_COMPAT`, in kotlin-utils.eclass, remove the identifier for it from `_KOTLIN_UTILS_ALL_VERSIONS`, which stops the identifier from being picked up by kotlin-utils.eclass when the value of `KOTLIN_COMPAT` is processed.

It might be more desirable to keep the Kotlin compiler and library packages for the feature release for a while even after the feature release has been retired from `KOTLIN_COMPAT`, so users would still be able to use the feature release outside Portage.  To allow Kotlin library packages for the feature release in question to be installed even if the feature release has been retired from `KOTLIN_COMPAT`, in kotlin-utils.eclass, add the identifier for it to `_KOTLIN_UTILS_LIBS_ONLY_VERSIONS`.

**`kotlin-utils.eclass`**

**Retire Kotlin 1.4 from`KOTLIN_COMPAT`, but exempt Kotlin library packages from the change**

```
_KOTLIN_UTILS_ALL_VERSIONS=( kotlin1-{5..6} )
readonly _KOTLIN_UTILS_ALL_VERSIONS
_KOTLIN_UTILS_LIBS_ONLY_VERSIONS=( kotlin1-4 )
readonly _KOTLIN_UTILS_LIBS_ONLY_VERSIONS
```
## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [ReadMe.md/Build environment requirements](https://github.com/JetBrains/kotlin/tree/v1.6.10#build-environment-requirements), [JetBrains/kotlin GitHub repository](https://github.com/JetBrains/kotlin), July 21st, 2021.  Retrieved on February 5th, 2022.
2. ↑ <sup>[2.0](https://wiki.gentoo.org#cite_ref-kotlin-js_2-0)</sup> <sup>[2.1](https://wiki.gentoo.org#cite_ref-kotlin-js_2-1)</sup> [compiler/cli/bin/kotlinc-js](https://github.com/JetBrains/kotlin/blob/v1.5.20/compiler/cli/bin/kotlinc-js#L6), [JetBrains/kotlin GitHub repository](https://github.com/JetBrains/kotlin), January 7th, 2021.  Retrieved on July 10th, 2021.
3. [↑](https://wiki.gentoo.org#cite_ref-3) [compiler/cli/bin/kotlinc (Line 95)](https://github.com/JetBrains/kotlin/blob/v1.5.20/compiler/cli/bin/kotlinc#L95), [JetBrains/kotlin GitHub repository](https://github.com/JetBrains/kotlin), February 21st, 2021.  Retrieved on July 10th, 2021.
4. [↑](https://wiki.gentoo.org#cite_ref-4) [compiler/cli/bin/kotlinc (Line 84)](https://github.com/JetBrains/kotlin/blob/v1.5.20/compiler/cli/bin/kotlinc#L84), [JetBrains/kotlin GitHub repository](https://github.com/JetBrains/kotlin), February 21st, 2021.  Retrieved on July 10th, 2021.
5. [↑](https://wiki.gentoo.org#cite_ref-5) [libraries/stdlib/js-v1/build.gradle](https://github.com/JetBrains/kotlin/blob/v1.5.20/libraries/stdlib/js-v1/build.gradle#L178), [JetBrains/kotlin GitHub repository](https://github.com/JetBrains/kotlin), April 17th, 2021.  Retrieved on July 10th, 2021.
6. [↑](https://wiki.gentoo.org#cite_ref-6) [core/builtins/build.gradle.kts](https://github.com/JetBrains/kotlin/blob/v1.5.20/core/builtins/build.gradle.kts#L64), [JetBrains/kotlin GitHub repository](https://github.com/JetBrains/kotlin), March 5th, 2021.  Retrieved on July 10th, 2021.
7. [↑](https://wiki.gentoo.org#cite_ref-7) [build.gradle.kts](https://github.com/JetBrains/kotlin/blob/v1.5.20/build.gradle.kts#L179), [JetBrains/kotlin GitHub repository](https://github.com/JetBrains/kotlin), April 30th, 2021.  Retrieved on February 10th, 2022.
