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
| every `projects/*.html` | The "What I'd change" paragraph |

One thing worth doing beyond filling blanks:

1. **Write the "What I'd change" sections yourself.** They are the part of this
   site an interviewer will actually quote back at you, and they have to be true.

## Publishing

The repo is private and the site should be public — GitHub Pages can't do that on
a free account. Any of these can, from this private repo, for free:

| Host | Notes |
|---|---|
| **Cloudflare Pages** | Recommended. Fastest, unlimited bandwidth, free custom domain. |
| Netlify | Simplest UI, 100 GB/month free. |
| Vercel | Fine too; the free tier is non-commercial, which a portfolio is. |

Setup is the same everywhere and takes about two minutes:

1. Sign in with GitHub and authorise access to `AkiraVD/portfolio`.
2. **Framework preset: None. Build command: leave empty. Output directory: `/`.**
   There is no build step — the repo *is* the site.
3. Deploy. You get a public URL; the repo stays private.

Then put the real URL into `og:url` in `index.html`.

If you'd rather use GitHub Pages, the repo has to be public (or you need GitHub
Pro). In that case, if the repo is not named `AkiraVD.github.io`, the site serves
under `/portfolio/` and the home link in `404.html` needs that prefix.

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
