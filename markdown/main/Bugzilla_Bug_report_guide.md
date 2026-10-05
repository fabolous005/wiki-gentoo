<!-- source: https://wiki.gentoo.org/wiki/Bugzilla/Bug_report_guide | group: Gentoo Wiki (Main) | wiki-title: Bugzilla/Bug report guide -->
---
title: Bugzilla/Bug report guide
url: https://wiki.gentoo.org/wiki/Bugzilla/Bug_report_guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-11"
fingerprint: "9f0f1cca0ea30fa4"
license: CC BY-SA 4.0
---

# Bugzilla/Bug report guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article explains how to report bugs using Gentoo's Bugzilla instance, which may be lightly customized to collect specific details for each Gentoo project area.

See [Bugzilla/Guide](https://wiki.gentoo.org/wiki/Bugzilla/Guide) for the recommended method of forensically troubleshooting and reporting helpful details on bugs related to Gentoo development.

## Best practices

- **Reread the text before submission**, the text cannot be edited afterwards. Also any text entered into a bug report will be usually e-mailed immediately to many people. Write in precise and clean language and avoid colloquial speech. **Hint:** Imagine you have **only one chance in your life** to write this very important bug report. You know that the recipient can read English, but it is not their native language.

- **Search for duplicates**, before creating a new bug.

- **Stay on topic** - A bug ticket is used for technical reports and chitchat should be avoided. Keep discussions in the [support channels](https://www.gentoo.org/support/) (forums, IRC or mailing lists).
- **Confirm the existence of a problem only once.** - It does not help solving the problem, if you and another person report it twice. But if your and the confirmer's systems differ in an obvious way and that would be helpful to know, add this information.
- **Open one bug ticket per topic** - Usually this means no more than one package and one bug per ticket. If your problem is not discussed in a bug, search for one related to your issue or create a new report. Do not hijack bugs.
- **No talk on TRACKER bugs.** - Those bugs are meta bugs. If you want to add useful information, add them to a related sub bug or create a new bug.
- Optional: [Gentoo consultants](https://wiki.gentoo.org/wiki/Project:Council/Consultants) provide also **commercial support** for bugs and ebuilds.
- [Attach the logs to the bug ticket](https://wiki.gentoo.org/wiki/Attach_the_logs_to_the_bug_ticket) if the ticket is about problems during runtime or installation.

## Gentoo Linux: Ebuilds (packages)

You should always add information about your system configuration to the bug. To do so, create a new attachment and paste the contents of:

`user $``emerge --info > /tmp/emerge--info.txt`
### Report a build-time bug (emerge failed)

![](https://wiki.gentoo.org/images/thumb/2/21/Bugzilla_screenshot_summary-keyword.png/300px-Bugzilla_screenshot_summary-keyword.png)

- First write the exact version of the package in the title of the bug report e.g. *sys-apps/package-2.3-r4*
- Add a short description to the title.
- **[Attach the logs to the bug ticket](https://wiki.gentoo.org/wiki/Attach_the_logs_to_the_bug_ticket)**

### Report a run-time bug

Files and information of interest ordered by priority:

- The exact version of the package in the title of the bug report e.g. *sys-apps/package-2.3-r4 crashes with error: Cannot proceed...*
- Description of the problem, so that other can reproduce it:
  - How is the program run (on the console, in a terminal, as a daemon, in which init runlevel etc.)
  - Any error output
  - What makes the program crash, behave wrong, not start
  - Is there a workaround?
  - What was the last working version of the package, if any?
  - What changed to make it not work?
- **[Attach the logs to the bug ticket](https://wiki.gentoo.org/wiki/Attach_the_logs_to_the_bug_ticket)**

### Report a version bump; a newer upstream release is available since a while

- Search Bugzilla before posting a bump request - is there already a bug open? **Has the local Portage tree been synced lately**; is it already in Portage?
- Avoid [zero-day bump requests](https://wiki.gentoo.org/wiki/Zero-day_bump_requests) (wait at least **48 hours** after the release announcement)
- Has it actually been **released** by upstream sources, **or is it just marked** in the source tree? Some projects mark a release in the tree a long time before it is officially released.
- Be sure to **mention if it compiles and runs well on your arch**. Any other helpful information you can provide is most welcome.
- Add a link to the upstream website if there is an announcement or release notes.
- Give a link or list of fixed bugs or new features (sometimes called **changelog**)
- Write a **summary** in the form *app-editors/vim-12.3.5: version bump*

#### Optional

- Does a simple copy work, or does the ebuild need changes? (changed dependencies, obsolete patch files, compared build system changes, read release notes carefully)
- [Test the ebuild](https://wiki.gentoo.org/wiki/Package_testing) in a local repository before submitting attachments
- Provide patches for proposed ebuild edits, with ideally some explanation of changes (file name should match the new version number, not old)
- Provide additional files (OpenRC init scripts, systemd unit files) as separate attachments (as needed)
- Do not paste files directly into comments; [use attachments.](https://wiki.gentoo.org/wiki/Attach_the_logs_to_the_bug_ticket)

### Request for a new package; ebuild request

If you request a new ebuild for a software to be added to the Gentoo repository, you must find or become a maintainer for the package.

If a bug report already exists for the package, you can help the effort by keeping information about the package up to date. If you add a -VERSION component to the package atom in the summary/title, then this can be updated with new releases over time while the bug report remains open to show there is a continuing interest in seeing it integrated into the Gentoo repository.

If no bug report exists for the prospective new package, you can file a bug report under the **Gentoo Linux** project and the component **New package**.

The **Summary** of your bug report should list a (preliminary) package atom *category/package*, perhaps with a *-VERSION* suffix, followed by a canonical short description of the package (the DESCRIPTION variable in an ebuild). It is important to disambiguate the name of the new package: if upstream uses different names for the same software, perhaps an abbreviation as well as the full name, you should mention both (all) of these in the Summary so that other people can find bug reports about the same software. If several (groups of) people track different bug reports about virtually the same ebuild request, this will duplicate the effort of ebuild research and development, and will divide people who have a common interest.

You should link to the upstream website (the HOMEPAGE variable in an ebuild) using the **URL** field. You should provide a list of features in the **Description** of the bug report. This may well be taken directly from the upstream website or from a manual or other documentation, and could be used later for the *longdescription* tag in metadata.xml.

You can attach an ebuild and related files that should go into the Gentoo repository directly to the bug report, or you can use the **See also** field to refer to a git [pull request](https://wiki.gentoo.org/wiki/Github_Pull_Requests).

You can help develop the package by setting up a [local repository](https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository) with your ebuilds, metadata, patches and other auxiliary files. If you need technical support with your ebuild development, many people would be glad to [help](https://www.gentoo.org/support/).

### Request stabilization

A bug ticket can be used for a [stable request](https://wiki.gentoo.org/wiki/Stable_request).

Everybody can request a stabilization. **Users do not need to worry about filling all fields or details in the bug.** The maintainer (or Proxied Maintainer) will CC the arches by adding **CC-ARCHES** to the *Keywords* field on the bug when appropriate.

A stable request can be filed either using the [pkgdev](https://wiki.gentoo.org/wiki/Pkgdev) utility, or manually.

For the pkgdev bugs route:

`user $``pkgdev bugs -s =dev-java/bndlib-7.0.0`
Checking =dev-java/bndlib-7.0.0 on 'amd64 arm64 ppc64'
Checking =dev-java/bnd-annotation-7.0.0 on 'amd64 arm64 ppc64'
Checking =dev-java/bnd-util-7.0.0 on 'amd64 arm64 ppc64'
Checking =dev-java/libg-7.0.0 on 'amd64 arm64 ppc64'
Checking =dev-java/osgi-service-log-1.3.0 on 'amd64 arm64 ppc64'
Merging =dev-java/bnd-util-7.0.0 into =dev-java/bndlib-7.0.0
Merging =dev-java/osgi-service-log-1.3.0 into =dev-java/bndlib-7.0.0, =dev-java/bnd-util-7.0.0

Eventually an [API key](https://bugs.gentoo.org/userprefs.cgi?tab=apikey) needs to be copied to \~/.bugz\_token.

To request stabilization of a package manually, [file a new bug](https://bugs.gentoo.org/enter_bug.cgi?product=Gentoo%20Linux) under the `Stabilization` component taking care to complete two special [bug fields](https://bugs.gentoo.org/page.cgi?id=fields.html):

- `Package list` - a fully qualified package per line, optionally followed by a space-delimited list of architectures to target. Formerly, this field was called `Atoms to stabilize` and contained fully qualified atoms, which is also still supported. [Nattka](https://wiki.gentoo.org/wiki/Nattka) can be used with `make-package-list` to generate the appropriate format for this field. Architectures should no longer be manually added to the `CC` field.
- `Runtime testing required` - see [https://bugs.gentoo.org/page.cgi?id=fields.html#cf\_runtime\_testing\_required](https://bugs.gentoo.org/page.cgi?id=fields.html#cf_runtime_testing_required)

Examples:

| Summary | foo-libs/libbar-1.2.3 stabilization request | 
|---|---|
| Runtime testing required | No | 
| Package list | foo-libs/libbar-1.2.3 ( *old syntax, still supported:* =foo-libs/libbar-1.2.3) | 
| Explanation |  | 

| Summary | app-foo/bar-1.2.3 and app-foo/baz-4.5.6 stabilization request | 
|---|---|
| Runtime testing required | Yes | 
| Package list | app-foo/bar-1.2.3 | 
|  | app-foo/baz-4.5.6 amd64 x86 | 
| Explanation |  | 

### Requesting Keywords

A bug ticket can be used for a keyword request.

As with [request stabilization](https://wiki.gentoo.org/wiki/Bugzilla/Bug_report_guide#Request_stabilization), everybody can request a keyword. **Users do not need to worry about filling all fields or details in the bug.** The maintainer (or Proxied Maintainer) will CC the arches by adding **CC-ARCHES** to the *Keywords* field on the bug when appropriate.


To request a new keyword for a package, [file a new bug](https://bugs.gentoo.org/enter_bug.cgi?product=Gentoo%20Linux) under the `Keywording` component taking care to complete two special [bug fields](https://bugs.gentoo.org/page.cgi?id=fields.html):

- `Package list` - a fully qualified package per line, optionally followed by a space-delimited list of architectures to target. If no architecture list is provided, all architectures in `CC` are assumed. Formerly, this field was called `Atoms to stabilize` and contained fully qualified atoms, which is also still supported.
- `Runtime testing required` - indicates if additional runtime testing should be performed beyond build and tests passing. If *undefined* the arch tester should use their best judgement

| Summary | foo-libs/libbar-1.2.3: add \~ppc keyword | 
|---|---|
| Runtime testing required | No | 
| Package list | foo-libs/libbar-1.2.3 \~ppc ( *old syntax, still supported:* =foo-libs/libbar-1.2.3) | 
| Explanation |  | 

## Kernel

Files and information of interest for kernel bug reports ordered by priority:

- Which kernel and version is used, on what architecture e.g. *gentoo-sources-3.4.2-r2* on *x86\_64*
- The kernel configuration file should be attached to the bug report (/usr/src/linux/.config)
- A list of all devices in the system can be acquired with *lspci -k*
- Log files during kernel initialization should be attached (/var/log/dmesg or /var/log/messages)

## Supplemental information for bug reports

| Information | When needed | How to collect | 
|---|---|---|
| `SRC_URI` reachable? | download failed | GENTOO\_MIRRORS="" ebuild foo-1.2.ebuild fetch | 
| OpenGL version | Games with OpenGL | glxinfo -B | 
| Linked libraries | Dependency is missing | Add missing dependency, compile, check with lddtree | 

## Trackers

Tracker bugs are virtual or meta bugs to cluster bugs with the same topic or focus area:

## See also

- [Attach the logs to the bug ticket](https://wiki.gentoo.org/wiki/Attach_the_logs_to_the_bug_ticket) — explains how to **attach log files to a bug ticket**
- [Bugzilla/Guide](https://wiki.gentoo.org/wiki/Bugzilla/Guide) — covers the recommended method of forensically reporting *specific details* of bugs within Gentoo.
- [Contributing to Gentoo](https://wiki.gentoo.org/wiki/Contributing_to_Gentoo) — explains how users can **contribute to the development of Gentoo**
- [Support](https://wiki.gentoo.org/wiki/Support) — provide **support** for technical issues encountered when installing or using Gentoo Linux
- [Troubleshooting](https://wiki.gentoo.org/wiki/Troubleshooting) — provide users with a set of techniques and tools to troubleshoot and fix problems with their Gentoo setups.
