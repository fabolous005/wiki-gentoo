<!-- source: https://wiki.gentoo.org/wiki/Kotlin/Package_Maintainer_Guide | group: Gentoo Wiki (Main) | wiki-title: Kotlin/Package Maintainer Guide -->
---
title: Kotlin/Package Maintainer Guide
url: https://wiki.gentoo.org/wiki/Kotlin/Package_Maintainer_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-25"
fingerprint: "2d00fa190fb3a30c"
license: CC BY-SA 4.0
---

# Kotlin/Package Maintainer Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide contains information for people interested in creating and maintaining ebuilds for packages that use Kotlin.  Maintainers of Kotlin library packages such as dev-java/kotlin-stdlib and dev-java/kotlin-reflect should also consult the [Kotlin library package maintainer guide](https://wiki.gentoo.org/wiki/Kotlin/Library_Package_Maintainer_Guide).

## The Kotlin ecosystem

Many Kotlin projects use Gradle as the build system, which is the first obstacle to build packages for those projects with Portage.  As of February 2022, Gradle is [not integrated with Portage](https://wiki.gentoo.org/wiki/Gentoo_Java_Packing_Policy#Gradle) yet, and there is not an eclass that can be used to build and package artifacts for a Gradle project easily.  Invoking Gradle directly in an ebuild is not feasible because Gradle downloads project dependencies from the Internet on its own, requiring `network-sandbox` to be disabled in Portage's `FEATURES` and preventing any dependencies installed by Portage to be reused<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, so it is against the best practices.

Yet it is still possible to build most Gradle-based Kotlin projects without Gradle thanks to the feasibility of reverse engineering and the [standalone Kotlin command-line compiler](https://kotlinlang.org/docs/command-line.html).  Those Gradle-based Kotlin projects use the Kotlin Gradle plugin, which calls the Kotlin compiler to compile Kotlin source files.  The plugin prints the Kotlin compiler arguments it would use to Gradle debug logs controlled by `gradle --debug`, which are [standard Kotlin compiler options](https://kotlinlang.org/docs/compiler-reference.html#compiler-options) recognized by the standalone compiler.  In general, invoking the standalone compiler using the same set of compiler arguments the Kotlin Gradle plugin would use produces the same results.

Gentoo packages for the standalone compiler and Kotlin eclasses are provided to support this approach of building Kotlin packages within Portage. The eclasses provide an easy-to-use interface to the standalone compiler. By following this guide, the set of Kotlin compiler arguments required to build a Kotlin project can be discovered quickly, and an ebuild for the project can thus be created.

The current framework for Kotlin support on Gentoo focuses on the Kotlin/JVM platform, which is the most widely used and the most mature target platform for computer applications among all [supported platforms](https://kotlinlang.org/docs/mpp-supported-platforms.html).  When a Kotlin project is built for the Kotlin/JVM platform, it can be well-integrated with other Java packages and JVM to augment the Java ecosystem on Gentoo.  A Java package on Gentoo can use a Kotlin package as a dependency like if it were a normal Java library.

## Eclasses

The following eclasses are available for packages that use Kotlin:

- [kotlin-utils.eclass](https://github.com/6-6-6/spark-overlay/blob/master/eclass/kotlin-utils.eclass)
- The eclass that provides the backbone for any eclasses and ebuilds that need to use the Kotlin compiler. It does not export any phases; rather, it provides various helper functions for decomposed steps to build a Kotlin package.
- This eclass is suitable for packages whose Kotlin part is small and not the major part, like [Reactor Core 3.4.14](https://github.com/reactor/reactor-core/tree/v3.4.14), which is a Java package with 415 Java source files and only 4 Kotlin source files.
- [kotlin.eclass](https://github.com/6-6-6/spark-overlay/blob/master/eclass/kotlin.eclass)
- The eclass for packages written entirely or mostly in Kotlin.  It exports various `src_*` phase functions that call the helper functions in kotlin-utils.eclass, so by inheriting this eclass, ebuilds need not call those helper functions on their own and do not need to define `src_*` phase functions at all if the default behaviors suffice.
- This eclass adds dependency on the Kotlin compiler by default. If the Kotlin compiler is not always needed, kotlin-utils.eclass should be used instead.

These eclasses all inherit java-pkg-simple.eclass because they use the same approach as java-pkg-simple.eclass for building a package targeting JVM: the build system employed by the upstream is circumvented if it is not integrated with Portage, and command-line tools are invoked directly to simulate the build steps performed by the build system.

There are some other kotlin-\*.eclass files that are generally not needed by normal Kotlin packages; those eclasses are mainly for Kotlin library and compiler packages.

## Obtaining Kotlin compiler arguments for a package

The most important step for creating a new Kotlin package is to find out the Kotlin compiler arguments that the Kotlin Gradle plugin would use to build the project. After the arguments have been collected, an ebuild can be written fairly quickly.

To summarize, the method of collecting the Kotlin compiler arguments is to build the project using Gradle with the `--debug` option and search for the arguments in the output.  As Gradle debug logs are very verbose, finding the arguments in the output is not a trivial task.  This section will introduce strategies that make this task easier.

An example of creating a package for a real-world Kotlin project will be presented below to demonstrate the entire procedure.  The package being created in the example is [kotlinx-cli 0.3.4](https://github.com/Kotlin/kotlinx-cli/tree/v0.3.4), which uses Gradle as the build system.

### Use a suitable version of JDK to run Gradle

As of February 2022, the recommended JDK versions for running Gradle are **JDK 8 and 11**; JDK 17 is not recommended for compatibility reasons.  Gradle has started to support JDK 17 only since version 7.3<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>, which was released in November 2021.  Many Gradle-based projects are still using a Gradle version older than it, for which JDK 17 cannot be used.

### Prepare the project's source tree

To build the project, its entire source tree, including the Gradle build script build.gradle\*, needs to be downloaded. The source tree can either be unpacked from an archive or cloned using a version control system. In the latter case, make sure the version being packaged is checked out, unless a live ebuild is to be created.

If the project uses the Gradle Wrapper, using the wrapper is recommended so the exact Gradle version the upstream uses will be run. To find out if the Gradle Wrapper is used, check if the source tree has a gradlew script in its root directory. The project being packaged in this example uses the Gradle Wrapper, thus it will be invoked in the following steps.

### Find out the Gradle compilation task for Kotlin/JVM

Gradle usually runs dozens of tasks to compile all sources of the project for the `assemble` [lifecycle task](https://docs.gradle.org/current/userguide/base_plugin.html#sec:base_tasks), which causes the Gradle debug logs to contain too much information.  Usually, only one of the tasks will invoke the Kotlin compiler to generate classes for Kotlin/JVM.  By running only this task with the `--debug` option, the amount of debug log messages to search for is greatly reduced.

Projects that target multiple platforms (including the project being packaged in this example) have multiple tasks that will invoke the Kotlin compiler, and each of them generates the classes for only one platform. In this case, only the task for the Kotlin/JVM platform should be run; tasks for Kotlin/JS, Kotlin/Native, Kotlin for Android etc. should be ignored.

To check the compilation task Kotlin/JVM, run ./gradlew assemble --dry-run to get a list of all tasks that would be executed for the `assemble` lifecycle task, and then find out the proper task by using task names as clues.

`user $``./gradlew assemble --dry-run````
Starting a Gradle Daemon (subsequent builds will be faster)
> Configure project :
INFRA: Sonatype publishing will not be possible due to missing staging repository id 'libs.repository.id'. Pass 'auto' for implicit staging.
> Configure project :kotlinx-cli
Kotlin Multiplatform Projects are an Alpha feature. See: https://kotlinlang.org/docs/reference/evolution/components-stability.html. To hide this message, add 'kotlin.mpp.stability.nowarn=true' to the Gradle properties.
Warning: Kotlin language settings function 'useExperimentalAnnotation' is deprecated and will be removed in next major releases. Please, use 'optIn' instead.
Some Kotlin/Native targets cannot be built on this linux_x64 machine and are disabled:
    * In project ':kotlinx-cli':
        * targets 'macosX64', 'macosArm64' (can be built with one of the hosts: macos_x64, macos_arm64)
To hide this message, add 'kotlin.native.ignoreDisabledTargets=true' to the Gradle properties.
:assemble SKIPPED
:kotlinx-cli:compileKotlinLinuxX64 SKIPPED
:kotlinx-cli:compileKotlinMacosArm64 SKIPPED
:kotlinx-cli:compileKotlinMacosX64 SKIPPED
:kotlinx-cli:compileKotlinMingwX64 SKIPPED
:kotlinNodeJsSetup SKIPPED
:kotlinYarnSetup SKIPPED
:kotlinNpmCachesSetup SKIPPED
:kotlinx-cli:jsIrPackageJson SKIPPED
:kotlinx-cli:jsLegacyPackageJson SKIPPED
:rootPackageJson SKIPPED
:kotlinNpmInstall SKIPPED
:kotlinx-cli:jsIrGenerateExternalsIntegrated SKIPPED
:kotlinx-cli:compileKotlinJsIr SKIPPED
:kotlinx-cli:jsIrProcessResources SKIPPED
:kotlinx-cli:jsIrMainClasses SKIPPED
:kotlinx-cli:jsIrPublicPackageJson SKIPPED
:kotlinx-cli:jsIrJar SKIPPED
:kotlinx-cli:compileKotlinJsLegacy SKIPPED
:kotlinx-cli:jsLegacyProcessResources SKIPPED
:kotlinx-cli:jsLegacyMainClasses SKIPPED
:kotlinx-cli:jsLegacyPublicPackageJson SKIPPED
:kotlinx-cli:jsLegacyJar SKIPPED
:kotlinx-cli:compileKotlinJvm SKIPPED
:kotlinx-cli:jvmProcessResources SKIPPED
:kotlinx-cli:jvmMainClasses SKIPPED
:kotlinx-cli:jvmJar SKIPPED
:kotlinx-cli:compileKotlinMetadata SKIPPED
:kotlinx-cli:metadataMainClasses SKIPPED
:kotlinx-cli:metadataJar SKIPPED
:kotlinx-cli:sourcesJar SKIPPED
:kotlinx-cli:assemble SKIPPED
Deprecated Gradle features were used in this build, making it incompatible with Gradle 7.0.
Use '--warning-mode all' to show the individual deprecation warnings.
See https://docs.gradle.org/6.6/userguide/command_line_interface.html#sec:command_line_warnings
BUILD SUCCESSFUL in 5s
```
As its name suggests, the `:kotlinx-cli:compileKotlinJvm` task shown in the example output is the compilation task for Kotlin/JVM.

### Run the task with debug level logging enabled

After knowing the correct compilation task, the debug logs can be collected from the output of ./gradlew --debug.

`user $``./gradlew :kotlinx-cli:compileKotlinJvm --debug`
As of Kotlin 1.6, the Gradle Kotlin plugin prints the compiler arguments in a message that contains the string `Kotlin compiler args:`.  There will also be two more messages before the message that are relevant to the compiler invocation:

- The first message contains the Kotlin compiler class called. For Kotlin/JVM, the class should be org.jetbrains.kotlin.cli.jvm.K2JVMCompiler.

- The second message contains the classpath with which the Kotlin compiler was called. This message can usually be ignored.

The following is a truncated example of messages printed for a compiler invocation:

2022-02-01T18:50:22.341-0800 \[DEBUG\] \[org.gradle.api.Task\] \[KOTLIN\] Kotlin compiler class: org.jetbrains.kotlin.cli.jvm.K2JVMCompiler
2022-02-01T18:50:22.341-0800 \[DEBUG\] \[org.gradle.api.Task\] \[KOTLIN\] Kotlin compiler classpath: /home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/...
2022-02-01T18:50:22.341-0800 \[DEBUG\] \[org.gradle.api.Task\] \[KOTLIN\] :kotlinx-cli:compileKotlinJvm Kotlin compiler args: -Xallow-no-source-files -classpath /home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib-jdk8/1.6.0/baf82c475e9372c25407f3d132439e4aa803b8b8/kotlin-stdlib-jdk8-1.6.0.jar:...

The output may still be too verbose to inspect. To make searching easier, any of the following strategies may be used:

- Use a pager like less for convenient scrolling and searching:

`user $``./gradlew :kotlinx-cli:compileKotlinJvm --debug | less`
- Write the output to a file for further inspection and print it to the console at the same time:

`user $``./gradlew :kotlinx-cli:compileKotlinJvm --debug | tee /tmp/kotlinx-cli-compileKotlinJvm.log`
- Filter out irrelevant lines and print only log messages for Kotlin compiler invocation:

`user $``./gradlew :kotlinx-cli:compileKotlinJvm --debug | grep -B2 'Kotlin compiler args:'`
2022-02-03T16:46:54.630-0800 \[DEBUG\] \[org.gradle.api.Task\] \[KOTLIN\] Kotlin compiler class: org.jetbrains.kotlin.cli.jvm.K2JVMCompiler
2022-02-03T16:46:54.631-0800 \[DEBUG\] \[org.gradle.api.Task\] \[KOTLIN\] Kotlin compiler classpath: /home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-compiler-embeddable/1.6.0/aa8da3bedc53c89219b84a0b40c7419431a13e76/kotlin-compiler-embeddable-1.6.0.jar, /home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-reflect/1.6.0/a215a7f914d5916dc5fd2d45cea16524e0220203/kotlin-reflect-1.6.0.jar, /home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib/1.6.0/a40b8b22529b733892edf4b73468ce598bb17f04/kotlin-stdlib-1.6.0.jar, /home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-script-runtime/1.6.0/55f72526ddcff6fc77d72cdf949c7e3d9bd95620/kotlin-script-runtime-1.6.0.jar, /home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-daemon-embeddable/1.6.0/d6242b37f4ce6b637d10847c7558446272ec8432/kotlin-daemon-embeddable-1.6.0.jar, /home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.intellij.deps/trove4j/1.0.20181211/216c2e14b070f334479d800987affe4054cd563f/trove4j-1.0.20181211.jar, /home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/net.java.dev.jna/jna/5.6.0/330f2244e9030119ab3030fc3fededc86713d9cc/jna-5.6.0.jar, /home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains/annotations/13.0/919f0dfe192fb4e063e7dacadee7f8bb9a2672a9/annotations-13.0.jar, /home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib-common/1.6.0/7857e365f925cfa060f941c1357cda1f8790502c/kotlin-stdlib-common-1.6.0.jar, /usr/lib64/openjdk-8/lib/tools.jar
2022-02-03T16:46:54.631-0800 \[DEBUG\] \[org.gradle.api.Task\] \[KOTLIN\] :kotlinx-cli:compileKotlinJvm Kotlin compiler args: -Xallow-no-source-files -classpath /home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib-jdk8/1.6.0/baf82c475e9372c25407f3d132439e4aa803b8b8/kotlin-stdlib-jdk8-1.6.0.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib-jdk7/1.6.0/da6bdc87391322974a43ccc00a25536ae74dad51/kotlin-stdlib-jdk7-1.6.0.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib/1.6.0/a40b8b22529b733892edf4b73468ce598bb17f04/kotlin-stdlib-1.6.0.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains/annotations/13.0/919f0dfe192fb4e063e7dacadee7f8bb9a2672a9/annotations-13.0.jar -d /tmp/kotlinx-cli/core/build/classes/kotlin/jvm/main -jdk-home /usr/lib64/openjdk-8 -module-name kotlinx-cli -no-stdlib -Werror -Xcommon-sources=/tmp/kotlinx-cli/core/commonMain/src/Options.kt,/tmp/kotlinx-cli/core/commonMain/src/ExperimentalCli.kt,/tmp/kotlinx-cli/core/commonMain/src/Descriptors.kt,/tmp/kotlinx-cli/core/commonMain/src/Arguments.kt,/tmp/kotlinx-cli/core/commonMain/src/ArgumentValues.kt,/tmp/kotlinx-cli/core/commonMain/src/ArgType.kt,/tmp/kotlinx-cli/core/commonMain/src/ArgParser.kt -Xmulti-platform -Xplugin=/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-scripting-compiler-embeddable/1.6.0/ee527b4e12e021023d295558add29c978cc64383/kotlin-scripting-compiler-embeddable-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-scripting-compiler-impl-embeddable/1.6.0/9ca5d07e5b64e747699081c51559803db6ec313d/kotlin-scripting-compiler-impl-embeddable-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-scripting-jvm/1.6.0/aea1438a95ff80b5ff71ac9fc998462297ddffea/kotlin-scripting-jvm-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-scripting-common/1.6.0/5bb427e36a331fde47846d026b687b3d5a244f7c/kotlin-scripting-common-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib/1.6.0/a40b8b22529b733892edf4b73468ce598bb17f04/kotlin-stdlib-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains/annotations/13.0/919f0dfe192fb4e063e7dacadee7f8bb9a2672a9/annotations-13.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib-common/1.6.0/7857e365f925cfa060f941c1357cda1f8790502c/kotlin-stdlib-common-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-script-runtime/1.6.0/55f72526ddcff6fc77d72cdf949c7e3d9bd95620/kotlin-script-runtime-1.6.0.jar -verbose -opt-in=kotlin.Experimental -opt-in=kotlinx.cli.ExperimentalCli /tmp/kotlinx-cli/core/jvmMain/src/Utils.kt /tmp/kotlinx-cli/core/commonMain/src/Options.kt /tmp/kotlinx-cli/core/commonMain/src/ExperimentalCli.kt /tmp/kotlinx-cli/core/commonMain/src/Descriptors.kt /tmp/kotlinx-cli/core/commonMain/src/Arguments.kt /tmp/kotlinx-cli/core/commonMain/src/ArgumentValues.kt /tmp/kotlinx-cli/core/commonMain/src/ArgType.kt /tmp/kotlinx-cli/core/commonMain/src/ArgParser.kt -jvm-target 1.8

- If the output has been saved to a file, it can be filtered in a similar manner:

`user $``grep -B2 'Kotlin compiler args:' /tmp/kotlinx-cli-compileKotlinJvm.log`
It is recommended that the message containing the compiler arguments is saved to a file to ease the inspection on it later on.

### Make a human-readable list of compiler arguments

The length of Kotlin compiler arguments can make inspections on them difficult. To decompose the arguments into lines of individual options and values, tr can be used to replace spaces with newline characters. If the arguments have been saved to a file, the following command can be used to complete this:

`user $``grep 'Kotlin compiler args:' /tmp/kotlinx-cli-compileKotlinJvm.log | sed -e 's/^.*Kotlin compiler args://' | tr ' ' '\n'`
-Xallow-no-source-files
-classpath
/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib-jdk8/1.6.0/baf82c475e9372c25407f3d132439e4aa803b8b8/kotlin-stdlib-jdk8-1.6.0.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib-jdk7/1.6.0/da6bdc87391322974a43ccc00a25536ae74dad51/kotlin-stdlib-jdk7-1.6.0.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib/1.6.0/a40b8b22529b733892edf4b73468ce598bb17f04/kotlin-stdlib-1.6.0.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains/annotations/13.0/919f0dfe192fb4e063e7dacadee7f8bb9a2672a9/annotations-13.0.jar
-d
/tmp/kotlinx-cli/core/build/classes/kotlin/jvm/main
-jdk-home
/usr/lib64/openjdk-8
-module-name
kotlinx-cli
-no-stdlib
-Werror
-Xcommon-sources=/tmp/kotlinx-cli/core/commonMain/src/Options.kt,/tmp/kotlinx-cli/core/commonMain/src/ExperimentalCli.kt,/tmp/kotlinx-cli/core/commonMain/src/Descriptors.kt,/tmp/kotlinx-cli/core/commonMain/src/Arguments.kt,/tmp/kotlinx-cli/core/commonMain/src/ArgumentValues.kt,/tmp/kotlinx-cli/core/commonMain/src/ArgType.kt,/tmp/kotlinx-cli/core/commonMain/src/ArgParser.kt
-Xmulti-platform
-Xplugin=/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-scripting-compiler-embeddable/1.6.0/ee527b4e12e021023d295558add29c978cc64383/kotlin-scripting-compiler-embeddable-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-scripting-compiler-impl-embeddable/1.6.0/9ca5d07e5b64e747699081c51559803db6ec313d/kotlin-scripting-compiler-impl-embeddable-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-scripting-jvm/1.6.0/aea1438a95ff80b5ff71ac9fc998462297ddffea/kotlin-scripting-jvm-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-scripting-common/1.6.0/5bb427e36a331fde47846d026b687b3d5a244f7c/kotlin-scripting-common-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib/1.6.0/a40b8b22529b733892edf4b73468ce598bb17f04/kotlin-stdlib-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains/annotations/13.0/919f0dfe192fb4e063e7dacadee7f8bb9a2672a9/annotations-13.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib-common/1.6.0/7857e365f925cfa060f941c1357cda1f8790502c/kotlin-stdlib-common-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-script-runtime/1.6.0/55f72526ddcff6fc77d72cdf949c7e3d9bd95620/kotlin-script-runtime-1.6.0.jar
-verbose
-opt-in=kotlin.Experimental
-opt-in=kotlinx.cli.ExperimentalCli
/tmp/kotlinx-cli/core/jvmMain/src/Utils.kt
/tmp/kotlinx-cli/core/commonMain/src/Options.kt
/tmp/kotlinx-cli/core/commonMain/src/ExperimentalCli.kt
/tmp/kotlinx-cli/core/commonMain/src/Descriptors.kt
/tmp/kotlinx-cli/core/commonMain/src/Arguments.kt
/tmp/kotlinx-cli/core/commonMain/src/ArgumentValues.kt
/tmp/kotlinx-cli/core/commonMain/src/ArgType.kt
/tmp/kotlinx-cli/core/commonMain/src/ArgParser.kt
-jvm-target
1.8

It is recommended that this output is saved to a file too since it still contains some very long arguments that can be decomposed, such as the value of `-classpath`, and `-Xcommon-sources=`.  For example, the following command saves the output to /tmp/kotlinx-cli-kotlinc-args.txt and prints it to the console at the same time:

`user $``grep 'Kotlin compiler args:' /tmp/kotlinx-cli-compileKotlinJvm.log | sed -e 's/^.*Kotlin compiler args://' | tr ' ' '\n' | tee /tmp/kotlinx-cli-kotlinc-args.txt`
## Kotlin eclasses primer

This section contains fundamental information regarding [the Kotlin eclasses](https://wiki.gentoo.org#Eclasses).

### Eclass variables to be set before inherit

This subsection covers `@PRE_INHERIT` variables, which are variables that should be defined before a Kotlin eclass is inherited.

#### Compatible Kotlin feature releases

A Kotlin project might be compatible with multiple Kotlin feature releases, even if it declares only one version in its Gradle build script.  To allow an ebuild to use any compatible Kotlin feature release to build the package instead of pin a specific feature release, the Kotlin eclasses **require** a `KOTLIN_COMPAT` variable to be set before `inherit` for declaring all compatible Kotlin feature release versions.

`KOTLIN_COMPAT` is similar to `PYTHON_COMPAT` and `LUA_COMPAT`: it must be declared as an array, and it should contain a list of identifiers for the Kotlin feature releases supported by the ebuild.  The identifier format is `kotlin1-x` for Kotlin 1.`x`.

The Kotlin eclasses will generate `KOTLIN_SINGLE_TARGET USE_EXPAND` flags for the ebuild, which work like `PYTHON_SINGLE_TARGET` and `LUA_SINGLE_TARGET`.  Those `USE_EXPAND` flags may be utilized by users to select the version of Kotlin compiler they would like to use to build the package before it is merged.

If a new package is being created, it is recommended that only a single identifier is added to `KOTLIN_COMPAT` first.  More versions can be added later after [compatibility tests](https://wiki.gentoo.org#Testing_a_package_on_another_Kotlin_feature_release).

Assuming the feature release of the Kotlin version declared in the Gradle build script is 1.`x`, the initial identifier added to `KOTLIN_COMPAT` is suggested to be for the first feature release in the following list (from top to bottom) that has a supported `KOTLIN_SINGLE_TARGET`:

- 1\.`x`
- 1.(`x` + 1)
- 1.(`x` + 2)
- 1.(`x` - 1)
- 1.(`x` - 2) or lower, which are unlikely to be compatible

For example, kotlinx-cli 0.3.4 declares Kotlin 1.6.0 in gradle.properties under its source tree<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>, which corresponds to feature release 1.6.  At the time this example was created, `kotlin1-6` was not a supported `KOTLIN_SINGLE_TARGET`, nor were `kotlin1-7` and `kotlin1-8`.  `kotlin1-5` was available, making Kotlin 1.5 the first version in the list that had a supported `KOTLIN_SINGLE_TARGET`.  So, the initial value of `KOTLIN_COMPAT` should be `( kotlin1-5 )`.

#### Declaration of special `USE` flags

Similar to `JAVA_PKG_IUSE` in java-pkg-2.eclass, the Kotlin eclasses recognize a `KOTLIN_IUSE` variable set before `inherit` that can be used to enable extra eclass functionality controlled by `USE` flags.  Supported `USE` flags include:

- [`source`](https://packages.gentoo.org/useflags/source)
- Install an archive of the package's source files so they can be viewed from some IDEs.
- Every ebuild should include this `USE` flag in `KOTLIN_IUSE`, unless there are no source files to archive (for instance, the ebuild is for a binary package or a metapackage).
- [`test`](https://packages.gentoo.org/useflags/test)
- Enable dependencies and/or preparations necessary to run the package's tests, which are controlled by `FEATURES=test`.
- If this `USE` flag is included, another eclass variable `KOTLIN_TESTING_FRAMEWORKS` should also be defined to include identifiers for testing frameworks to run in `src_test`.  This variable is analogous to `JAVA_TESTING_FRAMEWORKS` in java-pkg-simple.eclass.  Supported values include:
  - `junit-4`: Run tests with JUnit 4
  - `pkgdiff`: Compare `JAVA_JAR_FILENAME` with `JAVA_BINJAR_FILENAME` using [dev-util/japi-compliance-checker](https://packages.gentoo.org/packages/dev-util/japi-compliance-checker) and [dev-util/pkgdiff](https://packages.gentoo.org/packages/dev-util/pkgdiff) on available architectures

### Dependencies

A Kotlin package's dependencies pertaining to Java usually can be categorized into the following types:

- Kotlin compiler
- A provider of virtual/kotlin.
- Dependencies for `KOTLIN_IUSE`
- Tools needed to support `USE` flags defined in `KOTLIN_IUSE`.
- Kotlin libraries
- [Libraries](https://wiki.gentoo.org/wiki/Kotlin#Available_packages) that are a part of the Kotlin programming language itself, such as dev-java/kotlin-stdlib, dev-java/kotlin-stdlib-jdk8, dev-java/kotlin-test-junit, dev-java/kotlin-reflect, etc.
- JDK
- A provider of [virtual/jdk](https://packages.gentoo.org/packages/virtual/jdk).
- JRE
- A provider of [virtual/jre](https://packages.gentoo.org/packages/virtual/jre).
- Classpath dependencies
- Third-party Kotlin libraries, Java libraries, and other JVM libraries that should be present in the classpath for *both* javac during build time *and* JVM during runtime.
- Build-only JVM libraries
- JVM libraries that are needed only during build time.
- Runtime-only JVM libraries
- JVM libraries that are needed only during runtime.

This subsection will cover how each type of dependencies should be handled in a Kotlin package's ebuild.

#### Kotlin compiler

kotlin.eclass automatically inserts dependency specifications for virtual/kotlin into `DEPEND`, so the ebuild does not need to include them.

kotlin-utils.eclass does **not** automatically insert virtual/kotlin into `DEPEND`.  If the package needs to use the Kotlin compiler, the dependency needs to be added by the ebuild.  A `KOTLIN_UTILS_DEPS` output variable is provided by kotlin-utils.eclass for conveniently inserting dependency specifications for virtual/kotlin that are conditional upon `KOTLIN_SINGLE_TARGET USE_EXPAND` flags, so the correct feature release of Kotlin compiler respecting `KOTLIN_SINGLE_TARGET` can be pulled.

#### Dependencies for `KOTLIN_IUSE`

If `KOTLIN_IUSE` contains any [recognized `USE` flags](https://wiki.gentoo.org#Declaration_of_special_USE_flags), then some extra build dependencies are needed to support them.

kotlin.eclass automatically inserts specifications for those dependencies, so the ebuild does not need to include them.

kotlin-utils.eclass does **not** automatically insert the dependency specifications.  If the package defines any recognized `USE` flags in `KOTLIN_IUSE`, the dependency needs to be added by the ebuild.  A `kotlin-utils_iuse_depend` function is provided by kotlin-utils.eclass for conveniently inserting those dependency specifications into `DEPEND`.

#### Kotlin libraries

None of the Kotlin eclasses automatically insert dependency specifications for any Kotlin libraries into the dependency classes, thus ebuilds must declare Kotlin libraries they need themselves.

Same as the Kotlin compiler, Kotlin library packages are slotted based on the feature releases, so it is important to ensure that the correct slot is defined in the dependency specifications. It is recommended that Kotlin libraries and the Kotlin compiler are all in the equal slot for the same feature release. This is because the versions of Kotlin libraries and compiler pulled and used by the Kotlin Gradle plugin are matched by default. Using Kotlin libraries for a feature release different from the compiler might not be supported by JetBrains or the package upstream.

A `kotlin-utils_gen_slot_dep` function is provided to allow the correct slot of Kotlin libraries that matches the feature release of Kotlin compiler selected via `KOTLIN_SINGLE_TARGET` to be used.  It takes a set of dependencies as input and echoes a series of dependency specifications for them that are conditional upon `KOTLIN_SINGLE_TARGET USE_EXPAND` flags.  Instances of string `${KOTLIN_SLOT_DEP}` in the input will be replaced by the proper slot.

##### `CP_DEPEND`

java-utils-2.eclass recognizes a special, undocumented `CP_DEPEND` variable for dependencies that should be present in the classpath for both javac during build time and JVM during runtime.  By using `CP_DEPEND`, there is no need to repeat the same dependencies in `JAVA_GENTOO_CLASSPATH` for adding them to the classpath.

**The output of `kotlin-utils_gen_slot_dep` cannot be used in `CP_DEPEND`.**  `kotlin-utils_gen_slot_dep` echoes a dependency specification containing `USE`-conditional operators, which will be treated as invalid tokens by java-utils-2.eclass.  For instance, the following dependency declarations do not work:

This will lead to an error like the following when the package is being compiled:

```
>>> Compiling source in /var/tmp/portage/dev-java/kotlinx-cli-0.3.4/work/kotlinx-cli-0.3.4 ...
 * ERROR: dev-java/kotlinx-cli-0.3.4::spark-overlay failed (compile phase):
 *   Invalid CP_DEPEND atom kotlin_single_target_kotlin1-4?, ensure a SLOT is included
 * 
 * Call stack:
 *     ebuild.sh, line  127:  Called src_compile
 *   environment, line 2733:  Called kotlin_src_compile
 *   environment, line 2681:  Called kotlin-utils_src_compile
 *   environment, line 2565:  Called java-pkg_gen-cp 'JAVA_GENTOO_CLASSPATH'
 *   environment, line 1595:  Called die
 * The specific snippet of code:
 *               die "Invalid CP_DEPEND atom ${atom}, ensure a SLOT is included";
```
Therefore, to add dependencies included in the argument to `kotlin-utils_gen_slot_dep` to the classpath, they must be declared in `JAVA_GENTOO_CLASSPATH` instead.

The format of dependency specifications recognized in `JAVA_GENTOO_CLASSPATH` is different from that of the ebuild dependency classes: `${CATEGORY}/${PN}:${SLOT}` should be written as `${PN}` if `SLOT` equals `0` or `${PN}-${SLOT}` otherwise.  A `kotlin-utils_gen_slot_cp` function is provided to make the format conversion easier.

Like `kotlin-utils_gen_slot_dep` (whose name ends in `dep`), `kotlin-utils_gen_slot_cp` (whose name ends in `cp`) replaces instances of `${KOTLIN_SLOT_DEP}` with the slot of Kotlin libraries that the package links to when it is compiled.  However, **`kotlin-utils_gen_slot_cp` must be called from a phase function** because the slot depends on which `KOTLIN_SINGLE_TARGET USE_EXPAND` flag is enabled, so it can only be determined while the package is being built.

#### Other dependencies

Other non-Kotlin dependencies and tools that are not pulled as dependencies by the Kotlin eclasses can be declared normally as if they were for a normal Java package.

- JDK
- `>=virtual/jdk-1.8:*` should be put into `DEPEND` regardless of which EAPI is used.
- JRE
- `>=virtual/jre-1.8:*` should be put into `RDEPEND`.
- Classpath dependencies
- These dependencies can be put into `CP_DEPEND`. `${CP_DEPEND}` should be added to both `DEPEND` and `RDEPEND` if it is not empty, regardless of which EAPI is used.
- Build-only JVM libraries
- These libraries should be put into `DEPEND` regardless of which EAPI is used.
- Runtime-only JVM libraries
- These libraries should be put into `RDEPEND`.

## Converting Kotlin compiler arguments to an ebuild

After collecting the Kotlin compiler arguments needed to build a Kotlin package and knowing the basics about Kotlin eclasses, an installable ebuild for the Kotlin package is only a few variables away. kotlin-utils.eclass provides some variables for conveniently setting common Kotlin compiler options; it automatically uses the values of these variables to assemble the complete Kotlin compiler command and invokes it to run the Kotlin compiler.

Each of the following subsection contains instructions to convert the compiler option described by the subsection title to an eclass variable's value.  When applicable, the example from [#Obtaining Kotlin compiler arguments for a package](https://wiki.gentoo.org#Obtaining_Kotlin_compiler_arguments_for_a_package) will continue to be used for demonstration.

### `-classpath`

The `-classpath` option for the Kotlin compiler is identical to the option with the same name for javac: it includes paths to JARs and directories containing classes that may be needed by the sources being compiled.

[Kotlin libraries](https://wiki.gentoo.org/wiki/Kotlin#Available_packages) whose JAR appears in the classpath should be handled with instructions in [#Kotlin libraries](https://wiki.gentoo.org#Kotlin_libraries); other JARs' providers should be put into `CP_DEPEND`.

If the value for the `-classpath` option in the Kotlin compiler arguments is too long to read, the following command may be used to make it more readable:

`user $``grep -A1 -e '-classpath' /tmp/kotlinx-cli-kotlinc-args.txt | tail -n1 | tr ':' '\n'`
/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib-jdk8/1.6.0/baf82c475e9372c25407f3d132439e4aa803b8b8/kotlin-stdlib-jdk8-1.6.0.jar
/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib-jdk7/1.6.0/da6bdc87391322974a43ccc00a25536ae74dad51/kotlin-stdlib-jdk7-1.6.0.jar
/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib/1.6.0/a40b8b22529b733892edf4b73468ce598bb17f04/kotlin-stdlib-1.6.0.jar
/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains/annotations/13.0/919f0dfe192fb4e063e7dacadee7f8bb9a2672a9/annotations-13.0.jar

This may be converted to the following dependency declaration:

### `-Xcommon-sources`

This option usually contains a set of Kotlin source files and is thus very long. The following command can be used to produce a more readable list of files:

`user $``grep '^-Xcommon-sources=' /tmp/kotlinx-cli-kotlinc-args.txt | sed -e 's/-Xcommon-sources=//' | tr ',' '\n' | sort`
/tmp/kotlinx-cli/core/commonMain/src/ArgParser.kt
/tmp/kotlinx-cli/core/commonMain/src/ArgType.kt
/tmp/kotlinx-cli/core/commonMain/src/Arguments.kt
/tmp/kotlinx-cli/core/commonMain/src/ArgumentValues.kt
/tmp/kotlinx-cli/core/commonMain/src/Descriptors.kt
/tmp/kotlinx-cli/core/commonMain/src/ExperimentalCli.kt
/tmp/kotlinx-cli/core/commonMain/src/Options.kt

Although the number of files might look overwhelming, it is common that the files all reside in only one or two directories. Please find the smallest set of directories that contain all the files. Subdirectories may be omitted as long as it would not cause extra files to be included in the set. In this example, all the source files are under the core/commonMain directory in the project's source tree, so this directory alone constitutes the smallest set of directories containing all the files.

Then, the smallest set of directories may be defined inside an array and assigned to the `KOTLIN_COMMON_SOURCES_DIR` variable.

To verify if the smallest set of directories is correct, run the following commands, and compare their output with the list of files in the value of the `-Xcommon-sources` option.  Replace the value of `KOTLIN_COMMON_SOURCES_DIR` with what would be defined in the ebuild, and replace /tmp/kotlinx-cli/ with the path to the project's source tree's root (a trailing slash `/` is needed):

`user $``KOTLIN_COMMON_SOURCES_DIR=( core/commonMain )``user $``find "${KOTLIN_COMMON_SOURCES_DIR[@]/#//tmp/kotlinx-cli/}" -type f -name '*.kt' | sort`
/tmp/kotlinx-cli/core/commonMain/src/ArgParser.kt
/tmp/kotlinx-cli/core/commonMain/src/ArgType.kt
/tmp/kotlinx-cli/core/commonMain/src/Arguments.kt
/tmp/kotlinx-cli/core/commonMain/src/ArgumentValues.kt
/tmp/kotlinx-cli/core/commonMain/src/Descriptors.kt
/tmp/kotlinx-cli/core/commonMain/src/ExperimentalCli.kt
/tmp/kotlinx-cli/core/commonMain/src/Options.kt

### \*.kt source files

The \*.kt file paths that are standalone arguments are the Kotlin source files to be compiled.  For these files, please also come up with the smallest set of directories containing all of them, and define them in an array assigned to the `KOTLIN_SRC_DIR` variable.  The smallest set of directories can be verified similarly.

`user $``KOTLIN_SRC_DIR=( core/jvmMain core/commonMain )``user $``find "${KOTLIN_SRC_DIR[@]/#//tmp/kotlinx-cli/}" -type f -name '*.kt'`
/tmp/kotlinx-cli/core/jvmMain/src/Utils.kt
/tmp/kotlinx-cli/core/commonMain/src/Options.kt
/tmp/kotlinx-cli/core/commonMain/src/ExperimentalCli.kt
/tmp/kotlinx-cli/core/commonMain/src/Descriptors.kt
/tmp/kotlinx-cli/core/commonMain/src/Arguments.kt
/tmp/kotlinx-cli/core/commonMain/src/ArgumentValues.kt
/tmp/kotlinx-cli/core/commonMain/src/ArgType.kt
/tmp/kotlinx-cli/core/commonMain/src/ArgParser.kt

### `-module-name`

This option's value can be set in the ebuild using the `KOTLIN_MODULE_NAME` variable.  This variable's default value is `${PN}`, so if the option's value in the Kotlin compiler arguments is the same as `${PN}`, then the variable can be omitted.

### `-api-version` and `-language-version`

These options do not always appear in the Kotlin compiler arguments for a package.  If they do, and they have the same value, the value can be set in the ebuild using the `KOTLIN_WANT_TARGET` variable.  If their values are not the same, or only one of the options is specified, then the arguments need to be added to the `KOTLIN_KOTLINC_ARGS` array.

### `-Xjava-source-roots`

If this option appears in the Kotlin compiler arguments, then the project contains Java source files in addition to Kotlin source files, and the Java source files are located in the directories specified in the value of this option.  These directories should be added to an array assigned to the `KOTLIN_JAVA_SOURCE_ROOTS` variable.

In addition, when the `-Xjava-source-roots` option is used, there should also be a Gradle compilation task for the Java source files which may be called `:compileJava` or alike.  This task invokes javac, whose arguments can be obtained by searching for occurrences of string `NormalizingJavaCompiler`:

`user $``grep 'NormalizingJavaCompiler' /tmp/kotlin-stdlib.log`
2022-02-06T14:35:18.620-0800 \[DEBUG\] \[org.gradle.api.internal.tasks.compile.NormalizingJavaCompiler\] Compiler arguments: -source 1.6 -target 1.6 -d /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/build/classes/java/main -encoding UTF-8 -h /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/build/generated/sources/headers/java/main -g -sourcepath  -proc:none -s /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/build/generated/sources/annotationProcessor/java/main -XDuseUnsharedTable=true -classpath /home/leo/Projects/forks/kotlin/libraries/stdlib/common/build/libs/kotlin-stdlib-common-1.6.255-SNAPSHOT.jar:/home/leo/Projects/forks/kotlin/core/builtins/build/libs/builtins-1.6.255-SNAPSHOT.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains/annotations/13.0/919f0dfe192fb4e063e7dacadee7f8bb9a2672a9/annotations-13.0.jar:/home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/build/classes/kotlin/main -Xlint:deprecation -Xlint:unchecked -Werror -proc:none -proc:none /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/AdaptedFunctionReference.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/CallableReference.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/DefaultConstructorMarker.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/FunctionAdapter.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/FunctionImpl.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/FunctionReference.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/FunctionReferenceImpl.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/InlineMarker.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/Intrinsics.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/MagicApiIntrinsics.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/MutablePropertyReference.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/MutablePropertyReference0.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/MutablePropertyReference0Impl.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/MutablePropertyReference1.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/MutablePropertyReference1Impl.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/MutablePropertyReference2.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/MutablePropertyReference2Impl.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/PropertyReference.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/PropertyReference0.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/PropertyReference0Impl.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/PropertyReference1.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/PropertyReference1Impl.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/PropertyReference2.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/PropertyReference2Impl.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/Ref.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/Reflection.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/ReflectionFactory.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/RepeatableContainer.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/SpreadBuilder.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/runtime/kotlin/jvm/internal/TypeIntrinsics.java /home/leo/Projects/forks/kotlin/libraries/stdlib/jvm/src/kotlin/collections/ArraysUtilJVM.java

For each javac option, please follow the action specified in the table below:

| javac option | Action | 
|---|---|
| `-source` and `-target` | If their values are the same, specify the value via `KOTLIN_JAVA_WANT_SOURCE_TARGET`; otherwise, add verbatim to `KOTLIN_JAVAC_ARGS` | 
| `-encoding` | If the value is `UTF-8`, ignore; otherwise, set the value using the `JAVA_ENCODING` variable | 
| `-h` | If the directory specified in the value contains no files, ignore; otherwise, add path of the directory relative to `S` to `KOTLIN_JAVAC_ARGS` | 
| `-s` | Ditto | 
| `-sourcepath` | If no value is specified for the option, ignore; otherwise, add path of the directory relative to `S` to `KOTLIN_JAVAC_ARGS` | 
| `-classpath` | Ignore | 
| `-d` | Ignore | 
| \*.java source files | Ignore | 
| `-Werror` | Ignore <sup>1</sup> | 
| Duplicate options | Ignore | 
| Other options | Add verbatim to `KOTLIN_JAVAC_ARGS` | 

- <sup>1</sup> If `-Werror` is included in `KOTLIN_JAVAC_ARGS`, `src_compile` is very likely to fail because the [bootstrap class path warning](https://wiki.gentoo.org/wiki/Java_Developer_Guide#Bootstrap_class_path) is common when javac is called from Portage, and the warning will cause a compiler error when `-Werror` is in effect.

### Options to ignore

The following options in the Kotlin compiler argument for a package should be ignored and not added to the ebuild.

- `-Xplugin`
- This option enables Kotlin compiler plugins at the specified paths. This is only required when the Kotlin compiler is called by the Kotlin Gradle plugin; the standalone Kotlin command-line compiler comes with all default Kotlin compiler plugins installed and enabled, so in general, there is no need to include any extra plugins using this option.
- However, if the Kotlin project uses an extension plugin, a custom plugin or a third-party plugin, then this option should not be ignored and should be added to `KOTLIN_KOTLINC_ARGS`.
- Here are some ways to check whether a Kotlin project uses a non-default Kotlin compiler plugin:
  - Look at the project's Gradle build script to see if any Kotlin compiler plugin artifact is pulled as a dependency
  - Go through the list of JARs in the value of this option, and see if there is any JAR that has a file name does not contain `kotlin` and does not exist under the lib directory under the Kotlin compiler installation path

- `-no-reflect`
- This option can be ignored if the `-no-stdlib` option is set because `-no-stdlib` implies `-no-reflect`<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>.
- `-jdk-home`
- This option can be ignored unless a different JDK from the one pointed by `JAVA_HOME` is intended to be used<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup>.
- `-d`
- This option merely specifies the output path where classes built by the Kotlin compiler should be stored, thus it is bound to vary when the project's source tree is unpacked to a different path. The Kotlin eclasses will set a proper output path using this option, so ebuilds need not include it.
- `-verbose`
- This option can be ignored if the extra verbose output of the Kotlin compiler is not desired.
- `-version`
- This option can be ignored if the Kotlin compiler version information printed to standard output when this option is enabled is not desired.

### Other options

All other options that neither have a dedicated subsection above nor have been mentioned in [#Options to ignore](https://wiki.gentoo.org#Options_to_ignore) should be added to the array assigned to the `KOTLIN_KOTLINC_ARGS` variable.

### Options changed between Kotlin feature releases

The command-line interface of the Kotlin compiler usually changes between feature releases. If the feature release of Kotlin compiler used to build the Kotlin project within Portage is different from the feature release of the Kotlin Gradle plugin used by the project, those changes might cause package build failures. Fortunately, some failures can be fixed quickly and easily.

For the example used throughout this guide, the instructions in the previous sections should lead to production of the ebuild below.  As per the criteria listed in [#Compatible Kotlin feature releases](https://wiki.gentoo.org#Compatible_Kotlin_feature_releases), `kotlin1-5` was selected as the initial and only element for `KOTLIN_COMPAT` despite the fact that the Kotlin Gradle plugin's version is 1.6.0.

**`kotlinx-cli-0.3.4.ebuild`**

```
# Copyright 2022 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2
EAPI=8
KOTLIN_COMPAT=( kotlin1-5 )
KOTLIN_IUSE="source"
inherit kotlin
DESCRIPTION="Pure Kotlin implementation of a generic CLI parser"
HOMEPAGE="https://github.com/Kotlin/kotlinx-cli"
SRC_URI="https://github.com/Kotlin/kotlinx-cli/archive/refs/tags/v${PV}.tar.gz -> ${P}.tar.gz"
LICENSE="Apache-2.0"
SLOT="0"
KEYWORDS="~amd64"
KOTLIN_LIBS='
	dev-java/kotlin-stdlib-jdk8:${KOTLIN_SLOT_DEP}
	dev-java/kotlin-stdlib-jdk7:${KOTLIN_SLOT_DEP}
	dev-java/kotlin-stdlib:${KOTLIN_SLOT_DEP}
'
KOTLIN_DEPEND="$(kotlin-utils_gen_slot_dep "${KOTLIN_LIBS}")"
CP_DEPEND="
	dev-java/jetbrains-annotations:13
"
DEPEND="
	>=virtual/jdk-1.8:*
	${KOTLIN_DEPEND}
	${CP_DEPEND}
"
RDEPEND="
	>=virtual/jre-1.8:*
	${KOTLIN_DEPEND}
	${CP_DEPEND}
"
# Restore value of S overridden by java-pkg-simple.eclass to default
S="${WORKDIR}/${P}"
KOTLIN_SRC_DIR=( core/jvmMain core/commonMain )
KOTLIN_COMMON_SOURCES_DIR=( core/commonMain )
KOTLIN_KOTLINC_ARGS=(
	-jvm-target 1.8
	-no-stdlib
	-opt-in=kotlin.Experimental
	-opt-in=kotlinx.cli.ExperimentalCli
	-Xallow-no-source-files
	-Xmulti-platform
	-Werror
)
pkg_setup() {
	kotlin_pkg_setup
	JAVA_GENTOO_CLASSPATH="$(kotlin-utils_gen_slot_cp "${KOTLIN_LIBS}")"
}
```
When the package is being installed, the following error will be reported because `-opt-in` is a new Kotlin compiler option added in Kotlin 1.6 and is not available on Kotlin compiler 1.5:

\* kotlinx-cli-0.3.4.tar.gz BLAKE2B SHA512 size ;-) ...                  \[ ok \]
 \* Using: openjdk-8
 \* Using Kotlin compiler package: kotlin-bin-1.5
>>> Unpacking source...
>>> Unpacking kotlinx-cli-0.3.4.tar.gz to /var/tmp/portage/dev-java/kotlinx-cli-0.3.4/work
>>> Source unpacked in /var/tmp/portage/dev-java/kotlinx-cli-0.3.4/work
>>> Preparing source in /var/tmp/portage/dev-java/kotlinx-cli-0.3.4/work/kotlinx-cli-0.3.4 ...
>>> Source prepared.
>>> Configuring source in /var/tmp/portage/dev-java/kotlinx-cli-0.3.4/work/kotlinx-cli-0.3.4 ...
>>> Source configured.
>>> Compiling source in /var/tmp/portage/dev-java/kotlinx-cli-0.3.4/work/kotlinx-cli-0.3.4 ...
 \* Compiling ...
error: invalid argument: -opt-in=kotlin.Experimental
info: use -help for more information
 \* ERROR: dev-java/kotlinx-cli-0.3.4::spark-overlay failed (compile phase):
 \*   kotlin-utils\_kotlinc failed

Removing the `-opt-in` options from `KOTLIN_KOTLINC_ARGS` will produce the following warnings instead:

```
core/commonMain/src/ArgParser.kt:136:6: warning: this class can only be used with the compiler argument '-Xopt-in=kotlin.RequiresOptIn'
    @OptIn(ExperimentalCli::class)
     ^
core/commonMain/src/ArgParser.kt:177:33: warning: this API is experimental. It may be changed in the future without notice.
    private var usedSubcommand: Subcommand? = null
                                ^
```
The first warning message implies that there is a `-Xopt-in` option available on Kotlin compiler 1.5, which should be equivalent to Kotlin compiler 1.6's `-opt-in` option judging from the similarity between their names.  In fact, all errors and warnings on Kotlin compiler 1.5 can be eliminated by replacing `-opt-in` with `-Xopt-in` in `KOTLIN_KOTLINC_ARGS`.

## Pruning dependency list

The Kotlin Gradle plugin often includes JARs that are unnecessary in the `-classpath` option's value for the Kotlin compiler.  It is recommended that the dependencies providing those redundant JARs are removed from the ebuild's dependency classes, so users will not be forced to install unnecessary dependency packages.

One method of checking for unnecessary dependencies is to use the jdeps tool, which is shipped with JDK, to inspect the Java packages needed by the Kotlin project's JAR artifact.

### Build a Kotlin project's JAR using Gradle

The JAR artifact of the Kotlin project built by Gradle is preferred to the JAR generated by the ebuild because Gradle can certainly reproduce an exact upstream build of the project.  To build the project's JAR artifact for the Kotlin/JVM platform using Gradle, use the same method in [#Find out the Gradle compilation task for Kotlin/JVM](https://wiki.gentoo.org#Find_out_the_Gradle_compilation_task_for_Kotlin.2FJVM) to get a list of all tasks that would be run for the `assemble` lifecycle task, then find the task whose name suggests that it builds the JAR for Kotlin/JVM.  For the example used throughout this guide, the task is `:kotlinx-cli:jvmJar`.

Once the correct task is found, run it to build the JAR:

`user $``./gradlew :kotlinx-cli:jvmJar````
> Configure project :
INFRA: Sonatype publishing will not be possible due to missing staging repository id 'libs.repository.id'. Pass 'auto' for implicit staging.
> Configure project :kotlinx-cli
Kotlin Multiplatform Projects are an Alpha feature. See: https://kotlinlang.org/docs/reference/evolution/components-stability.html. To hide this message, add 'kotlin.mpp.stability.nowarn=true' to the Gradle properties.
Warning: Kotlin language settings function 'useExperimentalAnnotation' is deprecated and will be removed in next major releases. Please, use 'optIn' instead.
Some Kotlin/Native targets cannot be built on this linux_x64 machine and are disabled:
    * In project ':kotlinx-cli':
        * targets 'macosX64', 'macosArm64' (can be built with one of the hosts: macos_x64, macos_arm64)
To hide this message, add 'kotlin.native.ignoreDisabledTargets=true' to the Gradle properties.
Deprecated Gradle features were used in this build, making it incompatible with Gradle 7.0.
Use '--warning-mode all' to show the individual deprecation warnings.
See https://docs.gradle.org/6.6/userguide/command_line_interface.html#sec:command_line_warnings
BUILD SUCCESSFUL in 4s
2 actionable tasks: 2 executed
```
If the task is the correct one, a JAR will be created somewhere under the project's source tree. It can be found using find:

`user $``find . -type f -name '*.jar'`
./gradle/wrapper/gradle-wrapper.jar
./core/build/libs/kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar

If the project uses Gradle wrapper, there will be a gradle-wrapper.jar in the output, which can be ignored. The (other) JAR in the output, ./core/build/libs/kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar, is then the JAR artifact of the Kotlin project for Kotlin/JVM.

### Analyze the JAR's dependencies

jdeps can be invoked against the project's JAR to get a list of Java packages it depends on after the JAR has been built:

`user $``jdeps ./core/build/libs/kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar````
kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar -> not found
kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar -> /usr/lib64/openjdk-8/jre/lib/rt.jar
   kotlinx.cli (kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar)
      -> java.io                                            
      -> java.lang                                          
      -> java.lang.annotation                               
      -> java.util                                          
      -> kotlin                                             not found
      -> kotlin.annotation                                  not found
      -> kotlin.collections                                 not found
      -> kotlin.jvm.functions                               not found
      -> kotlin.jvm.internal                                not found
      -> kotlin.ranges                                      not found
      -> kotlin.reflect                                     not found
      -> kotlin.text                                        not found
```
There will likely be a lot of Java packages that are "not found", which is expected.  These packages can be resolved by adding the JARs that provide them to the `-classpath` option's value for jdeps.

To start with, use java-config, which is a specialized Gentoo utility, to generate a classpath of packages that would be in the ebuild's `CP_DEPEND` and `JAVA_GENTOO_CLASSPATH`.  Then, the classpath can be supplied to jdeps.

`user $``PKGS=( kotlin-stdlib-jdk8-1.5 kotlin-stdlib-jdk7-1.5 kotlin-stdlib-1.5 jetbrains-annotations-13 )``user $``jdeps -cp "$(java-config -dp "${PKGS[@]}")" ./core/build/libs/kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar````
kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar -> /usr/share/kotlin-stdlib-1.5/lib/kotlin-stdlib.jar
kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar -> /usr/lib64/openjdk-8/jre/lib/rt.jar
   kotlinx.cli (kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar)
      -> java.io                                            
      -> java.lang                                          
      -> java.lang.annotation                               
      -> java.util                                          
      -> kotlin                                             kotlin-stdlib.jar
      -> kotlin.annotation                                  kotlin-stdlib.jar
      -> kotlin.collections                                 kotlin-stdlib.jar
      -> kotlin.jvm.functions                               kotlin-stdlib.jar
      -> kotlin.jvm.internal                                kotlin-stdlib.jar
      -> kotlin.ranges                                      kotlin-stdlib.jar
      -> kotlin.reflect                                     kotlin-stdlib.jar
      -> kotlin.text                                        kotlin-stdlib.jar
```
If the classpath is complete, then there should be no more "not found" Java packages in the output.

Note that in this example, although the classpath contains four different packages' JARs, only a single JAR, kotlin-stdlib.jar, is actually used to satisfy the dependencies of the project's JAR. If only kotlin-stdlib.jar is added to the classpath for jdeps, the output will be the same, and there are still no Java packages that are "not found".

`user $``PKGS=( kotlin-stdlib-1.5 )``user $``jdeps -cp "$(java-config -dp "${PKGS[@]}")" ./core/build/libs/kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar````
kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar -> /usr/share/kotlin-stdlib-1.5/lib/kotlin-stdlib.jar
kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar -> /usr/lib64/openjdk-8/jre/lib/rt.jar
   kotlinx.cli (kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar)
      -> java.io                                            
      -> java.lang                                          
      -> java.lang.annotation                               
      -> java.util                                          
      -> kotlin                                             kotlin-stdlib.jar
      -> kotlin.annotation                                  kotlin-stdlib.jar
      -> kotlin.collections                                 kotlin-stdlib.jar
      -> kotlin.jvm.functions                               kotlin-stdlib.jar
      -> kotlin.jvm.internal                                kotlin-stdlib.jar
      -> kotlin.ranges                                      kotlin-stdlib.jar
      -> kotlin.reflect                                     kotlin-stdlib.jar
      -> kotlin.text                                        kotlin-stdlib.jar
```
As a sanity check, the string `not found` may be searched in the output of jdeps using grep.  If the following command prints nothing, then the classpath is complete.

`user $``jdeps -cp "$(java-config -dp "${PKGS[@]}")" ./core/build/libs/kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar | grep 'not found'`
Thus, dev-java/kotlin-stdlib-jdk8, dev-java/kotlin-stdlib-jdk7 and dev-java/jetbrains-annotations:13 can be removed from the ebuild's dependency classes, producing a more compact set of dependencies:

## Testing a package

To help assure a high quality of packages, any JAR generated by a Kotlin package's ebuild should be tested for consistency and equivalence with the JAR built using Gradle.

### Enable the `src_test` phase

For some Kotlin projects, the upstream includes a test suite in the project's source tree, which can be run using Gradle's `check` lifecycle task.  If the testing frameworks used in the test suite are [integrated with the Kotlin eclasses](https://wiki.gentoo.org#Declaration_of_special_USE_flags), and Gentoo packages for the test dependencies are available, then the Kotlin eclasses can execute the tests in the `src_test` phase.

All Kotlin packages are encouraged to support running the test suite from `src_test` whenever possible because this enables the standard way to check and verify packages on Gentoo and helps other maintainers easily test their changes to packages.

#### Obtain Kotlin compiler arguments for test sources

The test sources from the test suite need to be compiled into test classes before the tests can be executed, which means that the Kotlin compiler arguments that the Kotlin Gradle plugin would use to compile the test sources need to be collected too.  The method to find out the compiler arguments for test sources is similar to the steps for [#Obtaining Kotlin compiler arguments for a package](https://wiki.gentoo.org#Obtaining_Kotlin_compiler_arguments_for_a_package): first find out the Gradle task for compiling the test classes for Kotlin/JVM, then collect the compiler arguments from Gradle debug logs.

For test sources, the `check` lifecycle task should be used instead of assemble when ./gradle --dry-run is used to get a list of tasks to be run for it:

`user $``./gradlew check --dry-run````
> Configure project :
INFRA: Sonatype publishing will not be possible due to missing staging repository id 'libs.repository.id'. Pass 'auto' for implicit staging.
> Configure project :kotlinx-cli
Kotlin Multiplatform Projects are an Alpha feature. See: https://kotlinlang.org/docs/reference/evolution/components-stability.html. To hide this message, add 'kotlin.mpp.stability.nowarn=true' to the Gradle properties.
Warning: Kotlin language settings function 'useExperimentalAnnotation' is deprecated and will be removed in next major releases. Please, use 'optIn' instead.
Some Kotlin/Native targets cannot be built on this linux_x64 machine and are disabled:
    * In project ':kotlinx-cli':
        * targets 'macosX64', 'macosArm64' (can be built with one of the hosts: macos_x64, macos_arm64)
To hide this message, add 'kotlin.native.ignoreDisabledTargets=true' to the Gradle properties.
:check SKIPPED
:kotlinNodeJsSetup SKIPPED
:kotlinYarnSetup SKIPPED
:kotlinNpmCachesSetup SKIPPED
:kotlinx-cli:jsIrPackageJson SKIPPED
:kotlinx-cli:jsIrTestPackageJson SKIPPED
:kotlinx-cli:jsLegacyPackageJson SKIPPED
:kotlinx-cli:jsLegacyTestPackageJson SKIPPED
:rootPackageJson SKIPPED
:kotlinNpmInstall SKIPPED
:kotlinx-cli:jsIrGenerateExternalsIntegrated SKIPPED
:kotlinx-cli:compileKotlinJsIr SKIPPED
:kotlinx-cli:compileKotlinJsLegacy SKIPPED
:kotlinx-cli:jsIrProcessResources SKIPPED
:kotlinx-cli:jsIrMainClasses SKIPPED
:kotlinx-cli:jsIrPublicPackageJson SKIPPED
:kotlinx-cli:jsIrJar SKIPPED
:kotlinx-cli:jsLegacyProcessResources SKIPPED
:kotlinx-cli:jsLegacyMainClasses SKIPPED
:kotlinx-cli:compileTestKotlinJsIr SKIPPED
:kotlinx-cli:jsIrTestProcessResources SKIPPED
:kotlinx-cli:jsIrTestClasses SKIPPED
:kotlinx-cli:compileTestDevelopmentExecutableKotlinJsIr SKIPPED
:kotlinx-cli:jsIrTestTestDevelopmentExecutableCompileSync SKIPPED
:kotlinx-cli:jsIrNodeTest SKIPPED
:kotlinx-cli:jsIrTest SKIPPED
:kotlinx-cli:jsLegacyPublicPackageJson SKIPPED
:kotlinx-cli:jsLegacyJar SKIPPED
:kotlinx-cli:compileTestKotlinJsLegacy SKIPPED
:kotlinx-cli:jsLegacyTestProcessResources SKIPPED
:kotlinx-cli:jsLegacyTestClasses SKIPPED
:kotlinx-cli:jsLegacyNodeTest SKIPPED
:kotlinx-cli:jsLegacyTest SKIPPED
:kotlinx-cli:compileKotlinJvm SKIPPED
:kotlinx-cli:jvmProcessResources SKIPPED
:kotlinx-cli:jvmMainClasses SKIPPED
:kotlinx-cli:compileTestKotlinJvm SKIPPED
:kotlinx-cli:jvmTestProcessResources SKIPPED
:kotlinx-cli:jvmTestClasses SKIPPED
:kotlinx-cli:jvmTest SKIPPED
:kotlinx-cli:compileKotlinLinuxX64 SKIPPED
:kotlinx-cli:linuxX64ProcessResources SKIPPED
:kotlinx-cli:linuxX64MainKlibrary SKIPPED
:kotlinx-cli:compileTestKotlinLinuxX64 SKIPPED
:kotlinx-cli:linkDebugTestLinuxX64 SKIPPED
:kotlinx-cli:linuxX64Test SKIPPED
:kotlinx-cli:compileTestKotlinMacosArm64 SKIPPED
:kotlinx-cli:linkDebugTestMacosArm64 SKIPPED
:kotlinx-cli:macosArm64Test SKIPPED
:kotlinx-cli:compileTestKotlinMacosX64 SKIPPED
:kotlinx-cli:linkDebugTestMacosX64 SKIPPED
:kotlinx-cli:macosX64Test SKIPPED
:kotlinx-cli:compileKotlinMingwX64 SKIPPED
:kotlinx-cli:mingwX64ProcessResources SKIPPED
:kotlinx-cli:mingwX64MainKlibrary SKIPPED
:kotlinx-cli:compileTestKotlinMingwX64 SKIPPED
:kotlinx-cli:linkDebugTestMingwX64 SKIPPED
:kotlinx-cli:mingwX64Test SKIPPED
:kotlinx-cli:allTests SKIPPED
:kotlinx-cli:check SKIPPED
Deprecated Gradle features were used in this build, making it incompatible with Gradle 7.0.
Use '--warning-mode all' to show the individual deprecation warnings.
See https://docs.gradle.org/6.6/userguide/command_line_interface.html#sec:command_line_warnings
BUILD SUCCESSFUL in 900ms
```
In the above example, the compilation task for test sources for Kotlin/JVM is `:kotlinx-cli:compileTestKotlinJvm`.  This task can then be run with ./gradlew --debug so the Kotlin compiler arguments can be collected.

`user $``./gradlew :kotlinx-cli:compileTestKotlinJvm --debug | tee /tmp/kotlinx-cli-compileTestKotlinJvm.log`
For the test compilation task, there might be two sets of Kotlin compiler arguments printed to the debug logs: the first set is for the main sources of the package itself, and the second set is the one for the test sources.  The task name pertaining to a set of compiler arguments is printed before the `Kotlin compiler args:` string in the log message:

```
2022-02-04T10:47:07.372-0800 [DEBUG] [org.gradle.api.Task] [KOTLIN] :kotlinx-cli:compileTestKotlinJvm Kotlin compiler args: ...
                                                                    ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```
Assuming the last set of Kotlin compiler arguments in the debug logs is for the test sources, grep 'Kotlin compiler args:' | tail -n1 can be used to extract it alone and filter out all other messages containing Kotlin compiler arguments.

`user $``grep 'Kotlin compiler args:' /tmp/kotlinx-cli-compileTestKotlinJvm.log | tail -n1 | sed -e 's/^.*Kotlin compiler args://' | tr ' ' '\n' | tee /tmp/kotlinx-cli-test-kotlinc-args.txt`
-Xallow-no-source-files
-classpath
/tmp/kotlinx-cli/core/build/classes/kotlin/jvm/main:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-test-common/1.6.0/bd1508e34851f23496bda2b806ec53021d5131ea/kotlin-test-common-1.6.0.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-test-annotations-common/1.6.0/eab998aba507e227380d14d475f0ff4c7098a272/kotlin-test-annotations-common-1.6.0.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib-jdk8/1.6.0/baf82c475e9372c25407f3d132439e4aa803b8b8/kotlin-stdlib-jdk8-1.6.0.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-test-junit/1.6.0/c76eb0205c75bf595bf3b905dee4962d7845fdf4/kotlin-test-junit-1.6.0.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib-jdk7/1.6.0/da6bdc87391322974a43ccc00a25536ae74dad51/kotlin-stdlib-jdk7-1.6.0.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-test/1.6.0/527fdd8f1d8d4b9066faecf041c9ccabe73f8848/kotlin-test-1.6.0.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib/1.6.0/a40b8b22529b733892edf4b73468ce598bb17f04/kotlin-stdlib-1.6.0.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/junit/junit/4.12/2973d150c0dc1fefe998f834810d68f278ea58ec/junit-4.12.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains/annotations/13.0/919f0dfe192fb4e063e7dacadee7f8bb9a2672a9/annotations-13.0.jar:/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.hamcrest/hamcrest-core/1.3/42a25dc3219429f0e5d060061f71acb49bf010a0/hamcrest-core-1.3.jar
-d
/tmp/kotlinx-cli/core/build/classes/kotlin/jvm/test
-Xfriend-paths=/tmp/kotlinx-cli/core/build/classes/kotlin/jvm/main,/tmp/kotlinx-cli/core/build/libs/kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar
-jdk-home
/usr/lib64/openjdk-8
-module-name
kotlinx-cli
-no-stdlib
-Werror
-Xcommon-sources=/tmp/kotlinx-cli/core/commonTest/src/SubcommandsTests.kt,/tmp/kotlinx-cli/core/commonTest/src/OptionsTests.kt,/tmp/kotlinx-cli/core/commonTest/src/HelpTests.kt,/tmp/kotlinx-cli/core/commonTest/src/ErrorTests.kt,/tmp/kotlinx-cli/core/commonTest/src/DataSourceEnum.kt,/tmp/kotlinx-cli/core/commonTest/src/ArgumentsTests.kt
-Xmulti-platform
-Xplugin=/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-scripting-compiler-embeddable/1.6.0/ee527b4e12e021023d295558add29c978cc64383/kotlin-scripting-compiler-embeddable-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-scripting-compiler-impl-embeddable/1.6.0/9ca5d07e5b64e747699081c51559803db6ec313d/kotlin-scripting-compiler-impl-embeddable-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-scripting-jvm/1.6.0/aea1438a95ff80b5ff71ac9fc998462297ddffea/kotlin-scripting-jvm-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-scripting-common/1.6.0/5bb427e36a331fde47846d026b687b3d5a244f7c/kotlin-scripting-common-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib/1.6.0/a40b8b22529b733892edf4b73468ce598bb17f04/kotlin-stdlib-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains/annotations/13.0/919f0dfe192fb4e063e7dacadee7f8bb9a2672a9/annotations-13.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib-common/1.6.0/7857e365f925cfa060f941c1357cda1f8790502c/kotlin-stdlib-common-1.6.0.jar,/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-script-runtime/1.6.0/55f72526ddcff6fc77d72cdf949c7e3d9bd95620/kotlin-script-runtime-1.6.0.jar
-verbose
-opt-in=kotlin.Experimental
-opt-in=kotlinx.cli.ExperimentalCli
/tmp/kotlinx-cli/core/commonTest/src/SubcommandsTests.kt
/tmp/kotlinx-cli/core/commonTest/src/OptionsTests.kt
/tmp/kotlinx-cli/core/commonTest/src/HelpTests.kt
/tmp/kotlinx-cli/core/commonTest/src/ErrorTests.kt
/tmp/kotlinx-cli/core/commonTest/src/DataSourceEnum.kt
/tmp/kotlinx-cli/core/commonTest/src/ArgumentsTests.kt
-jvm-target
1.8

The Kotlin compiler arguments for the test sources are usually mostly identical to those for the main sources with the following differences:

- `-classpath` usually contains extra test dependencies.
- An extra `-Xfriend-paths` option might be used in test sources compilation, which is for allowing test sources to access internal members of classes in the main module.
- `-Xcommon-sources` usually points to a different set of files.
- The list of \*.kt source files passed into the Kotlin compiler will always change.

#### Add Kotlin compiler arguments to ebuild

Once the Kotlin compiler arguments for test sources are collected, they can be added to the ebuild in a way that is analogous to [#Converting Kotlin compiler arguments to an ebuild](https://wiki.gentoo.org#Converting_Kotlin_compiler_arguments_to_an_ebuild) with a few minor differences.

- Extra test dependencies specified in the value of `-classpath` should be added to `DEPEND` and `JAVA_TEST_GENTOO_CLASSPATH`.
- The value of the `-Xfriend-paths` option needs to be properly handled.
- For each other type of Kotlin compiler arguments, the name of the eclass variable for it starts with `KOTLIN_TEST_` instead of `KOTLIN_`.  For example, the eclass variable for directories containing test source files is `KOTLIN_TEST_SRC_DIR`.

##### `-classpath`

The value of `-classpath` used in test sources compilation might contain the following elements, as shown in the example below:

- A directory in the project's source tree that contains the classes built from the main source files
- JARs of test dependencies
- JARs in the value of `-classpath` used to compile the main source files

`user $``grep -A1 -e '-classpath' /tmp/kotlinx-cli-test-kotlinc-args.txt | tail -n1 | tr ':' '\n'`
/tmp/kotlinx-cli/core/build/classes/kotlin/jvm/main
/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-test-common/1.6.0/bd1508e34851f23496bda2b806ec53021d5131ea/kotlin-test-common-1.6.0.jar
/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-test-annotations-common/1.6.0/eab998aba507e227380d14d475f0ff4c7098a272/kotlin-test-annotations-common-1.6.0.jar
/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib-jdk8/1.6.0/baf82c475e9372c25407f3d132439e4aa803b8b8/kotlin-stdlib-jdk8-1.6.0.jar
/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-test-junit/1.6.0/c76eb0205c75bf595bf3b905dee4962d7845fdf4/kotlin-test-junit-1.6.0.jar
/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib-jdk7/1.6.0/da6bdc87391322974a43ccc00a25536ae74dad51/kotlin-stdlib-jdk7-1.6.0.jar
/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-test/1.6.0/527fdd8f1d8d4b9066faecf041c9ccabe73f8848/kotlin-test-1.6.0.jar
/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-stdlib/1.6.0/a40b8b22529b733892edf4b73468ce598bb17f04/kotlin-stdlib-1.6.0.jar
/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/junit/junit/4.12/2973d150c0dc1fefe998f834810d68f278ea58ec/junit-4.12.jar
/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.jetbrains/annotations/13.0/919f0dfe192fb4e063e7dacadee7f8bb9a2672a9/annotations-13.0.jar
/home/leo/.nobackup/gradle/.gradle/caches/modules-2/files-2.1/org.hamcrest/hamcrest-core/1.3/42a25dc3219429f0e5d060061f71acb49bf010a0/hamcrest-core-1.3.jar

Everything except the JARs of test dependencies can be ignored. The following JARs can be ignored as well because they do not contain any file required for compiling any test sources:

- kotlin-test-common\*.jar
- kotlin-test-annotations-common\*.jar

Therefore, in this example, only the following files are *not* ignored:

- kotlin-test-junit\*.jar
- kotlin-test\*.jar
- junit-4\*.jar
- hamcrest-core-1.3.jar

The presence of junit-4\*.jar indicates that the tests use JUnit 4, which is a testing framework supported by `KOTLIN_IUSE="test"` and `KOTLIN_TESTING_FRAMEWORKS`.  Thus, the following definitions of these variables should be added to the ebuild:

With these variable definitions, kotlin.eclass will automatically add `test? ( dev-java/junit:4 )` to `DEPEND`.  kotlin-utils.eclass does not automatically do this, but the ebuild only needs to [call the `kotlin-utils_iuse_depend` function in `DEPEND`](https://wiki.gentoo.org#Dependencies_for_KOTLIN_IUSE) to achieve it.  Therefore, the ebuild does not need to declare the test dependency on JUnit 4 itself.  In addition, because hamcrest-core-1.3.jar is a dependency of JUnit 4, dev-java/hamcrest-core:1.3 will be pulled with dev-java/junit:4 together, so it does not need to explicitly declared either.

The only unresolved test dependencies at this point are kotlin-test-junit\*.jar and kotlin-test\*.jar, which are provided by dev-java/kotlin-test-junit and dev-java/kotlin-test respectively. Since the latter package is a dependency of the former one, declaring merely dev-java/kotlin-test-junit as a dependency will also cause both packages to be pulled.

In conclusion, declaring dev-java/kotlin-test-junit alone is sufficient for satisfying test dependencies.

##### `-Xfriend-paths`

Any classes and JARs specified after `-Xfriend-paths` in a Kotlin compiler invocation will have their internal members visible to the input source files during the compiler execution, which is why this option is often used when test sources are being compiled: all the classes and JARs being tested will be included in the value of `-Xfriend-paths` so the test sources can access everything in them for the sake of verification.

The proper value of `-Xfriend-paths` in the context of an ebuild is `${JAVA_JAR_FILENAME}`.  java-pkg-simple.eclass declares the `JAVA_JAR_FILENAME` variable for the name of the JAR file that `src_compile` will create, and it defines a default value for it.  Because Kotlin eclasses inherit java-pkg-simple.eclass, they honor this variable as well.  Therefore, an ebuild for a Kotlin package can freely use `JAVA_JAR_FILENAME` even if it does not explicitly define this variable.

Since `JAVA_JAR_FILENAME` contains the classes generated by the Kotlin compiler during `src_compile`, adding it alone to the value of the `-Xfriend-paths` option in the ebuild is sufficient to simulate the effect of `-Xfriend-paths` when the project is built by Gradle.

There is not a dedicated eclass variable for `-Xfriend-paths` yet, so ebuilds should define it in `KOTLIN_TEST_KOTLINC_ARGS`:

#### Example ebuild with `src_test` enabled

At this point in the example, the ebuild would look like this:

**`kotlinx-cli-0.3.4.ebuild`**

**with dependencies simplified, compiler arguments modified for Kotlin 1.5, and`src_test` enabled**

```
# Copyright 2022 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2
EAPI=8
KOTLIN_COMPAT=( kotlin1-5 )
KOTLIN_IUSE="source test"
KOTLIN_TESTING_FRAMEWORKS="junit-4"
inherit kotlin
DESCRIPTION="Pure Kotlin implementation of a generic CLI parser"
HOMEPAGE="https://github.com/Kotlin/kotlinx-cli"
SRC_URI="https://github.com/Kotlin/kotlinx-cli/archive/refs/tags/v${PV}.tar.gz -> ${P}.tar.gz"
LICENSE="Apache-2.0"
SLOT="0"
KEYWORDS="~amd64"
KOTLIN_LIBS='
	dev-java/kotlin-stdlib:${KOTLIN_SLOT_DEP}
'
KOTLIN_DEPEND="$(kotlin-utils_gen_slot_dep "${KOTLIN_LIBS}")"
DEPEND="
	>=virtual/jdk-1.8:*
	${KOTLIN_DEPEND}
	test? (
		$(kotlin-utils_gen_slot_dep '
			dev-java/kotlin-test-junit:${KOTLIN_SLOT_DEP}
		')
	)
"
RDEPEND="
	>=virtual/jre-1.8:*
	${KOTLIN_DEPEND}
"
# Restore value of S overridden by java-pkg-simple.eclass to default
S="${WORKDIR}/${P}"
KOTLIN_SRC_DIR=( core/jvmMain core/commonMain )
KOTLIN_COMMON_SOURCES_DIR=( core/commonMain )
KOTLIN_KOTLINC_ARGS=(
	-jvm-target 1.8
	-no-stdlib
	-Xopt-in=kotlin.Experimental
	-Xopt-in=kotlinx.cli.ExperimentalCli
	-Xallow-no-source-files
	-Xmulti-platform
	-Werror
)
KOTLIN_TEST_SRC_DIR=( core/commonTest )
KOTLIN_TEST_COMMON_SOURCES_DIR=( core/commonTest )
KOTLIN_TEST_KOTLINC_ARGS=(
	-Xfriend-paths="${JAVA_JAR_FILENAME}"
	"${KOTLIN_KOTLINC_ARGS[@]}"
)
pkg_setup() {
	kotlin_pkg_setup
	JAVA_GENTOO_CLASSPATH="$(kotlin-utils_gen_slot_cp "${KOTLIN_LIBS}")"
	JAVA_TEST_GENTOO_CLASSPATH="$(kotlin-utils_gen_slot_cp '
		kotlin-test-junit-${KOTLIN_SLOT_DEP}
	')"
}
```
### Manually check the package

Unfortunately, for some Kotlin packages, it might not be feasible to enable the `src_test` phase because of missing Gentoo packages for test dependencies or absence of a test suite provided by the upstream.  In this case, the packages can still be checked manually.

#### Test classes using Gradle

If there is a test suite for a package, but some test dependencies do not have a Gentoo package available, then the classes compiled for the package within Portage can be unpacked from the package's JAR to the correct location in the project's source tree before the tests are launched using Gradle.

First, find the Gradle task for both compiling the main source files for Kotlin/JVM and *executing* the tests for Kotlin/JVM from the output of `./gradlew check --dry-run`; the [#Obtain Kotlin compiler arguments for test sources](https://wiki.gentoo.org#Obtain_Kotlin_compiler_arguments_for_test_sources) section contains an example of the output.  In that example, judging from their names, `:kotlinx-cli:compileKotlinJvm` is the task to compile the main sources, and `:kotlinx-cli:jvmTest` is the task to run the tests for Kotlin/JVM.

If this has not been done yet, install the ebuild being manually tested onto the system.

Next, the path to the top-level directory where classes compiled from the main source files will be stored needs to be found. Clean the project first to avoid existence of irrelevant build artifacts interfering with the path search; then, run the main sources compilation task once, so the classes will be generated in the project's source tree.

`user $``./gradlew clean``user $``./gradlew :kotlinx-cli:compileKotlinJvm`
The classes will be put somewhere in the source tree and can be located using find:

`user $``find . -type f -name '*.class'`
./core/build/classes/kotlin/jvm/main/kotlinx/cli/UtilsKt.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/OptionsKt.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/MultipleOption.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/SingleNullableOption.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/SingleOption.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/AbstractSingleOption.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/Option.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/MultipleOptionType$RepeatedDelimited.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/MultipleOptionType$Delimited.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/MultipleOptionType$Repeated.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/MultipleOptionType.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ExperimentalCli.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgDescriptor.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/OptionDescriptor.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/Descriptor$defaultValueSet$2.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/Descriptor.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgumentsKt.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/MultipleArgument.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/SingleNullableArgument.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/SingleArgument.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/AbstractSingleArgument.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/Argument.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/CLIEntity.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/DefaultRequiredType$None.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/DefaultRequiredType$Required.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/DefaultRequiredType$Default.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/DefaultRequiredType.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/CLIEntityWrapper.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgumentMultipleValues.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgumentSingleNullableValue.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgumentSingleValue.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/AbstractArgumentSingleValue.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ParsingValue.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ParsingException.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$Choice$1.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$Choice.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$Companion$Choice$2.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$Companion$Choice$1.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$Companion.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$Double.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$Int.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$String.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$Boolean.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgParserKt.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgParser$outputAndTerminate$1.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgParser$OptionPrefixStyle.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgParser$ValueOrigin.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgParser.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgParserResult.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/Subcommand.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgumentValueDelegate$DefaultImpls.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgumentValueDelegate.class
./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgumentsQueue.class

In this example, the top-level directory's path is ./core/build/classes/kotlin/jvm/main.  The directory hierarchy under this path (kotlinx/cli) is the standard Java package layout for `kotlinx.cli`, the main Java package of this Kotlin project.  
kotlinx/cli is thus not the top-level directory of classes artifacts and should not be included in the top-level directory's path.

At this point, classes that were generated when the ebuild was installed can be unpacked from the JAR installed by the ebuild to the system to this top-level directory using [app-arch/unzip](https://packages.gentoo.org/packages/app-arch/unzip).  The directory should be specified via the `-d` option, and the path to the JAR installed by the ebuild can be obtained using java-config -p:

`user $``unzip -o -d ./core/build/classes/kotlin/jvm/main "$(java-config -p kotlinx-cli)"`
Archive:  /usr/share/kotlinx-cli/lib/kotlinx-cli.jar
  inflating: ./core/build/classes/kotlin/jvm/main/META-INF/MANIFEST.MF  
  inflating: ./core/build/classes/kotlin/jvm/main/META-INF/kotlinx-cli.kotlin\_module  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/UtilsKt.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/OptionsKt.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/MultipleOption.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/SingleNullableOption.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/SingleOption.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/AbstractSingleOption.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/Option.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/MultipleOptionType$RepeatedDelimited.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/MultipleOptionType$Delimited.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/MultipleOptionType$Repeated.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/MultipleOptionType.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ExperimentalCli.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgDescriptor.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/OptionDescriptor.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/Descriptor$defaultValueSet$2.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/Descriptor.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgumentsKt.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/MultipleArgument.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/SingleNullableArgument.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/SingleArgument.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/AbstractSingleArgument.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/Argument.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/CLIEntity.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/DefaultRequiredType$None.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/DefaultRequiredType$Required.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/DefaultRequiredType$Default.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/DefaultRequiredType.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/CLIEntityWrapper.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgumentMultipleValues.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgumentSingleNullableValue.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgumentSingleValue.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/AbstractArgumentSingleValue.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ParsingValue.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ParsingException.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$Choice$1.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$Choice.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$Companion$Choice$2.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$Companion$Choice$1.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$Companion.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$Double.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$Int.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$String.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType$Boolean.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgType.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgParserKt.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgParser$outputAndTerminate$1.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgParser$OptionPrefixStyle.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgParser$ValueOrigin.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgParser.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgParserResult.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/Subcommand.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgumentValueDelegate$DefaultImpls.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgumentValueDelegate.class  
  inflating: ./core/build/classes/kotlin/jvm/main/kotlinx/cli/ArgumentsQueue.class

Finally, the tests can be launched by running the task for executing them.  However, remember to **exclude the main source files compilation task using the `-x` option**, or Gradle will recompile those files and overwrite the classes otherwise, causing the wrong classes to be tested against.

`user $``./gradlew :kotlinx-cli:jvmTest -x :kotlinx-cli:compileKotlinJvm````
> Configure project :
INFRA: Sonatype publishing will not be possible due to missing staging repository id 'libs.repository.id'. Pass 'auto' for implicit staging.
> Configure project :kotlinx-cli
Kotlin Multiplatform Projects are an Alpha feature. See: https://kotlinlang.org/docs/reference/evolution/components-stability.html. To hide this message, add 'kotlin.mpp.stability.nowarn=true' to the Gradle properties.
Warning: Kotlin language settings function 'useExperimentalAnnotation' is deprecated and will be removed in next major releases. Please, use 'optIn' instead.
Some Kotlin/Native targets cannot be built on this linux_x64 machine and are disabled:
    * In project ':kotlinx-cli':
        * targets 'macosX64', 'macosArm64' (can be built with one of the hosts: macos_x64, macos_arm64)
To hide this message, add 'kotlin.native.ignoreDisabledTargets=true' to the Gradle properties.
Deprecated Gradle features were used in this build, making it incompatible with Gradle 7.0.
Use '--warning-mode all' to show the individual deprecation warnings.
See https://docs.gradle.org/6.6/userguide/command_line_interface.html#sec:command_line_warnings
BUILD SUCCESSFUL in 3s
2 actionable tasks: 2 executed
```
After the test execution, a test report might be available and can be accessed via opening a file named index.html in the project's source tree:

`user $``find . -type f -name index.html`
./core/build/reports/tests/jvmTest/index.html

#### Check API compatibility

If a test suite is not available at all, the package can still be checked for API compatibility with the JAR built by Gradle using [dev-util/japi-compliance-checker](https://packages.gentoo.org/packages/dev-util/japi-compliance-checker).  The tool can be installed by running:

`root #``emerge --ask dev-util/japi-compliance-checker`
If this has not been done yet, install the ebuild whose JAR is being checked onto the system.

Next, build a JAR for the Kotlin project using Gradle and find its location in the project's source tree using the instructions in [#Build a Kotlin project's JAR using Gradle](https://wiki.gentoo.org#Build_a_Kotlin_project.27s_JAR_using_Gradle).  Then, use japi-compliance-checker to compare the JAR in the source tree with the JAR installed by the ebuild:

`user $``japi-compliance-checker --lib=kotlinx-cli-jvm -v2 gentoo ./core/build/libs/kotlinx-cli-jvm-0.3.4-SNAPSHOT.jar "$(java-config -p kotlinx-cli)"`
Preparing, please wait ...
WARNING: set #1 version number to 0.3.4-SNAPSHOT (use --v1=NUM option to change it)
Using Java 1.8.0\_312
Reading classes 0.3.4-SNAPSHOT ...
Reading classes gentoo ...
Comparing classes ...
Creating compatibility report ...
Binary compatibility: 100%
Source compatibility: 100%
Total binary compatibility problems: 0, warnings: 0
Total source compatibility problems: 0, warnings: 0
Report: compat\_reports/kotlinx-cli-jvm/0.3.4-SNAPSHOT\_to\_gentoo/compat\_report.html

## Testing a package on another Kotlin feature release

Once an ebuild that contains only one identifier in its `KOTLIN_COMPAT` is verified to be working on the corresponding Kotlin feature release, it may be tested on other Kotlin feature releases available on Gentoo, so `KOTLIN_COMPAT` can include all feature releases compatible with the ebuild.

To enable convenient Kotlin ebuild testing against different Kotlin feature releases, the `KOTLIN_COMPAT_OVERRIDE` environment variable can be set to force a certain feature release to be used to build a package.  Its value should follow the same syntax as `KOTLIN_COMPAT`.  When `KOTLIN_COMPAT_OVERRIDE` is set, the Kotlin eclasses will limit the following packages to the feature release specified in its value:

- virtual/kotlin
- Kotlin libraries that satisfy *all* the following criteria:
  - The slot is specified to be the verbatim string `${KOTLIN_SLOT_DEP}`
  - The package name is passed in as an argument in an invocation of the `kotlin-utils_gen_slot_cp` function
  - The output of the aforementioned function invocation is added to `JAVA_GENTOO_CLASSPATH` or `JAVA_TEST_GENTOO_CLASSPATH`

For example, the following command tests kotlinx-cli-0.3.4.ebuild using Kotlin 1.6:

`root #``KOTLIN_COMPAT_OVERRIDE="kotlin1-6" ebuild kotlinx-cli-0.3.4.ebuild clean test install`
If the ebuild's `src_test` phase is not enabled and thus it needs to be [manually checked](https://wiki.gentoo.org#Manually_check_the_package), the following command may be used instead to build the package using a different Kotlin feature release and install it to the system:

`root #``KOTLIN_COMPAT_OVERRIDE="kotlin1-6" ebuild kotlinx-cli-0.3.4.ebuild clean merge`
If no compatibility issues are detected, then the Kotlin feature release can be added to `KOTLIN_COMPAT` in the ebuild.

## Common issues

This section documents some common issues and their solutions or workarounds.

### java.lang.OutOfMemoryError: GC overhead limit exceeded

For some Kotlin packages (especially larger packages), the following error might occur during `src_compile`:

\>>> Compiling source in /var/tmp/portage/dev-java/okhttp-4.7.2-r2/work/okhttp-parent-4.7.2 ...
 \* Compiling ...
ERROR: Exception while analyzing expression at (172,30) in /var/tmp/portage/dev-java/okhttp-4.7.2-r2/work/okhttp-parent-4.7.2/okhttp/src/main/kotlin/okhttp3/internal/cache/CacheInterceptor.kt
org.jetbrains.kotlin.utils.KotlinExceptionWithAttachments: Exception while analyzing expression at (172,30) in /var/tmp/portage/dev-java/okhttp-4.7.2-r2/work/okhttp-parent-4.7.2/okhttp/src/main/kotlin/okhttp3/internal/cache/CacheInterceptor.kt
	at org.jetbrains.kotlin.types.expressions.ExpressionTypingVisitorDispatcher.logOrThrowException(ExpressionTypingVisitorDispatcher.java:246)
	...
Caused by: java.lang.OutOfMemoryError: GC overhead limit exceeded
	at java.lang.Class.getDeclaredMethods0(Native Method)
	...
exception: org.jetbrains.kotlin.util.KotlinFrontEndException: Exception while analyzing expression at (172,30) in /var/tmp/portage/dev-java/okhttp-4.7.2-r2/work/okhttp-parent-4.7.2/okhttp/src/main/kotlin/okhttp3/internal/cache/CacheInterceptor.kt
	at org.jetbrains.kotlin.types.expressions.ExpressionTypingVisitorDispatcher.logOrThrowException(ExpressionTypingVisitorDispatcher.java:253)
	...
Caused by: java.lang.OutOfMemoryError: GC overhead limit exceeded
	at java.lang.Class.getDeclaredMethods0(Native Method)
	...
 \* ERROR: dev-java/okhttp-4.7.2-r2::spark-overlay failed (compile phase):
 \*   kotlin-utils\_kotlinc failed
 \* 
 \* Call stack:
 \*     ebuild.sh, line  127:  Called src\_compile
 \*   environment, line 2764:  Called kotlin\_src\_compile
 \*   environment, line 2711:  Called kotlin-utils\_src\_compile
 \*   environment, line 2600:  Called kotlin-utils\_kotlinc '-d' 'target/classes' '-classpath' '...' '@kotlin\_sources.lst'
 \*   environment, line 2578:  Called die

As the name of the underlying error (java.lang.OutOfMemoryError) suggests, this is caused by the JVM executing the Kotlin compiler running out of memory.

To solve this issue, simply allocate more memory to the JVM.  This can be done by setting a proper maximum Java heap size using the `-Xmx` option for JVM.  The Kotlin eclasses supports a `KOTLIN_KOTLINC_JAVA_OPTS` variable for specifying options for the JVM that runs the Kotlin compiler.  The exact proper value varies depending on the package, but a reasonable value can usually be found after only a few trial-and-errors.

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) Yuan Liao. [Introducing ebuilds That Build Kotlin Core Libraries from Source](https://leo3418.github.io/2021/07/05/gentoo-build-kt-src.html#difficulty-in-building-kotlin-from-portage), [Leo3418's Personal Site](https://leo3418.github.io/), July 5th, 2021.  Retrieved on July 10th, 2021.
2. [↑](https://wiki.gentoo.org#cite_ref-2) [Gradle 7.3 Release Notes](https://docs.gradle.org/7.3/release-notes.html), November 9th, 2021.  Retrieved on February 5th, 2022.
3. [↑](https://wiki.gentoo.org#cite_ref-3) [gradle.properties](https://github.com/Kotlin/kotlinx-cli/blob/v0.3.4/gradle.properties#L7), [Kotlin/kotlinx-cli GitHub repository](https://github.com/Kotlin/kotlinx-cli), December 2nd, 2021.  Retrieved on February 4th, 2022.
4. [↑](https://wiki.gentoo.org#cite_ref-4) [Kotlin compiler options/-no-reflect](https://kotlinlang.org/docs/compiler-reference.html#no-reflect), [Kotlin docs](https://kotlinlang.org/docs/home.html), December 1st, 2021.  Retrieved on February 4th, 2022.
5. [↑](https://wiki.gentoo.org#cite_ref-5) [Kotlin compiler options/-jdk-home path](https://kotlinlang.org/docs/compiler-reference.html#jdk-home-path), [Kotlin docs](https://kotlinlang.org/docs/home.html), December 1st, 2021.  Retrieved on February 4th, 2022.
