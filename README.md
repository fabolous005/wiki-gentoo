# wiki-gentoo

Archive of a curated subset of the [Gentoo Wiki](https://wiki.gentoo.org),
pulled via its sitemap / MediaWiki API and converted to clean Markdown.

## Scope

| Group | Source | Content |
|---|---|---|
| **Gentoo Handbook** | Handbook namespace (NS 560), 4 fixed titles | The "Full" AMD64 handbook, split into its 4 parts: Installation, Working, Portage, Networking |
| **Gentoo Overlay** | Overlay namespace (NS 520), full enumeration | Every page in the Overlay namespace |
| **Gentoo Knowledge** | Knowledge Base namespace (NS 500), full enumeration | Every page in the Knowledge Base namespace |
| **Gentoo Wiki (Main)** | Main namespace (NS 0), full enumeration | Every article in the main namespace |

All four sets are filtered to English/language-neutral pages only (see
below), and the Main namespace additionally excludes redirect stubs.

## Repo layout

```
gentoo-wiki-archiver          # the archiver script (mirrors /usr/local/bin/gentoo-wiki-archiver)
links/
  all_target_links.tsv        # group<TAB>wiki_title<TAB>url, one row per page
  all_target_links.txt        # same URLs, one per line
markdown/
  main/                        # Gentoo Wiki (Main) pages
  knowledge/                   # Gentoo Knowledge (Knowledge Base) pages
  overlay/                     # Gentoo Overlay pages
  handbook/                    # Gentoo Handbook (4 parts)
state/
  run_summary.json             # ok/skipped/failed counts from the last run
  failed_urls.txt              # url<TAB>error for any page that failed after retries
```

Each Markdown file starts with a `<!-- source: ... | group: ... |
wiki-title: ... -->` provenance comment, followed by trafilatura's own
YAML front matter (title/url/date/license/fingerprint).

## How the list is built

1. **Main namespace (NS 0)**: enumerated directly from the MediaWiki API —
   `action=query&list=allpages&apnamespace=0&apfilterredir=nonredirects`,
   paginated. This excludes redirect stubs (e.g. `Overlay`, `Overlayfs`,
   `Overlays` all redirect to the real article `OverlayFS`/`Ebuild
   repository`) at the source, since MediaWiki serves a redirect's
   *target* content with HTTP 200 directly at the redirect's own URL —
   a generic page fetch can't otherwise tell the difference.
2. **Knowledge Base (NS 500) / Overlay (NS 520)**: enumerated from the
   per-namespace sitemaps listed in
   `https://wiki.gentoo.org/sitemap/sitemap-index-gentoowiki.xml`.
   *Known gap:* unlike the Main namespace, these are not yet filtered
   through `apfilterredir=nonredirects`, so a handful of redirect pages
   may still be included.
3. **Handbook (NS 560)**: only the 4 explicit titles
   `Handbook:AMD64/Full/{Installation,Working,Portage,Networking}` are
   kept — the Handbook namespace also contains per-architecture
   (x86, ARM, PPC, ...) and per-block variants that are out of scope here.

### Language-code filtering

Gentoo Wiki uses `Extension:Translate`: a language-neutral page `Foo` has
translated siblings at `Foo/<langcode>` (including `Foo/en`, a distinct
explicit-English copy). Any URL whose **last** path segment exactly
matches one of the following codes is dropped, keeping only the single
bare/default page per title:

```
ar, bg, ca, cs, da, de, el, en, en-gb, eo, es, fa, fi, fr, he, hr, hu,
id, it, ja, ko, lt, nl, pl, pt, pt-br, rue, ro, ru, sk, sl, sr-ec, sv,
ta, th, tr, uk, uz, vi, zh, zh-cn, zh-hans, zh-hant, zh-tw
```

## How pages are downloaded

Each URL is fetched with `trafilatura.fetch_url()` and converted with
`trafilatura.extract(output_format="markdown", favor_precision=True,
prune_xpath=[...])`. The `prune_xpath` list strips MediaWiki chrome that
would otherwise pollute the article text: the language-switcher box,
Gentoo's "quick-nav"/`gw-box` templates, inline edit-section links, and
the footer print/category links.

Downloads run concurrently (`ThreadPoolExecutor`, default: one worker
per CPU core), are resumable (an existing output file is skipped unless
`--force`), and retried up to 3 times with backoff on transient failure.
Anything that still fails is logged to `state/failed_urls.txt` instead
of silently dropped.

## Usage

The script requires the venv at `/opt/gentoo-wiki-archiver/venv`
(trafilatura + requests) — it's referenced directly in its shebang, so
it runs standalone once that venv exists.

```sh
gentoo-wiki-archiver                              # (re)build list + download everything
gentoo-wiki-archiver --list-only                   # only rebuild links/*
gentoo-wiki-archiver --group "Gentoo Overlay"      # restrict to one group
gentoo-wiki-archiver --jobs 8                      # override concurrency (default: CPU core count)
gentoo-wiki-archiver --limit 20                    # cap total pages (testing)
gentoo-wiki-archiver --force                       # redownload even if the output file exists
gentoo-wiki-archiver --cache-dir /some/other/dir   # override the workdir (default: /var/lib/wiki-gentoo)
```

The canonical, executable copy lives at `/usr/local/bin/gentoo-wiki-archiver`;
the copy in this repo (`./gentoo-wiki-archiver`) is kept in sync for
provenance/version history alongside the data it produces.

## Current status

- `links/` is up to date: 2,830 URLs (Main 2,778 / Knowledge 44 / Overlay 4
  / Handbook 4).
- `markdown/` currently only contains the 4 **Gentoo Overlay** pages,
  downloaded as a smoke test of the pipeline end-to-end. The full run
  (Main + Knowledge Base + Handbook, ~2,826 more pages) has not been
  executed yet.

## Incremental updates (lastmod diffing)

Every sitemap entry carries a `<lastmod>` timestamp. The link list now
carries it too (`links/all_target_links.tsv` has a 4th column: `group`,
`title`, `url`, `lastmod`), and `state/last_fetched.json` records the
`lastmod` that was in effect the last time each URL was *successfully*
written to disk.

On each run, a page is **skipped** (no network fetch) unless:
- its output `.md` file doesn't exist yet (new page), or
- it's tracked in `state/last_fetched.json` AND that recorded `lastmod`
  differs from the current one (edited upstream since last run), or
- `--force` is passed.

A URL with no prior entry in `state/last_fetched.json` but an existing
output file is trusted as-is (first run after the feature was added, or
state file lost) — it will get a real diff check on the next run once
its `lastmod` is recorded. Main-namespace `lastmod` values come from a
dedicated fetch of the NS_0 sitemap (purely for timestamps — page
enumeration itself still goes through the API, not the sitemap, per the
redirect-exclusion reasoning above).

Verified: a full run with a warm cache (nothing changed upstream) completes
in ~14s with `ok=0 skipped=2830 failed=0`, vs. several minutes for a cold
full fetch.

## Daily cron job

`/usr/local/bin/gentoo-wiki-cron.sh` (root crontab, `30 3 * * *`):
1. Runs `gentoo-wiki-archiver` (list rebuild + incremental download, as above).
2. `git add -A`; if `git status --porcelain` is non-empty, commits
   (message includes the run's ok/skipped/failed counts via `jq`) and
   pushes to `origin main`. No-op commit when nothing changed.

Log: `/var/log/gentoo-wiki-archiver.log` (appended, not rotated — watch
its size over time).

## Known caveats

1. Knowledge Base (NS 500) and Overlay (NS 520) link lists are sitemap-based
   and may still include a few redirect pages (see "known gap" above).
2. Gentoo's custom `root #` / `user $` shell-prompt templates render a bit
   awkwardly in a handful of code blocks after Markdown conversion
   (cosmetic only — command text itself is intact).
3. Content is licensed CC BY-SA by the Gentoo Foundation (see each file's
   front matter); this repo is a personal archive/mirror, not a
   redistribution-ready republish.
