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

## `.well-known/assetlinks.json` — Digital Asset Links

Associates the Android app with this domain so it can use **Restore Credentials** (the
WebAuthn-style key that carries a signed-in session to a new device). Without a valid
association the app cannot create the credential at all, and the failure is silent.

JSON takes no comments, so the notes live here:

- **Relation is `delegate_permission/common.get_login_creds` only.** `handle_all_urls` is
  commonly pasted alongside it and is deliberately **absent** — that one declares App Links,
  which would offer the app as a handler for koczancraft.com URLs. Unrelated; don't add it.
- **Three fingerprints, and they are not interchangeable:**

  | fingerprint | key | signs |
  |---|---|---|
  | `37:8A:8F:09…` | Play app-signing | every build installed from the Play Store |
  | `A8:7C:1F:68…` | upload | a locally built **release** APK (→ prod Firebase) |
  | `5D:B0:6D:91…` | debug (`~/.android/debug.keystore`) | a **debug** build (→ dev Firebase) |

- 🚨 **The debug entry is for pre-release testing and should be removed before release.**
  A release build never presents it. It is here because the build-type environment switch means
  a debug build is the only thing that talks to the dev Firebase project, so without it the
  feature could only ever be exercised against production. The risk of leaving it is small — an
  Android debug keystore is generated per machine, not a shared well-known key — but it is a
  production domain delegating login credentials to a development key, so it should not outlive
  its purpose. Tracked as task 0b.8 of `android-restore-credentials` in the app repo.
- **Verify the SERVED response after any change**, not the file in the repo: it must be `200`
  with a JSON content-type and **no redirect** (an apex→www or http→https hop fails association
  silently). Google's own Digital Asset Links API is the authority worth checking against, not
  curl alone.

## Email routing
- **Contact form** → `support@koczancraft.com` (app user support). Set the Web3Forms key's
  destination to support@ (see below).
- **Privacy policy contact** (`privacy.html`) → `privacy@koczancraft.com`.

## Before launch
- Set `WEB3FORMS_KEY` in `index.html` (free key at https://web3forms.com → destination
  `support@koczancraft.com`).

## Deploy
Vercel project with **Root Directory = `mtgdraftcompanion.koczancraft.com`**, framework
**Other**, no build. Assign the subdomain `mtgdraftcompanion.koczancraft.com` to it. Pushes to
`main` auto-deploy.
