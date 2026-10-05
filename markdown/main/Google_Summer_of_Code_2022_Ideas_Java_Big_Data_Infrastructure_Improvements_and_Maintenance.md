<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2022/Ideas/Java_Big_Data_Infrastructure_Improvements_and_Maintenance | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2022/Ideas/Java Big Data Infrastructure Improvements and Maintenance -->
---
title: Google Summer of Code/2022/Ideas/Java Big Data Infrastructure Improvements and Maintenance
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2022/Ideas/Java_Big_Data_Infrastructure_Improvements_and_Maintenance
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-03-31"
fingerprint: b3218ceec677f7ab
license: CC BY-SA 4.0
---

# Google Summer of Code/2022/Ideas/Java Big Data Infrastructure Improvements and Maintenance

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

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
