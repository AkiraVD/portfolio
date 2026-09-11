# CLAUDE.md

Guidance for Claude Code working in this repository.

## What this is

AkiraVD's personal portfolio, built to get hired. Static HTML and one stylesheet —
**no build step, no framework, no JavaScript, no dependencies.** Open `index.html`
in a browser and it works. Published on GitHub Pages at
<https://akiravd.github.io/>.

```
index.html          front page: whoami, measured, projects, experience, contact
terminal.css        the entire design system, dark only
projects/*.html     one page per project (lst, uictl, smt, todo)
img/*.png           screenshots, cropped and quantised
404.html            served by Pages on a bad link; carries its own inlined styles
.nojekyll           stops Pages running Jekyll over the files
design/             gitignored — mockup working files, not part of the site
```

## Rules that matter

**Keep it dependency-free.** No build step, no bundler, no JS framework, no
analytics. Fast first paint and crawlable HTML are the point — an SPA would cost
both, and would argue against what the site is selling.

**Every claim is a measurement.** The site's whole thesis is measured numbers
rather than adjectives ("49.9 MB with eight tabs open", "microseconds, not
milliseconds").
Never add a performance claim without a real number behind it, and never invent
one. If a number isn't verified, leave it out.

**Keep employer internals out.** The experience section deliberately describes
achievements without naming the employer's architecture, data model or domain
vocabulary. Keep it that way: say "a hot-path lookup", not the internal name for
it. Scale numbers and outcomes are fine; internal design detail is not.

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

**Design vocabulary: the page is a terminal.** Every page is one shell window —
title bar with three dots, then prompt lines (`$ cat measured.txt`) as section
headings, output indented beneath, ASCII rules between sections, a blinking block
cursor at the end. JetBrains Mono throughout, one green accent on the prompt,
numbers and links. All colours and spacing live as custom properties at the top of
`terminal.css`.

**Dark only, deliberately.** There is no light mode and no
`prefers-color-scheme` block: a terminal that turns beige isn't a terminal. Set
`color-scheme: dark`. This is the one place the site commits to a single look
rather than serving both.

**Plain language inside the shell framing.** The terminal conceit is the
container; the words inside it are for a non-technical reader. Describe what a
project *does* ("lets a computer click its own buttons"), never what it is built
out of. No jargon tag rows, no `µs`/`ms` without a plain-English gloss beside it.
The framing is already asking a recruiter to work — the copy must not.

**Keep the copy short.** The front page is ~370 words and each project page
~300–350. If a section grows past that, cut rather than extend.

## Project pages

Each is a shell session over the project's directory, in this order:

```
$ cd ~                              back to the front page
$ cat <name>/README.md              title, lede, spec table, optional screenshot
$ cat <name>/problem.md             one paragraph
$ cat <name>/decisions.md           3–4 short items, each an "## " heading
$ cat <name>/measured.txt           optional table
$ cd ../<next>                      next project, then the email
```

Spec rows are mono key/value pairs in snake_case (`built_with`, `verified_on`,
`source`) — they read as output, not as a printed table.

There is deliberately no "What I'd change" section. It existed as a bracketed
placeholder, was never written, and has been dropped. Don't reintroduce it.

To add a project: copy `projects/todo.html`, replace the content, add an
`.ls-item` row to `index.html`, and point the previous page's `cd ../` link at it.
There is no index to regenerate.

## Working on it

```sh
python3 -m http.server 8000     # then open http://localhost:8000
```

Check it at phone width before pushing — the layout collapses to one column at
820px, and tightens again at 520px. A headless browser renders a page to PNG at
any width without touching a live browser session, which is the quickest way to
actually look. Which browser that is depends on the machine (see below); this picks
whichever is installed:

```sh
shot() {  # usage: shot <width> <url> <out.png>
  if command -v google-chrome >/dev/null; then
    google-chrome --headless --disable-gpu --hide-scrollbars \
      --window-size="$1",2600 --screenshot="$3" "$2"
  else  # Brave flatpak is sandboxed: it can only write where --filesystem allows
    flatpak run --filesystem=/tmp com.brave.Browser --headless --disable-gpu \
      --hide-scrollbars --window-size="$1",2600 --screenshot="$3" "$2"
  fi
}
shot 400 http://127.0.0.1:8000/ /tmp/phone.png
```

Keep screenshot output under `/tmp` so the same command works on both machines.

## Machines

The site itself needs almost nothing: **git, `gh` (logged in as AkiraVD), Python 3
for the preview server, and any Chromium-family browser** for headless screenshots.
No Node, no package manager — nothing here should ever need one.

The user works on this repo from more than one PC. **`git fetch` before editing** —
the other machine may have pushed.

| | Main PC (`phuongld-B660M-DS3H-AX-DDR4`) | Second PC |
|---|---|---|
| OS | Ubuntu 24.04.5 LTS, kernel 7.0 | not yet recorded |
| Desktop | GNOME 46, **Wayland** | not yet recorded |
| Python | 3.12.3 | not yet recorded |
| Headless browser | `google-chrome` 153 | Brave, flatpak `com.brave.Browser` (no Chrome) |
| git / gh | 2.43.0 / 2.98.0 | not yet recorded |
| Node | default is **v12** (pinned for unrelated work); v18/v22 via fnm | not yet recorded |

Machine-specific traps:

- **Wayland (main PC):** screenshotting the live desktop can't target a specific
  window — other apps end up on top. Use the headless `shot` above instead.
- **Node v12 default (main PC):** if any script ever does need Node, v12 fails on
  modern syntax with `SyntaxError: Unexpected token '?'`. Call
  `~/.local/share/fnm/node-versions/v22.23.1/installation/bin/node` explicitly
  rather than changing the default.
- **Brave flatpak (second PC):** without `--filesystem=/tmp` the screenshot fails
  silently or with a write error, because the sandbox can't see the output path.

When working on a machine not in this table, or one whose column says "not yet
recorded", fill in its column (`uname -r`, `python3 --version`, `git --version`,
`gh --version`, which browser exists) and commit it with the change.

## Deploying

The repo is **public** and named `AkiraVD.github.io`, so GitHub Pages serves it as
a user site at the **root** of <https://akiravd.github.io/> — not under a
`/<repo>/` subpath. The source is the `main` branch, folder `/ (root)`. There is no
build command and no `CNAME`; a `CNAME` is only needed if a custom domain is added
later.

Because it serves at a domain root, all paths in the site are either relative or
root-absolute. `404.html` carries its own inlined styles so it renders at any depth
without a stylesheet path to get wrong. Don't reintroduce a `/<repo>/` prefix — it
would break every link. If the repo is ever renamed away from `AkiraVD.github.io`,
the site drops to a subpath and this all has to be revisited.
