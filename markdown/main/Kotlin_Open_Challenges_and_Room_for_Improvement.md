<!-- source: https://wiki.gentoo.org/wiki/Kotlin/Open_Challenges_and_Room_for_Improvement | group: Gentoo Wiki (Main) | wiki-title: Kotlin/Open Challenges and Room for Improvement -->
---
title: Kotlin/Open Challenges and Room for Improvement
url: https://wiki.gentoo.org/wiki/Kotlin/Open_Challenges_and_Room_for_Improvement
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-03-18"
fingerprint: "6f00f60900e75309"
license: CC BY-SA 4.0
---

# Kotlin/Open Challenges and Room for Improvement

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page lists things that still can be done to improve the Kotlin packages and support on Gentoo.

## More streamlined installation process

At this moment, third-party Kotlin packages cannot be directly installed with a single emerge invocation on a system where dev-lang/kotlin-bin has not been installed yet; such attempt would end up with circular dependencies. The following example demonstrates the issue by trying to install dev-java/okio from the Spark overlay:

`root #``emerge --ask dev-java/okio`
These are the packages that would be merged, in order:
Calculating dependencies... done!
\[ebuild  N    \~\] dev-java/okio-2.6.0-r2:2.6::spark-overlay  USE="-source -test" KOTLIN\_SINGLE\_TARGET="kotlin1-4 -kotlin1-5 -kotlin1-6" 255 KiB
\[nomerge       \]  dev-java/kotlin-stdlib-1.4.32:1.4::spark-overlay  USE="-binary -source -test" KOTLIN\_SINGLE\_TARGET="kotlin1-4" 
\[nomerge       \]   dev-java/kotlin-core-builtins-1.4.32:1.4::spark-overlay  USE="-source" KOTLIN\_SINGLE\_TARGET="kotlin1-4" 
\[nomerge       \]    virtual/kotlin-1.4:1.4::spark-overlay 
\[ebuild  N    \~\]     dev-lang/kotlin-bin-1.4.32:1.4::spark-overlay  61408 KiB
\[ebuild  N    \~\]      dev-java/kotlin-stdlib-1.4.32:1.4::spark-overlay  USE="-binary -source -test" KOTLIN\_SINGLE\_TARGET="kotlin1-4" 33371 KiB
\[ebuild  N    \~\]       dev-java/kotlin-core-builtins-1.4.32:1.4::spark-overlay  USE="-source" KOTLIN\_SINGLE\_TARGET="kotlin1-4" 0 KiB
\[ebuild  N    \~\]        virtual/kotlin-1.4:1.4::spark-overlay  0 KiB
Total: 5 packages (5 new), Size of downloads: 95033 KiB
 \* Error: circular dependencies:
(virtual/kotlin-1.4:1.4/1.4::spark-overlay, ebuild scheduled for merge) depends on
 (dev-lang/kotlin-bin-1.4.32:1.4/1.4::spark-overlay, ebuild scheduled for merge) (runtime)
  (dev-java/kotlin-stdlib-1.4.32:1.4/1.4::spark-overlay, ebuild scheduled for merge) (buildtime)
   (virtual/kotlin-1.4:1.4/1.4::spark-overlay, ebuild scheduled for merge) (buildtime)
It might be possible to break this cycle
by applying the following change:
- dev-java/kotlin-stdlib-1.4.32 (Change USE: +binary)
Note that this change can be reverted, once the package has been installed.
Note that the dependency graph contains a lot of cycles.
Several changes might be required to resolve all cycles.
Temporarily changing some use flag for all packages might be the better option.

Although the solution proposed by Portage, which is to enable the `binary` `USE` flag for dev-java/kotlin-stdlib, *does* fix the issue and thus allow dev-java/okio to be installed, relying on the users themselves to apply the solution is not feasible.  Multiple instances of users getting overwhelmed by unexpected error messages from Portage and then neglecting Portage's suggestion that would actually work have been observed in the [#gentoo](ircs://irc.libera.chat/#gentoo) ([webchat](https://web.libera.chat/#gentoo)) IRC channel.  Furthermore, requiring two emerge commands instead of just one might also compromise the user experience, even for expert Gentoo users.

This issue partly stems from the fact that there is an inherent circular dependency between the Kotlin compiler and Kotlin libraries.  The Kotlin compiler is [self-hosting](<https://en.wikipedia.org/wiki/Self-hosting_(compilers)>) since the bulk of it is written in Kotlin itself, and these Kotlin portions of code depend on kotlin-stdlib and kotlin-reflect; yet kotlin-stdlib and kotlin-reflect are both mainly written in Kotlin too and thus need a Kotlin compiler to build.  [Bootstrapping](<https://en.wikipedia.org/wiki/Bootstrapping_(compilers)>) is a common technique used to address such a situation.

Currently, the Kotlin toolchain for Gentoo is split into multiple packages, including a compiler package — namely dev-lang/kotlin-bin — and several library packages — which are dev-java/kotlin-stdlib, dev-java/kotlin-reflect, and others.  Only the library packages support being built from source; the compiler package installs pre-built binary files provided by the upstream, which is implied by its `-bin` package name suffix.  Therefore, only the library packages need to go through the bootstrapping process.

The Kotlin compiler and library ebuilds currently implement the bootstrapping process as follows:

1. Pre-built binary JARs of kotlin-stdlib and kotlin-reflect are installed by [dev-java/kotlin-stdlib-bootstrap](https://github.com/6-6-6/spark-overlay/tree/master/dev-java/kotlin-stdlib-bootstrap) and [dev-java/kotlin-reflect-bootstrap](https://github.com/6-6-6/spark-overlay/tree/master/dev-java/kotlin-reflect-bootstrap).  No source files need to be compiled for them, so they do not have any build-time dependencies.  These JARs are installed to ${EROOT}/usr/share/kotlin-stdlib-${SLOT}/lib/kotlin-stdlib.jar and ${EROOT}/usr/share/kotlin-reflect-${SLOT}/lib/kotlin-reflect.jar respectively.  These JARs are not intended to be used by other Java or Kotlin packages, so they are not registered with java-config.
2. dev-lang/kotlin-bin is installed.  The Kotlin compiler's dependency libraries are installed to ${EROOT}/opt/kotlin-bin-${SLOT}.  The JAR for each library with a dedicated package available (e.g. annotations-13.0.jar provided by dev-java/jetbrains-annotations:13) is replaced by a symbolic link to the JAR installed by the package.  kotlin-stdlib.jar and kotlin-reflect.jar are symbolic links to ${EROOT}/usr/share/kotlin-stdlib-${SLOT}/lib/kotlin-stdlib.jar and ${EROOT}/usr/share/kotlin-reflect-${SLOT}/lib/kotlin-reflect.jar respectively for this reason.  Before those Kotlin libraries are compiled from source, these symbolic links point to the pre-built binary JARs installed by dev-java/kotlin-stdlib-bootstrap and dev-java/kotlin-reflect-bootstrap.
  1. At this point, every binary file for the Kotlin toolchain installed on the system is pre-built by the upstream.  The toolchain is usable and effectively at [stage 1](<https://en.wikipedia.org/wiki/Bootstrapping_(compilers)#Process>).
3. The Kotlin library packages, which are built from source, can now be installed.  In particular, dev-java/kotlin-stdlib and dev-java/kotlin-reflect each installs its JAR to ${EROOT}/usr/share/kotlin-stdlib-${SLOT}/lib/kotlin-stdlib.jar and ${EROOT}/usr/share/kotlin-reflect-${SLOT}/lib/kotlin-reflect.jar respectively as well to replace the pre-built binary JARs (dev-java/kotlin-stdlib-bootstrap and dev-java/kotlin-reflect-bootstrap weakly block dev-java/kotlin-stdlib and dev-java/kotlin-reflect for this reason).  This replacement allows the new JARs built from source within Portage to be used by the Kotlin compiler.  dev-java/kotlin-stdlib and dev-java/kotlin-reflect also register their JAR with java-config because they are *the* Kotlin library packages intended to be used by other packages as dependencies.
  1. At this point, binary files for Kotlin libraries installed on the system are built from source, whereas the remaining binary files for the Kotlin toolchain are still pre-built by the upstream. The Kotlin libraries have effectively reached stage 2.

Because the Kotlin compiler can be linked with either the pre-built binary JARs or the JARs built from source within Portage, dev-lang/kotlin-bin contains the following specifications in its `DEPEND`:

This causes the circular dependencies error presented in the above example in the following way:

1. As a package containing Kotlin source files that use kotlin-stdlib, dev-java/okio depends on virtual/kotlin and dev-java/kotlin-stdlib. The only existing provider of virtual/kotlin as of now is dev-lang/kotlin-bin. So, dev-lang/kotlin-bin and dev-java/kotlin-stdlib are added to the dependency graph.
2. dev-java/kotlin-stdlib also depends on virtual/kotlin because it needs to compile Kotlin source files, so for the same reason, it depends on dev-lang/kotlin-bin.
3. dev-lang/kotlin-bin uses an any-of dependency specification defined by the `||` operator to specify that either dev-java/kotlin-stdlib-bootstrap or dev-java/kotlin-stdlib may be used to satisfy the dependency on kotlin-stdlib. **Portage will pick dev-java/kotlin-stdlib instead of dev-java/kotlin-stdlib-bootstrap to satisfy the any-of dependency specification** because the former package has already been added to the dependency graph.
4. The circular dependency is thus produced.

### Attempted solutions

The following solutions that were believed to be able to resolve the issue have been attempted, but they have been deemed to be not meeting the expectations after experiments and tests.

#### Two Kotlin compiler packages

In this attempted solution, a new dev-lang/kotlin-bootstrap package derived from dev-lang/kotlin-bin was created. dev-lang/kotlin-bootstrap did not use symbolic links for kotlin-stdlib and kotlin-reflect; instead, it always used the pre-built binary JARs provided by the upstream for those libraries, so it could eliminate the any-of dependency specification that would otherwise not work as intended.

Then, dev-lang/kotlin-bin was changed to a package that unconditionally depended on dev-java/kotlin-stdlib and dev-java/kotlin-reflect, which also allowed the any-of dependency specification to be removed.

virtual/kotlin was not changed; dev-lang/kotlin-bootstrap was *not* registered as a provider of it because it was intended to be used only to bootstrap the Kotlin libraries, not to build other third-party Kotlin packages.

On the other hand, the Kotlin library packages would now have an any-of dependency specification for dev-lang/kotlin-bootstrap and virtual/kotlin.  The idea was to fall back to dev-lang/kotlin-bootstrap if no providers of virtual/kotlin had been installed.  This condition would hold at least before the Kotlin libraries had reached stage 2.  However, if any provider of virtual/kotlin was available, then it would be used as the first preference, even if dev-lang/kotlin-bootstrap was installed.  This behavior was implemented using the `has_version` ebuild helper function, in a way similar to [what dev-lang/go ebuilds are doing](https://gitweb.gentoo.org/repo/gentoo.git/tree/dev-lang/go/go-1.18.ebuild?id=56b98ea4533f3da308ae1623608cf05e8b054f0e#n115).

The intended bootstrapping process with this implementation was:

1. dev-lang/kotlin-bootstrap would be installed first because it did not have any dependency on the Kotlin toolchain.
2. dev-java/kotlin-stdlib and dev-java/kotlin-reflect could then be built with dev-lang/kotlin-bootstrap.
3. Finally, dev-lang/kotlin-bin could be installed since the dependencies on dev-java/kotlin-stdlib and dev-java/kotlin-reflect would have been satisfied. virtual/kotlin might also be installed together.
4. dev-lang/kotlin-bootstrap could be optionally cleaned at this point because virtual/kotlin had been installed.

Unfortunately, this would cause the `emerge --ask dev-lang/kotlin-bin` command mentioned in [Kotlin#Emerge](https://wiki.gentoo.org/wiki/Kotlin#Emerge) to return a circular dependencies error too, in the following way:

1. dev-lang/kotlin-bin was explicitly requested to be installed in the command-line arguments, so its dependencies, including dev-java/kotlin-stdlib, were added to the dependency graph.
2. dev-java/kotlin-stdlib had the any-of dependency specification defined by the `||` operator. **Portage would pick virtual/kotlin instead of dev-lang/kotlin-bootstrap to satisfy it** because a provider of the virtual, which was dev-lang/kotlin-bin, had already been added to the dependency graph.
3. virtual/kotlin would cause dev-lang/kotlin-bin to be added to the dependency graph again, producing a circular dependency.

## kotlin-libs.eclass

### Support module-info.java for Java 9 and above

The Kotlin library packages are now eligible for a Java 9+ retrofit that incorporates their module-info.java into the JARs being built.  When the Kotlin library packages were initially created during July 2021, virtual/jdk:11 was not stabilized.  Now that Java 11 is available as the system VM for users on stable keywords, it is time to update the Kotlin library packages and eclasses to let them support the [Java Platform Module System](https://en.wikipedia.org/wiki/Java_Platform_Module_System) (JPMS) introduced in Java 9.

Kotlin library packages that provide module-info.java can be searched by finding files with this file name in the libraries directory under the Kotlin programming language project's source tree.

`user $``find libraries -type f -name module-info.java`
libraries/kotlin.test/junit/src/java9/java/module-info.java
libraries/kotlin.test/junit5/src/java9/java/module-info.java
libraries/kotlin.test/jvm/src/java9/java/module-info.java
libraries/kotlin.test/testng/src/java9/java/module-info.java
libraries/reflect/api/src/java9/java/module-info.java
libraries/stdlib/jdk7/java9/module-info.java
libraries/stdlib/jdk8/java9/module-info.java
libraries/stdlib/jvm/java9/module-info.java
libraries/tools/kotlin-gradle-plugin-integration-tests/src/test/resources/testProject/kapt2/jpms-module/src/main/java/module-info.java
libraries/tools/kotlin-maven-plugin-test/src/it/java9/test-jlink-modular-artifacts/app/src/main/java/module-info.java

Kotlin library packages that both are [available on Gentoo](https://wiki.gentoo.org/wiki/Kotlin#Available_packages) and provide module-info.java include:

- dev-java/kotlin-reflect
- dev-java/kotlin-stdlib
- dev-java/kotlin-stdlib-jdk7
- dev-java/kotlin-stdlib-jdk8
- dev-java/kotlin-test
- dev-java/kotlin-test-junit

Please note that although java-pkg-simple.eclass, which is the basis of the Kotlin eclasses, supports compiling module-info.java ([bug #796875](https://bugs.gentoo.org/show_bug.cgi?id=796875)), Kotlin library packages' module-info.java files are supposed to be handled in a different way, which is to follow the standards defined in [JEP 238](https://openjdk.java.net/jeps/238), making the resulting JARs *multi-release JARs*:

- java-pkg-simple.eclass puts module-info.eclass at the top level of the resulting JAR, whereas in Kotlin library packages' JARs, it is supposed to be stored at META-INF/versions/9/module-info.class:

`user $``unzip -l kotlin-stdlib-1.6.255-SNAPSHOT.jar | grep -F module-info.class`
982  02-01-1980 00:00   META-INF/versions/9/module-info.class

- The META-INF/MANIFEST.MF file in the JAR of each Kotlin library package that provides module-info.java should contain the line `Multi-Release: true`:

**`META-INF/MANIFEST.MF`**

**for kotlin-stdlib 1.6.10 built from Gradle**

Therefore, instead of relying on java-pkg-simple.eclass, the Kotlin library packages' module-info.java should be handled with logic compliant with JEP 238. It is recommended that such logic is added to java-pkg-simple.eclass because some packages in the Gentoo ebuild repository are having a similar situation too, so this could benefit both the Gentoo ebuild repository and the Spark overlay. If this is not feasible, the above behaviors may be implemented in kotlin-libs.eclass.

### Target Java 1.8 instead of 1.6

Unlike the majority of Java packages in the Gentoo ebuild repository, which target Java 1.8, Kotlin library packages in the Spark overlay currently target Java 1.6 to follow the upstream<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.  If the Kotlin packages and eclasses are aimed for submission to the official Gentoo ebuild repository, this might become an issue that must be resolved.

First and foremost, the [Java project](https://wiki.gentoo.org/wiki/Project:Java) now tests all new Java ebuilds against JDK 17, which has dropped support for using Java 1.6 as the target.  Hence, the Kotlin library packages cannot be built with JDK 17 yet, making them less future-proof.  Even though virtual/jdk:17 is currently masked, testing against and supporting JDK 17 early would help the future transition to Java 17 on Gentoo go smoother.

Next, the Java versions the Kotlin library packages are declared to depend on are not strictly consistent with the Java versions the JARs built by those packages actually support.  Currently, the JDK and JRE dependencies of all Kotlin library packages are following the requirements of [Gentoo Java Packing Policy](https://wiki.gentoo.org/wiki/Gentoo_Java_Packing_Policy#Minimum_JRE.2FJDK_Version):

```
DEPEND=">=virtual/jdk-1.8:*"
RDEPEND=">=virtual/jre-1.8:*"
```
The version specifications in the JDK and JRE atoms have implications to not only Portage's dependency resolver but also Java eclasses as to the values of the `-source` and `-target` options for javac.  With `>=virtual/jdk-1.8:*` and `>=virtual/jre-1.8:*`, javac arguments will include `-source 1.8 -target 1.8`.  With `>=virtual/jdk-11:*` and `>=virtual/jre-11:*`, they will become `-source 11 -target 11` instead.

However, the upstream uses `-source 1.6 -target 1.6` to compile Java sources in most Kotlin library modules, and the Kotlin library packages for Gentoo are following suit.  This breaks the consistency between the Java version specified in `DEPEND` and `RDEPEND` and the Java version specified for the `-source` and `-target` options.

To comply with the Java packaging conventions on Gentoo, the Kotlin library packages should compile Java sources with `-source 1.8 -target 1.8`.  This would require the following actions:

- Change the value of `KOTLIN_JAVA_WANT_SOURCE_TARGET` [in kotlin-libs.eclass](https://github.com/6-6-6/spark-overlay/blob/16c79715f51338dc550cddb2e3911036fefd6d83/eclass/kotlin-libs.eclass#L149) to `1.8`
- Change all occurrences of `-jvm-target 1.6` in Kotlin compiler arguments in Kotlin eclasses and library ebuilds to `-jvm-target 1.8`
- Test the changes thoroughly by running test suites of both the Kotlin library packages themselves and third-party Kotlin packages

## dev-java/kotlin-reflect

### Reproduce the JAR built by Gradle

As mentioned in [Kotlin#Available packages](https://wiki.gentoo.org/wiki/Kotlin#Available_packages), there is not a way to reproduce from Portage a JAR for kotlin-reflect that is structurally identical to the upstream's pre-built JAR (which is presumably built with Gradle) yet, as the following command would fail:

`root #``FEATURES="test" ALLOW_TEST="all" emerge --ask --oneshot dev-java/kotlin-reflect````
WARNING: different package names ("kotlin-reflect" and "kotlin")
comparing packages ...
creating report ...
result: CHANGED (0.3%)
report: kotlin-reflect-pkgdiff.html
 * ERROR: dev-java/kotlin-reflect-1.6.10::spark-overlay failed (test phase):
 *   pkgdiff returns 1, check the report in /var/tmp/portage/dev-java/kotlin-reflect-1.6.10/work/kotlin-1.6.10/kotlin-reflect-pkgdiff.html
 * 
 * Call stack:
 *     ebuild.sh, line  127:  Called src_test
 *   environment, line 2946:  Called kotlin-libs_src_test
 *   environment, line 2576:  Called java-pkg-simple_test_with_pkgdiff_
 *   environment, line 1110:  Called die
```
Although this does not seem to affect the behavioral correctness of dev-java/kotlin-reflect, it would be great if a JAR whose structure is identical to the JAR pre-built by the upstream can be produced by the ebuild.  Because kotlin-reflect does not have a test suite, the tests enabled by `KOTLIN_TESTING_FRAMEWORKS="pkgdiff"` are the only ways to verify the JAR built by the ebuild as of now.

The acceptance criterion of this task's result is that the aforementioned command completes without errors.  After that, the [test restriction](https://github.com/6-6-6/spark-overlay/blob/16c79715f51338dc550cddb2e3911036fefd6d83/dev-java/kotlin-reflect/kotlin-reflect-1.6.10.ebuild#L19-L21) on dev-java/kotlin-reflect can be lifted.

## dev-java/kotlinx-coroutines-core-bin

### Build the package from source

[kotlinx.coroutines](https://github.com/Kotlin/kotlinx.coroutines) is a Kotlin coroutines support extension that is not a Kotlin core library under the Kotlin programming project proper; yet it is a dependency of the Kotlin compiler to support the [REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) launched via the kotlin command.  Currently, only a Gentoo package that directly installs the binary JAR pre-built by the upstream is available.  Using the instructions in the [Kotlin Package Maintainer Guide](https://wiki.gentoo.org/wiki/Kotlin/Package_Maintainer_Guide), it might be feasible to create a Gentoo package that builds kotlinx.coroutines from source, just like other Kotlin core libraries.

## Automation tool for Kotlin library ebuild generation

Currently, maintenance of the Kotlin library packages heavily relies on a *manual* procedure to collect compiler arguments that the Kotlin Gradle plugin uses to build them, which is outlined in the Kotlin Package Maintainer Guide.  As a more agile development model has been adapted by the Kotlin programming language project since Kotlin 1.5, the upstream has started to deliver a new feature release every six months<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>.  With this release cadence, the feasibility of relying on the manual procedure has been compromised.

Luckily, the bulk of the compiler arguments collection procedure involves only invocations of ./gradlew --debug and common Unix utilities like grep and sed, making it possible to automate some, if not all, steps in the procedure. A tool that automates these steps to the maximum possible extent could help Kotlin library package maintainers cope with the rapid release schedule adapted by the upstream.

The automation tool can either generate complete and installable ebuilds or create partial ebuilds that the maintainers can amend to make them fully-functional.  Ideally, it should at least be able to run a Gradle task in the Kotlin programming language project's source tree with the `--debug` option enabled, collect the debug logs, wrangle with the logs to find the arguments to the Kotlin compiler and javac, and output the corresponding values of kotlin-utils.eclass variables like `KOTLIN_COMMON_SOURCES_DIR`.  A great implementation of such a tool should also be general enough so it can create ebuilds for not just the Kotlin library packages but most third-party Kotlin packages as well.

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [libraries/stdlib/jvm/build.gradle](https://github.com/JetBrains/kotlin/blob/v1.6.10/libraries/stdlib/jvm/build.gradle#L7), [JetBrains/Kotlin GitHub repository](https://github.com/JetBrains/kotlin), January 29th, 2021.  Retrieved on February 11th, 2022.
2. [↑](https://wiki.gentoo.org#cite_ref-2) [eclass/kotlin-libs.eclass](https://github.com/6-6-6/spark-overlay/blob/16c79715f51338dc550cddb2e3911036fefd6d83/eclass/kotlin-libs.eclass#L123-L124), [6-6-6/spark-overlay GitHub repository](https://github.com/6-6-6/spark-overlay), February 5th, 2022.  Retrieved on February 11th, 2022.
3. [↑](https://wiki.gentoo.org#cite_ref-3) Alina Dolgikh. [New Release Cadence for Kotlin and the IntelliJ Kotlin Plugin](https://blog.jetbrains.com/kotlin/2020/10/new-release-cadence-for-kotlin-and-the-intellij-kotlin-plugin/), [The Kotlin Blog](https://blog.jetbrains.com/kotlin/), October 8th, 2020.  Retrieved on February 11th, 2022.
