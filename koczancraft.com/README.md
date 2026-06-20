# koczancraft.com

The Koczan Craft brand & portfolio home. Single self-contained `index.html`
(HTML + CSS + JS), white/premium, bilingual (EN / 日本語).

## Sections
1. **Hero** — "I make the things I love." One person, many crafts.
2. **About** — the portfolio intro (leather → apps, made slowly, by hand).
3. **Koczan Craft Leather** — intro + `@koczancraft` follow card + recent Instagram posts.
4. **Apps** — MTG Draft Companion card → links to `mtgdraftcompanion.koczancraft.com`.

## Before / after launch — quick edits

- **Instagram posts:** open `index.html`, find `var IG_POSTS = [ … ]` near the bottom, and
  paste up to 3 recent post URLs (e.g. `https://www.instagram.com/p/XXXXXXXXXXX/`). Empty =
  a clean fallback card. Swap the URLs anytime to "refresh" the feed.
- **Contact email:** the footer links to `mailto:contact@koczancraft.com` — make sure that
  mailbox/forwarder exists (the same address used on the MTG privacy page).

## Local preview

```bash
cd koczancraft.com && python3 -m http.server 8000   # → http://localhost:8000
```

## Deploy
Vercel project with **Root Directory = `koczancraft.com`**, framework **Other**, no build
step. Assign the apex domain `koczancraft.com` to this project.
