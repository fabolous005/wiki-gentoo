<!-- source: https://wiki.gentoo.org/wiki/Bash/Installing_completion_files | group: Gentoo Wiki (Main) | wiki-title: Bash/Installing completion files -->
---
title: Bash/Installing completion files
url: https://wiki.gentoo.org/wiki/Bash/Installing_completion_files
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-08-14"
fingerprint: "1f6eba1a6b650e21"
license: CC BY-SA 4.0
---

# Bash/Installing completion files

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page shortly lists guidelines for installing bash-completion files in a consistent and forward-compatible way.

1. Completion files are to be installed to the directory specified by `$(get_bashcompdir)` from bash-completion-r1 (dobashcomp and newbashcomp respect that).
  - Allowed exception: if the build system has hardcoded `/usr/share/bash-completion/completions` path, it is acceptable to leave it as-is and not use the eclass.
  - Allowed exception: if the build system uses pkg-config to determine completionsdir and doesn't allow override, it is acceptable to leave it as-is. However, you need to DEPEND on [app-shells/bash-completion](https://packages.gentoo.org/packages/app-shells/bash-completion) then -- so it's better to actually make the path configurable.
2. Completion files must be named after **all** commands they're completing. For example, completion for pulseaudio has to be named 'pulseaudio' and not 'pulseaudio-bash-completion.sh'.
  - If a completion file provides completions for multiple commands, it should be installed for one of the commands, and symlinked for the others. For example, 'pacat', 'paplay', 'parecord' etc. should be all symlinked to 'pulseaudio'. Extra aliases can be created using 'bashcomp\_alias' function from the eclass.
3. The 'have' function that has been often used in completion files has been deprecated and will not work with autoloaded completions.
  - When 'have' was used to conditionally load completions, it should be removed and completions can be loaded unconditionally. This is because the completion file won't be sourced before the command is typed anyway, and it is not likely that users type commands for non-installed programs.
  - When 'have' was used **inside** the completion function for some reason, it may be replaced with '\_have'. However, it should be noted that the latter does not set 'have' variable like 'have' did.
4. `USE=`[bash-completion](https://packages.gentoo.org/useflags/bash-completion)
  - The use of the USE flag is justified when it pulls in additional dependencies.
  - USE flag can also be used to control installing compatdir (`/etc/bash_completion.d`) files. However, use of that directory is strongly discouraged.
5. Run-time dependencies on [app-shells/bash-completion](https://packages.gentoo.org/packages/app-shells/bash-completion) are really unnecessary. If you don't have bash-completion, completions won't be loaded and there will be no issue.
  - To enforce a particular version of bash-completion, it is better to use reversed blocker atom.  For example, `>=app-shells/bash-completion-1.9` should be replaced with `!!<app-shells/bash-completion-1.9`. This way, it is possible to enforce meeting the version requirements when bash-completion is installed, and not force installing it when it is not.
  - If the build system uses pkg-config to find bash-completion and refuses to install the completions if it is not installed, it is acceptable to add a build-time dependency on app-shells/bash-completion. However, it is recommended to fix the build system instead.
