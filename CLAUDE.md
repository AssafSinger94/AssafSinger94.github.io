# Project notes — Assaf Singer's personal academic site

Context file for future editing sessions (human or Claude). The README
covers setup and deployment; this file captures the design decisions,
content conventions, and gotchas that aren't obvious from reading the
HTML.

## What this is

A single-page static academic site for Assaf Singer — PhD student at the
Technion advised by Dr. Or Litany. Purpose: give faculty, recruiters,
and fellowship reviewers a clean landing page for bio, publications,
talks, and contact info. Live at **https://assafsinger94.github.io**.

Deliberately minimal: hand-written HTML + CSS, no build step, no JS
framework, no tracking beyond Google Analytics.

## Stack & structure

```
index.html        All page content. Single file, clearly labeled sections.
styles.css        All styling. CSS custom properties up top for theming.
README.md         Setup, local preview, deploy to GitHub Pages.
CLAUDE.md         This file — conventions, common edits, gotchas.
.gitignore        .DS_Store, Thumbs.db, .trash/
assets/
  profile.jpg                    Headshot (hero)
  cv.pdf                         Current CV (linked from the CV icon)
  time-to-move.gif               TTM thumbnail
  dino-tracker.gif               DINO-Tracker thumbnail
  sigmorphon.png                 SIGMORPHON thumbnail (diagram)
  time-to-move-vision-day.png    Talk thumbnail
  dino-tracker-vision-day.png    Talk thumbnail
```

No build step. Edit → commit → push → GitHub Pages redeploys in ~1 min.

## Local preview

```bash
cd ~/Documents/Claude/Projects/personal_webpage
python3 -m http.server 8000
# open http://localhost:8000
```

## Page sections (in order, as they appear in index.html)

1. Header / Hero — headshot, name, tagline, icon nav (email, scholar,
   github, linkedin, x, CV).
2. About me — two paragraphs.
3. News — reverse chronological list.
4. Publications — main pubs, then an "Earlier work" subheading.
5. Talks — each with a YouTube thumbnail linking to the recording.
6. Contact — just email.
7. Footer — year (auto-updated by a one-line script) + email.

Hidden (HTML-commented-out) sections: Research Interests, Teaching,
"Outside research I love tennis." Uncomment to re-enable.

## Content conventions

These are the style choices used throughout — match them when adding
new content:

- **Paper titles**: italicized with `<em>`. Used in News ("Oral talk
  at *Israel Computer Vision Day*...") and Talks ("Oral presentation -
  *Time-to-Move...*").
- **Equal contribution**: asterisk after the author's surname initial
  (e.g. `A. Singer*`) with a trailing note
  `<span style="color:var(--faint); font-size:0.85em">(* equal contribution)</span>`.
- **Assaf's own name in author lists**: wrapped in `<span class="me">`
  to make it bold/highlighted.
- **Dashes**: use hyphens `-`, never em-dashes `—`. This was an
  explicit preference — find-and-replace if you catch any `—` sneaking in.
- **Non-breaking spaces**: `&nbsp;` to keep venue names (`ECCV&nbsp;2024`,
  `ICLR&nbsp;2026`) and phrases (`video&nbsp;generation`) from
  breaking awkwardly. Also used with non-breaking hyphen `&#8209;` in
  `ReALM&#8209;GEN`.
- **News items**: `<time>` for the date, `<span>` for the body.
  Exciting items get a trailing `!` (acceptances). Talks don't.
- **Tagline**: kept short, describes research areas. Current:
  "I work on generative AI, video generation, and dynamic 3D/4D
  scene understanding."

## How to make common edits

### Add a news item

Copy an existing `<li>` in `<ul class="news">` near the top of `index.html`.
Keep the list reverse-chronological (newest first).

```html
<li>
  <time>Mon YYYY</time>
  <span>What happened. Use <strong>Bold</strong> for paper names and
  venues; <em>italics</em> for events/workshops.</span>
</li>
```

### Add a publication

Copy the Time-to-Move `<article class="pub">` block as your template.
Slot order: thumbnail link, title (`<h3>`), authors (`<p class="authors">`),
venue (`<p class="venue">`), and pill links (`<div class="pub-links">`).

Thumbnails live under `assets/`. Standard behavior is
`object-fit: cover`, which works for most GIFs/images. If your
thumbnail is a diagram that shouldn't be cropped (like SIGMORPHON),
use `<div class="thumb thumb-contain">` instead of `<a class="thumb">`.

Pill links typically include: Project page, arXiv, Code. Add more
(PDF, BibTeX, Video) as needed — any `<a>` inside `.pub-links` picks
up the pill styling.

### Add a talk

Copy a `<article class="talk">` block. The thumbnail is an image of
Assaf giving the talk; the whole thumbnail links to the YouTube
recording, and the body has a "YouTube" pill link to the same URL.
The `<div class="pub-links">` class is reused here on purpose — the
CSS selector was broadened to cover both pubs and talks.

### Update the CV

Drop the new file in at `assets/cv.pdf` (replacing in place). The
header link and any references in the page resolve to the same path,
so no HTML changes needed.

### Update the headshot

Replace `assets/profile.jpg` in place. Keep aspect roughly square for
the circular crop to look right.

### Show or hide a section

Research Interests, Teaching, and the "tennis" line are inside HTML
comments. Un-comment to re-enable, or wrap a new section in `<!-- -->`
to hide.

## Design tokens (in styles.css)

- Accent color: Technion navy `#1f3a68` (`--accent`).
- Text: near-black / white in dark mode.
- System font stack (no web fonts).
- Mobile breakpoint: 640px (layout collapses to single column below).
- Dark mode: automatic via `prefers-color-scheme: dark`. All colors
  are declared as CSS custom properties and overridden under the dark
  media query — don't hard-code colors in new rules, use the tokens.
- Icon buttons: 40px circles, inline SVG at `currentColor`. To add
  another icon, copy one of the `<a>` blocks in `<nav class="links">`,
  replace the SVG `<path>`, and set `aria-label` + `title`.

## External links used on the site

Useful inventory when doing link audits or updates:

- Advisor (Or Litany): https://orlitany.github.io/
- Weizmann (Tali Dekel): https://www.weizmann.ac.il/math/dekel/home
- NYU (Kyunghyun Cho): https://kyunghyuncho.me/
- NYU (Katharina Kann): https://kkann.github.io/
- Pagaya: https://pagaya.com/
- GitHub: https://github.com/AssafSinger94
- Google Scholar: https://scholar.google.com/citations?user=SquIKeMAAAAJ
- LinkedIn: https://www.linkedin.com/in/assaf-singer/
- X/Twitter: https://x.com/assaf_singer
- Email: assaf.singer@campus.technion.ac.il

Paper-specific:

- Time-to-Move: arXiv https://arxiv.org/abs/2511.08633 · Code https://github.com/time-to-move/TTM · Project https://time-to-move.github.io/
- DINO-Tracker: arXiv https://arxiv.org/abs/2403.14548 · Code https://github.com/AssafSinger94/dino-tracker · Project https://dino-tracker.github.io/
- SIGMORPHON: arXiv https://arxiv.org/abs/2006.11830 · Code https://github.com/AssafSinger94/pointer-generator-transformer-inflection

Talk recordings:

- Time-to-Move (Vision Day 2026): https://www.youtube.com/live/8mTGJ0oaKdQ?si=MpZ5iIOITC1TEbjp&t=7004
- DINO-Tracker (Vision Day 2025): https://www.youtube.com/live/2fEDy-uTAII?si=c3Vb3ezSPhS4n0Ow&t=12130

## Google Analytics

- Property: GA4, Measurement ID `G-FY385Y0EFB`.
- Snippet lives in `<head>` of `index.html`, `anonymize_ip: true`.
- Dashboard: https://analytics.google.com → Personal site property.
- Most useful reports: Realtime (confirm tracking), Acquisition →
  Traffic acquisition (referrers), Engagement → Events
  (`outbound_click`, `file_download` for CV downloads are auto-tracked
  by Enhanced Measurement), User → Demographic details.
- To swap to a different property later, find/replace
  `G-FY385Y0EFB` in `index.html` (two occurrences).

## Git workflow

The repo is hosted at
`git@github.com:AssafSinger94/AssafSinger94.github.io.git`. Normal flow:

```bash
cd ~/Documents/Claude/Projects/personal_webpage
# ... make edits ...
git add -A
git commit -m "Describe the change"
git push
```

GitHub Pages redeploys within ~1 minute. No CI, no workflow file.

## Known gotchas

- **Folder name uses an underscore** (`personal_webpage`, not
  `personal webpage`). The Cowork sandbox mount handles underscores
  reliably; spaces caused mount path issues in earlier sessions.
- **Sandbox has no network to github.com**, so `git push` always runs
  from the user's own terminal, not inside Claude's sandbox.
- **Sandbox can leave stale `.git/*.lock` files** that it can't
  delete itself (FUSE permissions). If a `git commit` inside the
  sandbox complains about a lock file, clear them from the user's
  terminal:
  ```bash
  rm -f .git/index.lock .git/HEAD.lock .git/objects/maintenance.lock
  ```
- **SIGMORPHON thumbnail uses `thumb-contain`** (object-fit: contain)
  rather than the default cover, because it's a diagram and cropping
  would cut off labels. Keep this modifier if you swap the image.
- **`.pub-links` pill styling is broadened** to cover both `.pub` and
  `.talk` links. If you add a new section with pill links, reuse
  `<div class="pub-links">` — don't invent a new class.
- **GitHub username is `AssafSinger94`** (not `assafsinger` or
  `AssafSinger` — both are taken / unavailable). The site URL is
  therefore permanently `https://assafsinger94.github.io` unless a
  custom domain is set up later (see README).

## Possible future additions

Not on any timeline — noted here so ideas aren't lost:

- Custom domain (e.g. assafsinger.com) — see README for DNS steps.
- Favicon beyond the inline "AS" SVG (upload a 32×32 PNG to root as
  `favicon.ico` and add `<link rel="icon" href="/favicon.ico">`).
- Open Graph image — currently reuses `assets/profile.jpg`; a
  dedicated 1200×630 card would look better when shared on Twitter/X
  and LinkedIn.
- Recent Preprints / Workshops sections if Publications grows long.
- BibTeX copy-to-clipboard buttons per publication.
