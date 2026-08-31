# GPS Alarm — Web

An alarm that rings when you reach a place, not a time — set a destination, doze off on the train, get woken at your stop.

The web repository of the GPS Alarm container. `../CLAUDE.md` governs product behavior,
domain vocabulary, user-facing copy, and releases. This file governs how the site is built.

**Web is not a mirror, and this one is not an app.** It is a **static site** — the public
pages the stores link to: the privacy policy and the support page. It has no layers, no
build step and no dependencies, and `../tools/parity-check.py` (written the day a second app
platform lands) does not measure it. It never ships product behavior; where it describes the
app it repeats `../docs/domain.md`, and a sentence that exists only here is a defect.

## The tree

```
gps-alarm-web/
├── CLAUDE.md
├── README.md
├── index.html       support page — how it works, permissions, purchases, contact
├── privacy.html     privacy policy — generated from ../docs/privacy.md (see below)
├── style.css        the only stylesheet; colours mirror the app's tokens
└── .nojekyll        GitHub Pages serves the files as they are
```

**This tree is the single source of truth for this repository's structure. When a file is
added, removed, or renamed, update it in the same change.**

## Status

Plain HTML and CSS, no generator, no package manifest. Hosted on **GitHub Pages** from the
`main` branch root of this repository; a push to `main` is a deploy (allow a minute).

## Domain

| What | Value |
|---|---|
| Production URL | `https://graboosky.github.io/gps-alarm-web/` |
| Privacy policy | `https://graboosky.github.io/gps-alarm-web/privacy.html` |
| Support | `https://graboosky.github.io/gps-alarm-web/` |
| DNS / registrar | none — GitHub's `github.io` subdomain |
| Host | GitHub Pages (branch `main`, path `/`) |

App Store Connect fetches the privacy policy and support URLs during review, and a 404
stops the submission. Both URLs above must return 200 before any store submission —
`../docs/release.md` puts web first in the order for this reason. The iOS app links the
privacy URL from its paywall (`LegalLinks.privacyPolicy`); change it there and here together.

## Build & test

There is no build. To check a change locally:

```bash
open gps-alarm-web/index.html
```

After pushing, confirm both pages answer:

```bash
curl -sI https://graboosky.github.io/gps-alarm-web/privacy.html | head -1
```

## Generated content

- **`privacy.html` is a copy of `../docs/privacy.md`.** The Markdown in the container is the
  source of truth; the HTML carries the same paragraphs and the same effective date. When the
  policy changes: edit `docs/privacy.md` first, then paste the new text into `privacy.html`,
  bump the effective date in both, and push this repository before the app that depends on
  the change ships. The page says so in an HTML comment at the top of the article.
- `index.html` repeats product behavior from `../docs/domain.md` in support-page words. When
  a behavior described there changes, this page changes in the same release.

## Conventions

- One stylesheet, `style.css`. Colours are the app's: accent `#FF9500` / `#FF9F0A` (dark),
  grouped background `#F2F2F7` / `#000000`, text `#1C1C1E` / `#F2F2F7`. Change them here and in
  the iOS `designSystem/` together; there is no shared token file across the boundary.
- Both appearances via `prefers-color-scheme`; no JavaScript.
- Copy is sentence case except page titles; user-facing wording that also appears in the app
  is taken from `../docs/domain.md`.
- No analytics, no third-party scripts, no fonts fetched from anywhere — the privacy policy
  promises exactly that.
