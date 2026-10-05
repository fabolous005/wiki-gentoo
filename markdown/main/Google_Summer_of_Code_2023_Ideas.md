<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2023/Ideas | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2023/Ideas -->
---
title: Google Summer of Code/2023/Ideas
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2023/Ideas
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-03-09"
fingerprint: "36b5b39808358769"
license: CC BY-SA 4.0
---

# Google Summer of Code/2023/Ideas

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![GSoC 2022 logo GSoC 2022 logo](https://wiki.gentoo.org/images/c/ce/Gsoc2016-sun-160x160.png)

Want to spend your summer contributing full-time to Gentoo, and get paid for it? Gentoo is in its 11th year in the Google Summer of Code. In the past, **most of our successful students have become Gentoo developers**, so your chances of becoming one are very good if you're accepted into this program.

Most ideas listed here have a contact person associated with them. Please get in touch with them earlier rather than later to develop your idea into a complete application. You can find many of them on Libera.Chat's IRC network under the same username. If there is no contact information, please join the [gentoo-soc mailing list](https://www.gentoo.org/get-involved/mailing-lists/) or [#gentoo-soc on the Libera.Chat IRC network](irc://irc.gentoo.org/gentoo-soc), and we will work with you to find a mentor and discuss your idea.

**You don't have to apply for one of these ideas! You can come up with your own**, and as long as it fits into Gentoo, we'll be happy to work with you to develop it. Remember, your project needs to have deliverables in less than 3 months of work in most cases. Be ambitious but not too ambitious ;)

## Students, please read this first

We have a custom application template that we will ask you to fill out. Here it is:

**Congratulations on applying for a project with Gentoo!** To improve your chances of succeeding with this project, we want to make sure you're sufficiently prepared to invest a full summer's worth of time on it. **In addition to the usual application, there are 2 specific actions and 2 pieces of info we would like to see from you:**

- **Use the tools that you will use in your project to make changes to code** (e.g., source code management \[SCM\] software such as CVS, Subversion, or git). Please use the same SCM as you will use for your project to check out one of our [repositories](https://www.gentoo.org/get-involved/get-code/), make a change to it, and post that change as a [patch](<https://en.wikipedia.org/wiki/Patch_(Unix)>) on a mailing list or [bug](https://bugs.gentoo.org/). Please fix a real bug reported in [Bugzilla](https://bugs.gentoo.org/) to show that you can use the tools to make a meaningful change. Your contact in Gentoo can help you determine which SCM and repository you should use for this as well as a good bug to fix. If your idea doesn't have a contact, please get in touch with us on the [gentoo-soc mailing list](https://www.gentoo.org/get-involved/mailing-lists/) or in real-time [on IRC](irc://irc.gentoo.org/gentoo-soc). Once you've made your change, link to it from your application.

- **Participate in our development community.** Please make a post to one of our [mailing lists](https://www.gentoo.org/get-involved/mailing-lists/) and link to it from your application ([archives.gentoo.org](https://archives.gentoo.org) holds past postings). The gentoo-soc list would be a good starting point, if you aren't subscribed to any others already. The best posts would be an introduction of the project you're applying for and a little background about you, to introduce yourself to the community and get some broader input about your project.

- **Give us your contact info and working hours.** Please provide your email address, home mailing address, and phone number. This is a requirement and provides for accountability on both your side and ours. Also, please tell us what hours you will be working and responsive to contact via email and IRC; these should sum to at least 35 hours a week.

These actions are things you will do extremely commonly as an open-source developer, and they really aren't that hard, so don't let them hold you back! The remainder of the application is free-form. **Please read our [application guidelines](https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2023/Application_Guidelines) and [Google's FAQ](https://developers.google.com/open-source/gsoc/faq) to complete it.** Good luck!

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

The overall target experience should be to create an easy to install tool that Gentoo users can use to keep their systems up-to-date. In GSoC 2023, we created [gentoo\_update](https://github.com/Lab-Brat/gentoo_update) with an associated [mobile app](https://github.com/Lab-Brat/gentoo_update_flutter)! This year we can pick up where that project left off and make the mobile app's back-end fully self-hosted on a Gentoo Linux system.

Notification to users when something goes wrong was an important part of the 2023 project and improving the mobile app would help easy of user of Gentoo users. 


| Contacts | Required Skills | 
|---|---|
|  |  | 
| Expected Project Size | Expected Outcomes | 
|  |  | 
| Project Difficulty |  | 
| Easy/Medium/Hard |  | 

Gentoo has a CI system (the tinderbox) to automatically test installation of packages with various configurations. However, during development, changes to ebuilds may break packages in ways that only affect Gentoo prefix and which are not caught by the regular testing. It would be great to have some CI infrastructure in place for Gentoo Prefix bootstraps, but also a similar CI system to the tinderbox running not on vanilla Gentoo, but on Gentoo prefix, to quickly identify problems with new packages as they are introduced into the tree.


| Contacts | Required Skills | 
|---|---|
|  |  | 
| Expected Project Size | Expected Outcomes | 
| 350 hours | Running CI system testing Gentoo prefix packages and automated bootstraps of Gentoo Prefix on a few host platforms. | 
| Project Difficulty |  | 
| medium |  | 

What should the future of Gentoo networking configuration be? Explore potential improvements within the aegis of netifrc & other network configuration systems.

Can all of netifrc configurations be represented by systemd-networkd, netplan or other systems? Can other network configuration systems successfully integrate in all variations of Gentoo init systems (openrc, systemd, epoch, runit).



| Contacts | Required Skills | 
|---|---|
|  |  | 
| Expected Project Size | Expected Outcomes | 
| 350 |  | 
| Project Difficulty |  | 
| medium |  | 

This project will add structured/machine-readable formats for emerge views.

Various example steps:

- \`emerge -p $PKG\` should show a structured output about the change, package, version, slots, use flags etc.
- Emerge errors should have structured output, making them easier to understand what's wrong, and the boundaries of each error (because many errors seem to flow together if there are multiple errors).
- Structured progress output for other tools to watch build status, esp during parallel package building.



| Contacts | Required Skills | 
|---|---|
| [Robbat2](mailto:robbat2@gentoo.org) |  | 
| Expected Project Size | Expected Outcomes | 
| 350 |  | 
| Project Difficulty |  | 
| hard |  | 

Portage's codebase is aging (as old as Gentoo!) and needs modernization for contemporary Python style and techniques. This project involves picking components of the Portage codebase, understanding it, annotating it (with extensive comments), and then refactoring as appropriate.

A lot of this technical code debt makes implementing new GLEPs and features more expensive and time-consuming than it needs to be. This project is to work on modernizing the codebase in appropriately-sized chunks. Rewriting whole modules is less important than documenting the existing code, as well as researching the reasons for its existing behaviour, and refactoring (even small amounts) in the course of that work.



| Contacts | Required Skills | 
|---|---|
|  |  | 
| Expected Project Size | Expected Outcomes | 
| 175 hours or 350 hours, depending on appetite for larger refactoring. |  | 
| Project Difficulty |  | 
| Easy or Medium. Some sections will be easier and the codebase is large enough so that people can choose components according to the appropriate difficulty. |  | 

Probabilistic programming is a new paradigm of Bayesian statistics to automatically compile generative models into inference ones.  Anglican is a Turing-complete academic probabilistic programming language with practical applications, built on top of Clojure.  Clojure (`dev-lang/clojure::gentoo`) is a dialect of LISP hosted on Java virtual machine, with its libraries including Anglican collected into a maven repository "Clojars".

This project aims to offer Gentoo users with Turing-complete probabilistic programming, by building a repository of Clojure and Anglican libraries using java-ebuilder (`app-portage/java-ebuilder::gentoo`).


| Contacts | Required Skills | 
|---|---|
| [Benda Xu](mailto:heroxbd@gentoo.org) | An applicant should: | 
| Expected Project Size | Expected Outcomes | 
| 350 hours. | Anglican, its dependencies and necessary auxiliary libraries in an overlay or `::gentoo`. | 
| Project Difficulty |  | 
| hard |  | 

Newer versions of compilers are planning on becoming significantly stricter with the C code they will accept or reject. This is a huge problem for the general Linux ecosystem.

Even worse is that some of these failures are silent and lead to incorrect behaviour at runtime. Extra care must be taken to detect this and we must prioritise these cases because they're the most harmful. With the release of Clang 16 (expected March 2023), the following specified warnings will be treated as errors:

- -Werror=implicit-function-declaration
- -Werror=implicit-int
- -Werror=int-conversion (been enabled in clang 15)
- -Werror=incompatible-function-pointer-types (for GCC a student might have to use -Werror=incompatible-pointer-types instead)

Also, in the coming years with C2x (likely C23) additional changes like removing certain deprecated prototypes will be made.

The above changes will affect affect Gentoo packages in the following ways:

- Lots of packages fail to build with these settings
- Sometimes packages build successfully but their ./configure scripts have misdetected features or otherwise made the wrong conclusion about the system because they expect a test to succeed when it now fails.

Please read [Modern C porting](https://wiki.gentoo.org/wiki/Modern_C_porting) in detail and see also [bug #870412](https://bugs.gentoo.org/show_bug.cgi?id=870412).

Students can test and provide patches for affected packages on both glibc and musl systems in parallel, but keeping glibc as the primary target might help fixes reach users faster.




| Contacts | Required Skills | 
|---|---|
|  |  | 
| Expected Project Size | Expected Outcomes | 
| 350 |  | 
| Project Difficulty |  | 
| Medium |  |
