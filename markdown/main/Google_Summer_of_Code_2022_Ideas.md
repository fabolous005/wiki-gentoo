<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2022/Ideas | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2022/Ideas -->
---
title: Google Summer of Code/2022/Ideas
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2022/Ideas
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-03-31"
fingerprint: b731b89c4a75a738
license: CC BY-SA 4.0
---

# Google Summer of Code/2022/Ideas

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![GSoC 2022 logo GSoC 2022 logo](https://wiki.gentoo.org/images/c/ce/Gsoc2016-sun-160x160.png)

Want to spend your summer contributing full-time to Gentoo, and get paid for it? Gentoo is in its 10th year in the Google Summer of Code. In the past, **most of our successful students have become Gentoo developers**, so your chances of becoming one are very good if you're accepted into this program.

Most ideas listed here have a contact person associated with them. Please get in touch with them earlier rather than later to develop your idea into a complete application. You can find many of them on Libera.Chat's IRC network under the same username. If there is no contact information, please join the [gentoo-soc mailing list](https://www.gentoo.org/get-involved/mailing-lists/) or [#gentoo-soc on the Libera.Chat IRC network](irc://irc.gentoo.org/gentoo-soc), and we will work with you to find a mentor and discuss your idea.

**You don't have to apply for one of these ideas! You can come up with your own**, and as long as it fits into Gentoo, we'll be happy to work with you to develop it. Remember, your project needs to have deliverables in less than 3 months of work in most cases. Be ambitious but not too ambitious ;)

## Students, please read this first

We have a custom application template that we will ask you to fill out. Here it is:

**Congratulations on applying for a project with Gentoo!** To improve your chances of succeeding with this project, we want to make sure you're sufficiently prepared to invest a full summer's worth of time on it. **In addition to the usual application, there are 2 specific actions and 2 pieces of info we would like to see from you:**

- **Use the tools that you will use in your project to make changes to code** (e.g., source code management \[SCM\] software such as CVS, Subversion, or git). Please use the same SCM as you will use for your project to check out one of our [repositories](https://www.gentoo.org/get-involved/get-code/), make a change to it, and post that change as a [patch](<https://en.wikipedia.org/wiki/Patch_(Unix)>) on a mailing list or [bug](https://bugs.gentoo.org/). Please fix a real bug reported in [Bugzilla](https://bugs.gentoo.org/) to show that you can use the tools to make a meaningful change. Your contact in Gentoo can help you determine which SCM and repository you should use for this as well as a good bug to fix. If your idea doesn't have a contact, please get in touch with us on the [gentoo-soc mailing list](https://www.gentoo.org/get-involved/mailing-lists/) or in real-time [on IRC](irc://irc.gentoo.org/gentoo-soc). Once you've made your change, link to it from your application.

- **Participate in our development community.** Please make a post to one of our [mailing lists](https://www.gentoo.org/get-involved/mailing-lists/) and link to it from your application ([archives.gentoo.org](https://archives.gentoo.org) holds past postings). The gentoo-soc list would be a good starting point, if you aren't subscribed to any others already. The best posts would be an introduction of the project you're applying for and a little background about you, to introduce yourself to the community and get some broader input about your project.

- **Give us your contact info and working hours.** Please provide your email address, home mailing address, and phone number. This is a requirement and provides for accountability on both your side and ours. Also, please tell us what hours you will be working and responsive to contact via email and IRC; these should sum to at least 35 hours a week.

These actions are things you will do extremely commonly as an open-source developer, and they really aren't that hard, so don't let them hold you back! The remainder of the application is free-form. **Please read our [application guidelines](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2022/Application_Guidelines) and [Google's FAQ](https://developers.google.com/open-source/gsoc/faq) to complete it.** Good luck!

## Ideas

### Create new idea



### Existing ideas

**Our best proposals, and a significant proportion of our total acceptances every year, come from student-initiated ideas** rather than those suggested by Gentoo developers. We highly encourage you to suggest your own idea based on what you think would make Gentoo a better distribution. If you do so, we **strongly recommend** you work with a potential mentor to develop your idea **before** proposing it formally. You can find a potential mentor by contacting BlueKnight or via discussion on the gentoo-soc mailing list or #gentoo-soc IRC channel.


| Contacts | Required Skills | 
|---|---|
|  |  | 
| Expected Project Size | Expected Outcomes | 
| 175 hours/350 hours |  | 
| Project Difficulty |  | 
| Easy/Medium/Hard |  | 

Portage's codebase is aging (as old as Gentoo!) and needs modernisation for contemporary Python style and techniques. This project involves picking components of the Portage codebase, understanding it, annotating it (with extensive comments), and then refactoring as appropriate.

A lot of this technical code debt makes implementing new GLEPs and features more expensive and time-consuming than it needs to be. This project is to work on modernising the codebase in appropriately-sized chunks. Rewriting whole modules is less important than documenting the existing code, as well as researching the reasons for its existing behaviour, and refactoring (even small amounts) in the course of that work.



| Contacts | Required Skills | 
|---|---|
|  |  | 
| Expected Project Size | Expected Outcomes | 
| 175 hours or 350 hours, depending on appetite for larger refactoring. |  | 
| Project Difficulty |  | 
| Easy or Medium. Some sections will be easier and the codebase is large enough so that people can choose components according to the appropriate difficulty. |  | 

Improving on the new binpkgs code, we could construct consistent and clean build environments for packages. By generating binpkgs as SquashFS, portage could construct build environments in a mount namespace, with constricted available packages, to detect build system bugs like automatic dependency detection, hard-coded paths, direct calls to gcc/clang, missing (or even extraneous) dependencies. This decoupling of the build and host environment also allows us to break apart build and install steps in the build graph, so that circular dependencies could be solved by building temporary packages with cycle-breaking flags (for instance, `harfbuzz[-truetype]`, `freetype[harfbuzz]`, `harfbuzz[truetype]`, install both).


| Contacts | Required Skills | 
|---|---|
|  |  | 
| Expected Project Size | Expected Outcomes | 
| 175 hours |  | 
| Project Difficulty |  | 
| Hard, mainly because handling Portage codebase |  | 

The alternative libc musl is strict about standards compliance. This project is to work in improving the number of packages, especially popular and important ones, which build and work correctly (including test suites passing) on musl systems.

We already have a substantial list ([bug #430702](https://bugs.gentoo.org/show_bug.cgi?id=430702)) of packages which need porting to musl, but there will inevitably be more not yet found by our automated testing. The work for this project can be scoped as necessary.

Students would be asked to work on a *musl porting guide* to aid future work and other Gentoo developers based on their experiences and knowledge developed during the project, building on [existing stub/draft work](https://wiki.gentoo.org/wiki/User:Sam/Musl_porting_notes).



| Contacts | Required Skills | 
|---|---|
|  |  | 
| Expected Project Size | Expected Outcomes | 
| 175 hours | Any subset of the following items, as long as it matches the project size: | 
| Project Difficulty |  | 
| Easy to medium depending on which bugs are worked on. There are enough which are easy but experienced candidates will have the opportunity to take on harder bugs if interested. |  | 

Gentoo's eclasses for language support (or "bindings") have gone through several evolutions until the community largely settled on the "Python model" (now used for Lua and others).

Ruby and Java haven't yet been adapted to this newer dependency model. This often leads to counterintuitive conflicts for users and confusing errors. It's not certain that the dependencies specified are completely accurate as-is or can be expressed as necessary.

This project will lead to a significantly improved user experience *and* fix a whole slew of bugs in the process!

We also have a smaller-scope opportunity to work on the OCaml ecosystem.

Options:

1. Convert either (or both) the Java and Ruby eclasses to this modern style/model. This will make developing ebuilds with Java (or Ruby) support far easier and make dependencies more correct for users, reducing the pain of upgrades. Candidates may work on either (or both if they're feel adventurous) the Java or Ruby ecosystems. Both are in need of similar treatment:
  1. In a new revision to use the modern Python-style `JAVA_COMPAT` and `RUBY_COMPAT` configurations;
  2. Introduce appropriate functions for use in ebuilds (again based on the Python model and, nowadays, Lua);
  3. Document these frameworks and their usage;
  4. Update ebuilds to make use of them!
2. OCaml
  1. Unify the OCaml eclasses if possible into one eclass with multiple options/variables;
  2. Add a mechanism into OCaml eclasses to indicate if they support *ocamlopt*;
  3. Audit our existing OCaml packages to properly support -ocamlopt and ocamlopt setups;
  4. Document these frameworks and their usage;
  5. Update ebuilds to make use of them!

For more on Ruby, see:




| Contacts | Required Skills | 
|---|---|
|  |  | 
| Expected Project Size | Expected Outcomes | 
| 175 hours or 350 hours, depending on what tasks are planned. This project could easily go as far as 350 hours depending on discussions with the candidate: it will take longer if doing both Java *and* Ruby, and it'll take longer if converting either a significant number of ebuilds in tree, or all ebuilds in tree. 170 hours if doing the OCaml option instead. |  | 
| Project Difficulty |  | 
| Hard (relatively). It should be totally manageable for somebody familiar with ebuilds (and eclasses if possible), but if you are new to working with ebuilds, another task may be more suitable. Ruby is likely to be easier than Java because Java has mixed build systems (e.g. Maven). The task will be easier if the candidate has familiarity with the language & ecosystem they choose. The OCaml option is easier but has less impact for our users; we have a number of OCaml packages (mostly as dependencies for useful applications) but Java and Ruby are far larger ecosystems, and hence have more impact. |  | 

The [Spark overlay](https://github.com/6-6-6/spark-overlay) is an ebuild repository for JVM-based big data infrastructure systems. Currently, it enables users to install [Apache Spark](https://spark.apache.org/) and the [H2O machine learning platform](https://github.com/h2oai/h2o-3) to a Gentoo system easily via Portage. It is also the home of the first set of [Kotlin library ebuilds](https://wiki.gentoo.org/wiki/Kotlin#Available_packages) that are built from source and [Kotlin eclasses](https://wiki.gentoo.org/wiki/Kotlin/Package_Maintainer_Guide#Eclasses) which allow more ebuilds for third-party Kotlin packages (e.g. [okio](https://github.com/square/okio), [clikt](https://github.com/ajalt/clikt)) to be created.

The Spark overlay has featured in two previous GSoCs ([2020](https://summerofcode.withgoogle.com/archive/2020/projects/5562637166837760), [2021](https://summerofcode.withgoogle.com/archive/2021/projects/5162433018068992)) and is still being actively maintained. It has gone through a massive update of packages for Java 11 after it had been enabled for users on a stable keyword ([bug #810613](https://bugs.gentoo.org/show_bug.cgi?id=810613)), a repository-wide migration to Log4j >=2.17.1 after it had been added to the official Gentoo ebuild repository ([bug #830910](https://bugs.gentoo.org/show_bug.cgi?id=830910)), as well as several additional security updates to packages, including Jetty 9.4.44, Jersey 2.35, and Jackson 2.13.0. These maintenance efforts have been striving to match the quality of packages in the Spark overlay to Java packages in the Gentoo repository to the maximum possible extent.

Despite continuous maintenance activities, the Spark overlay could still use some improvements that the current maintainer has not made due to his limited availability. The list below might look overwhelming, but **you are more than welcome to just plan to do a subset of the tasks in your project proposal**, as long as **the amount of work they might involve reasonably matches the GSoC program's length**.

- The Apache Spark version shipped in the Spark overlay should be updated. The upstream has released version 3.2.1 in January 2022, whereas the Spark overlay currently provides 3.0.0-preview2, which is a pre-release version.
- Some packages in the Spark overlay are still on a vulnerable version and should be updated to a patched version. Affected packages include [Hadoop 2.7.4](https://hadoop.apache.org/cve_list.html), [Netty 4.1.42](https://www.cvedetails.com/vulnerability-list/vendor_id-20711/Netty.html), and possibly more.
- More H2O extensions should be added to the Spark overlay. Currently, packages for Algos and TargetEncoder extensions are offered. Some other key extensions that are not shipped in the Spark overlay yet include XGBoost and AutoML.
- The aforementioned Kotlin ecosystem for Gentoo has some potential areas of improvement, which have been documented in [Kotlin/Open Challenges and Room for Improvement](https://wiki.gentoo.org/wiki/Kotlin/Open_Challenges_and_Room_for_Improvement).
- Resolve some other issues in the Spark overlay's [issue tracker](https://github.com/6-6-6/spark-overlay/issues).
- The Spark overlay currently does not have a reliable mechanism to report security issues of packages in it. The infamous Log4j 2 vulnerability disclosed in December 2021 has drawn attention from both software developers and non-professional users to security of Java packages. While critical vulnerabilities of vital JVM libraries like Log4j can usually be easily noticed by Spark overlay maintainers thanks to wide news coverage on such important events, other critical vulnerabilities of less commonly-used packages might not receive the maintainers’ attention in time. This caused unacceptable postponement in delivery of the Jetty 9.4.44, Jersey 2.35, and Jackson 2.13.0 security updates.



| Contacts | Required Skills | 
|---|---|
|  |  | 
| Expected Project Size | Expected Outcomes | 
| 175 hours or 350 hours, depending on what tasks are planned | Any subset of the following items, as long as it matches the project size: | 
| Project Difficulty |  | 
| Medium to hard, depending on what tasks are planned |  | 

[RISC-V](https://riscv.org) is an emerging open CPU architecture that is starting to be adopted well beyond the embedded domain; the European Processor Initiative ([EPI](https://www.european-processor-initiative.eu)) project is a clear example of this.

[Gentoo Prefix](https://wiki.gentoo.org/wiki/Project:Prefix) is a key component in the European Environment for Scientific Software ([EESSI](https://eessi.github.io/docs)) project, which is a collaboration between various partners in the High-Performance Computing (HPC) community to build a common stack of scientific software installations for HPC systems and beyond, including laptops, personal workstations, and cloud infrastructure.

RISC-V is one of the target CPU architectures in the EESSI project, and good support for RISC-V in Gentoo Prefix is a crucial first step towards supporting RISC-V in EESSI.



| Contacts | Required Skills | 
|---|---|
|  |  | 
| Expected Project Size | Expected Outcomes | 
| 175 hours or 350 hours, depending on which tasks are planned. |  | 
| Project Difficulty |  | 
| Medium to hard, depending on which tasks are planned and what issues are encountered during bootstrapping. |  | 

The Bootstrap of Gentoo Prefix on a new environment is fragile and often need manual intervention. It is because Gentoo Prefix runs on a vast number of hosts, and the ::gentoo repository is fast moving. The reliability of bootstrap is crucial for new users to adopt Prefix and is the foundation for more use scenarios.

Stacked Prefix (also known as prefix-stack) was introduced to manage cygwin/win32 setups. It matured with EAPI 7, when BROOT variable are used to express the directory prefix of BDEPEND. This project envision using prefix-stack to bootstrap Prefix, achieving 3 goals:

- Unify the bootstrap logic of Prefix standalone (aka RAP) and guest (aka prefix-rpath);
- Simplify the logic of Prefix bootstrap;
- Substantially speed up the bootstrap if portage is available on the host.

You will need to test out various toolchain bootstrap setups and find out a universal routine for a diverse number of host environments. You are expected to decipher and debug subtle toolchain induced errors.




| Contacts | Required Skills | 
|---|---|
| [Benda Xu](mailto:heroxbd@gentoo.org) |  | 
| Expected Project Size | Expected Outcomes | 
| 350 hours | A revised Gentoo Prefix bootstrap script that leverages prefix-stack and directly use host portage if available. | 
| Project Difficulty |  | 
| hard |  |
