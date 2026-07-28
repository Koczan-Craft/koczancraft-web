# koczancraft-web

The Koczan Craft web monorepo — a growing portfolio of small, self-contained static
sites, **one folder per subdomain**. Each folder deploys as its own Vercel project, and
every push to `main` auto-deploys the affected sites.

> **Org:** `Koczan-Craft` (GitHub) · **Owner:** Michal Koczan
> Bilingual (EN / 日本語) throughout, matching the house style.

## Layout

```
koczancraft-web/
├─ koczancraft.com/                    → https://koczancraft.com
│  ├─ index.html                       brand home: Leather Craft + an Apps teaser (white/premium)
│  └─ assets/  (ig/, mtg-icon.png, …)  self-hosted images
├─ apps.koczancraft.com/               → https://apps.koczancraft.com
│  ├─ index.html                       apps hub: one card per app, grows + categories (white/premium)
│  ├─ privacy.html                     policy index: one row per app → that app's own policy
│  └─ assets/  (mtg-icon.png, recipetune-icon.png, …)
├─ mtgdraftcompanion.koczancraft.com/  → https://mtgdraftcompanion.koczancraft.com
│  ├─ index.html                       MTG Draft Companion app landing (dark/gold)
│  ├─ privacy.html                     canonical privacy policy (store links here)
│  └─ icon.png
├─ recipetune.koczancraft.com/         → https://recipetune.koczancraft.com
│  ├─ index.html                       RecipeTune app landing (coral/near-white, EN + 日本語)
│  ├─ privacy.html                     canonical privacy policy (store links here)
│  ├─ CONTENT-BRIEF.md                 content/design brief the page was built from
│  └─ icon.png, assets/
└─ <new-site>/  (future)               → https://<sub>.koczancraft.com
```

**Three-tier flow:** the brand home (`koczancraft.com`) has a Leather Craft section + an Apps
teaser → the Apps teaser links to the **apps hub** (`apps.koczancraft.com`) → each app card in
the hub links to that app's own landing (e.g. `mtgdraftcompanion.koczancraft.com`).

**Adding an app:** add a card to `apps.koczancraft.com/index.html` (copy the MTG `.app-card`).
Group cards under `<h2 class="cat-title">` blocks when categories are wanted. If the app gets its
own landing, add a folder for its subdomain too.

The MTG landing is **canonical here** now (moved out of the app repo's `docs/landing/`) so all
web pages live in one place. The app repo's old copy is superseded — edit the page **here**.

## How it's connected & automated
- **Brand home → apps:** `koczancraft.com` has an MTG Draft Companion card (with the app icon)
  that links to `mtgdraftcompanion.koczancraft.com`. The MTG page footer links back to
  `koczancraft.com`. New portfolio pieces just add a card + a folder.
- **Auto-deploy:** each folder is its own Vercel project (Root Directory = folder). Push to
  `main` → Vercel rebuilds the affected project automatically. No build step (static).

## Add a new site
1. Create a folder `mysite/` with an `index.html` (copy `koczancraft.com/` as a starting point —
   it already has the EN/JA toggle + premium base styles).
2. In Vercel: **New Project → import this repo → Root Directory = `mysite`**, framework **Other**.
3. Add the subdomain to that Vercel project (see DNS below).

## DNS (domain registered on Vercel)
`koczancraft.com`'s nameservers are Vercel's, so DNS is managed there.
- **Apex `koczancraft.com`** → assign to the `koczancraft.com` project.
- **Subdomains** (`mtgdraftcompanion`, future `leather`, …) → add the subdomain to the relevant
  Vercel project; Vercel creates the record automatically.

## House style
- One HTML file per site, CSS + JS inline (no bundler, no deps beyond Google Fonts).
- `data-i18n` keys + an `I18N` dictionary + the `applyLang()` toggle for EN/日本語.
- White/premium for the brand home; each app can carry its own theme (the MTG page is dark/gold).

## Privacy policies
**Each app owns its own policy, on its own subdomain** — that URL is what its store listing points
at, and it's the only place that app's data handling is described. Never write one shared policy:
the apps differ (MTG shares data with your playgroup and uses analytics; RecipeTune shares nothing
and uses none), and a merged policy would be wrong for both.

`apps.koczancraft.com/privacy.html` is the **index** — one row per app, linking out to each
policy. Every site's footer links to that one stable URL, so adding an app never means editing
footers:

```
koczancraft.com  ─┐
apps hub         ─┼─→ apps.koczancraft.com/privacy.html ─┬─→ mtgdraftcompanion…/privacy.html
(future sites)   ─┘   (index, one row per app)           └─→ recipetune…/privacy.html
```

**Adding an app:** add one `<a class="policy" data-policy="…">` block to the index, and write that
app's own `privacy.html`. Nothing else changes. The index appends `?lang=en|ja` to each outbound
link, so the language carries across the origin boundary — every app policy must honour that param
(they all read it in `applyLang()`).

**Keeping a policy true:** a policy describes what that app's *shipped binary* does. When an app's
ads/analytics/data handling changes, update its `privacy.html` in the same change — and remember
the store data-safety / privacy-label forms must match the binary too (those cannot over-disclose,
even where a policy safely could).
