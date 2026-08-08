# Personal homepage

Static site — plain HTML and CSS, no build step, no dependencies.

```
index.html            all content; remaining gaps are marked with EDIT comments
style.css             all styling; the knobs are the variables at the top
assets/
  profile.jpg         headshot, cropped square from me-original.JPG
  me-original.JPG     the full-size original, kept for re-cropping
Haoqi_Hu_Resume.docx  NOT published — see below
```

**The resume is deliberately not linked from the site**, and there is no CV
section or download link. If you ever push this folder to a public repo, the
`.docx` would still be publicly readable even though nothing links to it — so
either delete it, move it out of this folder, or add it to `.gitignore` (a
`.gitignore` excluding it is already in place).

## Editing

Open `index.html` and search for `EDIT` — each comment marks something still
left to fill in. The content itself is real, taken from your resume and Google
Scholar.

To restyle, change the variables at the top of `style.css`:

| Variable      | What it does                          |
| ------------- | ------------------------------------- |
| `--accent`    | link color (set the dark-mode one too) |
| `--max-width` | content column width                  |
| `--font-sans` | typeface                              |

Dark mode follows the visitor's OS setting. To drop it, delete the
`@media (prefers-color-scheme: dark)` block.

### Photo

`assets/profile.jpg` is a 400×400 square crop of `assets/me-original.JPG`,
framed head-and-shoulders. It gets masked into a circle by the CSS.

To re-crop from the original at a different framing:

```sh
# -c <height> <width> --cropOffset <top> <left>, then scale to 400px
sips -c 480 480 --cropOffset 720 564 assets/me-original.JPG --out /tmp/c.jpg
sips -Z 400 /tmp/c.jpg --out assets/profile.jpg
```

Keep it square and under ~200 KB so the page loads fast.

### Removing a section

Delete its `<section>` block in `index.html` **and** its link in the `<nav>`.

## Previewing

Just open the file:

```sh
open index.html
```

Or serve it, which more closely matches production:

```sh
python3 -m http.server 8000    # then visit http://localhost:8000
```

## Deploying to GitHub Pages

Free, and gives you `https://<username>.github.io`.

1. Create a GitHub repo named exactly **`<your-username>.github.io`**.

2. Push this folder to it:

   ```sh
   git init
   git add .
   git commit -m "Initial homepage"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-username>.github.io.git
   git push -u origin main
   ```

3. In the repo: **Settings → Pages → Source → Deploy from a branch → `main` / `root`**.

The site is live at `https://<your-username>.github.io` within a minute or two.
Every later `git push` redeploys it.

### Custom domain

If you own a domain, add a file named `CNAME` at the repo root containing just
the domain (e.g. `williamhu.com`), then point your DNS at GitHub:

- **Apex** (`williamhu.com`) — four `A` records to `185.199.108.153`,
  `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
- **Subdomain** (`www.williamhu.com`) — one `CNAME` to `<your-username>.github.io`

Then enable **Enforce HTTPS** under Settings → Pages.

## Before you publish

- [x] Photo in place (`assets/profile.jpg`)
- [x] Publications verified against Google Scholar (author order included)
- [x] Resume kept off the site and out of git
- [x] GitHub and LinkedIn linked in the header
- [x] Bosch dates confirmed: Jan 2020 – Dec 2021
- [ ] Add arXiv/PDF links for the 4 papers marked `<a href="#">arXiv</a>`
- [ ] Update `og:image` to an absolute URL once you have a domain
- [ ] Check it on your phone
