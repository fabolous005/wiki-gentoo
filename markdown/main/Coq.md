<!-- source: https://wiki.gentoo.org/wiki/Coq | group: Gentoo Wiki (Main) | wiki-title: Coq -->
---
title: Coq
url: https://wiki.gentoo.org/wiki/Coq
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-02-08"
fingerprint: "84404a0e6afb7a4e"
license: CC BY-SA 4.0
---

# Coq

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**Coq** is a formal proof management system, for formalizing and machine-checking proof written in [Ocaml](https://en.wikipedia.org/wiki/OCaml). Coq is based on the Calculus of Inductive Constructions.<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> From the official website

It (Coq, red.) provides a formal language to write mathematical definitions, executable algorithms and theorems together with an environment for semi-interactive development of machine-checked proofs.[\[2\]](https://wiki.gentoo.org#cite_note-2)


## Installation


### USE flags for
            [sci-mathematics/coq](https://packages.gentoo.org/packages/sci-mathematics/coq)
            
            Coq/Rocq is a proof assistant written in O'Caml

| [+ocamlopt](https://packages.gentoo.org/useflags/+ocamlopt) | Enable ocamlopt support (ocaml native code compiler) -- Produces faster programs (Warning: you have to disable/enable it at a global scale) | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [gui](https://packages.gentoo.org/useflags/gui) | Enable support for a graphical user interface | 
| [native-compiler](https://packages.gentoo.org/useflags/native-compiler) | Enable "native\_compute" and compile the Coq Standard Library | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

Coq can be installed from the official gentoo repositories.

`root #``emerge --ask sci-mathematics/coq`
## User interfaces

Coq supports multiple user interfaces, such as VSCode, [Emacs](https://wiki.gentoo.org/wiki/Emacs), [Vim](https://wiki.gentoo.org/wiki/Vim)/[Neovim](https://wiki.gentoo.org/wiki/Neovim) as well as its own [CoqIDE](https://wiki.gentoo.org/index.php?title=CoqIDE&action=edit&redlink=1).

### VSCode

Users of Visual Studio Code can use the VsCoq extention which is currently maintained by the coq-community.[\[3\]](https://wiki.gentoo.org#cite_note-3)

### Emacs

Users of Emacs can use the major Coq mode [Proof General](https://proofgeneral.github.io/) and extend that with the minor Coq mode [Company-Coq](https://github.com/cpitclaudel/company-coq).

### Vim/Neovim

Users of Vim/Neovim can use the [Coqtail](https://github.com/whonore/Coqtail) plugin.

## Further reading

For a thorough introduction to logic in Coq see the free e-book [Logical Foundation](https://softwarefoundations.cis.upenn.edu/lf-current/index.html) from the University of Pennsylvania.
