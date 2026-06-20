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
│  ├─ index.html                       brand / portfolio home (white/premium)
│  └─ assets/  (ig/, mtg-icon.png, …)  self-hosted images
├─ mtgdraftcompanion.koczancraft.com/  → https://mtgdraftcompanion.koczancraft.com
│  ├─ index.html                       MTG Draft Companion app landing (dark/gold)
│  ├─ privacy.html                     canonical privacy policy (store links here)
│  └─ icon.png
└─ <new-site>/  (future)               → https://<sub>.koczancraft.com
```

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

## Keeping policy in sync
The store privacy URL is `https://mtgdraftcompanion.koczancraft.com/privacy.html`. When
ads/analytics/data handling changes, update `privacy.html` **here**, consistent with the app's
`PrivacyInfo.xcprivacy` + `docs/STORE.md` in the app repo.
