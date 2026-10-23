---
version: alpha
name: SignpostMe
description: A UK private-healthcare brand whose marketing surfaces pair a deep teal canvas with a single warm-peach action colour. Headlines are Plus Jakarta Sans at ExtraBold 800, body is Inter, and the page floor is a cream off-white rather than pure white. Shapes are soft but not pill-shaped — 10px on buttons and inner tiles, 20px on cards and photo tiles — and depth comes from 2px pale-teal borders and one deep-teal glow shadow rather than elevation tiers. Sections alternate teal → cream/white → mint or lighter teal → cream → teal → deep-teal footer, and the recurring motif is a thin diagonal peach line cutting across a teal panel like a signpost arm. Two surfaces share the system — core SignpostMe (health cover, FCA-regulated) and SignpostMe Wellness powered by Phlo (GLP-1 weight loss, GPhC-regulated).

# This block mirrors the CSS `@theme` tokens in §3 and the component recipes in §7 so that
# design tooling can read the system without parsing prose. §3 remains the build source of
# truth: if the two ever disagree, §3 wins and this block must be corrected to match.

colors:
  teal: "#00818A"           # brand-primary. Hero/section/nav bg, link colour on light
  teal-deep: "#105151"      # footer, message bar, text on peach, glow shadow
  teal-dark: "#4EAC98"      # lighter-teal band, drawer bg, input focus ring, icon fills
  teal-mid: "#3E9586"       # hover on teal-dark
  mint: "#A8DDA8"           # highlight word in headlines on teal, testimonial band bg
  mint-light: "#D3EED3"
  mint-lighter: "#A6D6CB"
  mint-deep: "#82BA92"
  logo-mint: "#46C9B5"      # only in the wordmark "Me"
  peach: "#F1A88A"          # PRIMARY CTA and the diagonal line motif
  peach-hover: "#C4977F"
  cream: "#FFFDF8"          # page background
  info: "#E7EEEE"           # soft grey-teal section bg (team section)
  flag: "#FFF8E2"           # pale yellow pill bg (trust flags)
  flag-dark: "#E7E7D3"      # darker flag tint (Figma palette sheet)
  line: "#CFDCDC"           # card borders, dividers, toggle borders
  ink: "#1C2B2B"            # headings and body on light
  ink-muted: "#5C5B5B"      # secondary text, captions, legal
  placeholder: "#8D8D8D"    # input placeholder, light grey labels
  disabled-bg: "#A0A1A1"
  disabled-fg: "#757575"
  teal-tint: "#21C2BA1A"    # rgb(33 194 186 / 0.10), tinted card bg
  star: "#FAC906"           # review stars
  error: "#F28482"
  white: "#FFFFFF"          # cards and inputs only, never the page floor
  on-teal: "#FFFFFF"        # alias: text on teal, teal-dark, teal-deep
  on-peach: "#105151"       # alias: text on the peach CTA is always teal-deep

typography:
  display:
    fontFamily: "'Plus Jakarta Sans', Arial, sans-serif"
    fontSize: "clamp(40px, 4vw + 24px, 64px)"
    fontWeight: 800
    lineHeight: 1.05
    letterSpacing: 0
  h1:
    fontFamily: "'Plus Jakarta Sans', Arial, sans-serif"
    fontSize: 42px
    fontWeight: 800
    lineHeight: 1.1
    letterSpacing: 0
  h2:
    fontFamily: "'Plus Jakarta Sans', Arial, sans-serif"
    fontSize: 32px
    fontWeight: 800
    lineHeight: 1.15
    letterSpacing: 0
  h3:
    fontFamily: "'Plus Jakarta Sans', Arial, sans-serif"
    fontSize: 28px
    fontWeight: 800
    lineHeight: 1.2
    letterSpacing: 0
  h4:
    fontFamily: "'Plus Jakarta Sans', Arial, sans-serif"
    fontSize: 22px
    fontWeight: 800
    lineHeight: 1.25
    letterSpacing: 0
  sub:
    fontFamily: "'Plus Jakarta Sans', Arial, sans-serif"
    fontSize: 16px
    fontWeight: 700
    lineHeight: 1.2
    letterSpacing: 0
  lead:
    fontFamily: "'Inter', Arial, sans-serif"
    fontSize: 18px
    fontWeight: 400
    lineHeight: 1.5
    letterSpacing: 0
  body:
    fontFamily: "'Inter', Arial, sans-serif"
    fontSize: 16px
    fontWeight: 400
    lineHeight: 1.5
    letterSpacing: 0
  body-strong:
    fontFamily: "'Inter', Arial, sans-serif"
    fontSize: 16px
    fontWeight: 700
    lineHeight: 1.5
    letterSpacing: 0
  small:
    fontFamily: "'Inter', Arial, sans-serif"
    fontSize: 14px
    fontWeight: 400
    lineHeight: 1.45
    letterSpacing: 0
  xs:
    fontFamily: "'Inter', Arial, sans-serif"
    fontSize: 12px
    fontWeight: 400
    lineHeight: 1.4
    letterSpacing: 0
  price:
    fontFamily: "'Plus Jakarta Sans', Arial, sans-serif"
    fontSize: 48px
    fontWeight: 800
    lineHeight: 1
    letterSpacing: 0
  button:
    fontFamily: "'Plus Jakarta Sans', Arial, sans-serif"
    fontSize: 16px
    fontWeight: 800
    lineHeight: 1
    letterSpacing: 0
  milestone:
    fontFamily: "'Plus Jakarta Sans', Arial, sans-serif"
    fontSize: 20px
    fontWeight: 700
    lineHeight: 1
    letterSpacing: 0

rounded:
  sm: 5px       # nav active pill
  md: 10px      # buttons, inner tiles, milestone box, calculator wrapper, inputs, FAQ items
  lg: 20px      # cards, flags/pills, image tiles
  pill: 999px   # reserved; the brand does not use fully-rounded buttons

spacing:
  # Webflow margin ladder (§6); section and gutter values from §3
  xs: 5px
  sm: 10px
  md: 20px
  lg: 40px
  xl: 60px
  xxl: 120px
  gutter: 24px
  gutter-desktop: 60px
  card: 32px
  card-mobile: 24px
  section: 96px
  section-mobile: 64px

components:
  # ─── Buttons (§7.1) ───
  # height 44px is computed from the recipe (16px text at line-height 1 + 12px padding ×2 + 2px border ×2), not measured
  button-primary:
    backgroundColor: "{colors.peach}"
    textColor: "{colors.on-peach}"
    borderColor: "{colors.peach}"
    borderWidth: 2px
    typography: "{typography.button}"
    rounded: "{rounded.md}"
    padding: 12px 32px
    height: 44px
  button-primary-hover:
    backgroundColor: "{colors.peach-hover}"
    textColor: "{colors.on-peach}"
    borderColor: "{colors.peach-hover}"
    rounded: "{rounded.md}"
  button-inverse:
    backgroundColor: "{colors.white}"
    textColor: "{colors.teal}"
    borderColor: "{colors.white}"
    borderWidth: 2px
    typography: "{typography.button}"
    rounded: "{rounded.md}"
    padding: 12px 32px
    height: 44px
  button-outline:
    backgroundColor: transparent
    textColor: "{colors.teal}"
    borderColor: "{colors.teal}"
    borderWidth: 2px
    typography: "{typography.button}"
    rounded: "{rounded.md}"
    padding: 12px 32px
    height: 44px
  button-outline-inverse:
    backgroundColor: transparent
    textColor: "{colors.on-teal}"
    borderColor: "{colors.on-teal}"
    borderWidth: 2px
    typography: "{typography.button}"
    rounded: "{rounded.md}"
    padding: 12px 32px
    height: 44px
  button-disabled:
    backgroundColor: "{colors.disabled-bg}"
    textColor: "{colors.disabled-fg}"
    typography: "{typography.button}"
    rounded: "{rounded.md}"
    padding: 12px 32px
    height: 44px

  # ─── Pills and flags (§7.2) ───
  pill-flag:
    backgroundColor: "{colors.flag}"
    textColor: "{colors.teal-deep}"
    borderColor: "{colors.flag}"
    borderWidth: 1px
    typography: "{typography.xs}"
    rounded: "{rounded.lg}"
    padding: 4px 14px
  pill-flag-primary:
    backgroundColor: "{colors.mint}"
    textColor: "{colors.teal}"
    typography: "{typography.xs}"
    rounded: "{rounded.lg}"
    padding: 4px 14px
  pill-flag-outline:
    backgroundColor: transparent
    textColor: "{colors.ink}"
    borderColor: "{colors.ink}"
    borderWidth: 1px
    typography: "{typography.xs}"
    rounded: "{rounded.lg}"
    padding: 4px 14px

  # ─── Bands and bars (§7.3, §7.7, §7.8, §7.10, §7.15) ───
  trust-marquee:
    backgroundColor: "{colors.cream}"
    textColor: "{colors.teal-deep}"
    typography: "{typography.xs}"
  announcement-bar:
    backgroundColor: "{colors.teal-deep}"
    textColor: "{colors.on-teal}"
    typography: "{typography.small}"
    padding: 10px 20px
  navbar:
    backgroundColor: "{colors.teal}"
    textColor: "{colors.on-teal}"
    typography: "{typography.small}"
    height: 70px
  nav-active-pill:
    backgroundColor: "{colors.white}"
    textColor: "{colors.teal}"
    typography: "{typography.small}"
    rounded: "{rounded.sm}"
    padding: 5px
  sticky-cta-bar:
    backgroundColor: "{colors.white}"
    textColor: "{colors.ink}"
    borderColor: "{colors.line}"
    borderWidth: 2px
    padding: 16px
  cta-band:
    backgroundColor: "{colors.teal}"
    textColor: "{colors.on-teal}"
  team-section:
    backgroundColor: "{colors.info}"
    textColor: "{colors.ink}"
  footer:
    backgroundColor: "{colors.teal-deep}"
    textColor: "{colors.on-teal}"
    typography: "{typography.small}"
    paddingTop: 40px
  footer-legal:
    backgroundColor: "{colors.teal-deep}"
    textColor: "{colors.on-teal}"
    typography: "{typography.xs}"

  # ─── Hero (§7.9) ───
  hero:
    backgroundColor: "{colors.teal}"
    textColor: "{colors.on-teal}"
    typography: "{typography.display}"
    padding: 60px 0
    motifColor: "{colors.peach}"
  hero-headline-accent:
    textColor: "{colors.mint}"
    typography: "{typography.display}"
  hero-image-tile:
    rounded: "{rounded.lg}"

  # ─── Cards (§7.4, §7.11, §7.12, §7.13) ───
  card:
    backgroundColor: "{colors.white}"
    textColor: "{colors.ink}"
    borderColor: "{colors.line}"
    borderWidth: 2px
    typography: "{typography.body}"
    rounded: "{rounded.lg}"
    padding: "{spacing.card}"
  card-tint:
    backgroundColor: "{colors.teal-tint}"
    textColor: "{colors.ink}"
    typography: "{typography.body}"
    rounded: "{rounded.lg}"
    padding: "{spacing.card}"
  card-mint:
    backgroundColor: "{colors.mint}"
    textColor: "{colors.ink}"
    typography: "{typography.body}"
    rounded: "{rounded.lg}"
    padding: "{spacing.card}"
  card-icon-box:
    backgroundColor: "{colors.teal}"
    textColor: "{colors.on-teal}"
    rounded: "{rounded.md}"
    size: 40px
  testimonial-band:
    backgroundColor: "{colors.mint}"
    textColor: "{colors.ink}"
  testimonial-card:
    backgroundColor: "{colors.white}"
    textColor: "{colors.ink}"
    typography: "{typography.body}"
    rounded: "{rounded.lg}"
    padding: "{spacing.card}"
    starColor: "{colors.star}"
  faq-item:
    backgroundColor: "{colors.white}"
    textColor: "{colors.ink}"
    borderColor: "{colors.line}"
    borderWidth: 2px
    typography: "{typography.body-strong}"
    rounded: "{rounded.md}"
    padding: 16px 20px

  # ─── Steps, stats, price (§7.5, §7.16, §11) ───
  milestone-number:
    backgroundColor: "{colors.teal}"
    textColor: "{colors.on-teal}"
    typography: "{typography.milestone}"
    rounded: "{rounded.md}"
    size: 60px
  stat-badge:
    backgroundColor: "{colors.white}"
    textColor: "{colors.teal-deep}"
    numberColor: "{colors.teal}"
    lineColor: "{colors.peach}"
    rounded: "{rounded.lg}"
  stat-badge-on-photo:
    backgroundColor: transparent
    textColor: "{colors.on-teal}"
    numberColor: "{colors.mint}"
  price-block:
    backgroundColor: "{colors.cream}"
    textColor: "{colors.ink}"
    typography: "{typography.price}"

  # ─── Calculator (§7.6, Wellness only) ───
  calculator-wrapper:
    backgroundColor: "{colors.white}"
    textColor: "{colors.ink}"
    rounded: "{rounded.md}"
    padding: 20px
  calculator-slider:
    backgroundColor: "{colors.teal}"
    fillColor: "{colors.teal-deep}"
    thumbColor: "{colors.white}"
    thumbBorderColor: "{colors.teal-deep}"
    rounded: "{rounded.lg}"
    height: 40px
  calculator-toggle:
    backgroundColor: "{colors.teal}"
    knobColor: "{colors.white}"
    rounded: "{rounded.pill}"

  # ─── Forms (§7.14, §14) ───
  text-input:
    backgroundColor: "{colors.white}"
    textColor: "{colors.ink}"
    borderColor: "{colors.line}"
    borderWidth: 2px
    placeholderColor: "{colors.placeholder}"
    typography: "{typography.body}"
    rounded: "{rounded.md}"
    height: 48px
  text-input-focused:
    backgroundColor: "{colors.white}"
    textColor: "{colors.ink}"
    borderColor: "{colors.teal-dark}"
    rounded: "{rounded.md}"
  text-input-error:
    backgroundColor: "{colors.white}"
    textColor: "{colors.ink}"
    borderColor: "{colors.error}"
    helperColor: "{colors.error}"
    rounded: "{rounded.md}"
  toggle-group:
    backgroundColor: "{colors.white}"
    textColor: "{colors.ink}"
    borderColor: "{colors.line}"
    borderWidth: 2px
    selectedBackground: "{colors.teal}"
    selectedText: "{colors.on-teal}"
  focus-ring:
    borderColor: "{colors.teal-dark}"
    borderWidth: 2px
    offset: 2px
    onTealColor: "{colors.on-teal}"

  # ─── Text (§5, §10) ───
  legal-caption:
    textColor: "{colors.ink-muted}"
    typography: "{typography.xs}"
---

# SignpostMe — design.md

Agent-first design specification for building SignpostMe-branded marketing landing pages.
Source of truth extracted on 2026-10-22 from:

- `https://signpostme.com/` — core brand (Next.js App Router + MUI, "SignpostMe health cover")
- `https://wellnessbyphlo.signpostme.com/` — Wellness surface (Webflow, "SignpostMe Wellness, powered by Phlo")
- `https://wellnessbyphlo.signpostme.com/lps/lp-wls` — paid-ads landing page on the Wellness surface
- Figma `GfxumnaSlcB9jzZ1NMRt89` ("Cignpost", node 473-85) — ingested, see §15

How to use this file as an agent:

1. Read §1 (essence) and §2 (surface decision) first. Every page declares one surface.
2. Copy the token block in §3 verbatim into `app/globals.css`. Do not invent colours, radii or sizes.
3. Build only from the component recipes in §7. If a component is missing, extend §7 before writing JSX.
4. Every CTA on a Wellness page must use the contract in §12. No exceptions.
5. Run the checklist in §17 before opening a PR.
6. The YAML front matter mirrors §3 and §7 for design tooling; §3 is the build source of truth. §19 lists what is still unconfirmed.

---

## 1. Brand essence

SignpostMe is "private healthcare done differently": fast, affordable, human, UK-regulated. The visual
language is **deep teal + warm peach**, big friendly extra-bold headlines, soft 10–20px radii, cream
off-white backgrounds, and real people in photography. Nothing is clinical-cold or glossy. The
recurring motif is a thin diagonal peach line cutting across teal panels, like a signpost arm.

Two surfaces share this language:

| | Core SignpostMe | SignpostMe Wellness (powered by Phlo) |
|---|---|---|
| What it sells | Health cover / private health insurance | GLP-1 weight-loss service (Wegovy, Mounjaro) |
| Operator | Regent Financial Network Ltd (FCA 304130) | Phlo Technologies Ltd (GPhC 2205052) |
| Where ads land today | `signpostme.com` | `wellnessbyphlo.signpostme.com/lps/*` |
| Logo | `SignpostMe` wordmark, "Me" in `#46C9B5` | `SignpostMe | Wellness · powered by phlo` lockup |

Tone words: reassuring, direct, plain-English, lightly warm. Never salesy-shouty, never medical jargon
without a plain gloss.

### 1.1 Key characteristics

- **One action colour.** Peach `#F1A88A` is the only CTA fill on a page; text on it is always `#105151`. Nothing else is peach except the diagonal line motif.
- **Teal is the canvas, cream is the floor.** Teal `#00818A` carries hero, bands and nav; the page background is cream `#FFFDF8`, never pure white. White is for cards and inputs.
- **ExtraBold sentence-case headlines.** Plus Jakarta Sans 800 at every heading level, letter-spacing 0, no uppercase tracking, no italics. Inter 400/700 for everything you read.
- **10 / 20 radius, never pill.** Buttons, inner tiles and inputs are 10px; cards, flags and photo tiles are 20px. Fully-rounded buttons are off-brand.
- **Borders, not shadows.** Depth comes from a 2px `#CFDCDC` border and, on floating CTAs only, one deep-teal glow. No elevation tiers.
- **Diagonal peach line motif.** Two thin peach lines at ~35° across a teal panel; the ad tiles, OG image, hero and stat badges all carry it.
- **Fixed section rhythm.** teal → cream/white → mint or lighter teal → cream → teal → deep-teal footer.
- **Number up front.** Price and headline statistic appear in the hero and again in a dedicated block; every claim carries a footnoted source.

---

## 2. Surface decision — differences you must choose between

Both surfaces use the same families, hues and shapes, but they were built by different teams and
disagree on details. **Pick one row per line for the new site, or keep both as themes.** The
"Recommended for ad LPs" column is my suggestion; it is not yet confirmed by the brand owner.

| Attribute | Core (`signpostme.com`) | Wellness (Webflow) | Recommended for ad LPs |
|---|---|---|---|
| Dark text colour | `#1C2B2B` (teal-black) | `#212121` (neutral) | `#1C2B2B` — **confirmed by the Figma palette sheet (§15)** |
| Secondary dark | none (uses `#105151`) | `#0A1318`, accent `#020E14` | Drop. Use `#105151` for all "dark" needs |
| Root font size | 10px (`html { font-size: 62.5% }`), rem-based | 16px, px-based | 16px root, px tokens below (simpler in Tailwind) |
| Body size | 18px / 400 | 14px standard, 16px large, 20px xl | 16px body, 14px small, 18px lead |
| Body line-height | `initial` (~1.2) | 140% desktop / 130% mobile | 1.5 |
| Heading line-height | 1 | 110% | 1.1 |
| Display / hero | `clamp(40px, 15.38px + 7.692vw, 50px)` 800 | 72px desktop / 50px mobile, 700 | `clamp(40px, 4vw + 24px, 64px)` 800 |
| H1 | 50px 800 | 42px 700 | 42px 800 |
| H2 | 30px 800 | 32px 700 | 32px 800 |
| H3 | 24px 800 | 28px 700 | 28px 800 |
| H4 | 16px 800 | 22px desktop / 18px mobile, 700 | 22px 800 |
| Heading weight | 800 (ExtraBold) | 700 (Bold, variable font) | 800 — reads as more "SignpostMe" |
| Button radius | 10px | 10px (`--border-radius-standard`) | 10px |
| Button padding | 10px 20px | 12px 40px | 12px 32px |
| Button type | PJS 800 18px, no border | PJS 700 16px, 2px border same colour | PJS 800 16px, 2px border same colour |
| Primary CTA colour | `#F1A88A` text `#105151` hover `#C4977F` | identical | identical |
| Card radius | 20px | 20px large / 10px standard | 20px cards, 10px inner tiles |
| Card border | 2px `#CFDCDC` | 2px `#CFDCDC`-like / none on cream | 2px `#CFDCDC` on white; none on cream |
| Section padding Y | 60px / 80px | 120px desktop, 80px tablet, 60px mobile | 96px desktop, 64px mobile |
| Container | MUI 12-col, ~1200px | 1300 / 1100 / 800 | 1200 standard, 1100 med, 800 narrow |
| Extra colours | mint scale `#D3EED3 #A6D6CB #82BA92`, error `#F28482`, flag `#FFF8E2` | pink `#FF66C4` (defined, unused), `#3E9586`, info bg `#E7EEEE` | Keep mint scale + error + flag + info bg. Drop pink. |
| Nav | Teal bar, logo left, white pill "Login" | Teal bar, logo centred, "Log in" outline + "Get started" peach | Ad LPs: logo only (no nav), see §11 |
| Footer | `#105151`, FCA / insurer legal | `#105151`, GPhC / Phlo legal | Per surface (§13) |
| Trackers | GTM `GTM-WSZ8SRM9` | GTM `GTM-PLLZX8TX`, Intercom `skj74892`, Cookiebot | Per surface (§12) |
| Font loading | `next/font` static 400/700/800 woff2 | Self-hosted variable TTF | `next/font/google` variable |

Where this document says "canonical", it means the Recommended column. Surface-specific overrides
are expressed with `data-surface="core"` or `data-surface="wellness"` on `<html>`.

---

## 3. Tokens (copy into `app/globals.css`)

Tailwind v4 `@theme` block plus CSS custom properties. Hex values are exact from the live sites.

```css
@import "tailwindcss";

@theme {
  /* Wipe Tailwind's default palette, fonts, radii and shadows so ONLY the tokens below
     generate utilities. Without this an agent can write bg-zinc-900 with no hex anywhere. */
  --color-*: initial;
  --font-*: initial;
  --radius-*: initial;
  --shadow-*: initial;

  /* ---- Colour: brand ---- */
  --color-teal:        #00818A;  /* brand-primary. Hero/sections/nav bg, link colour on light */
  --color-teal-deep:   #105151;  /* brand-primary-accent. Footer, message bar, text on peach, glow shadow */
  --color-teal-dark:   #4EAC98;  /* mint-dark. Band bg, drawer bg, input focus ring, icon fills */
  --color-teal-mid:    #3E9586;  /* tertiary-two-accent. Hover on teal-dark */
  --color-mint:        #A8DDA8;  /* brand-tertiary. Highlight word in headlines, testimonial band bg, flag bg */
  --color-mint-light:  #D3EED3;
  --color-mint-lighter:#A6D6CB;
  --color-mint-deep:   #82BA92;
  --color-logo-mint:   #46C9B5;  /* only in the wordmark "Me" */
  --color-peach:       #F1A88A;  /* PRIMARY CTA. Diagonal line motif */
  --color-peach-hover: #C4977F;
  --color-cream:       #FFFDF8;  /* page background */
  --color-info:        #E7EEEE;  /* soft grey-teal section bg (team section) */
  --color-flag:        #FFF8E2;  /* pale yellow pill bg (trust flags) */
  --color-flag-dark:   #E7E7D3;  /* darker flag tint (Figma palette sheet) */
  --color-line:        #CFDCDC;  /* card borders, dividers, toggle borders */
  --color-ink:         #1C2B2B;  /* headings and body on light */
  --color-ink-muted:   #5C5B5B;  /* secondary text */
  --color-placeholder: #8D8D8D;  /* input placeholder, light grey labels */
  --color-disabled-bg: #A0A1A1;
  --color-disabled-fg: #757575;
  --color-teal-tint:   rgb(33 194 186 / 0.10);  /* #21C2BA1A, tinted card bg */
  --color-star:        #FAC906;  /* review stars (core star-icon.svg) */
  --color-error:       #F28482;
  --color-white:       #FFFFFF;

  /* ---- Type ---- */
  --font-heading: "Plus Jakarta Sans", Arial, sans-serif;
  --font-body:    "Inter", Arial, sans-serif;

  --text-display: clamp(40px, 4vw + 24px, 64px);
  --text-display--line-height: 1.05;
  --text-h1: 42px;  --text-h1--line-height: 1.1;
  --text-h2: 32px;  --text-h2--line-height: 1.15;
  --text-h3: 28px;  --text-h3--line-height: 1.2;
  --text-h4: 22px;  --text-h4--line-height: 1.25;
  --text-sub: 16px; --text-sub--line-height: 1.2;   /* eyebrow / sub-head, PJS 700 */
  --text-lead: 18px; --text-lead--line-height: 1.5;
  --text-body: 16px; --text-body--line-height: 1.5;
  --text-small: 14px; --text-small--line-height: 1.45;
  --text-xs: 12px;  --text-xs--line-height: 1.4;
  --text-price: 48px; --text-price--line-height: 1;  /* "£89" */

  /* ---- Radius ---- */
  --radius-sm: 5px;
  --radius-md: 10px;   /* buttons, inner tiles, milestone number box, calculator wrapper */
  --radius-lg: 20px;   /* cards, flags/pills, image tiles */
  --radius-pill: 999px;

  /* ---- Shadow ---- */
  --shadow-glow: 0 4px 20px 0 rgb(16 81 81 / 0.75);   /* #105151BF, CTA hover / floating CTA */
  --shadow-glow-soft: 0 4px 20px 0 rgb(16 81 81 / 0.5); /* #10515180, dropdowns */
  --shadow-card: 0 0 0 1px rgb(0 0 0 / 0.10), 0 1px 3px rgb(0 0 0 / 0.10);

  /* ---- Spacing / layout ---- */
  --spacing-section: 96px;
  --spacing-section-mobile: 64px;
  --spacing-gutter: 24px;
  --container-standard: 1200px;
  --container-med: 1100px;
  --container-narrow: 800px;

  /* ---- Border ---- */
  --border-standard: 2px;
}

html { background: var(--color-cream); color: var(--color-ink); }
body { font-family: var(--font-body); font-size: var(--text-body); line-height: var(--text-body--line-height); }
h1,h2,h3,h4 { font-family: var(--font-heading); font-weight: 800; letter-spacing: 0; }
```

`data-surface` on `<html>` controls **only** the logo lockup, the legal footer block (§13) and the GTM
container (§12). It does not change tokens. The block above is the Recommended column of §2.

### 3.1 Alternative token sets (both surfaces, as-built)

If the brand owner picks a different column in §2, replace the matching lines in the `@theme` block
with the values below. Each block is complete for every row that differs in §2.

```css
/* ---- Core as-built (signpostme.com, MUI theme, rem = 10px converted to px) ---- */
--color-ink: #1C2B2B;
--text-display: clamp(40px, 15.38px + 7.692vw, 50px);  --text-display--line-height: 1;
--text-h1: 50px;  --text-h1--line-height: 1;
--text-h2: 30px;  --text-h2--line-height: 1;
--text-h3: 24px;  --text-h3--line-height: 1.25;
--text-h4: 16px;  --text-h4--line-height: 1;
--text-body: 18px;  --text-body--line-height: 1.2;
--text-small: 14px; --text-small--line-height: 1;
/* headings 800; button PJS 800 18px, padding 10px 20px, radius 10px, no border;
   card padding 30px 20px, radius 20px, border 2px #CFDCDC; section padding 60px (80px on bands);
   container MUI 12-col ~1200px; shadows: MUI elevation 1–4 plus 0 4px 20px #105151BF */

/* ---- Wellness as-built (Webflow :root, px) ---- */
--color-ink: #212121;                 /* headings and body */
--color-secondary: #0A1318;           /* outline button border/text, dividers at 20% */
--color-secondary-accent: #020E14;    /* eyebrow / sub-head text */
--color-tertiary-three: #FF66C4;      /* defined, not used visibly */
--text-display: 72px; /* 50px ≤ 991px */  --text-display--line-height: 1.1;
--text-h1: 42px;  --text-h1--line-height: 1.1;
--text-h2: 32px;  --text-h2--line-height: 1.1;
--text-h3: 28px;  --text-h3--line-height: 1.1;
--text-h4: 22px; /* 18px mobile */  --text-h4--line-height: 1.1;
--text-sub: 16px;
--text-lead: 20px; /* p-xl, 16px mobile */
--text-body: 14px;  --text-body--line-height: 1.4; /* 1.3 mobile */
--text-small: 12px;
--spacing-section: 120px; /* 80px tablet, 60px mobile */
--spacing-gutter: 60px;   /* 25px mobile */
--container-standard: 1300px; --container-med: 1100px; --container-narrow: 800px;
/* headings 700 (variable font); button PJS 700 16px, padding 12px 40px, radius 10px, border 2px same colour;
   radii 20 / 10 / 5; margins 5 10 20 40 60 120; paddings 20 40 60 80 */
```

Fonts in Next.js:

```ts
// app/fonts.ts
import { Plus_Jakarta_Sans, Inter } from "next/font/google";
// `variable` names deliberately do NOT match the @theme keys (--font-heading / --font-body).
// @theme is resolved at build time and must hold the literal family names above; next/font only
// needs to register the @font-face, which happens when these class names are on <html>.
export const pjs = Plus_Jakarta_Sans({ subsets: ["latin"], weight: ["700", "800"], variable: "--font-pjs", display: "swap" });
export const inter = Inter({ subsets: ["latin"], weight: ["400", "700"], variable: "--font-inter", display: "swap" });
// app/layout.tsx:  <html className={`${pjs.variable} ${inter.variable}`}>
```

### 3.2 Radius scale

| Token | Value | Use |
|---|---|---|
| `--radius-sm` | 5px | Nav active-link pill (core) |
| `--radius-md` | 10px | Buttons, inner tiles, milestone number box, calculator wrapper, inputs, FAQ items, nav active pill on Wellness |
| `--radius-lg` | 20px | Cards, flags/pills, hero image tiles, testimonial cards, slider track |
| `--radius-pill` | 999px | Reserved. The calculator unit toggle (radius 34 on a 34px track) is the only fully-rounded control; buttons are never pill-shaped |

Rule of thumb: interactive = 10px, container = 20px. There is no 0px and no 4px in the system.

### 3.3 Elevation and depth

| Level | Treatment | Use |
|---|---|---|
| Flat | No border, no shadow | Default for every section band and for cards sitting on cream |
| Border | 2px `--color-line` (`#CFDCDC`) | Cards on white, inputs, FAQ items, toggle groups, sticky bar top edge |
| Card | `--shadow-card` (1px ring at 10% + 1px/3px drop at 10%) | Defined in §3; no live use was observed. Reserve for a card that must float over a photo and cannot take a border |
| Glow soft | `--shadow-glow-soft` (0 4px 20px `#105151` at 50%) | Dropdown menus |
| Glow | `--shadow-glow` (0 4px 20px `#105151` at 75%) | Primary CTA hover and the floating / sticky CTA only |

Core as-built uses MUI elevation 1–4; do not replicate it (§16). Depth otherwise comes from
decoration, not shadow: the diagonal peach lines on teal panels, the diagonal white clip on the
core hero photograph, the 2×2 photo mosaic with 20px gutters, and the teal → cream band rhythm
itself. See §8 for the motif rules.

---

## 4. Colour usage rules

- **Page background is cream `#FFFDF8`, not white.** White `#FFFFFF` is for cards and inputs only.
- **Teal `#00818A` is the dominant brand colour.** Use it for hero backgrounds, section bands, nav bar, link colour on light, icons on white.
- **Peach `#F1A88A` is reserved for the primary CTA and the diagonal line motif.** Never use peach as a background panel or body text. Text on peach is always `#105151`.
- **Mint `#A8DDA8`** is the highlight: the second line of a hero headline on teal, the testimonial section background, "+" badges, bullet ticks on teal. On cream it is a background band only, never text (contrast fails).
- **Deep teal `#105151`** is for footer, announcement bar, text on peach, number boxes, glow shadows, and the thumb ring on the slider.
- **Teal-dark `#4EAC98`** is the "lighter teal" band (the app section on core, the mobile drawer), input focus rings, and icon fills.
- **Ink `#1C2B2B`** for all text on light backgrounds. White text on teal, teal-dark, deep teal.
- Section rhythm on both live sites alternates: teal → cream/white → mint or teal-dark → cream → teal → footer. Follow it.
- Contrast (WCAG 2.x, computed): ink on cream 14.5:1 · white on deep teal 9.1:1 · `#105151` on flag yellow 8.5:1 · `#5C5B5B` on cream 6.7:1 · white on teal 4.7:1 (AA normal text, just) · `#105151` on peach 4.6:1 (AA) · teal on cream 4.6:1 (AA) · **white on teal-dark `#4EAC98` 2.7:1 (fails normal text; headings ≥24px bold only)** · **mint on cream 1.5:1 (backgrounds only)**.

### 4.1 Colour catalogue by role

Every token in §3, grouped the way an agent chooses between them. Hex values are repeated here so
this section can be read on its own.

**Brand and action**

- **Teal** (`--color-teal` — `#00818A`): the dominant brand colour. Hero and section backgrounds, nav bar, link colour on light, icons on white, milestone boxes, selected toggle items.
- **Deep teal** (`--color-teal-deep` — `#105151`): footer, announcement bar, text on peach, number boxes, glow shadows, slider thumb ring, line icons on light.
- **Teal dark** (`--color-teal-dark` — `#4EAC98`): the lighter-teal band (core app section, mobile drawer), input focus ring, icon fills.
- **Teal mid** (`--color-teal-mid` — `#3E9586`): hover state on teal-dark only.
- **Peach** (`--color-peach` — `#F1A88A`): the primary CTA and the diagonal line motif. Never a background panel, never text.
- **Peach hover** (`--color-peach-hover` — `#C4977F`): hover fill and border on the primary CTA.
- **Logo mint** (`--color-logo-mint` — `#46C9B5`): the "Me" in the wordmark and the second headline line on the OG / ad tile. Never recolour it, never use it elsewhere.

**Highlight**

- **Mint** (`--color-mint` — `#A8DDA8`): second line of a hero headline on teal, testimonial band background, "+" badges, bullet ticks on teal, stat-badge number on photos. Background only on cream (1.5:1, fails as text).
- **Mint light / lighter / deep** (`#D3EED3` / `#A6D6CB` / `#82BA92`): sub-tints from the Figma palette sheet. Available for illustration and charts; no live component uses them.

**Surface**

- **Cream** (`--color-cream` — `#FFFDF8`): the page floor. Every page starts here.
- **White** (`--color-white` — `#FFFFFF`): cards, inputs, calculator wrapper, inverse buttons, sticky bar. Never the page background.
- **Info** (`--color-info` — `#E7EEEE`): soft grey-teal section band (team section).
- **Flag** (`--color-flag` — `#FFF8E2`): pale yellow trust-pill fill.
- **Flag dark** (`--color-flag-dark` — `#E7E7D3`): darker flag tint from the palette sheet; no live use yet.
- **Teal tint** (`--color-teal-tint` — `rgb(33 194 186 / 0.10)`): tinted card background for the three S-factor cards.

**Hairline**

- **Line** (`--color-line` — `#CFDCDC`): the only divider colour. 2px card borders, input borders, toggle borders, sticky bar top edge, FAQ item borders. Wellness as-built also draws dividers at 20% of `#0A1318`; that value is dropped.

**Text**

- **Ink** (`--color-ink` — `#1C2B2B`): headings and body on every light surface (14.5:1 on cream).
- **Ink muted** (`--color-ink-muted` — `#5C5B5B`): secondary text, captions, legal (6.7:1 on cream).
- **Placeholder** (`--color-placeholder` — `#8D8D8D`): input placeholders and light grey labels only.
- **On teal** (`#FFFFFF`): white text on teal, teal-dark and deep teal. On teal-dark it passes only at ≥24px bold (2.7:1).
- **On peach** (`#105151`): the only permitted text colour on the peach CTA (4.6:1).

**Semantic and state**

- **Error** (`--color-error` — `#F28482`): input error border and helper text.
- **Star** (`--color-star` — `#FAC906`): review stars, 16px.
- **Disabled** (`--color-disabled-bg` `#A0A1A1` on `--color-disabled-fg` `#757575`): disabled buttons, no border.

Dropped values, kept here so nobody re-adds them: Wellness pink `#FF66C4` (defined, unused), Wellness neutral dark `#212121` and secondaries `#0A1318` / `#020E14` (superseded by ink and deep teal, see §2 and §15).

---

## 5. Typography

| Role | Family | Size | Weight | Line | Colour | Notes |
|---|---|---|---|---|---|---|
| Display (hero H1) | Plus Jakarta Sans | `--text-display` | 800 | 1.05 | white on teal | Two-line pattern: line 1 white, line 2 mint. Full stop at end of each line on core ("Private healthcare. / Get answers…") |
| H1 (LP hero) | PJS | 42 | 800 | 1.1 | white | LP uses H1 + H2 price + H3 strap stacked |
| H2 section | PJS | 32 | 800 | 1.15 | ink / white | |
| H3 | PJS | 28 | 800 | 1.2 | | Sub-section, FAQ heading, "SignpostMe Wellness team" |
| H4 card title | PJS | 22 | 800 | 1.25 | | 18px on mobile |
| Eyebrow / sub-head | PJS | 16 | 700 | 1.2 | ink or teal (Webflow uses `#020E14`) | "How it works", "SignpostMe Wellness". Sentence case, not uppercase |
| Lead | Inter | 18 | 400 | 1.5 | | Hero strap, section intro |
| Body | Inter | 16 | 400 | 1.5 | | |
| Body bold | Inter | 16 | 700 | 1.5 | | Used for emphasis inside paragraphs, e.g. "**could** lose" |
| Small | Inter | 14 | 400 | 1.45 | | Trust pills, card body on LP, footer links |
| Caption / legal | Inter | 12 | 400 | 1.4 | `#5C5B5B` | Disclaimers, "*times may vary…" |
| Price | PJS | 48 | 800 | 1 | | "£89" with 14px "Prices from" above and "Free Tracked 24 delivery" below |
| Button | PJS | 16 | 800 | 1 | | Core uses 18px; LP uses 16px |
| Milestone number | PJS | 20 | 700 | 1 | white on teal | 60×60 box, radius 10 |

Rules: headings are sentence case. No uppercase tracking anywhere except the small MUI "contained small" button on core (14px uppercase, avoid on LPs). No italics. Max measure 80ch (`.max-ch-80` on Webflow).

### 5.1 Font families and substitutes

Two families carry the whole system and the split is strict: **Plus Jakarta Sans** at 800 (700 for
eyebrows, milestone numbers and the Wellness as-built headings) for every heading, price, button and
number; **Inter** at 400/700 for lead, body, small, caption and legal. Fallback stack is `Arial,
sans-serif` for both. Neither family is used at a weight below 400 and nothing is set in italic.

Both are available from Google Fonts, so no substitute is needed. Load them with `next/font/google`
as shown in §3 (static 700/800 and 400/700 subsets), not with a `<link>` tag. Wellness as-built
self-hosts a variable TTF of Plus Jakarta Sans; core as-built uses `next/font` static woff2. Do not
mix the two loading strategies on one site.

---

## 6. Layout and spacing

- Container: 1200px standard, 1100 for text-heavy sections, 800 for FAQ / single-column copy. Gutter 24px mobile, 60px desktop on Wellness.
- Section vertical padding: 96px desktop, 64px mobile. Hero: 60px top/bottom with the image bleeding to the edge on desktop.
- Grid: 12-col. Common splits: hero 6/6 (text left, image right); "how it works" 5/7 (text left, image right) then a 3-up step row; feature cards 3-up; benefits 2-up on core.
- Vertical rhythm inside a section: eyebrow → 10px → H2 → 20px → lead → 40px → content → 40px → CTA.
- Margin scale (Webflow): 5, 10, 20, 40, 60, 120. Padding scale: 20, 40, 60, 80.
- Mobile breakpoint 640px for button full-width; 991px for nav collapse (Webflow default).
- Image tiles on hero: 2×2 mosaic of rounded (20px) photos/renders, one tile is a teal stat card ("Lose up to **50%** more weight than dieting alone").

### 6.1 Spacing scale

- **Margin ladder** (Webflow, `--spacing-*` in the front matter): 5 · 10 · 20 · 40 · 60 · 120px. Use these for gaps between elements.
- **Padding ladder** (Webflow): 20 · 40 · 60 · 80px. Use these inside bands and wrappers.
- **In-section rhythm:** eyebrow → 10px → H2 → 20px → lead → 40px → content → 40px → CTA.
- **Card padding:** 32px desktop, 24px mobile. Pills 4px 14px. Buttons 12px 32px. FAQ items 16px 20px. Calculator wrapper 20px.
- **Section padding:** 96px desktop, 64px mobile. Hero 60px top and bottom.
- **Gutter:** 24px mobile; Wellness uses 60px on desktop, 25px mobile.
- **Containers:** 1200 standard · 1100 medium (text-heavy) · 800 narrow (FAQ, single column).

### 6.2 Breakpoints

Only two breakpoints are observed on the live sites; the display size scales continuously through its `clamp()`.

| Name | Width | Key changes |
|---|---|---|
| Mobile | ≤ 640px | Buttons and both hero CTAs full width. Hero stacks text above the image mosaic. Cards stack 1-up and drop to 24px padding. Calculator becomes single column. Milestone row stacks vertically, connecting line hidden. H4 22→18px; Wellness lead 20→16px. Section padding 96→64px. Trust marquee stays horizontal. Sticky CTA bar persists. |
| Tablet | 641–991px | Desktop tokens apply. Nav collapses to the mobile drawer at 991px (Webflow default). Wellness as-built uses 80px section padding and 50px display here; canonical keeps 96px and the clamp. |
| Desktop | ≥ 992px | Full nav. Sticky CTA bar hidden while the hero CTA is visible. Hero image bleeds to the viewport edge. |
| Wide | ≥ 1200px | Content caps at the 1200px container; teal bands and the hero image stay full-bleed. |

The reference mobile viewport is 390px; screenshots in the PR checklist (§17) are taken at 390px and 1440px.

### 6.3 Touch targets and collapsing

Sizes below are computed from the recipes in §7, not measured separately.

| Control | Size | Status |
|---|---|---|
| Button (16px / line-height 1 + 12px padding + 2px border) | 44px tall | Meets 44px |
| Input | 48px tall | Meets |
| FAQ item (16px text + 16px padding) | 48px tall | Meets |
| Milestone number box | 60×60px | Not interactive |
| Card icon box | 40×40px | Not interactive |
| Calculator slider thumb | 40px | Under 44px; the 40px-tall track supplies the hit area |
| Calculator unit toggle | 60×34px | Under 44px tall; pad the label row so the hit area reaches 44px |
| Trust pill | ~20px tall | Not interactive; never make a pill a link without enlarging it |

Collapsing order: nav → drawer at 991px; hero 6/6 → stacked; "how it works" 5/7 → stacked; 3-up card rows → 1-up (no 2-up intermediate observed); 3×2 feature grid → 1-up; footer 3–4 columns → stacked groups; marquee never wraps. Images keep their 20px radius at every width.

---

## 7. Component recipes

### 7.1 Button

```
Primary   bg #F1A88A  text #105151  border 2px #F1A88A  hover bg+border #C4977F
Inverse   bg #FFFFFF  text #00818A  border 2px #FFFFFF  (on teal panels)
Outline   bg transparent text #00818A border 2px #00818A  (on light)
Outline-inverse  bg transparent text #FFF border 2px #FFF (on teal)
Disabled  bg var(--color-disabled-bg) text var(--color-disabled-fg) no border
Shape     radius 10px · padding 12px 32px · PJS 800 16px · line-height 1 · inline-flex gap 10px
Motion    transition color, border-color, background-color 250ms ease-out
Glow      optional box-shadow var(--shadow-glow) on floating / sticky CTA only
Mobile    full width below 640px
Icon      optional 16px arrow-right (white or #105151) trailing
```

Labels used live: "Get started", "Get your quote", "Start free consultation", "From £89 – Get started",
"Explore weight loss", "Learn more & get your quote", "Explore the app", "Log in", "Sign up".

### 7.2 Pill / flag (trust badge, stat chip)

```
bg #FFF8E2  text #105151  border 1px #FFF8E2  radius 20px  padding 4px 14px  Inter 12px
icon 14px line icon in #105151 leading
variants: [primary]  bg #A8DDA8 text #00818A ; [outline] border 1px var(--color-ink) (Webflow uses #0A1318) ; on-teal: bg rgba(255,255,255,.9)
```
Used for: "1.6 million people are on the NHS diagnostic waiting list right now", "Connect with a doctor in minutes*", and the trust strip items.

### 7.3 Trust marquee

Five pills repeated 3× in a flex row, `animation: upsellSlide 50s linear infinite` translating
`-100%`. Items in order: Confidential & discreet · Free Royal Mail Tracked 24 delivery ·
Qualified & experienced team · UK sourced medications · Fully compliant with UK regulations.
Icons: package, delivery, award, medication, shield-tick (line style, `#105151`). Sits directly under
the hero on a cream strip. Reduce-motion: static wrap.

### 7.4 Card

```
bg #FFFFFF  border 2px #CFDCDC  radius 20px  padding 32px (24px mobile)
title H4 22/800  body 14–16px  optional 40×40 icon box (bg #00818A radius 10, white icon) top-left
variant "tint": bg var(--color-teal-tint) no border — used for the three S-factor cards
variant "mint": bg #A8DDA8 text #1C2B2B — core "One policy" panel uses white cards on mint band
```

### 7.5 Milestone / step

Row of three. Each: 60×60 box bg `#00818A` radius 10 containing PJS 700 20px white number; a 2px
teal line connects boxes at 50% height; below: H4 title + 14px body, centred. Mobile: stack vertically, line hidden.

### 7.6 Weight-loss calculator (Wellness only)

Wrapper: bg white (`#FFF9` on teal section) radius 10 padding 20px, inside a teal section.
Header row: H4 "What our weight loss service could do for you" left, unit toggle right.
Toggle: 60×34 track teal, 26px white knob, radius 34, labels "kgs" / "lbs".
Slider: track 40px tall radius 20 bg `#00818A`, filled portion `#105151`, thumb 40px white with 6px `#105151` border.
Readouts: "Select your current weight:" → value PJS 800 24px + unit 12px; "Weight you could lose:" → value PJS 800 32px.
Logic (from live JS): default 144 kg; min 63 kg / 140 lb, max 226 kg / 500 lb; loss = value × 0.20 rounded.
Disclaimer 12px below, with "research" linking to NEJM `10.1056/NEJMoa2206038`.

### 7.7 Announcement / message bar

Full-width bg `#105151`, white 14px centred text, 10px 20px padding, bold for the key phrase. Sits above the nav. Must have an expiry date in the CMS; two live bars are stale (May dispatch notice, June heatwave).

### 7.8 Navbar

- Core: bg `#00818A`, 70px, logo left (white wordmark 150px), centre links white 14px with the active one as a white pill (`#FFF` bg, teal text, radius 5, padding 5px), right "Login" white pill button.
- Wellness home: same bar, logo lockup centred, left dropdown "Weight loss service", right "Log in" outline-inverse + "Get started" primary.
- **Ad LP: logo lockup only, centred, no links, no login.** Exits live in the footer only.

### 7.9 Hero

Teal `#00818A` panel with the diagonal peach line motif (SVG, 2–3px `#F1A88A` lines at ~35°). Left: optional pills, display headline (white + mint line), 18px white lead, CTA row (primary + outline-inverse), tick list (14px white, mint tick icon). Right: photo or 2×2 tile mosaic, radius 20. On core the hero image is a full-bleed photo clipped by a diagonal white edge.

### 7.10 Sticky bottom CTA bar (LP only)

Fixed bottom, bg white, top border 2px `#CFDCDC`, 16px padding. Left: "Weight loss service" 14px bold. Middle: "Prices from" 12px over "£89" PJS 800 24px. Right: primary button "Get started". Appears after the hero scrolls out. Hidden on desktop ≥991px if the hero CTA is visible.

### 7.11 Testimonial card

bg white radius 20 padding 32 on a mint `#A8DDA8` band. 5 gold stars (`var(--color-star)`, `#FAC906`) 16px, quote 16px Inter, attribution 12px bold name + "Product: …" line.

### 7.12 FAQ accordion

Narrow container 800. Each item: white bg, border 2px `#CFDCDC`, radius 10, padding 16px 20px, question 16px 700, chevron right, answer 14px. One open at a time.

### 7.13 Feature icon grid (core "Why choose")

3×2 grid of white cards (radius 20, border 2px `#CFDCDC`), 32px teal line icon, H4 title, 14px body. Section title H2 in a left column, grid in the right 8 columns.

### 7.14 Inputs

Height 48px, radius 10, border 2px `#CFDCDC`, focus border `#4EAC98`, error border and helper `#F28482`, placeholder `var(--color-placeholder)`, bg white. Toggle-button groups: 2px `#CFDCDC` border, selected item bg teal text white.

### 7.15 Footer

See §13 for legal content. Structure: bg `#105151`, white text, 40px top padding, 3–4 columns (brand/about, useful info, newsletter, badges), 12px legal paragraph, social icons 24px white (Facebook, Instagram, LinkedIn, email). Wellness adds a teal `#00818A` CTA band above the footer: "SignpostMe Wellness is ready when you are" + primary button.

### 7.16 Stat badge (from Figma `WL-50-graphic` and siblings, §15)

```
Tile      800×800 white, radius 20 when placed in a mosaic, two peach #F1A88A 3px diagonal lines
          top-left and bottom-right
Stack     centred: "Lose up to" PJS 700 ~44px #105151
          "▾50%" PJS 800 ~190px #00818A with a small down-triangle
          "more weight / than dieting alone" PJS 700 ~56px #105151
On-photo  number in mint #A8DDA8, text white, no lines, no tile (overlaid bottom-left of the hero portrait)
Variants  50% (vs dieting alone) · 20% (bodyweight in a year) · 20.7% Wegovy · 20% Mounjaro · 17% Wegovy tablets
```

Reuse these exact figures and their drug attribution; do not create new percentages (§10 claim rules).
Scale the three text sizes proportionally when the tile is rendered smaller than 800px.

---

## 8. Imagery and iconography

- **Photography:** real people, natural light, warm tones, smiling, over-shoulder or portrait. Clinician shots (scrubs, headset) on core; patient + superintendent pharmacist on Wellness. Always on a teal or cream panel with 20px radius.
- **Product renders:** teal "Helping you to a healthier life" box, phone mock with the "This month's progress ↓8lbs" screen. Reuse the existing assets below rather than regenerating.
- **Motif:** diagonal peach lines on teal (`hero-bg-pattern.png`, `background-pattern.svg`), white diagonal lines on the app section. Never more than two lines per panel.
- **Icons:** 1.5px line icons, `#105151` on light / white on teal, 24–32px. Tick-in-circle for lists, shield-tick for compliance, package for discretion.
- **Logos:** white wordmark on teal; `#105151` wordmark on light. Lockup for Wellness includes "powered by phlo" with the Phlo mark. Minimum width 120px. Never recolour the "Me" accent.

Existing assets (hot-linkable during build, copy into `/public/brand` for production):

```
Core (https://signpostme.com/…)
  /images/signpost-me-white-logo.svg              white wordmark, "Me" #46C9B5
  /images/header/signpost-me-wellness.svg         dark lockup #105151
  /images/header/signpost-me-health-cover.svg     dark lockup #105151
  /images/icons/app-icon.svg                      #4EAC98
  /images/icons/star-icon.svg                     #FAC906
  /images/hero/nextImageExportOptimizer/home-hero-opt-3840.WEBP
  /images/nextImageExportOptimizer/cyber-essentials-plus-opt-1200.WEBP
Wellness (cdn.prod.website-files.com/6a673e1243c0c77fda884e2b/…)
  6a688ee2011dd90b7a2d24f3_SP_phlo-inverse.png                 white lockup + powered by phlo
  6a673e1243c0c77fda884ea8_powered-by-phlo.avif
  6abe25334bbdfaab108bd4cf_…_LP-home-hero-1x1.avif             LP hero mosaic
  6a68929ba2ab6bea78b89bf0_…_home-hero-4x3.avif
  6a69fcc9538df362f9029804_…_home-WL-4x3.avif
  6a688d15adff9322b7908447_…_SPMW-phlo-1x1.avif                 logo tile with peach lines
  6a673e1243c0c77fda884e8b_…_clinical-team.jpg                  Dennis Ouko portrait
  6a68963bf7fb138de2d20a46_…_order-delivered.avif
  6abe2cceb56a1828fd76e085_SignpostMe Wellness Product Showcase.avif
  6a673e1243c0c77fda884e8a_background-pattern.svg
  6a687fbd1dd1296529279baa_…_hero-bg-pattern.png / 6a6881aaaf20f82e2ef8facb_hero-bg-pattern-mob.avif
Icons (…/6a673e1243c0c77fda884e2e/)
  6a687b6662fd8b0974d3aa3a_package.svg · 6a687cd168eaddd21d4c5be6_delivery.svg
  6a686f3acc70dc8a92d49400_icon_award.svg · 6a687bc1c77b1fc087bc1857_medication.svg
  6a687e10ccc186dba8859824_sheild-tick.svg
```

---

## 9. Motion

- Buttons: 250ms ease-out colour transitions only. No scale.
- Marquee: 50s linear infinite (trust strip), 80s (review strip). Pause on hover, static under `prefers-reduced-motion`.
- Reveal: fade-up 20px, 400ms, once, on section entry. Keep it subtle; the live sites barely animate.
- Accordion: 200ms height.
- Toggle knob: 400ms.
- No parallax, no hero video, no autoplay carousels except the marquee.

---

## 10. Voice and copy

Patterns observed:

- Headline = promise, sub-line = mechanism. "Private healthcare. / Get answers and treatment in days, not months."
- Numbers up front: "Prices from £89", "From £12.50 per month*", "Lose up to 50% more weight than dieting alone", "1.6 million people are on the NHS diagnostic waiting list".
- Triads everywhere: 3 steps, 3 cards, 3 S-factors (Suitability, Safety, Success).
- Reassurance phrases: "Your health comes first", "Your healthcare journey is in safe hands", "Human", "We handle the billing and admin."
- Asterisks for claims, footnoted in 12px.
- British spelling (centre, programme). Sentence case headings. Ampersand allowed in short labels ("Safe & supported").

Claim rules (Wellness, GLP-1):

- Weight-loss claims must cite a source. Live sites cite NEJM 2022 `10.1056/NEJMoa2206038` ("Tirzepatide Once Weekly for the Treatment of Obesity", SURMOUNT-1, 72 weeks) for "up to 20% over a 72-week period". That is a tirzepatide (Mounjaro) figure; do not attach it to Wegovy copy. "Up to 50% more weight than dieting alone" is used in hero copy without a citation on the live site; do not escalate it and ask Phlo for its source before reuse.
- Never name a medication in ad-landing hero copy without the prescribing caveat. "If approved" precedes any delivery promise.
- Always show: GPhC registration, superintendent pharmacist name, Phlo Technologies Ltd, UK address.
- No before/after photos, no personal testimonials about medication on paid LPs (ASA/MHRA risk).

Known copy defects on live sites to not replicate: "Qualified & experience team" → "experienced"; "Lose up 50%" → "Lose up to 50%"; "review your information assess" → "review your information to assess"; "MRPharmSGPhC" → "MRPharmS, GPhC"; "Cignpost Wellness" alt text; "Help center" → "Help centre"; stale dated announcement bars; empty alt text on hero images; LP `<title>` is "lp-wls".

---

## 11. Ad landing page anatomy (Wellness)

Section order from the live `lp-wls`, keep it:

1. Announcement bar (optional, dated)
2. Nav: logo lockup only
3. Hero: H1 "Weight loss service" · H2 "Prices from £89" · H3 strap · primary CTA "From £89 – Get started" · outline-inverse "Explore weight loss" · 3 tick proofs · 2×2 image mosaic
4. Trust marquee
5. Three benefit cards (Proven results that last / Safe & supported / Breakthrough treatments)
6. Teal band: H2 "Lose up to 20% of your bodyweight" + calculator
7. Teal CTA strip: H3 "Weight loss that actually works" + inverse button "Start free consultation"
8. "What you can expect": three phase cards with tick lists and note
9. Price block: "Prices from / £89 / Free Tracked 24 delivery" + primary CTA
10. Three S-factors (icon, H4, body) + "powered by phlo" badge
11. Team section on `#E7EEEE`: H3, "Fully GPhC certified" pill, bold lead, portrait with name card
12. How it works: eyebrow, H2 "Get started in 3 easy steps", image with "Order received / approved / delivered" chips, milestone row
13. Final CTA: eyebrow "SignpostMe Wellness", H2, primary CTA, product showcase image
14. Footer (§13)
15. Sticky bottom CTA bar

Variant guidance for per-ad pages: swap hero headline, strap, proof ticks, mosaic tile and price only. Keep sections 4, 10, 11, 14 fixed for compliance. Route pattern `/lp/[slug]` with slug = campaign name.

---

## 12. CTA and tracking contract

```
Primary CTA href (Wellness):
  https://app.wellnessbyphlo.signpostme.com/getting-started?condition=weight-loss&variant=starter
Login:
  https://app.wellnessbyphlo.signpostme.com/sign-in
Secondary:
  /weight-loss-service   (learn more)
Core quote CTA:
  https://quote.signpostme.com   (plus business.signpostme.com for employer plans)
```

- Preserve `condition` and `variant` query params exactly. New variants are added by the app team, not the LP.
- Forward `utm_source, utm_medium, utm_campaign, utm_content, utm_term, fbclid, gclid, ttclid` from the LP URL onto the CTA href (append, do not overwrite).
- GTM containers: `GTM-WSZ8SRM9` (core), `GTM-PLLZX8TX` (Wellness). Wellness also loads Intercom app `skj74892` and Cookiebot. Load Meta Pixel and TikTok pixel through GTM, never inline, and only after Cookiebot consent.
- Fire a `cta_click` dataLayer event with `{ surface, slug, position: hero|band|price|final|sticky }` on every CTA.
- LP pages: `robots: noindex, follow`. `<title>` must be human: "Weight loss service from £89 | SignpostMe Wellness".

---

## 13. Legal and regulatory blocks (per surface; core is verbatim, Wellness is corrected)

**Core footer**

> Copyright ©2026 Regent Financial Network Limited. All Rights Reserved. SignpostMe is a trading style of Regent Financial Network Limited which is authorised and regulated by the Financial Conduct Authority (FCA), registration number: 304130 and is registered in England and Wales, company number: 04296341. Registered office: The Stables, Peper Harow, Godalming, England, GU8 6BQ. Email: support@signpostme.com.
>
> SignpostMe Health Insurance is underwritten by Capacity Insights, a trading name of Healix Insurance Services Limited, on behalf of Hamilton Insurance DAC, the Insurer. Hamilton Insurance DAC is a designated activity company authorised by the Central Bank of Ireland and subject to limited regulation by the Financial Conduct Authority and the Prudential Regulation Authority in connection with its UK branch.

Links: About SignpostMe · Contact us · News · Terms of use · Privacy Policy · Cookie Policy · Modern Slavery Act · Accessibility information · Fair Value Assessment. Badge: Cyber Essentials Plus.

**Wellness footer** (live text reads "MRPharmSGPhC" run together; corrected below. Confirm the registrant line with Phlo before shipping.)

> **SignpostMe Wellness** — Unit 1 Fifth Street, Trafford Park, Manchester, England, M17 1JX
> **Dennis Ouko** MRPharmS, GPhC Registration Number: 2205052 — Phlo Technologies Ltd

Links: Weight loss service · Log in · Contact us · FAQs · Privacy Policy · Terms & Conditions. Contact: wellnessbyphlo@signpostme.com · 020 3475 3231.

---

## 14. Accessibility

- All text ≥ 4.5:1 except large (≥24px bold) at 3:1. Mint is never text on cream.
- Every image has alt text; decorative pattern images use `alt=""`.
- Focus ring: 2px `#4EAC98` offset 2px on light, 2px white on teal.
- Sticky bar must not cover focused elements; add `scroll-padding-bottom: 96px`.
- Marquee and reveal animations respect `prefers-reduced-motion`.
- Accordion uses `<button aria-expanded>` and `<div role="region">`.

---

## 15. Figma (ingested 2026-10-22)

File `GfxumnaSlcB9jzZ1NMRt89` "Cignpost", owned by Phlo (Chris Taylor), last edited 2026-10-01.
"Cignpost" was the clinic's name before the SignpostMe rebrand; the file has a divider page
"EVERYTHING BELOW IS SIGNPOST REBRAND". No published styles and no readable variables (the token
lacks `file_variables:read`), so tokens were read from the rendered palette sheet.

**Page map**

| Page | What it holds | Use for LPs |
|---|---|---|
| `↳ Logos` (452:2997) | SP_mark, SP_phlo1/2/3 lockups, SP_phlo-inverse, SPW-phlo, favicons 32–512, `SPMW-opengraph` 1200×630 | Yes: export logos and OG from here |
| `↳ Website updates` (473:85) | Source art for every Webflow image: `LP-home-hero-1x1`, `home-hero-4x3`, `home-WL-4x3`, `image-1x1/4x3`, `hero-bg-pattern(-mob/-white)`, `SPMW-phlo-1x1`, `dennis-ouko`, `order-delivered`, stat graphics `WL-20/50/20-weg/20-mounj/17-wg-tab`, `foundayo-switch`, `weg-tab-packs-1x1`, pen-box/box packaging, 5 trust icons at 128px, the palette sheet (489:522), screenshots of the core site | Yes: this is the asset library |
| `PIPP Design System` (54:96) | Phlo's patient-app design system: League Spartan headings, Inter body, mint `#25C2BA` / cerulean `#1A719B` / pink `#FF66C4` primitives, "Cignpost Colour Theme V3", theme contrast tests, "Starter Flow Test – Cignpost" | **No.** This styles `app.wellnessbyphlo.signpostme.com`, not the marketing site. Know it exists so the LP → app handoff doesn't surprise you |
| `Partner assets` | Dose instructions, partner co-brand, icons | Reference only |
| `Webflow Site` | Morrisons Clinic section, `wegovy-24-info-hero-grid` | Reference only |
| `_Old`, `Figma designs` | Pre-rebrand Cignpost Clinic work | No |

**Palette sheet (489:522) — matches §3 exactly.** Confirmed hexes with the sheet's own sub-tints:
`#00818A` · `#A8DDA8` (`#82BA92`, `#D3EED3`) · `#4EAC98` (`#3E9586`, `#A6D6CB`) · `#105151` ·
`#F1A88A` (`#C4977F`) · `#FFF8E2` (`#E7E7D3`, `#FFFDF8`) · `#46C9B5` · `#1C2B2B` · `#FFFFFF`
(`#CFDCDC`, `#E7EEEE`). One addition to §3: `--color-flag-dark: #E7E7D3` (darker flag tint).
**This resolves the §2 "dark text" row: `#1C2B2B` is the brand value; `#212121` was a Webflow default.**
The Webflow var `#FF66C4` (pink) is a leftover from the app design system and is not in the web palette.

**Stat badge component (from `WL-50-graphic` etc.), specified in §7.16**
800×800 white tile with two peach `#F1A88A` 3px diagonal lines in the top-left and bottom-right.
Centred stack: "Lose up to" PJS 700 ~44px `#105151` · "▾50%" PJS 800 ~190px `#00818A` with a small
down-triangle · "more weight / than dieting alone" PJS 700 ~56px `#105151`. On-photo variant
(`LP-home-hero-1x1`): number in mint `#A8DDA8`, text white, no lines. Variants exist for 20% (bodyweight
in a year), 20.7% Wegovy, 20% Mounjaro, 17% Wegovy tablets. Reuse these exact figures and their drug
attribution; do not create new percentages.

**OpenGraph / ad-tile style (`SPMW-opengraph`)**
Background `#105151`, two peach line fragments top-left and bottom-right, headline PJS 800 white with the
second line in `#46C9B5`, SignpostMe Wellness lockup centred below. This is the Meta ad-tile look;
LP heroes use `#00818A` (lighter) so the ad → LP transition darkens-to-lightens, which is intended.

**Hero mosaic (`LP-home-hero-1x1`, 1000×1000)**
Left column 490×1000 portrait with the stat badge overlaid bottom-left; right column two 490×490 tiles
(phone mock "This month's progress ↓8lbs", teal box render). 20px gutters, 20px radius on every tile.

**Export plan**: render each frame in `↳ Website updates` and `↳ Logos` at 2× via the Figma API into
`/public/brand/` and `/public/lp/`, keeping Figma frame names as filenames.

---

## 16. Do / Don't

Do
- Start every page with `data-surface` set.
- Use cream page background and white cards.
- Put the price in the hero and again in a dedicated price block.
- Keep one primary CTA colour per page (peach).
- Use the diagonal peach line motif on at least one teal panel.
- Write headings in sentence case with a full stop only in the display two-liner.

Don't
- Don't use pink `#FF66C4`, it exists in the Webflow vars but is not used anywhere visible.
- Don't put mint text on cream or white.
- Don't use MUI elevation shadows; use `--shadow-glow` or none.
- Don't add nav links or a login button to an ad LP.
- Don't use rounded-full buttons; the brand radius is 10px.
- Don't ship a dated announcement bar without an expiry.
- Don't change `condition`/`variant` params or strip UTMs.

---

## 17. Pre-PR checklist

- [ ] Tokens from §3 only; `grep -E '#[0-9a-f]{6}' app components` returns nothing outside `globals.css`.
- [ ] Fonts loaded via `next/font`, no Google Fonts `<link>`.
- [ ] Lighthouse mobile ≥ 95 performance, 100 accessibility, LCP < 2.0s on the LP.
- [ ] All CTAs match §12 and forward UTMs (test with `?utm_source=test&fbclid=1`).
- [ ] Legal block for the declared surface is present verbatim (§13).
- [ ] `<title>`, meta description, OG image set; `noindex` on `/lp/*`.
- [ ] Screenshot at 390px and 1440px attached to the PR and compared with `lp-wls`.
- [ ] Copy passes the claim rules in §10; no defects from the "known defects" list.

---

## 18. Iteration guide

1. Work on one component at a time. Reference tokens by their `--color-*` / `--text-*` / `--radius-*` name in prose and by `{colors.x}` / `{typography.x}` / `{rounded.x}` in the front matter.
2. Section numbers §1–§17 are frozen: `BUILD-GUIDE.md` and `PIPELINE.md` cite them. Add new material as a subsection (`7.17`, `6.4`) or as a new top-level section after §19. Never insert a section before §17.
3. A new colour, size or radius goes into the `@theme` block in §3 first, then into the front matter, then into the component that needs it. The §3 block is the build source; the front matter mirrors it.
4. A new component or variant gets a numbered recipe in §7 **and** a matching entry in the front matter `components:` block, named the same. Variants are separate entries (`button-primary-hover`, `card-tint`), not nested states.
5. Only Default and Hover are documented for buttons; Focus is the ring in §14. Do not invent Active/Pressed or Loading states without observing them on the live sites.
6. New CTAs default to `--radius-md` (10px), peach fill, `#105151` text, 2px matching border. New cards default to white, 2px `--color-line`, `--radius-lg` (20px), 32px padding.
7. Surface differences (logo lockup, legal block, GTM container) are expressed only through `data-surface`; never fork tokens per surface.
8. When a §2 row is confirmed by the brand owner, update the Recommended column, the §3 block, the front matter, and remove the matching line from §19.

---

## 19. Known gaps

- **§2 Recommended column is unconfirmed** by the brand owner except the dark-text row, which the Figma palette sheet settled at `#1C2B2B`. Every other "Recommended" choice (16px root, 800 headings, 12px 32px button padding, 96/64 section padding) is the author's reconciliation of two as-built sites.
- **"Up to 50% more weight than dieting alone" has no citation** on the live site. Ask Phlo for the source before reusing it; do not escalate it.
- **Wellness footer registrant line** reads "MRPharmSGPhC" live; the corrected form in §13 needs confirming with Phlo before shipping.
- **Figma variables are unreadable** (token lacks `file_variables:read`) and the file publishes no styles. Tokens were read from the rendered palette sheet (489:522); if a variable set is later exposed, diff it against §3.
- **`--shadow-card` has no observed use.** It is defined because core as-built ships MUI elevation; treat it as a placeholder until a component needs it.
- **Reveal animation (§9) is a recommendation**, not an observation. The live sites only animate the marquees, the accordion and the toggle knob.
- **Button Active/Pressed and Loading states** are not observed on either site. Only Default and Hover are specified.
- **Hover states for cards, FAQ items and nav links** are not documented; the live sites show none beyond cursor changes.
- **In-app surfaces** (`app.wellnessbyphlo.signpostme.com`, `quote.signpostme.com`, `business.signpostme.com`) are out of scope; they use the PIPP design system (§15), not this file.
- **Tablet** (641–991px) has no bespoke layout on either site; the breakpoint table in §6.2 inherits desktop tokens there by inference.
- **Pink `#FF66C4`** exists in the Webflow variables and the PIPP system but nowhere visible on the marketing surfaces; it is deliberately excluded.
- **Stale announcement bars** on the live Wellness site (May dispatch notice, June heatwave) are content defects, not design; §7.7 requires an expiry date to prevent recurrence.
