<!-- source: https://wiki.gentoo.org/wiki//etc/portage/repo.postsync.d | group: Gentoo Wiki (Main) | wiki-title: /etc/portage/repo.postsync.d -->
---
title: "/etc/portage/repo.postsync.d"
url: https://wiki.gentoo.org/wiki//etc/portage/repo.postsync.d
hostname: gentoo.org
sitename: "/etc/portage/repo.postsync.d"
date: "2026-08-27"
fingerprint: "9a175c0c2674f9f0"
license: CC BY-SA 4.0
---

# /etc/portage/repo.postsync.d

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

/etc/portage/repo.postsync.d is a directory for user supplied postsync hooks to be run once after each repository has been synced. The hooks are run in lexical order and will have arguments for `repository name`, `sync-uri` and `location` supplied. Hooks are treated as scripts and need the executable bit set chmod +x.

## Metadata cache generation examples

An example hook using [Egencache](https://wiki.gentoo.org/wiki/Egencache) to generate metadata cache is located in /usr/share/portage/config/repo.postsync.d/example.

### Pkgcore example

Alternatively metadata cache can be generated with pmaint from [sys-apps/pkgcore](https://packages.gentoo.org/packages/sys-apps/pkgcore). Pmaint is significantly faster than egencache for generating metadata<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

FILE **`/etc/portage/repo.postsync.d/99-generate-cache-pmaint`**

```
#!/usr/bin/env bash
# Make it executable (chmod +x) for Portage to process it.
# Requires sys-apps/pkgcore for pmaint
# Your hook can control it's actions depending on any of the three
# parameters passed in to it.
#
# They are as follows:
#
# The repository name.
repository_name=${1}
# The URI to which the repository was synced.
sync_uri=${2}
# The path to the repository.
repository_path=${3}
# Portage assumes that a hook succeeded if it exits with 0 code. If no
# explicit exit is done, the exit code is the exit code of last spawned
# command. Since our script is a bit more complex, we want to control
# the exit code explicitly.
ret=0
if [[ -n "${repository_name}" ]]; then
	# Repository name was provided, so we're in a post-repository hook.
	echo "* In post-repository hook for ${repository_name}"
	echo "** synced from remote repository ${sync_uri}"
	echo "** synced into ${repository_path}"
	# Gentoo, Guru, kde and science come with pregenerated cache but the other repositories usually don't.
	# Generate them to improve performance.
	# https://www.gentoo.org/support/news-items/2025-10-07-cache-enabled-mirrors-removal.html
	if [[ "${repository_name}" != "gentoo" ]] && [[ "${repository_name}" != "guru" ]] && \
		[[ "${repository_name}" != "kde" ]] && [[ "${repository_name}" != "science" ]]
	then
		if ! pmaint regen "${repository_name}" --threads $(nproc) --pkg-desc-index --use-local-desc ${PORTAGE_VERBOSE+--verbose}
		then
			echo "!!! pmaint regen failed!"
			ret=1
		fi
	fi
fi
exit "${ret}"
```
## See also

- [postsync.d (AMD64 Handbook)](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Advanced#Executing_tasks_after_ebuild_repository_syncs) — a directory for  user supplied postsync hooks to be run once after all repositories
- [Ebuild\_repository#Cache\_generation](https://wiki.gentoo.org/wiki/Ebuild_repository#Cache_generation) — generating metadata cache for repositories
- [Pkgcore](https://wiki.gentoo.org/wiki/Pkgcore) — tooling for ebuild QA and metadata generation
- [pkgcraft](https://wiki.gentoo.org/wiki/Pkgcraft) — experimental tooling ecosystem for Gentoo written in Rust

## External resources

- [portage - the heart of Gentoo](https://dev.gentoo.org/~zmedico/portage/doc/man/portage.5.html) — portage man page.
