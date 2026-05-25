---
version: alpha
name: BATLLE
description: Editorial haute couture — Schiaparelli-inspired, strictly black and white with atelier cream and chalk-red accents. Asymmetric grids, sculptural typography, Playfair Display × Work Sans.
colors:
  pure-white: "#FFFFFF"
  pure-black: "#000000"
  anthracite: "#1A1A1A"
  atelier-cream: "#F5F2ED"
  chalk-red: "#C41E3A"
typography:
  display-hero:
    fontFamily: Playfair Display
    fontSize: clamp(4.5rem, 12vw, 10.5rem)
    lineHeight: 0.95
    letterSpacing: "0.04em"
    fontVariation: "'wght' 400"
  h1-editorial:
    fontFamily: Playfair Display
    fontSize: clamp(3rem, 6vw, 5rem)
    lineHeight: 0.95
  h2-section:
    fontFamily: Playfair Display
    fontSize: clamp(2.8rem, 5vw, 4.5rem)
    lineHeight: 1
  h3-project:
    fontFamily: Playfair Display
    fontSize: clamp(2rem, 3vw, 2.8rem)
    lineHeight: 1.15
  body-lg:
    fontFamily: Work Sans
    fontSize: 1.5rem
    lineHeight: 1.7
  body-md:
    fontFamily: Work Sans
    fontSize: 1.4rem
    lineHeight: 1.6
  label-caps:
    fontFamily: Work Sans
    fontSize: 1.05rem
    letterSpacing: "0.18em"
  label-sm-caps:
    fontFamily: Work Sans
    fontSize: 0.95rem
    letterSpacing: "0.15em"
  label-xs-caps:
    fontFamily: Work Sans
    fontSize: 0.6rem
    letterSpacing: "0.15em"
rounded:
  none: 0
spacing:
  section-y: 9rem
  gap-xl: 4rem
  gap-lg: 2rem
  gap-md: 1.5rem
  gap-sm: 0.75rem
  button-padding: "1.2rem 2.4rem"
components:
  button-primary:
    backgroundColor: "{colors.pure-black}"
    textColor: "{colors.pure-white}"
    padding: 12px 24px
    typography: "{typography.label-caps}"
  button-primary-hover:
    backgroundColor: "{colors.pure-black}"
    opacity: 0.85
  hero-cta-primary:
    backgroundColor: "{colors.pure-black}"
    textColor: "{colors.pure-white}"
    padding: 12px 24px
    typography: "{typography.label-caps}"
  hero-cta-secondary:
    textColor: "{colors.pure-black}"
    typography: "{typography.label-caps}"
    borderBottom: "1px solid rgba(0,0,0,0.4)"
  process-step-number:
    textColor: rgba(0,0,0,0.25)
    typography: "{typography.h1-editorial}"
  project-category-tag:
    textColor: rgba(var(--color-foreground), 0.5)
    typography: "{typography.label-caps}"
  press-cover-card:
    aspectRatio: "3 / 4"
    hoverScale: 1.05
  biography-portrait-frame:
    border: "1px solid rgba(0,0,0,0.4)"
    aspectRatio: "4 / 5"
    badgeBackground: "{colors.pure-black}"
    badgeTextColor: "{colors.pure-white}"
  overlay-piece-card:
    backgroundColor: "{colors.pure-white}"
    border: "1px solid rgba(0,0,0,0.15)"
    shadow: "0 4px 24px rgba(0,0,0,0.06)"
---

## Overview

BATLLE is a French fashion brand by Louise, a young designer trained at the
Institut Français de la Mode and AICP. The visual identity translates an
ancient-to-contemporary narrative: chevaleresses, sorcières, and powerful
historical women reinterpreted as modern clothing. Handmade in Paris, no
embroidery — the work is art-inspired, sculptural, and architectural.

The design language is **strictly black and white** with two deliberate
exceptions:
- **Atelier cream (#F5F2ED)** — a warm paper tone for secondary backgrounds
  (Savoir-Faire, Presse sections). Evokes the atelier's kraft paper and toile.
- **Chalk red (#C41E3A)** — a micro-interaction accent. Reserved exclusively
  for the Press section title and hover states on editorial links. Never used
  as a background or button fill.

The mood is **editorial, sculptural, haute couture** — reminiscent of
Schiaparelli's surrealist volumes but translated through a contemporary,
restrained lens. Asymmetry is the default. Symmetry is the exception.

Every section has a supertitle (all-caps, wide tracking) followed by a
Playfair Display headline with an italicized emotional anchor
("l'*armure* d'une femme", "Modéliste *avant tout*").

## Colors

The theme uses Shopify's `color_scheme` system with three schemes:

| Scheme | Background | Text | Button | Use |
|--------|-----------|------|--------|-----|
| scheme-1 (Studio) | #FFFFFF | #000000 | #000000 bg / #FFFFFF text | Hero, Collection, Louise, Contact |
| scheme-2 (Anthracite) | #1A1A1A | #FFFFFF | #FFFFFF bg / #000000 text | Moulage subsection, badges |
| scheme-3 (Atelier) | #F5F2ED | #000000 | #000000 bg / #FFFFFF text | Presse, Savoir-Faire |

- **{colors.pure-white}:** Primary page background. Crisp, gallery-like.
- **{colors.pure-black}:** All typography, interactive elements, footer, header.
- **{colors.anthracite}:** Used for the Moulage dark section inside Savoir-Faire (not a full scheme; hardcoded as `background: rgb(var(--color-foreground))` inverted).
- **{colors.atelier-cream}:** Secondary background. Provides warmth without breaking the B&W constraint.
- **{colors.chalk-red}:** The only non-neutral color. Applied via the `--batlle-chalk` CSS variable. Threshold: never >5% of any viewport.

No gradients. No drop shadows except one: the hero overlay card and the
biography portrait use `0 4px 24px rgba(0,0,0,0.06)` — a whisper of depth,
never heavy.

## Typography

Two fonts only. No fallbacks, no variations.

**Playfair Display** (`playfair_display_n4`, weight 400) — all headings,
section titles, process numbers, material values, press publication names.
Never uppercase. Italic used for emotional emphasis within titles
(`<em>` tags render as regular weight italic).

**Work Sans** (`work_sans_n4`, weight 400) — all body text, labels, buttons,
navigation, supertitles. Uppercase + wide tracking for supertitles and labels.

**Scale by role:**
- Hero brand name: `clamp(4.5rem, 12vw, 10.5rem)`, tracking 0.04em
- Section headlines: `clamp(2.8rem–3rem, 5–6vw, 4.5–5rem)`, line-height 0.95–1
- Project titles: `clamp(2rem, 3vw, 2.8rem)`, line-height 1.15
- Body: 1.4rem–1.5rem, line-height 1.6–1.7
- Supertitles: 1.05rem, tracking 0.18em, uppercase
- Labels (categories, material names): 0.95rem, tracking 0.15em, uppercase
- Micro-labels (figure tags, footer, press dates): 0.6rem–0.95rem, tracking 0.15em

Italic is used as a structural device: every main headline has an italicized
phrase that carries the emotional payload ("l'*armure* d'une femme", "Le moulage,
*la pensée du tissu*").

## Layout

**Section spacing:** Every section uses `padding: 9rem 0`. No exceptions.
Sections are stacked edge-to-edge with no gaps — the color change between
schemes provides the visual break.

**Grid system:** `page-width` container (max 1200px) centered. All custom
sections use native CSS Grid, not Shopify's flex-based utilities.

- **Hero:** 12-col grid (`lg:grid-cols-12`), content left 6 cols, image right
  6 cols. Image has an offset overlay card (`-bottom: 1.5rem, -left: 3rem`).
- **Collection:** 12-col grid with intentional asymmetry:
  - Item 1: spans 7 cols
  - Item 2: spans 5 cols, `margin-top: 8rem` (offset creates visual rhythm)
  - Item 3: spans 6 cols, starts at column 2
- **Savoir-Faire:** Two-part layout. First: sticky image (5 cols) + process
  steps (7 cols). Second: image (7 cols) + text (5 cols) in inverted scheme.
- **Biography:** Portrait (5 cols) + content (7 cols). Portrait uses an offset
  border frame (`inset: -1rem`, 1px border) with a name badge labeled
  "Louise" in Playfair italic, positioned `bottom: -1.25rem, right: -1.25rem`.
- **Presse:** 4-col grid for magazine covers + a featured article row
  (`3fr 7fr 2fr` grid) below the covers.
- **Contact:** Centered single column, max-width 48rem.

**Responsive:** All grids collapse to single column below 750px. Collection
asymmetry disappears on mobile — items stack vertically. The process hover
padding effect is disabled on touch.

## Elevation & Depth

Shadow is minimal and intentional. Only two elements use it:
- Hero overlay card: `0 4px 24px rgba(0,0,0,0.06)`
- Biography portrait frame: `0 4px 24px rgba(0,0,0,0.06)`

No elevation elsewhere. Depth is communicated through color contrast
(scheme changes) and layout asymmetry, not shadow layering.

## Shapes

Zero border-radius throughout. Every element uses sharp corners — cards,
buttons, images, overlays. The only exceptions are Dawn's built-in
form elements (newsletter inputs, variant pills) which inherit Dawn's
default `border-radius`.

**Borders** are thin (1px) and used as section separators, decorative frames,
and hover underlines. Opacity varies from 0.12 (subtle dividers) to 0.4
(active borders). All borders use `rgba(var(--color-foreground), opacity)` to
auto-adapt to the current color scheme.

## Components

### Hero (`batlle-hero.liquid`)
Full-viewport opening statement. Fixed header offset. Left: brand name at
hero scale, italic subtitle, body paragraph, two CTAs (primary filled button +
secondary underlined link). Right: hero image with offset overlay card
showing piece reference, title, and materials. Bottom: "↓ Faire défiler"
scroll hint with pulse animation.

### Project card (Collection block)
Image with 1.05× scale on hover, figure tag overlay ("Fig. 01"), category
label, title in Playfair, description in Work Sans. Right-aligned arrow
(→) slides in on hover. No price — collection pieces are bespoke.

### Process step (Savoir-Faire block)
Horizontal flex layout: large faded number (25% opacity), title + body,
hover arrow. Entire row shifts left 1rem on hover. Top border separator
between steps. Four fixed steps: Croquis, Patronage, Drapé, Confection main.

### Moulage subsection
Inverted color scheme (foreground background, background text). Left: 4:3
image in `grayscale(100%)`. Right: supertitle, headline, body paragraphs,
3-column material grid (Flou, Tailleur, Corseterie).

### Press cover
3:4 aspect ratio image with hover scale. Publication name in Playfair, date
in label-caps. Featured article row at bottom: publication + date (3 cols),
title + excerpt (7 cols), "Lire l'article →" link (2 cols). Row shifts left
on hover.

### Biography portrait
Offset border frame (`inset: -1rem`, 1px border) around 4:5 portrait image.
Name badge ("Louise") in Playfair italic, black background, positioned at
bottom-right corner. Skills rendered as a 3-column definition list with
label/value pairs.

### Contact CTAs
Centered layout. Primary: email as filled button (`{colors.pure-black}` bg).
Secondary: Instagram link with inline SVG icon, underlined. Info grid below:
Atelier + Contact in 2-column layout on desktop, stacked on mobile.

## Interactions

One transition curve governs all motion: `cubic-bezier(0.16, 1, 0.32, 1)`
with 0.4s duration (0.6s for image scale). This is an asymmetric ease-out —
starts fast, decelerates elegantly. Applied to:
- Image hover scale: `transform: scale(1.05)` (0.6s)
- Arrow hover reveal: `opacity 0→1, transform translateX(-0.5rem→0)` (0.4s)
- Process step indent: `padding-left 0→1rem` (0.4s)
- Press featured row indent: `padding-left 0→1rem` (0.4s)
- Button hover: `opacity 1→0.85` (0.3s)
- Border hover: `border-color` transition (0.3s)

No page transitions. No scroll-triggered animations beyond Dawn's built-in
`scroll-trigger animate--slide-in` (opt-in via theme settings).

## Do's and Don'ts

- **Do** use the Playfair Display italic `<em>` pattern in every headline.
  The italic word is the emotional anchor of the section.
- **Do** prefix every section with a supertitle (all-caps, 0.18em tracking).
  This establishes the editorial voice before the headline lands.
- **Do** use the 12-col asymmetric grid for Collection — equal-width grids
  read as e-commerce, not editorial.
- **Do** keep section padding at exactly 9rem top and bottom. Consistency
  creates the gallery-like rhythm.
- **Do** use scheme-3 (Atelier cream) for the Presse and secondary sections.
  It creates warmth without introducing a third "color."
- **Don't** use chalk red (#C41E3A) for anything other than the Presse
  section title and editorial link hovers. It's an accent, not a palette color.
- **Don't** add rounded corners. The sharpness reinforces the architectural,
  sculptural identity.
- **Don't** add drop shadows beyond the two specified elements. Depth comes
  from layout asymmetry and color contrast.
- **Don't** use gradients. Flat colors only.
- **Don't** center-align body text. Headlines and CTAs may be centered
  (Contact section), but body text is always left-aligned.
- **Don't** introduce a third font. Playfair + Work Sans is the complete set.
