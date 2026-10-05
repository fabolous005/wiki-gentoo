<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2022/Ideas/Language_eclass_modernisation | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2022/Ideas/Language eclass modernisation -->
---
title: Google Summer of Code/2022/Ideas/Language eclass modernisation
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2022/Ideas/Language_eclass_modernisation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-04-01"
fingerprint: "6f30c99d6c19acf0"
license: CC BY-SA 4.0
---

# Google Summer of Code/2022/Ideas/Language eclass modernisation

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

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
