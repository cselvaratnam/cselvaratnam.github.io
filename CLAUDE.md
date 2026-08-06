# Christian Selvaratnam — personal site

Jekyll site published by GitHub Pages at <https://cselvaratnam.github.io>, using
[Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/) as a **remote
theme** pinned to `4.28.0`.

## Traps — read these first

**Never delete `_data/ui-text.yml`.** Jekyll themes only supply `_layouts`,
`_includes`, `_sass` and `assets` — never `_data`. The theme's own copy of that
file is therefore *not* loaded. Strings the theme prints with a `| default:`
fallback (Feed, Toggle menu, Powered by) still appear, which disguises the
problem; the three printed *without* a fallback — `follow_label`, `meta_label`,
`toc_label` — silently render blank. Changing `locale` does not fix it. The same
reasoning is why `_data/navigation.yml` must live here too.

**This is a GitHub *user* site.** The repo must stay named
`cselvaratnam.github.io` and `baseurl` must stay empty.

**Pages source must be "Deploy from a branch"** (`main` / root), not GitHub
Actions, or `remote_theme` will not resolve.

**`_pages` must stay in `include:`** in `_config.yml` — folders starting with an
underscore are otherwise ignored.

**Local preview needs Ruby 3.x.** The macOS system Ruby is 2.6 and too old.
GitHub builds the site itself, so previewing is optional.

## Layout

Single column throughout. Set **once** in the `defaults` block at the foot of
`_config.yml` via `author_profile: false` (no sidebar) and `classes: wide` (no
empty gutter). Do not scatter these across individual pages.

## Structure

| File | Purpose |
|---|---|
| `index.md` | Landing page (`permalink: /`) |
| `_pages/*.md` | About, Books, Writing, Teaching — each with its own `permalink` |
| `_data/navigation.yml` | The masthead nav |
| `_data/ui-text.yml` | Theme interface strings — see above |
| `_config.yml` | All configuration, including the single-column defaults |
| `assets/images/` | Images, incl. `banner.jpg` used as the home page header |
| `assets/documents/` | PDFs linked from the site |
| `assets/css/main.scss` | Optional custom CSS; safe to delete |

Four pages only. Do not add pages, posts or a `_posts` collection unless asked.

## Publishing

`../sync.sh ["message"]` — sits one level up, outside the repo. It shows what is
about to go public, asks for confirmation (`-y` skips), commits and pushes.
GitHub rebuilds within a minute or two.

`../TODO.md` is Christian's private list. It lives outside the repo
deliberately: never publish it, never move it in.

## Writing conventions

British English, Guardian and Observer style, single quotation marks.

**Typography follows what the thing is — never use bold for titles:**

- Standalone works (books, publications, journals) → *italics*
- Parts of works (chapters, articles, posts) → 'single quotes'
- Programmes, courses and organisations → plain text

Entries are a title line followed by a metadata line (publisher and year, or
publication and date). Descriptions are optional and at most one sentence.

**Voice:** the About page is third person, as a formal biography. The landing
page and the intro paragraph at the top of Books, Writing and Teaching are first
person, and deliberately understated — no self-promotion.

Link text should describe its destination ('the books page'), not be a bare
'here'.

## Links

**Verify every external link before publishing it** — several on Christian's
linktree are dead. Check with `curl -sIL -o /dev/null -w "%{http_code}"`.

These hosts return 403/999 to `curl` but are fine in a browser — do not treat
them as broken: `scmpress.hymnsam.co.uk`, `churchtimes.co.uk`,
`asburyseminary.edu`, `linkedin.com`. The in-app browser gets through where
`curl` cannot, and the *Church Times* author page is a useful index.

Never invent a URL. If no working link exists, leave the entry unlinked and say
so.

## Care

Ask before publishing anything personal or third-party — email addresses, PDFs
of other organisations' publications, photographs of people. Git history is
public and effectively permanent.
