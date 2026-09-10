# CLAUDE.md

Guidance for Claude Code working in this repository.

## What this is

AkiraVD's personal portfolio, built to get hired. Static HTML and one stylesheet —
**no build step, no framework, no JavaScript, no dependencies.** Open `index.html`
in a browser and it works. Published on GitHub Pages at
<https://akiravd.github.io/portfolio/>.

```
index.html          front page: hero, measured numbers, work list, experience
style.css           the entire design system, light + dark
projects/*.html     one page per project (lst, uictl, claude-tele-bot, smt, todo)
img/*.png           screenshots, cropped and quantised
404.html            served by Pages on a bad link
.nojekyll           stops Pages running Jekyll over the files
design/             gitignored — mockup working files, not part of the site
```

## Rules that matter

**Keep it dependency-free.** No build step, no bundler, no JS framework, no
analytics. Fast first paint and crawlable HTML are the point — an SPA would cost
both, and would argue against what the site is selling.

**Every claim is a measurement.** The site's whole thesis is measured numbers
rather than adjectives ("49.9 MB with eight tabs open", "microseconds per spin").
Never add a performance claim without a real number behind it, and never invent
one. If a number isn't verified, leave it out.

**Never invent biography.** Job titles, dates, companies, cities and contact
details come from the user, not from inference. Placeholders are written in
`[SQUARE BRACKETS]` — find outstanding ones with:

```sh
grep -rn '\[[A-Z]' index.html projects/
```

**Images are never upscaled.** Each `<img>` carries `width`/`height` matching its
real pixel size, and `figure` caps at 920px. A dense UI screenshot scaled down
turns to mush and looks cheap — crop it tight instead so it can display at natural
size. Screenshots live in a `.frame` wrapper, not full-bleed.

**Design vocabulary.** Warm paper ground, Newsreader (serif display) + Karla
(body), one terracotta accent. All colours, spacing and type live as custom
properties at the top of `style.css`; dark mode redefines the same properties
under `@media (prefers-color-scheme: dark)`. Change a value once and it applies
everywhere. Minimal, but readable by a non-technical recruiter — plain language
in the top third of any page, no jargon walls.

**Keep the copy short.** The front page is ~470 words and each project page
~280–330. If a section grows past that, cut rather than extend.

## Project pages

Each follows the same shape: title + one-line lede + a spec table, an optional
screenshot, "The problem" (one paragraph), "Decisions worth defending" (3–4 short
items), an optional "Measured" table, and "What I'd change".

Keep **"What I'd change"** on every page. It's the section an interviewer quotes
back, and it must be the user's own honest words — leave the placeholder until
they write it rather than filling it in for them.

To add a project: copy `projects/todo.html`, replace the content, add a
`.work-item` block to `index.html`, and point the previous page's "Next:" link at
it. There is no index to regenerate.

## Working on it

```sh
python3 -m http.server 8000     # then open http://localhost:8000
```

Check it at phone width (~400px) before pushing — the layout collapses to one
column at 760–860px breakpoints.

## Deploying

Pushing to `main` publishes it: **Settings → Pages → Deploy from a branch →
`main` / root**. Because the repo is `portfolio` rather than `AkiraVD.github.io`,
the site lives under `/portfolio/`. Two absolute paths depend on that — the
stylesheet link and the home link in `404.html`, plus `og:url` in `index.html`.
If the repo is ever renamed, those three need updating; everything else is
relative.
