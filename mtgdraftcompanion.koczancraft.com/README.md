# mtgdraftcompanion.koczancraft.com

The **MTG Draft Companion** app landing page + privacy policy. Self-contained, bilingual
(EN / 日本語). This is the **canonical** copy — it moved here from the app repo
(`mtgdraftcompanion-public-version/docs/landing/`) so all web pages live in one place.

## Files
- `index.html` — the app landing page (features, how-it-works, contact form via Web3Forms).
- `privacy.html` — the privacy policy (ads-active). **Update this whenever the privacy/policy
  changes** — it's the page the App Store / Play listings link to.
- `icon.png` — app icon (favicon + brand lockup).

## ⚠️ Keeping policy in sync
The store privacy URL points at `https://mtgdraftcompanion.koczancraft.com/privacy.html`.
When ads/analytics/data handling changes, edit `privacy.html` **here** (and keep it consistent
with the app's `PrivacyInfo.xcprivacy` + `docs/STORE.md` in the app repo).

## Before launch
- Set `WEB3FORMS_KEY` in `index.html` (free key at https://web3forms.com → destination
  `contact@koczancraft.com`).
- Remove any `todo-note` blocks in `privacy.html` once the contact mailbox is live.

## Deploy
Vercel project with **Root Directory = `mtgdraftcompanion.koczancraft.com`**, framework
**Other**, no build. Assign the subdomain `mtgdraftcompanion.koczancraft.com` to it. Pushes to
`main` auto-deploy.
