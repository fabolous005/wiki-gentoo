<!-- source: https://wiki.gentoo.org/wiki/Portage/Help/Ruby_dependency_conflicts | group: Gentoo Wiki (Main) | wiki-title: Portage/Help/Ruby dependency conflicts -->
---
title: Portage/Help/Ruby dependency conflicts
url: https://wiki.gentoo.org/wiki/Portage/Help/Ruby_dependency_conflicts
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-21"
fingerprint: "9801ab72e84a9606"
license: CC BY-SA 4.0
---

# Portage/Help/Ruby dependency conflicts

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Background

Gentoo's eclasses for language support (or "bindings") have gone through several evolutions until the community largely settled on the "Python model" (now used for Lua and others).

Ruby, at the time of writing, hasn't (yet?) been adapted to this newer dependency model. This often leads to counterintuitive conflicts for users and confusing errors. It's not certain that the dependencies specified are completely accurate as-is or can be expressed as necessary.

For more on Ruby, see [bug #424339](https://bugs.gentoo.org/show_bug.cgi?id=424339), [bug #444828](https://bugs.gentoo.org/show_bug.cgi?id=444828), and [bug #702266](https://bugs.gentoo.org/show_bug.cgi?id=702266) for issues of that type. In a similar vein, there's also [bug #513888](https://bugs.gentoo.org/show_bug.cgi?id=513888).

## Issue

Users of `~arch` in particular will often see confusing autounmask messages from Portage requesting mixed `RUBY_TARGETS` be set.

The output may resemble something like:

`root #``emerge -p -uvDU @world`
\[...\]
The following USE changes are necessary to proceed:
 (see "package.use" in the portage(5) man page for more details)
# required by dev-lang/ruby-3.0.0::gentoo
# required by dev-ruby/rubygems-3.0.6::gentoo\[ruby\_targets\_ruby30\]
# required by virtual/rubygems-15::gentoo\[ruby\_targets\_ruby30\]
# required by dev-ruby/rdoc-6.2.0::gentoo\[-test,ruby\_targets\_ruby30\]
>=dev-ruby/did\_you\_mean-1.3.1 ruby\_targets\_ruby30

## Solution

1. Remove all references to `ruby_targets` in /etc/portage. Find them via grep -rsin "ruby" /etc/portage.
2. Use stable [dev-lang/ruby](https://packages.gentoo.org/packages/dev-lang/ruby), as shown (replace `~amd64` with your arch, `-~*` doesn't seem to work?) FILE**`/etc/portage/package.accept_keywords`** \# Use stable Ruby to avoid conflicts and excessive package.use entries dev-lang/ruby -\~amd64
3. Perform a world upgrade.
4. Depclean.
