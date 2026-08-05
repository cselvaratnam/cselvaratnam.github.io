# cselvaratnam.github.io

Personal site. [Jekyll](https://jekyllrb.com) + the
[Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/) theme, pulled
in as a **remote theme** and built by GitHub Pages.

Live at <https://cselvaratnam.github.io>.

## Editing

| I want to… | Edit |
|---|---|
| Change page text | `index.md`, `_pages/about.md`, `_pages/books.md`, `_pages/writing.md` |
| Change the top nav | `_data/navigation.yml` |
| Change site title, colours, theme version | `_config.yml` |
| Add custom CSS | `assets/css/main.scss` |

Commit and push to `main`; GitHub rebuilds automatically, usually within a
minute.

## Layout

Every page is single-column: no author sidebar, full content width. That is set
once in the `defaults` block at the bottom of `_config.yml`, not per page.

To change the look, set `minimal_mistakes_skin` in `_config.yml` to one of:
`default`, `air`, `aqua`, `contrast`, `dark`, `dirt`, `mint`, `neon`, `plum`,
`sunrise`.

## Previewing locally (optional)

Not required — GitHub builds the site itself. Needs Ruby 3.x (the macOS system
Ruby 2.6 is too old):

```bash
brew install ruby
bundle install
bundle exec jekyll serve --livereload
```

Then open <http://localhost:4000>.
