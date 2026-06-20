# koczancraft-web

The Koczan Craft web monorepo — a growing portfolio of small, self-contained static
sites, one folder per subdomain. Each folder deploys as its own Vercel project.

> **Org:** `KoczanCraft` (GitHub) · **Owner:** Michal Koczan
> Bilingual (EN / 日本語) throughout, matching the house style.

## Layout

```
koczancraft-web/
├─ koczancraft.com/      → https://koczancraft.com        (the brand / portfolio home)
│  ├─ index.html         self-contained: HTML + CSS + JS in one file
│  └─ assets/            images/fonts if/when needed
└─ <new-site>/  (future) → https://<sub>.koczancraft.com
```

The **MTG Draft Companion** app landing lives in the app repo
(`MTG-Draft-Companion/mtgdraftcompanion-public-version` → `docs/landing/`) and deploys
to `mtgdraftcompanion.koczancraft.com` from there. This monorepo just links to it.

## Add a new site

1. Create a folder `mysite/` with an `index.html` (copy `koczancraft.com/` as a starting
   point — it already has the EN/JA toggle + premium base styles).
2. In Vercel: **New Project → import this repo → set Root Directory = `mysite`**.
3. Add the domain/subdomain to that Vercel project (see DNS below).

## Deploy (Vercel)

These are **static** sites — no build step.

- **One Vercel project per folder.** When importing, set **Root Directory** to the folder
  (e.g. `koczancraft.com`). Framework preset: **Other**. Build command: none. Output: the
  folder itself.
- Because Root Directory is set per project, multiple subdomains can live in this one repo.

## DNS (the domain is registered on Vercel)

`koczancraft.com` is registered through Vercel, so DNS is managed there.

- **Apex `koczancraft.com`** → assign the domain to the `koczancraft.com` project.
- **Subdomains** (e.g. `mtgdraftcompanion`, future `leather`, etc.) → add the subdomain to
  the relevant Vercel project; Vercel creates the CNAME/record automatically since it's the
  nameserver. The MTG subdomain points at the **app repo's** Vercel project, not this one.

## House style

- One HTML file per site, CSS + JS inline (no bundler, no dependencies beyond Google Fonts).
- `data-i18n` keys + an `I18N` dictionary + the `applyLang()` toggle for EN/日本語.
- White/premium for the brand home; each app can carry its own theme (the MTG page is dark/gold).
# koczancraft-web
