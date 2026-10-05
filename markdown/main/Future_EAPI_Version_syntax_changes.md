<!-- source: https://wiki.gentoo.org/wiki/Future_EAPI/Version_syntax_changes | group: Gentoo Wiki (Main) | wiki-title: Future EAPI/Version syntax changes -->
---
title: Future EAPI/Version syntax changes
url: https://wiki.gentoo.org/wiki/Future_EAPI/Version_syntax_changes
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-21"
fingerprint: f7bc2c6fec418ec9
license: CC BY-SA 4.0
---

# Future EAPI/Version syntax changes

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Reordering to PACKAGE OP VERSION

Replace the current:

\[\<operator>\] \<cat/pkg> \[-\<version>\] \[:\<slot>\] ...
^^^^^^^^^^^^           ^^^^^^^^^^^^

with:

```
<cat/pkg> [:<slot>] [<operator> <version>]
          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```
e.g.:

| Old syntax | New syntax | Notes | 
|---|---|---|
| dev-foo/bar | dev-foo/bar | (unchanged) | 
| dev-foo/bar:4 | dev-foo/bar:4 | (unchanged) | 
| >=dev-foo/bar-4.1 | dev-foo/bar>=4.1 |  | 
| >dev-foo/bar-4.1:4 | dev-foo/bar:4>4.1 |  | 
| =dev-foo/bar-11 | dev-foo/bar==11 | (or =, TBD) | 

Advantages:

- more readable,
- easier to edit,
- easier to validate,
- easier to split package name from version,
- better ground for version ranges,
- in the future, this will allow us to dump restriction on package names not being confusing with versions.

Problems:

- Collides with slot operators:

| Old syntax | New syntax | 
|---|---|
| =dev-foo/bar-4.1:4= | dev-foo/bar:4===4.1 | 
| >dev-foo/bar-4.1:= | dev-foo/bar:=>4.1 | 

## Version ranges

### Problems to solve

#### A. Range dependencies vs slotting

Sometimes it is necessary to restrict acceptable versions of a package to a range. This is frequently accomplished using the following syntax:

\<dev-foo/bar-1.5
>=dev-foo/bar-1.3

If the package is slotted, this dependency can actually cause PM to install two versions of the package in different slots, neither of them matching the intended range, e.g. 1.2 (satisfies \<1.5) and 1.6 (satisfies >=1.3). For simple slotting, appending correct slot (e.g. :0) fixes that. For fully slotted packages, any-of dependencies are used instead:

|| (
  dev-foo/bar:1.5
  dev-foo/bar:1.4
  dev-foo/bar:1.3
)

Problems:

- verbosity, inconvenience,
- any-of dep makes it impossible to use :=.

#### B. Excluding single package versions

Sometimes it is necessary to exclude one or a few versions from a wide range, e.g. due to a bug or temporary incompatibility. This is either achieved using:

|| (
  ( \<dev-foo/bar-1.5 >=dev-foo/bar-1.3.2 )
  ( \<dev-foo/bar-1.3 >=dev-foo/bar-1.1 )
)

or:

\<dev-foo/bar-1.5
>=dev-foo/bar-1.1
!!dev-foo/bar-1.3.0
!!dev-foo/bar-1.3.1

The former form has the disadvantages of any-of group, the latter of blockers.

### Slot-binding dependency groups

The least-change solution to the problem is to introduce an additional dependency group type that forces all conditions in it to be enforced on the same slot:

&& (
  \<dev-foo/bar-1.5
  >=dev-foo/bar-1.3
)

Advantages:

- minimal syntax change,
- possibility to enforce many different conditions on the matched package without the need to repeat the range (e.g. different sets of USE flags).

Disadvantages:

- still verbose, package name repeated needlessly,
- adding a group requires determining interaction with other groups,
- solves only problem A.

### Arithmetic version ranges

The idea is to use arithmetic notion of ranges:

```
=dev-foo/bar-{1.3..1.5>
```
Problems:

- limited to few operators by design,
- we will probably have to support inclusive and exclusive ranges -- weird syntax,
- solves only problem A.

### Multiple version operator approach

#### Multiple operator conjunction

Support specifying multiple version restrictions. All of the restrictions must match (i.e. logical AND).

```
dev-foo/bar>=1.3<1.5
dev-foo/bar{>=1.3,<1.5}
dev-foo/bar>=1.3<1.5!=1.4.1
dev-foo/bar{>=1.3,<1.5,!=1.4.1}
```
Problems:

- solves only problem A,
- syntax without additional braces looks confusing with slot op :=, e.g. dev-foo/bar:===4.0.

#### Multiple operator conjunction or disjunction

This is the Exherbo approach. The restriction can be either AND-ed or OR-ed (but not both).

dev-foo/bar\[>=1.3&\<1.5\]
dev-foo/bar\[>=1.3&\<1.5&!=1.4.1\]
dev-foo/bar\[\<1.1|>=1.5\]
dev-foo/bar\[=1.1\*|=1.3\*|>=1.5\]

Problems:

- expressing two disjoint ranges requires multiple != exclusions,
- the proposed syntax uses infix operators while most of logic uses prefix (i.e. || ( ... )) (pointed out by \[User:Ulm\]).

#### Full-blown logic

Alternatively, we can support AND, OR and nesting.

dev-foo/bar\[(>=1.3&\<1.5)|(>=0.9&\<1.1)\]

Problems:

- added complexity.

## Version suffixes and upper/lower bound problem

### Negative suffixes

Versions can be followed by zero or more suffixes, of which \_pre, \_alpha, \_beta and \_rc are negative, that is represent version evaluating lower than the preceding version components. It is a cause of somewhat common mistakes.

For example:

\<dev-foo/bar-1.5

matches 1.5\_rc1. And:

\>=dev-foo/bar-1.5
!>=dev-foo/bar-1.5

both do not match 1.5\_rc1.

It can be somewhat reasonably solved via appending \_alpha suffix (the lowest negative suffix supported):

\<dev-foo/bar-1.5\_alpha\_alpha
>=dev-foo/bar-1.5\_alpha\_alpha

but see below.

### Upper/lower bound

The version syntax has infinite range and precision. There are no formal restrictions on values, lengths or count of version components. The suffixes can be used to infinitely construct versions lower or higher than the base version.

In other words, for each version X, one can always construct a lower version X\_pre (X\_alpha, X\_beta, X\_rc) and a newer version X\_p. Therefore, it is impossible to fully reliably restrict version, including any possible suffixes. For example:

\<dev-foo/bar-1.5\_alpha\_alpha

will still match 1.5\_alpha\_alpha\_rc.

### Possible solution: explicit lower bound suffix

A possible solution is to include an additional suffix that can not be used in package versions, and always evaluates lower to any possible suffix. Example:

\<dev-foo/bar-1.5\_min

that will match any combination of 1.5\_alpha\_alpha\_alpha...

For completeness, a \_max suffix may be considered as well:

\>dev-foo/bar-1.5\_max

However, it is unclear if it should be limited to outgrowing suffixes only (i.e. >\_p999999...\_p999999....), or also additional version components.

### Existing: \~ operator

#### Current status

The existing \~ operator can be used to depend on a specific version of package while allowing for Gentoo-specific revisions to differ.

\~dev-foo/bar-1.4.2

is equivalent to range:

\>=dev-foo/bar-1.4.2
\<dev-foo/bar-1.4.2\_p    # smallest next valid version

Problems:

- non-symmetry (\~ is modification of =, but no variants for \<, \<=... are provided),
- inability to further restrict allowed revisions (i.e. v1.4.2, r2+).

#### Alternative: version ranges

We can possibly replace the \~ operator with explicit version ranges:

dev-foo/bar\[>=1.4.2,\<1.4.2\_p\]

Problems:

- decreased readability,
- suffers from the lower-bound problem (\_p\_pre \< \_p).

#### Alternative: regular operators matching only versions, explicit suffixes for revisions

As an additional alternative, it has been considered that we could change the regular version operators \<, \<=, =, >=, > to match on versions only, ignoring revision. This would make it more in line with upstream dependency specifications. Explicit revision suffix would be necessary to match revisions.

For example:

dev-foo/bar==1.4.0        # version 1.4.0, any revision
dev-foo/bar==1.4.0-r0     # version 1.4.0, r0
dev-foo/bar>1.4.0         # version >1.4.0 (but not a revision of 1.4.0)
dev-foo/bar>1.4.0-r0      # version >1.4.0 or 1.4.0 with r>0
dev-foo/bar\<=1.4.0        # version \<=1.4.0, including any revision of 1.4.0
dev-foo/bar\<=1.4.0-r0     # version \<1.4.0, or 1.4.0-r0

Advantages:

- no new operators,
- solves end-of-rev-range problem (below) as well.

Problems:

- change of behavior on existing operators -- likely to cause confusion and mayhem,
- different behavior of operator depending on whether revision is specified or not -- even more confusion,
- still a bit of redundancy (\<X \<=> \<X-r0 and >=X \<=> >=X-r0) but less irritating due to no new ops.

#### Alternative: regular operators matching only versions, additional ops for revisions

The alternative variant of the above, with revision matching explicitly requiring special operators.

dev-foo/bar==1.4.0        # version 1.4.0, any revision
dev-foo/bar===1.4.0-r0    # version 1.4.0, r0
dev-foo/bar>1.4.0         # version >1.4.0 (but not a revision of 1.4.0)
dev-foo/bar>==1.4.0-r1    # version >1.4.0 or 1.4.0 with r>1
dev-foo/bar\<=1.4.0        # version \<=1.4.0, including any revision of 1.4.0
dev-foo/bar\<==1.4.0-r0    # version \<1.4.0, or 1.4.0-r0

Advantages:

- solves end-of-rev-range problem,
- -r becomes prohibited with normal operators, so it'll more likely blow up verbosely than silently misbehave,
- proposed weird syntax limits new ops to ===, >== and \<== since revisions are integers and so > and >= is pretty much redundant.

Disadvantages:

- change of behavior on existing operators -- likely to cause confusion and mayhem,
- may look kinda weird with additional = switching between version and revision precision,
- intentional lack of > and \< for revisions might get someone confused, trying >= and \<= to achieve the same behavior.

### Missing: end-of-rev-range operators

#### Problem

Currently we lack a way to express -r9999... that would match all possible revisions of the package. It could be useful with > and \< (>= and \<= would be equivalent, since the max value is not limited) operators:

\>dev-foo/bar-1.4-r9999...      # matches 1.4\_p, 1.4.0, 1.5...
\<=dev-foo/bar-1.4-r9999...     # matches \~1.4 or less

Problems:

- kinda ugly,
- revision value is not limited and some developers already use 8-digit revisions (dates).

#### Using next valid version

Alternatively, we can use the next valid version:

\>=dev-foo/bar-1.4\_p
\<dev-foo/bar-1.4\_p

Advantages:

- no extra syntax necessary,
- always matches all revisions.

Problems:

- still ugly-ish,
- to strictly match any crazy variant possible, we would be always be adding *\_p* e.g. \~1.4\_p -> >=1.4\_p\_p,
- suffers from the lower bound problem.

#### Range representation

The \<= variant can be represented using range conjunction.

dev-foo/bar\[\<1.4|\~1.4\]

The > variant would require negation of \~:

dev-foo/bar\[>1.4&!\~1.4\]

Advantages:

- requires only \~ and !\~.

Problems:

- kinda awkward.

#### Additional operators

This could be accomplished via adding special >\~ and \<=\~ operators, with \~ indicating *match versions without considering revisions*:

\>\~dev-foo/bar-1.4              # version >1.4
\<=\~dev-foo/bar-1.4             # version \<=1.4

(the >=\~ and \<\~ would logically be equivalent to >= and \<)

Problems:

- may be a bit confusing: >\~1.4 looks like it starts with -r0 but it actually does not cover -r9999...,
- (1.4-r3 \<=\~ 1.4 \<=\~ 1.4-r1) is true \[but that probably shouldn't be considered since we are comparing versions\],
- ruby uses \~> for another purpose,
- redundancy or non-symmetry: we can add >=\~ and \<\~ ops, or reduce both to >\~ and \<\~ (in both cases additional confusion possible).

#### Explicit -rMAX

Alternatively, a special -r value can be added to indicate a value larger than any valid revision.

\>dev-foo/bar-1.4-rMAX
\<=dev-foo/bar-1.4-rMAX

Advantages:

- clearly readable.

Problems:

- additional magic value that is not valid in versions.

#### Regular operators matching versions only

Alternative full \~ replacements that solve this problem:

- [regular operators matching only versions, explicit suffixes for revisions](https://wiki.gentoo.org#Alternative:_regular_operators_matching_only_versions.2C_explicit_suffixes_for_revisions),
- [regular operators matching only versions, additional ops for revisions](https://wiki.gentoo.org#Alternative:_regular_operators_matching_only_versions.2C_additional_ops_for_revisions).

### Missing: revision ranges

#### Problem

Sometimes it may be useful to combine \~ with restricting on specific revision range. For example, when upstream requires a specific version and we require a specific revision due to Gentoo fixes. For example:

\~dev-foo/bar-1.4.2
>=dev-foo/bar-1.4.2-r3

#### Additional operators (GLSA solution)

Gentoo GLSAs already specify four additional operators for this specific purpose: rlt, rle, rge, rgt:

\<vulnerable range="rle">1.19\</vulnerable>
\<unaffected range="rge">4.5.2-r5\</unaffected>
\<unaffected range="rgt">4.4.12\</unaffected>

It is unnecessary to include four operators, since in all cases the previous/next revision value is predictable. For symmetry, rle and rge can included:

dev-foo/bar RGE 1.4-r2  # 1.4-r2 .. 1.4-r9999...
dev-foo/bar RLE 1.4-r6  # 1.4 .. 1.4-r6

(note: I didn't have any idea for a fancy symbol for it)

Problems:

- additional operators for a seemingly rare case,
- does not cover non-lower/upper-bound revision ranges (i.e. -r4 .. -r7).

#### Solution using version ranges

The canonical solution using other operators:

dev-foo/bar\[\~1.4&>=1.4-r2\]
dev-foo/bar\[\~1.4&\<=1.4-r6\]
dev-foo/bar\[>=1.4-r4&\<=1.4-r7\]

Advantages:

- no new operators needed.

## Component-wide prefix comparison

### Existing: =...\* operator

#### Current status

The existing = operator can be used with \* wildcard on the end of the version to indicate component-wide prefix comparison.

=dev-foo/bar-1\*

is equivalent to range:

\>=dev-foo/bar-1\_alpha\_alpha... # this can go on to infinity
\<dev-foo/bar-2         # next valid value of the last version component

Problems:

- non-symmetry (\<, \<=... do not support \* variants),
- often confused with wildcard,
- sometimes confused not to match \_pre, \_rc... suffixes.

#### Alternative using version ranges

Alternatively, the same can be expressed using:

dev-foo/bar\[>=1\_alpha\_alpha&\<2\]

or, if matching \_alpha\_alpha is not desired:

dev-foo/bar\[>=1&\<2\]

Advantages:

- more flexible

Problems:

- suffers from the lower bound problem.

### Missing: \<=, >= prefix-wide comparisons

#### Problem

It is sometimes useful to depend on a range of versions starting at the lower or upper bound of a specific prefix. For example:

\>=dev-foo/bar-1.4\_alpha\_alpha   # all 1.4\* and newer
\<dev-foo/bar-1.5        # all 1.4\* and older

The former case suffers from the lower-bound problem, while the latter requires figuring out the correct next version.

#### Prefix-wise comparison operators

For symmetry, \* support could be added to other operators to indicate prefix-wide comparisons, e.g.:

\<dev-foo/bar-1.4\*      # \<=> \<dev-foo/bar-1.4\_alpha...
\<=dev-foo/bar-1.4\*     # \<=> \<=dev-foo/bar-1.4\_p....
>=dev-foo/bar-1.4\*     # \<=> >=dev-foo/bar-1.4\_alpha...
>dev-foo/bar-1.4\*      # \<=> >dev-foo/bar-1.4\_p...

This means that the specific comparison is applied to all version components up to the specified part. Or speaking otherwise, it is a union of versions matching =dev-foo/bar-1.4\* and \</\<=/>=/>dev-foo/bar-1.4.

Advantages:

- solves lower and upper bound problem.

Problems:

- potentially confusing,
- (1.4.2 \<= 1.4\* \<= 1.4.1) is true \[but we should be considering prefixes only anyway\].

#### Alternative: version ranges with =...\*

The same can be expressed using conjunctive and disjunctive version ranges combined with the =...\* and !=...\* operator:

dev-foo/bar\[>=1.4|==1.4\*\]     # ==1.4\* adds 1.4\_alpha...
dev-foo/bar\[\<=1.4|==1.4\*\]     # ==1.4\* adds 1.4\_p...
dev-foo/bar\[>1.4&!=1.4\*\]     # !=1.4\* removes 1.4\_p...
dev-foo/bar\[\<1.4&!=1.4\*\]     # !=1.4\* removes 1.4\_alpha...

Advantages:

- more obvious when reading.

Disadvantages:

- less obvious to write correctly.

#### Alternative: version ranges with implicit/explicit lower bound

The more obvious solution is to explicitly use lower (or upper) bound:

\>=dev-foo/bar-1.4\_alpha\_alpha
\<dev-foo/bar-1.5

Problems:

- we're back to lower/upper bound problem.

### Missing: prefix match sub-ranges

#### Problem

Sometimes it is necessary to restrict version to a specific prefix, and further restrict the minimal subversion, e.g.:

=dev-foo/bar-1.4\*
>=dev-foo/bar-1.4.2

#### Ruby \~> operator

Ruby (and Exherbo) use \~> operator for this specific case. It is equivalent to >= on the specified version combined with prefix matching of the version stripped of the last component. For example:

\~>dev-foo/bar-1.4.2

indicates that the version needs to be at least 1.4.2 but in 1.4\* range.

Problems:

- non-symmetric (is there a use case for \< variant?),
- collides with current meaning of \~,
- unclear wrt different kinds of versions (letters, suffixes, negative suffixes...).

#### Alternative: version ranges

Again, this can be implemented using version ranges:

dev-foo/bar\[>=1.4.2&==1.4\*\]
dev-foo/bar\[>=1.4.2&\<1.5\_alpha\_alpha\]
