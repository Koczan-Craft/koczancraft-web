# RecipeTune landing page — content brief

This is the source-of-truth brief for designing `recipetune.koczancraft.com`. It compresses
everything from the app repo (`recipe-app-public`) that a designer/copywriter needs — one-liners,
features, selling points, competitive framing, brand tokens, and copy — so a design pass (e.g.
feeding this to a design tool) doesn't have to re-derive it from the codebase.

Everything below is sourced from the app repo: `docs/brand-brief.md`,
`.claude/skills/design-critique/references/design-language.md`, `docs/research/feature-ideas-v2.md`,
`openspec/specs/*/spec.md`, and `HANDOFF.md`. Where something is a judgment call rather than an
established fact, it's flagged as such.

---

## 0. What this page is (and isn't)

- A single static marketing landing page, same shape as the other sites in this repo
  (`mtgdraftcompanion.koczancraft.com/index.html`): one HTML file, inline CSS + JS, EN/日本語
  toggle, no build step, no dependencies beyond Google Fonts.
- **This is a pre-launch page.** The app is feature-complete and daily-driven by the developer,
  but is **not yet store-listed**: Android still signs release builds with the debug keystore
  (blocking for a real Play Store release — see the app repo's `HANDOFF.md`), and there is no
  App Store Connect / Play Console listing yet. **Do not put real App Store / Google Play badges
  that link nowhere.** See §12 for the recommended pre-launch CTA.
- Visually this should **not** reuse the MTG Draft Companion page's dark/gold theme. RecipeTune
  has its own real design system (Guided Simplicity — coral on near-white, calm, premium) that the
  app itself uses; the landing page should look like a natural extension of the actual product,
  not a reskin of a sibling site.

---

## 1. The app in one line

**RecipeTune** turns a messy recipe screenshot — especially a Japanese one — into something you
can trust and actually cook. Then it tunes the recipe to you: your servings, your units, your
kitchen, with built-in cooking timers.

- **EN name:** RecipeTune
- **JA name:** レシピチューン
- **Name concept:** *tune* = adjust/adapt (servings scaling, unit conversion) — the recipe adapts
  to the cook, not the other way around.
- **What it is NOT:** not a recipe catalog, not a discovery feed, not a social network. It's a
  calm personal tool for the recipes you already have.

**Positioning line (from internal research, already bilingual — good hero copy candidate):**
- JA headline: 見出し：スクショしたレシピを、作れる形に。
- JA sub: サブ：材料と手順を自動整理。人数・単位・手持ちの食材に合わせて、安全に調整。
- EN equivalent: "Turn a screenshotted recipe into something you can actually cook." /
  "Ingredients and steps, organized automatically. Adjusted safely for servings, units, and what
  you have on hand."

---

## 2. Who it's for

- Home cooks who save recipes as screenshots (Instagram, blogs, LINE, cookbooks) and end up with
  a messy camera roll they don't trust.
- Anyone cooking from **Japanese-language recipes** — the app's extraction is Japanese-grammar
  aware (subgroup marks ☆/●, glued step references, circled step numbers ①②③, fraction glyphs),
  which most general recipe-manager apps get wrong or don't attempt.
- Bilingual households / people cooking across EN and JA.
- **Android users cooking from Japanese recipes** — a real, currently underserved audience (see
  §7 — the closest direct competitor is iPhone-only).
- Anyone who wants a recipe box that scales servings and converts units *correctly*, without
  turning cooking into data entry.

---

## 3. The core narrative (one hook, don't dilute it)

The loop the whole app is built around: **capture → trust → adapt → cook.**

> Snap or paste a recipe screenshot → RecipeTune extracts it into clean ingredients and steps →
> you glance over anything unusual and fix it in seconds → you **adapt it to how many people
> you're actually feeding, in the units you actually use** → you cook from it, with timers built
> into the steps.

If a feature doesn't serve one of those four verbs, it's not the headline — keep it supporting.

**Two of those four verbs are the actual flagship features and should get equal, co-lead billing
on the page — not a "hero feature + a grid of everything else" shape:**

1. **Trust the import** (§4A) — turning a messy screenshot into something you believe.
2. **Adapt it to you** (§4B) — safe servings scaling + unit conversion. This is less common
   elsewhere than people assume: most recipe apps either don't scale at all, or do naive
   multiplication that quietly breaks a recipe (rounding chaos, scaling things that shouldn't
   scale). Give this real visual weight — it's a genuine, demonstrable selling point, not a
   secondary utility.

---

## 4. Flagship features — two pillars, both deserve the page's primary visual weight

### 4A. Smart photo import, with an honest review step

- Snap a photo or pick an existing screenshot from your gallery — no retyping.
- Server-side extraction (secure Cloud Function, never on-device, API keys never touch the
  client) turns it into structured ingredients + steps.
- **Japanese-grammar-native**: correctly parses seasoning subgroups (☆を加える / A・Bのグループ),
  step cross-references, glued references, and fraction/quantity glyphs that trip up generic OCR.
- **Import review, not blind auto-save.** After extraction you see the parsed recipe with only
  the *genuinely uncertain* fields flagged — an amount with no unit, a step referencing a group
  that doesn't exist, a missing yield. Everything else looks calm and normal, because it doesn't
  need your attention. Tap a flagged field to see the source image crop + raw text and fix it in
  place. **Nothing saves until you say so.**
- Multi-image import: capture ingredients on one shot and steps on the next.
- The original photo is kept with the recipe, so you can always go back and double-check.

**Why this matters (use as supporting copy, not a swipe at anyone):** most recipe scanners show
a parsed result that looks equally confident whether it's right or wrong. RecipeTune only draws
your eye to the parts worth checking — the rest is just quietly correct.

### 4B. Safe serving scaling + unit conversion — adapt any recipe to you

**This is the other half of the flagship story, and it should be shown with the same weight as
import — screenshots, a dedicated section, real explanatory copy, not a one-line bullet.** It's
the part of the app that most directly matches the product's name ("tune") and it's a feature
most recipe apps either skip or get wrong.

- **Instant servings scaling.** Pick any target servings count and every ingredient amount
  recalculates proportionally, live, without ever touching the recipe you saved — go from a
  recipe for 2 to a dinner for 8 in one tap.
- **It's *safe* scaling, not just multiplication.** Naive "×4 everything" is where other apps
  quietly break a recipe. RecipeTune scales ingredients intelligently:
  - Smart rounding to familiar cooking fractions instead of ugly decimals (no "1.5 eggs" or
    "0.33 tsp" left to figure out).
  - A quiet, non-intrusive note when a scale is large enough that something needs a human
    decision (e.g. pan size, cook time, or oven heat don't scale the same way ingredients do).
  - Awareness that not everything in a recipe scales at all — frying oil depth, blanching water,
    pan/tray size are treated differently from ingredient amounts, so scaling never produces an
    absurd result (this is a known blind spot in most competing recipe apps — see §7).
- **Built-in unit converter**, first-class, not an afterthought: Japanese ⇄ metric cooking units
  (小さじ / 大さじ / カップ ⇄ ml / g) convert instantly and exactly, both as a standalone quick-convert
  tool and *live, inline* while viewing any recipe — no mental math, no separate app, no leaving
  the recipe to go look something up.
- **Together, these turn "a recipe" into "your recipe, tonight, for exactly who's eating."**

**Roadmap note (do not present as shipped today — fine to tease as "coming next" if the page
wants a forward-looking line, otherwise omit):** key-ingredient scaling — tell RecipeTune "I have
320g of chicken" and have the rest of the recipe adapt to match what's actually in your kitchen.
Also planned: representing yields/amounts as honest ranges (e.g. "2〜3人分") instead of forcing a
single number.

---

## 5. Supporting features (grouped — use as feature-grid/cards, not a wall of bullets)

These are real features but should sit *below* the two flagship pillars in §4 visually — smaller
cards, not competing for the same amount of attention as import and adaptive scaling/conversion.

**A. A real cook mode — not a separate "cook mode" screen**
- The recipe detail screen *is* the cooking surface. No mode to switch into.
- Tap a cook time mentioned inside a step (e.g. "1時間30分" or "simmer for 10 minutes") to start a
  timer — multiple timers can run at once.
- Tap a subgroup mark (☆/●) or a circled step number (③) referenced inside a step to see those
  ingredients (already scaled to your servings) or peek that step's text, right there — no
  scrolling back and forth between steps and ingredients.
- Keep-awake while a recipe is open, so the screen doesn't sleep mid-recipe.

**B. Built to last, not just to launch**
- One codebase, truly **cross-platform: iOS and Android** (see §7 — this is a real gap in the
  category).
- **Local-first**: recipes live on your device and are fully usable offline; sign in is optional
  and never gates the app.
- Bilingual **EN / 日本語** throughout the whole app, not just a translated menu.
- Light and dark theme support.

**C. Organize & find**
- Fast search across recipe titles and ingredient names, in English and Japanese.
- One optional category per recipe (Main / Side / Dessert) — kept deliberately simple, not a tag
  jungle.
- Custom ingredient subgroups you can create, rename, and merge right in the recipe editor —
  useful for Japanese seasoning groups (合わせ調味料) that generic recipe apps flatten.

**D. Respectful monetization**
- Free, with light advertising that stays out of the cooking flow: small, fixed-size banner slots
  on browse/convert/detail/settings; no ad appears while a cooking timer is active; no ad ever
  sits over sign-in.

*Not yet shipped — do not present as current features (fine to omit entirely from v1 copy):
share-sheet capture straight from Instagram/TikTok/Safari.*

---

## 6. Personality & tone of voice

**Premium · simple · optimistic · calm · trustworthy.** Think a well-made kitchen tool, not a
sticker or a growth-hacked app. Confident, warm, plain language — short sentences, no hype-stacking.

- Avoid leading with "AI-powered" as the headline — the story is *trust and craft*, not "look, AI."
  The extraction can be described factually ("automatically organizes...") without making the AI
  the star.
- Avoid superlative pile-ups ("the best, smartest, most powerful..."). One confident claim at a
  time reads more premium than five stacked ones.
- Error/edge copy anywhere on the page (e.g. a form validation message) should follow the app's own
  rule: human, blame-free, solution-first — never a bare "Error" or "Invalid input."
- Every section that ships English copy should have a Japanese counterpart of equal quality, not a
  literal translation afterthought — this is a bilingual product, and the page should feel that
  way structurally (same `data-i18n` pattern the sibling sites use).

---

## 7. How it stands out (competitive framing)

Use this as **positioning context for the copywriter**, not as verbatim public-facing copy.
**Do not name a specific competitor and make an unverified negative claim about it** (e.g. "weak
accuracy") on the public page — that's a comparative-advertising / factual-risk problem. Frame
RecipeTune around what it does, using category language, not competitor call-outs.

Three camps exist in the market; RecipeTune's actual gap is the space none of them cover:

1. **Japanese recipe catalog apps** (Cookpad, Kurashiru, DELISH KITCHEN, Nadia, and similar) win on
   discovery, browsing, and video — but they're catalogs for finding new recipes, not personal
   tools for organizing the recipes you already have.
2. **Global personal recipe managers** (the Paprika/Mela/Crouton/ReciMe category) are solid recipe
   boxes, but they're English-first, not built around Japanese recipe grammar, and largely
   iOS-only.
3. A close direct competitor exists that combines Japanese-language support, screenshot import, and
   serving scaling — but it's **iPhone-only**. (Internal note: レシピング. Don't name it on the
   public page unless the copy is framed as neutral market context, not a comparison claim.)

**RecipeTune's actual differentiators, safe to state plainly and specifically:**
- Japanese-grammar-native extraction (subgroups, step cross-references, glued references) — not a
  generic OCR-and-hope.
- An honest review-before-save step instead of silent auto-save.
- **Scaling that's actually safe, not just multiplication** — this is a bigger, more concrete
  selling point than it might first look, and worth its own emphasis on the page, not a footnote:
  most recipe managers either don't offer servings scaling at all, or naively multiply every
  number by the same factor. That naive approach is exactly where recipes quietly break — a
  scaled-up recipe with "1.5 eggs" and no acknowledgment that pan size, cook time, or oil-frying
  depth don't scale like an ingredient amount does. RecipeTune's scaling is aware of that
  distinction. Pair this with instant, exact Japanese ⇄ metric unit conversion built into every
  recipe (not a separate lookup), and "adapt it to you" becomes a real, demonstrable feature —
  not a marketing phrase.
- A cook mode built directly from the structured recipe (tappable timers and cross-references),
  not a separate "cooking view."
- **Genuinely cross-platform — iOS and Android from one app**, which is uncommon in this exact
  niche.

---

## 8. Visual identity — use the app's real design system, not a new one

Canonical source: `docs/brand-brief.md` and
`.claude/skills/design-critique/references/design-language.md` in the app repo. Summarized here:

### Color (one accent only — do not introduce a second brand hue)
| Role | Hex | Notes |
|---|---|---|
| Accent / coral (primary) | `#F0674A` | The brand color — primary buttons, key accents, mark |
| Accent pressed | `#D8543A` | Deeper coral for pressed/hover states |
| Accent tint | `#FCEBE7` | Soft coral wash, backgrounds, badges |
| Ink (primary text) | `#1A1A1A` | Headings, primary text |
| Ink secondary | `#6B6B6B` | Supporting text |
| Ink tertiary | `#9A9A9A` | Placeholders/hints |
| Background | `#FFFFFF` | Canvas |
| Background subtle | `#F6F6F4` | Warm off-white, recessed sections |
| Hairline | `#EDEDED` | Dividers, used sparingly |
| Dark background | `#121212` | Dark-mode canvas, if the page supports a dark theme |

**Contrast caution:** coral on white sits at ~3:1. Never use thin coral text or hairlines as the
only element — use solid coral shapes/fills, or pair coral with enough weight/size to clear
contrast, or use ink + a coral accent detail instead of coral body text.

### Typography
- **Latin:** Inter (the app's actual font — not Hanken Grotesk/Newsreader, which is what the MTG
  sibling page uses; RecipeTune should look like its own product).
- **Japanese:** Noto Sans JP.
- Geometric, even, confident — not decorative or script. One bold display heading per section;
  max two visible weights per screen plus the display weight.

### Shape, space, elevation (the "Guided Simplicity" language — carry it to the web page too)
- **Soft geometry**: rounded everything — buttons, cards, images, inputs. Buttons/chips/pills are
  fully rounded (`radius: 999`) unless there's a deliberate reason not to.
- **8pt spacing scale**: generous whitespace is a feature, not empty space to fill. Prefer
  multiples of 8 (16/24/32/48px) for section gaps and padding.
- **Gentle elevation**: soft, low, diffuse shadows for any card/panel — never hard drop shadows or
  heavy borders.
- **One hero, one accent per section** — a single focal element per screen region (usually the
  primary CTA or a piece of imagery); the accent color should not become a large background fill
  or be sprinkled everywhere.
- **Calm canvas**: near-white background, content sits on space. Don't fill the page edge-to-edge
  with boxes.

### Iconography
- Outline-style icons, consistent stroke weight, paired with labels where meaning isn't obvious —
  matches the in-app icon language.

---

## 9. Icon & image assets

Copied into this folder from the app repo's `assets/branding/` (all 1024×1024):

- `icon.png` — the app icon on a light/white background: a black spoon with a coral radiating
  "tuning" arc above it (the tune/dial + cooking-tool metaphor from the brand brief, already
  realized). Use this as the page favicon and the hero crest/mark, same role `icon.png` plays on
  `mtgdraftcompanion.koczancraft.com`.
- `assets/icon-dark.png` — dark-background variant of the same mark, for a dark-mode favicon/crest
  if the page supports a dark theme.
- `assets/icon-fg.png` — foreground-only layer (transparent background), useful if the design
  wants to place the mark over a custom color panel instead of its default white/dark tile.

**Not yet available — flag as open items, don't fabricate:**
- **Real app screenshots.** The app isn't store-listed yet; no polished marketing screenshots
  exist. Recommend either (a) a v1 page that leans on the icon + illustrative/abstract UI
  mockups rather than real screenshots, or (b) taking a small set of real screenshots (recipe
  list, recipe detail with a timer running, the import-review screen, the converter) from a
  device before this design pass, if screenshots are wanted for the hero/feature sections.
- **OG/social share image** (1200×630). None exists yet — a simple first version: white/off-white
  canvas, the icon, the wordmark, and the EN tagline, coral accent only on the mark. Needed for
  `og:image` / `twitter:image` meta tags (see the sibling page's `<head>` for the exact tag shape
  to mirror).

---

## 10. Suggested page structure

Mirrors the sibling sites' shape (topbar → hero → features → CTA → footer):

1. **Sticky topbar** — icon/wordmark on the left, EN/日本語 toggle on the right (same
   `lang-toggle` pattern as the other sites).
2. **Hero** — crest (`icon.png`), H1 using the name + hook, the bilingual tagline from §1, primary
   CTA (see §12 — not a store badge yet).
3. **Flagship spotlight #1 — Smart Import** (§4A): the review-before-save trust story.
4. **Flagship spotlight #2 — Adapt it to you** (§4B): servings scaling + unit conversion, given
   its own section with the same visual weight as #3 (imagery, a heading, real explanatory copy)
   — **not folded into the feature grid below.** This is the section the user most wants
   showcased; don't undersell it as "just a utility."
5. **Feature grid** — the smaller supporting features from §5 (real cook mode, cross-platform +
   local-first + bilingual, organize & find, monetization). Keep each card short — one idea per
   card, per the "one hero, one accent" rule.
6. **Why RecipeTune** — a short, non-disparaging differentiation band using §7's safe framing
   (Japanese-grammar-native, honest review step, safe scaling, truly cross-platform). No named
   competitor call-outs.
7. **CTA band** — pre-launch notify-me (see §12).
8. **Footer** — link back to `koczancraft.com` (matches the house cross-linking convention), any
   contact address, privacy link once one exists (see §14).

Keep the page as calm as the app: generous whitespace, one accent, no dense walls of copy. Two
flagship sections back to back is still "calm" as long as each stays simple — it's about giving
scaling/conversion its own room, not about adding more total copy.

---

## 11. Copy starting points (EN / JA pairs — refine, don't treat as final)

**Hero**
- EN: "Turn a messy recipe screenshot into something you can actually cook." /
  sub: "RecipeTune reads your recipe photos — Japanese included — organizes them, and tunes them
  to your servings, your units, your kitchen."
- JA: "スクショしたレシピを、作れる形に。" / "材料と手順を自動整理。人数・単位・手持ちの食材に合わせて、安全に調整。"

**Flagship spotlight #1 — Import**
- EN: "We show you what's worth checking — not everything." / "Import a photo, and RecipeTune
  flags only the parts that are genuinely unclear. Everything else is already right. Nothing saves
  until you say so."

**Flagship spotlight #2 — Adapt it to you (scaling + conversion)**
- EN headline options:
  - "Any recipe, exactly the right size."
  - "Cooking for 2 tonight, not 4? One tap."
  - "Scaled the right way, not just multiplied."
- EN body: "Change the servings and every ingredient recalculates instantly — rounded to amounts
  you'd actually measure, not ugly decimals, and never touching the recipe you saved. RecipeTune
  also knows what *shouldn't* scale the same way — pan size, cook time, oil for frying — so a
  recipe for 8 never turns into nonsense. Need 小さじ in ml, or カップ in grams? It converts
  instantly, right inside the recipe."
- JA headline: "人数に合わせて、ちょうどいい量に。"
- JA body: "人数を変えるだけで、材料の分量が自動で計算し直されます。元のレシピはそのまま。小さじ・
  大さじ・カップも ml・g にすぐ変換できます。"

**Supporting feature snippets** (short, one line each — for the smaller feature grid, §5)
- "Tap a time in any step to start a timer — no separate cook mode."
- "Your recipes work offline. Signing in is optional, always."
- "Every screen, in English and 日本語."
- "One app. iPhone and Android."

---

## 12. Call to action (pre-launch reality — read this before designing the CTA)

The app is **not store-listed yet** (see §0). Recommended v1 CTA, matching the pattern already
used on `mtgdraftcompanion.koczancraft.com` (a Web3Forms contact form, free tier):

- Primary CTA: **"Coming soon to iPhone & Android"** + a simple email capture ("Get notified when
  RecipeTune launches") wired to Web3Forms, same mechanism as the MTG page.
- **Open decision (needs an answer before wiring the form): which inbox should notify-me signups
  land in?** — e.g. a new `recipetune@koczancraft.com` alias, or route to the existing
  `hello@`/`support@koczancraft.com`. Pick one and set the Web3Forms key's destination
  accordingly (see the MTG folder's README for the pattern).
- Structure the CTA markup so swapping in real **App Store** / **Google Play** badges later is a
  small, isolated change (e.g. a single CTA component), not a page rewrite.

---

## 13. SEO / meta starting points

Mirror the `<head>` shape used in `mtgdraftcompanion.koczancraft.com/index.html` (title, meta
description, Open Graph, Twitter card, favicon). Suggested starting copy:

- `<title>`: "RecipeTune | Turn a recipe screenshot into something you can cook"
- meta description: "RecipeTune organizes recipe photos — Japanese included — into clean
  ingredients and steps, then adapts them to your servings, units, and kitchen. iOS & Android."
- `og:site_name`: "Koczan Craft"
- `og:url`: `https://recipetune.koczancraft.com`
- `og:image` / `twitter:image`: needs the OG asset from §9 once produced.

---

## 14. Technical constraints (must follow — house convention)

- **One `index.html`**, inline CSS + JS, no bundler, no dependencies beyond Google Fonts
  (Inter + Noto Sans JP — see §8, don't substitute other faces).
- **`data-i18n` + an `I18N` dictionary + an `applyLang()` toggle** for EN/日本語, same pattern as
  every other site in this monorepo.
- Static only, no build step. Will deploy as its own Vercel project with
  **Root Directory = `recipetune.koczancraft.com`**, framework "Other" — see the repo root
  `README.md`.
- **No `privacy.html` yet.** The app does ship Firebase (Auth/Firestore/Storage) and AdMob, so a
  real privacy policy will be needed before any store submission — mirror
  `mtgdraftcompanion.koczancraft.com/privacy.html`'s structure when that's written, consistent with
  data handling actually implemented in the app. Not required to design the landing page itself.

---

## 15. Open items — resolve before or during the design pass

- [ ] Pick a final tagline (EN + JA) from §11 or write a new one.
- [ ] Decide the notify-me email destination (§12) before wiring the Web3Forms key.
- [ ] Decide whether v1 ships with real app screenshots or an icon/illustration-only hero (§9).
- [ ] Produce the OG share image (§9) — simple version proposed above is enough for v1.
- [ ] Once there's a store listing, swap the CTA band for real store badges (§12) and add
      `privacy.html` (§14) before submitting to either store.
