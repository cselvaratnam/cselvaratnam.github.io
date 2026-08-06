# Christian Selvaratnam — personal site

## Keep this file up to date

**Update this file in the same session you learn something.** Do not wait to be
asked, and do not leave it for later.

Write it down whenever Christian states a preference, corrects a wording, makes
a decision about how the site is structured, or you hit a trap in the theme that
cost you time. If a rule here turns out to be wrong, fix it rather than adding a
second, contradictory rule next to it.

This file is the reason a fresh session does not repeat old mistakes. It is
worth more than tidy code.

## Where things live

| Path | What |
|---|---|
| `~/Documents/Website/cselvaratnam.github.io/` | The site repo — this file, all pages, everything published |
| `~/Documents/Website/sync.sh` | Publish script, deliberately **outside** the repo |
| `~/Documents/Website/TODO.md` | Christian's private list, outside the repo so it is never published |

This file stays **inside the repo**. That way it is version-controlled and
backed up on GitHub along with everything else. The one catch: if you start a
session in `~/Documents/Website` rather than in the repo folder, it will not be
picked up automatically — open the repo folder, or read this file first.

Note that `sync.sh` and `TODO.md` are **not** in git and
**not** in Dropbox. They exist on one disk only.

## Talk to Christian in plain English

He is not a developer. Explain things the way you would to a capable colleague
who does not work with code.

- Say what happened and what it means for him, not what the tooling did.
- Skip the jargon. If a technical term is genuinely needed, say what it means
  in the same sentence.
- Do not paste raw logs, selectors, measurements or API output at him unless he
  asks. Give the conclusion.
- Keep it short. One or two sentences usually does it.

Bad: 'The float removal collapsed the BFC so `.page` had zero computed height.'
Good: 'The page content was overflowing its container, which left a gap above
the footer. Fixed.'

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

**Do not delete the single-column override in `assets/css/main.scss`.** The
theme floats `.page` to the inline-end above `$large` and sets its width to
`calc(100% - 200px)` to reserve room for the author sidebar. We render no
sidebar, so that space is simply dead: content sits shunted right. `classes:
wide` does *not* fix it — it only zeroes the end padding. Measured before the
override at a 1280px viewport: 322px of space left, 22px right.

**This is a GitHub *user* site.** The repo must stay named
`cselvaratnam.github.io` and `baseurl` must stay empty.

**Pages source must be "Deploy from a branch"** (`main` / root), not GitHub
Actions, or `remote_theme` will not resolve.

**`_pages` must stay in `include:`** in `_config.yml` — folders starting with an
underscore are otherwise ignored.

**Local preview needs Ruby 3.x.** The macOS system Ruby is 2.6 and too old, and
errors confusingly (`Errno::EPERM` on `getcwd`) if the shell is sitting in a
TCC-blocked folder under `~/Documents`. GitHub builds the site itself, so
previewing is entirely optional:

```bash
brew install ruby && bundle install && bundle exec jekyll serve --livereload
```

## Layout

Single column throughout. Set **once** in the `defaults` block at the foot of
`_config.yml` via `author_profile: false` (no sidebar) and `classes: wide` (no
empty gutter), plus the CSS override described above. Do not scatter these
across individual pages.

To change the look, set `minimal_mistakes_skin` in `_config.yml` to one of:
`default`, `air`, `aqua`, `contrast`, `dark`, `dirt`, `mint`, `neon`, `plum`,
`sunrise`. Restart `jekyll serve` after editing `_config.yml` — it is the one
file Jekyll does not reload.

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

It handles two separate cases, because work can be waiting in two ways: edits
not yet saved, and saved work not yet sent to GitHub. An early version only
checked the first, and cheerfully reported 'everything is already published'
while a saved commit sat unsent. If you change the script, keep both checks.

**Publish once, at the end of a batch of edits.** Every push queues another
publish job at GitHub, and they run one at a time — pushing a dozen times in an
hour jammed the queue for over an hour and nothing reached the live site. The
build itself only takes about 34 seconds; the queue is the bottleneck. Make all
the edits, then sync once.

### When the live site will not update

Work through this in order. **Check GitHub's status first** — on 6 August 2026
an afternoon was lost diagnosing a repo that was perfectly fine while Pages was
in a major outage.

```bash
curl -s https://www.githubstatus.com/api/v2/components.json \
  | grep -A3 '"name": "Pages"'
curl -s https://www.githubstatus.com/api/v2/incidents/unresolved.json
```

Then:

1. `gh run list --limit 3` — is a run queued, running, cancelled or failed?
2. `gh api repos/cselvaratnam/cselvaratnam.github.io/deployments/<id>/statuses`
   — the timings are diagnostic. `waiting → queued → in_progress` within a few
   seconds followed by `failure` exactly ten minutes later is the deploy action
   timing out because GitHub never answers. That is an outage, not a config
   problem. Nothing in the repo will fix it.
3. Only if the build itself fails, look at the repo. `gh run view <id>
   --log-failed` gives the real error; the Pages API only ever says 'Page build
   failed.'

A cancelled run usually means a newer push superseded it — harmless.

Do not force a rebuild with `POST /pages/builds`. It races the workflow's own
deployment and cancels it, which looks exactly like a fresh failure and wastes
more time.

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

**Voice: first person throughout, including About.** Deliberately understated —
no self-promotion. Christian does not announce his own honorifics; 'Revd Canon
Dr' does not belong in his own prose.

**'The Belfrey' always takes a capital T**, including mid-sentence. Never 'the
Belfrey'. Full name on first use: The Belfrey (St Michael le Belfrey).

**'Revd', never 'Rev'd' or 'Rev.'** The full title — Revd Canon Dr Christian
Selvaratnam — belongs in the About page heading only. It never appears in his
own prose.

**Do not name the two book series he edits.** 'An editor for two academic book
series' is as far as it goes.

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
