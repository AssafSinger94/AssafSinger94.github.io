# Assaf Singer — personal academic site

A minimal static personal website. Just HTML + CSS, no build step.

## Files

```
index.html        Page content (edit this to add news / publications / talks)
styles.css        All styling
assets/
  profile.jpg     Headshot
  time-to-move.gif
  dino-tracker.gif
  sigmorphon.png
  cv.pdf          CV (linked from the header)
README.md         This file
```

## Local preview

From this folder, start a static server and open it in a browser:

```bash
# Python (no install needed)
python3 -m http.server 8000
# then visit http://localhost:8000
```

Or, if you prefer Node:

```bash
npx --yes serve .
```

## Deploying on GitHub Pages (username.github.io)

Your GitHub username is `AssafSinger94`, so the repo must be named
exactly `AssafSinger94.github.io` - that tells GitHub Pages to serve
it at the root of your personal site at `https://assafsinger94.github.io`.

1. Create a new **public** repository on GitHub named exactly
   `AssafSinger94.github.io`. Do not initialize it with a README (you already have one).

2. From this folder, push the contents to the new repo:

   ```bash
   cd "/Users/assaf.singer/Documents/Claude/Projects/personal webpage"

   git init -b main
   git add .
   git commit -m "Initial site"
   git remote add origin git@github.com:AssafSinger94/AssafSinger94.github.io.git
   git push -u origin main
   ```

   If you use HTTPS instead of SSH, replace the remote URL with
   `https://github.com/AssafSinger94/AssafSinger94.github.io.git`.

3. In the repo's **Settings → Pages**, confirm the source is set to
   *Deploy from a branch*, branch `main`, folder `/ (root)`.

4. Wait ~1 minute, then visit **https://assafsinger94.github.io** -
   your site is live.

### Updating the site

Edit `index.html` (or `styles.css`), then:

```bash
git add -A
git commit -m "Update news / publications"
git push
```

GitHub Pages redeploys automatically within a minute or so.

### Custom domain (optional)

If you later want a custom domain (e.g. `assafsinger.com`):

1. Create a file named `CNAME` in the repo root containing just your domain
   (e.g. `assafsinger.com`).
2. Point your DNS provider's A / CNAME records at GitHub Pages
   (see [GitHub's docs](https://docs.github.com/pages/configuring-a-custom-domain-for-your-github-pages-site)).

## Editing the content

The site is a single `index.html` with clearly-labeled sections. To
add a publication, copy an existing `<article class="pub"> ... </article>`
block and update the fields. To add a news item, copy one `<li>` in the
`<ul class="news">` list.

## Notes

- All images use `loading="lazy"` so the page loads quickly even with GIFs.
- The site respects the visitor's OS color scheme (light / dark) automatically.
- The layout collapses to a single column below 640px for comfortable mobile reading.
- There's no JavaScript except a one-liner that keeps the copyright year current.
