<!-- source: https://wiki.gentoo.org/wiki/Perl | group: Gentoo Wiki (Main) | wiki-title: Perl -->
---
title: Perl
url: https://wiki.gentoo.org/wiki/Perl
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-22"
categories: ['dev-perl', 'perl-core']
fingerprint: "53731b0ea1f7039e"
license: CC BY-SA 4.0
---

# Perl

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Perl** is a general purpose interpreted programming language with a powerful regular expression engine.

## Perl in Context

### Market Niche

Perl powered almost the entirety of the early world wide web. To some, Perl is a primarily thought of as a glue language. To others, it's thought of as the "Swiss Army Chainsaw" of languages that allow for quick hacks to solve to complex problems. Because Perl is highly performant compared to most interpreted languages and it has a shell-like syntax. Consequently, it is often easier to maintain than equivalent scripts written in Bash. Additionally, thanks to the Mojolicious framework deploying modern microservices in Perl is almost trivial. This allows for Perl to hold its own in modern DevOps environments.

### Pros

- Perl is multiparadigm: procedural, object oriented, and functional programming styles are supported.
- Perl makes writing microservices very easy with the Mojolicious web framework.
- The basic syntax is easy to learn, especially for those already familiar with [C](https://wiki.gentoo.org/wiki/C) or a POSIX shell.
- Modern Perl is as readable as Ruby.
- For an interpreted language, Perl is quite fast. Perl VM startup time is much faster than [Java](https://wiki.gentoo.org/wiki/Java) and execution times are typically on par with [Python](https://wiki.gentoo.org/wiki/Python).
- It has a vast library of modules stored in the CPAN library, which is comparable to size and scope to Python's Pip.
- Perl's Corinna object system is both low boilerplate and very expressive.
- Perl deprecates features far less frequently than other languages, thus scripts can run unmodified *much* longer than is typical for most languages, except perhaps C.

### Cons

- While Perl remains a popular choice for some tasks its developer community has shrank considerably from its heyday, having lost a lot of ground to Python. It now has a developer community similar in size to that of PHP.
- In Perl there is always "More than one way to do it!" (TIMTOWTDI). On the one hand this allows for very expressive code, on the other hand this can be confusing to inexperienced developers.
- Advanced uses of Perl involve a lot of "magic" behind the scenes that can be surprising to new developers. This allows for code that can be downright laconic.
- Perl's regular expression syntax, though wildly ported to other languages thanks to the PCRE library, takes a while to get used to.

### Complimentary Languages

Perl is closely related to [Raku](https://wiki.gentoo.org/wiki/Raku) and the two languages do share some syntax. Perl took inspiriation from C, grep, and awk. Both [PHP](https://wiki.gentoo.org/wiki/PHP) and [Ruby](https://wiki.gentoo.org/wiki/Ruby) took a lot of inspiration from Perl.

## Introduction

The Perl language itself is packaged as dev-lang/perl. There're three Perl-related categories:

1. [dev-perl](https://packages.gentoo.org/categories/dev-perl): Libraries in / for Perl, corresponding to dev-java, dev-python, etc. In most cases the package name directly corresponds to a [CPAN](https://wiki.gentoo.org/wiki/CPAN) distribution.
2. [perl-core](https://packages.gentoo.org/categories/perl-core): Packages in this category are modules included in dev-lang/perl, which are also independently packaged on CPAN. When modules are installed via perl-core, they override the counterpart in the core dev-lang/perl. This can be used for selective bugfixes. For the details, see below - but please **never manually install any perl-core packages with emerge**.
3. [virtual/perl-\*](https://packages.gentoo.org/packages/search?q=virtual%2Fperl): Virtual packages that allow choosing a module between perl-core/ packages and the one contained in the core dev-lang/perl. If you need a specific version of a core package, emerge the corresponding virtual - and the package manager will figure out if a perl-core/ package is needed or not.

More on perl-core<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>:

- It allows users to have more update versions of some modules that the core ones.
- It allows package-based installation, without bumping or patching the core perl.
- Sometimes these modules get deprecated in newer Perl. Such cases are handled by virtual/perl-\*. Search [\[1\]](https://bugs.gentoo.org/show_bug.cgi?id=608168#c1) for "Module::Build" for more in-depth stories.

Normally you do not have to care at all about perl-core packages or virtuals; if you need any specifics there they should be pulled in as dependencies.

## Installation

### USE flags

#### Global

Since many packages depend on the perl, Portage is aware of the [perl](https://packages.gentoo.org/useflags/perl) [USE flag](https://wiki.gentoo.org/wiki/USE_flag). It can be enabled (or disabled) globally:

**`/etc/portage/make.conf`**

```
USE="perl"
```
This is typically only required if carrying out a lot of Perl development locally.

#### Package


| [berkdb](https://packages.gentoo.org/useflags/berkdb) | Add support for sys-libs/db (Berkeley DB) | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [gdbm](https://packages.gentoo.org/useflags/gdbm) | Add support for sys-libs/gdbm (GNU database libraries) | 
| [minimal](https://packages.gentoo.org/useflags/minimal) | Install a very minimal build (disables, for example, plugins, fonts, most drivers, non-critical features) | 

As always, after adjusting USE flags, be sure to tell Portage to apply the changes to installed packages on the system:

`root #``emerge --ask --update --changed-use --deep --autounmask-keep-masks=y @world`
In addition, after changing the `ithreads` or `debug` use flag setting, you need to re-build all packages installing perl modules or linking to libperl:

`root #``perl-cleaner --reallyall`
## Upgrading

The official way of upgrading Perl, e.g. from Perl 5.36 to Perl 5.38, is upgrading your entire world, and upgrading Perl with it. This is because Portage needs to be able to rebuild packages depending on Perl. If you ask Portage to selectively only upgrade the Perl package itself, it can't do this and the emerge command will fail.

As in all cases when automatic rebuilds are involved, it helps a lot if you do regular updates and regularly run depclean.

### The official way

`root #````
emerge -uDNav --autounmask-keep-masks=y @world
```
`root #````
perl-cleaner --all
```
If this fails, please check your world file for packages which cannot be updated/reinstalled because they've been removed or are in some way masked.

### Some knowledge

- Perl modules are installed under *e.g.,* /usr/lib/perl5/vendor\_perl/5.36/. Note that the core Perl version number is present. When upgrading Perl by a major version, the packages providing these modules have to be re-emerged, too.
- The same is valid for all packages linking to libperl.
- The rebuilds 'should' be done automatically by emerge. `app-admin/perl-cleaner` exists to do them as well and can catch things missed by emerge (sadly Portage still has some bugs).
- During Perl upgrade, packages that depend on Perl may become unavailable.
- **No rebuilds are necessary during a point-release update** (i.e. from 5.36.0 to 5.36.1).

In order to upgrade your Perl installation to unstable (\~arch) version on an otherwise stable system, add the following text to `/etc/portage/package.accept_keywords`:

\# use Perl from \~arch
dev-lang/perl
perl-core/\*
virtual/perl-\*

Perl itself, the perl-core packages and the Perl virtuals 'must' have the same, consistent status (either all stable or all \~arch). What setting you use for dev-perl packages does not matter at all.

## Troubleshooting

### How do I list all Perl modules installed from CPAN?

The following [Bash](https://wiki.gentoo.org/wiki/Bash) one-liner can give you a list of all modules:

## See also

- [Awk](https://wiki.gentoo.org/wiki/Awk) — a scripting language for data extraction
- [Bash](https://wiki.gentoo.org/wiki/Bash) — the default shell on Gentoo systems and a popular [shell](https://wiki.gentoo.org/wiki/Shell) program found on many Linux systems.
- [CPAN](https://wiki.gentoo.org/wiki/CPAN) — the *Comprehensive Perl Archive Network*, [Perl]'s package ecosystem.
- [Grep](https://wiki.gentoo.org/wiki/Grep) — a tool for searching text files with regular expressions
- [PHP](https://wiki.gentoo.org/wiki/PHP) — a general-purpose server-side scripting language to produce dynamic web pages.
- [Python](https://wiki.gentoo.org/wiki/Python) — an extremely popular cross-platform object oriented programming language.
- [Raku](https://wiki.gentoo.org/wiki/Raku) — a high-level, general-purpose, and gradually typed programming language with low boilerplate objects, optionally immutable data structures, and an advanced macro system.
- [Ruby](https://wiki.gentoo.org/wiki/Ruby) — an interpreted programming language.
- [Sed](https://wiki.gentoo.org/wiki/Sed) — a program that uses regular expressions to programmatically modify streams of text

## External Resources

### Learning Modern Perl

- [Why Perl?](https://two-wrongs.com/why-perl.html) — A detailed but approachable article on the empowering nature of Perl while still remaining honest about its limitations.
- [Learn X in Y minutes, Where X=Perl](https://learnxinyminutes.com/docs/perl/) — A quick but well respected Perl primer.
- [perlsyn](https://perldoc.perl.org/perlsyn) — A guide to Perl's basic syntax.
- [Modern Perl, 4e](http://modernperlbooks.com/books/modern_perl_2016/index.html) — An in-depth guide to Perl though v5.22.
- [Perl Maven](https://perlmaven.com/) — A large collection of Perl tutorials written by a prominent member of the Perl community.
- [perlclasstut](https://github.com/Perl-Apollo/Corinna/blob/master/pod/perlclasstut.pod) — Object-Oriented Programming via the the Corinna object system which debuted in Perl 5.38.
- [Corinna](https://github.com/Perl-Apollo/Corinna) — The specification for Perl's modern object syntax as of Perl 5.38; very technically in depth, not really a tutorial.
- [Rosetta Code: Perl](https://rosettacode.org/wiki/Category:Perl) — examples of common programming tasks in Perl.
- [Exercism's Perl 5 track](https://exercism.org/tracks/perl5) — free interactive online lessons for learning Perl 5.

#### Learning Perl Regular Expressions

- [perlrequick](https://perldoc.perl.org/perlrequick) — Perl regular expressions quick start tutorial.
- [perlretut](https://perldoc.perl.org/perlretut) — Perl's in depth regular expressions tutorial.
- [Regular Expressions.info](https://www.regular-expressions.info/) — An extensive collection of regular expression related resources.
- [RegexOne](https://regexone.com/) — Learn basic regular expressions with simple interactive exercises.
- [Regex Crossword](https://regexcrossword.com/) — A crossword puzzle game with clues written as regular expression patterns.

#### Debugging and Testing

- [perldebug](https://perldoc.perl.org/perldebug) — A guide to Perl debugging.
- [perltrap](https://perldoc.perl.org/perltrap) — Traps and "foot-guns" for the unwary.
- [Test::Tutorial](https://perldoc.perl.org/Test::Tutorial) — A tutorial for writing basic tests with [Test Anything Protocol](https://testanything.org/) (TAP).

#### Cheat Sheets

- [perlcheat](https://perldoc.perl.org/perlcheat) — Perl 5's cheat sheet.
- [Regex Cheat Sheet](https://perlmaven.com/regex-cheat-sheet) — a Perl 5 regex cheat sheet.

### Modern Perl Tools

- [App::cpanminus](https://metacpan.org/pod/App::cpanminus) — A modern CPAN client.
- [Perl::Critic](https://metacpan.org/pod/Perl::Critic) — Critique Perl source code for best-practices.
- [Perl::Tidy](https://metacpan.org/pod/Perl::Tidy) — Beautify and enhance the readability of Perl source.
- [App::opan](https://metacpan.org/pod/App::opan) — A CPAN overlay with local repository (darkpan) support and module version pinning capability.
- [Perlbrew](https://perlbrew.pl/) — An Perl installation management tool allowing programmers to decouple their code from system Perl.
- [zarn](https://github.com/htrgouvea/zarn) — A lightweight static analysis tool for modern Perl application security.
- [(R)?ex](https://www.rexify.org/) — A user friendly automation framework written in Perl.

#### Popular Perl Frameworks

- [Mojolicious](https://mojolicious.org/) — Mojolicious is a fresh take on web development.
- [Catalyst](http://catalyst.perl.org) — the Elegant MVC web application framework.
- [Dancer](https://perldancer.org/) — Dancer is a simple but powerful web application framework.
- [Template::Toolkit](https://metacpan.org/pod/Template::Toolkit) — Perl's famous Template Processing System.

#### Code Documentation

- [perlpod](https://perldoc.perl.org/perlpod) — the Plain Old Documentation (POD) format, Perl's native documentation format.
- [perlpodstyle](https://perldoc.perl.org/perlpodstyle) — Perl's Plain Old Documentation (POD) style guide.
- [podchecker](https://metacpan.org/pod/podchecker) — A tool for checking the syntax of Plain Old Documentation (POD) documentations.
- [pod2man](https://perldoc.perl.org/pod2man) — A tool for converting POD data to roff (man page) format.
- [pod2html](https://perldoc.perl.org/pod2html) — A tool for converting POD files to HTML format.
- [Doxygen::Filter::Perl](https://metacpan.org/pod/Doxygen::Filter::Perl) — A Perl code pre-filter for [Doxygen](https://en.wikipedia.org/wiki/Doxygen)-style comments.

### Miscellaneous

- [Awesome Perl](https://github.com/uhub/awesome-perl) — a curated list of Perl frameworks.
- [Perl Power Tools](https://github.com/briandfoy/PerlPowerTools) — BSD tools rewritten in Perl.
