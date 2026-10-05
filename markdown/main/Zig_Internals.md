<!-- source: https://wiki.gentoo.org/wiki/Zig/Internals | group: Gentoo Wiki (Main) | wiki-title: Zig/Internals -->
---
title: Zig/Internals
url: https://wiki.gentoo.org/wiki/Zig/Internals
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-09"
fingerprint: "26065e0acc57efd0"
license: CC BY-SA 4.0
---

# Zig/Internals

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes how [Zig](https://wiki.gentoo.org/wiki/Zig) compiles Zig source code into an executable.[\[1\]](https://wiki.gentoo.org#cite_note-1)

## Tokenizer

Zig splits the input buffer into Token, defined at this [file](https://github.com/ziglang/zig/blob/master/lib/std/zig/tokenizer.zig):

**`lib/std/zig/tokenizer.zig`**

```
pub const Token = struct {
        tag: Tag,
        loc: Loc,
        pub const Loc = struct {
            start: usize,
            end: usize,
        };
        ...
        pub const Tag = enum {
            invalid,
            invalid_periodasterisks,
            identifier,
            string_literal,
            multiline_string_literal_line,
            char_literal,
            eof,
            ...
    };
    ...
    pub const Tag = enum {
        invalid,
        invalid_periodasterisks,
        identifier,
        string_literal,
        multiline_string_literal_line,
        char_literal,
        eof,
    pub const Tokenizer = struct {
        buffer: [:0]const u8,
        index: usize,
    ...
        const State = enum {
            start,
            expect_newline,
            identifier,
            builtin,
            string_literal,
    ...
        /// After this returns invalid, it will reset on the next newline, returning tokens starting from there.
        /// An eof token will always be returned at the end.
        pub fn next(self: *Tokenizer) Token {
    ...
```
Token tag field defines what type of token it is, a keyword or doc comment. Token Loc field defines the token contents, not including at the end in Tokenizer buffer field.

Tokenizer buffer field is the input file.
Tokenizer index is for next() to start parsing at.
The enum State is meant the store the state inside next() is currently at to help contruct Token in [DFA](https://en.wikipedia.org/wiki/Deterministic_finite_automaton).

std.zig.Ast.parse() uses next() to get tokens.

## Parser

Zig's parser then executes std.zig.Ast.parse() to turn tokens to abstract syntax tree:

**`lib/std/zig/Ast.zig`**

```
...
/// The root AST node is assumed to be index 0. Since there can be no
/// references to the root node, this means 0 is available to indicate null.
nodes: NodeList.Slice,
extra_data: []Node.Index,
...
pub fn parse(gpa: Allocator, source: [:0]const u8, mode: Mode) Allocator.Error!Ast {
...
    switch (mode) {
        .zig => try parser.parseRoot(),
        .zon => try parser.parseZon(),
    }
...
}
...
```
std.zig.Ast.parse() uses std.zig.Parse.parseRoot() to parse the source file. The comments in parse\*() functions reference to this [grammar file](https://ziglang.org/documentation/master/#Grammar).

**`lib/std/zig/Parse.zig`**

```
...
/// Root <- skip container_doc_comment? ContainerMembers eof
pub fn parseRoot(p: *Parse) !void {
...
```
