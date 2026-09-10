# akiravd.github.io

Personal portfolio. Static HTML and one stylesheet — no build step, no framework,
no JavaScript. Open `index.html` in a browser and it works.

```
index.html          front page
style.css           the whole design system (light + dark)
projects/*.html     one page per project
img/*.png           screenshots, downsampled to 1400px / ~40KB each
404.html            served by GitHub Pages on a bad link
.nojekyll           stop Pages running Jekyll over it
```

## Before this goes live

Everything in `[SQUARE BRACKETS]` is a placeholder that needs a real value.
Find them all with:

```sh
grep -rn '\[[A-Z]' --include='*.html' index.html projects/
```

| Where | What's needed |
|---|---|
| `index.html` hero card | City, remote/hybrid, the roles you want |
| `index.html` experience | Two real jobs: title, company, dates, one measurable outcome each |
| `index.html` footer | Email, LinkedIn URL, CV PDF |
| every `projects/*.html` | The "What I'd change" paragraph, and the mailto in the footer |
| `projects/claude-tele-bot.html` | A repo link, or reword the card to say it's private |

Two things worth doing beyond filling blanks:

1. **Push `claude-tele-bot` to GitHub**, or drop it from the front page. Right now
   it is the only project a reader cannot go and check.
2. **Write the "What I'd change" sections yourself.** They are the part of this
   site an interviewer will actually quote back at you, and they have to be true.

## Publishing on GitHub Pages

```sh
git init && git add -A && git commit -m "Portfolio"
git branch -M main
git remote add origin git@github.com:AkiraVD/portfolio.git
git push -u origin main
```

Then in the repo: **Settings → Pages → Source: Deploy from a branch → `main` / `root`**.
This repo is `portfolio`, so it serves at `https://akiravd.github.io/portfolio/`.
Any other repo name serves it at `https://akiravd.github.io/<repo>/` — in that case
change the `og:url` in `index.html` and the leading-slash links in `404.html`.

## Checking it locally

```sh
python3 -m http.server -d . 8000    # then open http://localhost:8000
```

## Editing it

Colours, spacing and type live as custom properties at the top of `style.css`.
Dark mode is a second block of the same properties under
`@media (prefers-color-scheme: dark)` — change a value once and it applies to
both the front page and every project page.

To add a project: copy `projects/todo.html`, replace the content, and add a
`.work-item` block to `index.html`. There is no index to regenerate.
