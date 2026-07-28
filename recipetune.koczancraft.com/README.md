# recipetune.koczancraft.com

The **RecipeTune** app landing page. Self-contained, bilingual (EN / 日本語), following this
monorepo's one-folder-per-subdomain convention.

## Status: built, pre-launch

`index.html` is live-ready. The app is **not store-listed yet**, so the CTA is a notify-me email
capture rather than App Store / Google Play badges (see `CONTENT-BRIEF.md` §12).

## Files
- `index.html` — the page. One file, inline CSS + JS, no build step.
- `CONTENT-BRIEF.md` — the content/design brief this page was built from.
- `icon.png` — app icon on light background (favicon, hero crest, og:image), 1024×1024.
- `assets/icon-dark.png` — dark-background variant of the same mark.
- `assets/icon-fg.png` — foreground-only layer (transparent bg), for custom-color placements.
- `vercel.json` — rewrites `/en` and `/jp` to `index.html` for shareable language-pinned URLs.
- `app-ads.txt` — AdMob authorized sellers, same line as the other sites.

## Copy accuracy

The page states only what the app ships today, verified against `recipe-app-public`
(`openspec/specs/*`, `lib/features/*`). `CONTENT-BRIEF.md` §4B/§7/§11 describe **safe scaling** —
smart rounding away from "1.5 eggs", and awareness that pan size / cook time / frying oil don't
scale like ingredient amounts. Those come from `docs/research/feature-ideas-v2.md`, which is a
**roadmap** doc, and are **not implemented**: `serving-scaling/spec.md` specifies plain linear
`amount * target / base`, explicitly "not silently rounded".

What the page claims instead, all backed by specs: amounts scale linearly and display as common
cooking fractions (`localization`), amount-less ingredients and 分量外 pass through untouched
(`recipe-extraction`), the stored recipe is never modified (`serving-scaling`), and units convert
on Japanese-standard ratios with weight⇄volume by ingredient density (`unit-conversion`).

**If safe scaling ships, update the scaling section here** — the brief's original copy is the
right copy at that point.

## Open items
- **Notify-me inbox** (`CONTENT-BRIEF.md` §12) — `WEB3FORMS_KEY` in `index.html` is currently the
  MTG Draft Companion key, so signups land in that destination. Create a dedicated key and swap it
  before promoting this page.
- **OG image** (§9) — `og:image` currently points at `icon.png` (1024×1024, `twitter:card:
  summary`). A proper 1200×630 share image would let this use `summary_large_image` like the
  sibling sites.
- **Real screenshots** (§9) — the hero and scaling sections use illustrative CSS mockups, not real
  app screenshots.
- **`privacy.html`** (§14) — needed before either store submission; mirror
  `mtgdraftcompanion.koczancraft.com/privacy.html`.
- Once store-listed, swap the CTA band for real store badges.

## Deploy
In Vercel: **New Project → import this repo → Root Directory = `recipetune.koczancraft.com`**,
framework **Other**, no build step. Then add the `recipetune` subdomain to that project (DNS is
managed on Vercel — see the repo root `README.md`).

## Keeping this in sync
The app repo (`recipe-app-public`) is the source of truth for features and brand. If its design
system (`docs/brand-brief.md`) or feature set changes materially, refresh `CONTENT-BRIEF.md` and
this page to match.
