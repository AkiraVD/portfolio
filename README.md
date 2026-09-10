# akiravd.github.io

Personal portfolio. Static HTML and one stylesheet — no build step, no framework,
no JavaScript. Open `index.html` in a browser and it works.

Every page is styled as a single terminal window: prompt lines as section
headings, output indented beneath, ASCII rules between sections. Dark only — a
terminal that turns beige isn't a terminal.

```
index.html          front page
terminal.css        the whole design system (dark only)
projects/*.html     one page per project
img/*.png           screenshots, downsampled to 1400px / ~40KB each
404.html            served by GitHub Pages on a bad link; styles inlined
.nojekyll           stop Pages running Jekyll over it
```

## Before this goes live

Everything in `[SQUARE BRACKETS]` is a placeholder that needs a real value.
Find them all with:

```sh
grep -rn '\[[A-Z]' --include='*.html' index.html projects/
```

Four are left, all the same one:

| Where | What's needed |
|---|---|
| every `projects/*.html` | The "What I'd change" paragraph |

**Write those yourself.** They are the part of this site an interviewer will
actually quote back at you, and they have to be true.

## Publishing

The repo is public and named `AkiraVD.github.io`, so GitHub Pages serves it as a
user site at the root of <https://akiravd.github.io/> — not under a `/<repo>/`
subpath. Source is the `main` branch, folder `/ (root)`. There is no build
command and no `CNAME`; a `CNAME` is only needed for a custom domain.

Because it serves at a domain root, paths in the site are relative or
root-absolute. Don't reintroduce a `/<repo>/` prefix — it would break every
link. Renaming the repo away from `AkiraVD.github.io` drops the site to a subpath
and breaks it.

## Checking it locally

```sh
python3 -m http.server -d . 8000    # then open http://localhost:8000
```

Check phone width before pushing — one column below 820px, tighter below 520px.

## Editing it

Colours, spacing and type live as custom properties at the top of `terminal.css`,
and apply to the front page and every project page. There is no light-mode block
to keep in sync.

To add a project: copy `projects/todo.html`, replace the content, add an
`.ls-item` row to `index.html`, and point the previous page's `cd ../` link at it.
There is no index to regenerate.
