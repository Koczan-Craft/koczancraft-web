# recipetune.koczancraft.com

The **RecipeTune** app landing page. Self-contained, bilingual (EN / 日本語), following this
monorepo's one-folder-per-subdomain convention.

## Status: built, pre-launch

`index.html` is live-ready. The app is **not store-listed yet**, so the CTA is a notify-me email
capture rather than App Store / Google Play badges (see `CONTENT-BRIEF.md` §12).

## Files
- `index.html` — the page. One file, inline CSS + JS, no build step.
- `privacy.html` — the privacy policy (EN / 日本語), written from the app's actual data handling.
  This is the URL both stores should point at. Supports `?lang=ja`.
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
- Once store-listed, swap the CTA band for real store badges.

## Privacy policy — what it asserts, and what needs fixing in the app

`privacy.html` was written from the app's code, not from the MTG policy's shape. Differences that
matter, all verified in `recipe-app-public`:

- **Analytics/Crashlytics are disclosed but NOT YET IN THE APP.** `pubspec.yaml` has no
  `firebase_analytics` and no `firebase_crashlytics` today. The policy describes them anyway,
  because they are planned for launch and the policy only becomes binding at store submission.
  This is deliberate over-disclosure (safe direction), but it must be reconciled: **either add both
  SDKs before submitting, or delete the usage-&-diagnostics row in §1, the two providers in §5, and
  the analytics paragraph in §6.** The store data-safety / privacy-label forms must describe what
  the shipped binary actually does — those cannot over-disclose.
- **No push notifications.** Cooking timers use `flutter_local_notifications` (device-scheduled);
  there is no FCM and no notification token, unlike the MTG app.
- **Ads are always non-personalised.** `admob_ads_service.dart` hardcodes
  `AdRequest(nonPersonalizedAds: true)` on every request, and there is no
  `NSUserTrackingUsageDescription`, so no ATT prompt. The MTG policy's ATT paragraph does not apply.
- **Recipe photos are sent to OpenAI** (`functions/` uses the `openai` SDK, `gpt-4.1-mini`, image
  inline as base64; 20 imports/user/day). This is the app's only third-party content disclosure and
  gets its own section — it is the most consequential thing in the policy.
- **Nothing is shared between users.** `firestore.rules` / `storage.rules` are owner-only for
  recipes, images and settings.
- **US processing, not Tokyo.** `extractRecipe` is declared with `onCall({ secrets })` and no
  `region`, so it deploys to the default `us-central1`. The policy says United States — if the
  function or the Firestore location is ever moved, update §10.

Two things in the app should be fixed; the policy is written to be accurate either way:

1. **Account deletion misses the avatar.** `_eraseUserData` in
   `lib/features/auth/presentation/providers/auth_providers.dart` deletes recipes,
   `source.jpg`/`dish.jpg` and the settings doc, but never `avatars/{uid}/avatar.jpg`. That file
   survives account deletion — a real erasure gap. The policy deliberately does not claim the
   avatar is deleted; fix the code and the claim can be broadened.
2. **iOS camera capture will crash and be rejected.** `recipe_list_screen.dart` offers
   `ImageSource.camera`, but `ios/Runner/Info.plist` has no `NSCameraUsageDescription` (it has no
   `NS*UsageDescription` keys at all). iOS terminates the app when the camera is invoked without
   one. There is also no `PrivacyInfo.xcprivacy` manifest, which App Store review now requires.

## Deploy
In Vercel: **New Project → import this repo → Root Directory = `recipetune.koczancraft.com`**,
framework **Other**, no build step. Then add the `recipetune` subdomain to that project (DNS is
managed on Vercel — see the repo root `README.md`).

## Keeping this in sync
The app repo (`recipe-app-public`) is the source of truth for features and brand. If its design
system (`docs/brand-brief.md`) or feature set changes materially, refresh `CONTENT-BRIEF.md` and
this page to match.
