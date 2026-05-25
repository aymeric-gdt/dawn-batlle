# BATLLE — Shopify Dawn Theme Customization Plan

> **For Hermes:** Implement each task sequentially. Do NOT execute — implement only. User runs code himself.

**Goal:** Transform Dawn 15.4.1 into a sculptural editorial theme for BATLLE, a Schiaparelli-inspired French fashion brand by Louise. The brand operates in the tension between *l'atelier brut* (raw muslin, chalk marks, process) and *la silhouette sculpturale* (leather, architectural volumes, finished couture).

**Architecture:** Extend Dawn with 5 custom sections + 1 reused section (featured-collection). All new sections follow Dawn's Liquid/Schema/CSS patterns. French locale added.

**Tech Stack:** Shopify Liquid, Dawn 15.4.1 section architecture, CSS custom properties (Dawn's `--color-*` variables), JSON templates.

---

## ART DIRECTION (derived from 12 reference images)

### Palette — Atelier Brut × Sculptural Couture

| Role | Hex | CSS Variable | Origin |
|------|-----|-------------|--------|
| Fond principal | `#FFFFFF` | `--batlle-white` | Studio backdrop |
| Fond atelier | `#F5F2ED` | `--batlle-cream` | Cotton muslin / toile |
| Texte | `#000000` | `--batlle-black` | Finished leather pieces |
| Fond sombre | `#1A1A1A` | `--batlle-anthracite` | Sculptural shadows |
| Gris pierre | `#D4CFC7` | `--batlle-stone` | Editorial stone backdrops |
| **Accent craie** | `#C41E3A` | `--batlle-chalk` | Tailor's chalk red — THE ONLY color deviation |

The chalk red (`#C41E3A`) is used EXCLUSIVELY for micro-interactions and micro-accents: link hovers, thin rules, active indicators, punctuation marks. Never in large flats. It references the red/blue/green tailor's chalk marks visible in the atelier process shots.

### Typography

- **Headings:** Bodoni or Didot (high-contrast serif, Schiaparelli's typographic language). If unavailable in Shopify's font library, fallback to a sharp modern serif with strong thick/thin contrast.
- **Body:** Futura or geometric sans-serif — clean, technical, referencing pattern-making precision.
- The tension: serif = couture / sans-serif = atelier technique.

### Layout Language

- **Asymmetrical grid** — off-axis tension, never perfectly centered
- **Density rhythm** — full-bleed image blocks alternating with generous cream breathing space + small precise text
- **Thin black rules** (`1px solid #000`) as section separators — referencing seam lines
- **Image borders** — thin black frames on images (contact sheet / lookbook reference)
- **Subtle grain texture** on dark backgrounds (`scheme-2`) — referencing muslin weave and limestone
- **Chalk red** only for hover states, active links, thin accent rules — never backgrounds or large text blocks

### Section-specific DA notes

1. **Collection:** Full-bleed asymmetric image grid. Each project fills its cell. Titles offset, not centered. Images with thin black borders.
2. **E-Shop:** Editorial spacing — more generous than standard e-commerce. Product images on cream or white.
3. **Savoir-Faire:** Split layout — large editorial image left, text on cream background right. References the atelier documentary shots.
4. **Louise:** Portrait + text. Skill tags as thin-bordered labels (like garment hang tags).
5. **Presse:** Magazine spread cards on stone-grey background. "Vu en presse" title in chalk red.
6. **Contact:** Minimal — black text on white, generous whitespace, single thin black rule separator.

---

## TASK 0: Global Theme Foundation

### Task 0.1: Replace color schemes with Atelier Brut palette

**Objective:** Define 3 schemes that encode the atelier-to-couture spectrum. The chalk red accent is applied via CSS custom property, not as a scheme color.

**Files:**
- Modify: `config/settings_data.json`
- Modify: `config/settings_schema.json` (reduce color_scheme_group entries, ensure exactly 3 schemes)

**Details:**

Three color schemes encoding the BATLLE visual language:

- **scheme-1 (Studio Blanc)**: Background `#FFFFFF`, text `#000000`, button `#000000`, button text `#FFFFFF`. The clean studio backdrop.
- **scheme-2 (Atelier Sombre)**: Background `#1A1A1A`, text `#FFFFFF`, button `#FFFFFF`, button text `#000000`. The sculptural shadow space.
- **scheme-3 (Toile Crème)**: Background `#F5F2ED`, text `#000000`, button `#000000`, button text `#FFFFFF`. The muslin/toile workspace.

Set default scheme to `scheme-1`.

In `settings_schema.json`, ensure the `color_scheme_group` definitions are reduced to exactly schemes 1-3. Remove any extra entries (Dawn ships with 5+).

Additionally, inject custom CSS properties via `theme.liquid`'s `{% style %}` block:
```css
:root {
  --batlle-cream: #F5F2ED;
  --batlle-stone: #D4CFC7;
  --batlle-chalk: #C41E3A;
}
```
These supplement Dawn's `--color-*` variables and are used by custom sections.

---

### Task 0.2: Typography — Bodoni/Didot headings, Futura body

**Objective:** Set the type pairing that encodes the couture/atelier tension.

**Files:**
- Modify: `config/settings_schema.json` (typography section)

**Details:**

- `type_header_font` default: `bodoni_n4` (Bodoni — high contrast serif, Schiaparelli's typographic language). If Bodoni isn't available in Shopify's font picker, use `didot_n4` or the closest high-contrast modern serif.
- `type_body_font` default: `futura_n4` (geometric sans-serif — clean, technical, pattern-making precision). Fallback: a neutral geometric sans like `century_gothic_n4`.
- `heading_scale` default: `120` (headings run 20% larger than Dawn's default — bold editorial presence).
- `body_scale` default: `100`.

The tension between the sculptural serif and the technical sans-serif encodes the brand's duality.

---

### Task 0.3: Header — BATLLE large, Instagram icon

**Objective:** Customize the header for the brand.

**Files:**
- Modify: `sections/header.liquid` (schema, HTML)
- Modify: `snippets/social-icons.liquid` (if Instagram icon isn't there)

**Details:**

1. In the header, ensure the logo is displayed VERY LARGE. Dawn's header supports `logo_width` up to 300px — make the default logo width 250px.
2. Modify the header schema to add an Instagram URL setting: `social_instagram_link` type `text`, id `instagram_link`, label "Instagram profile URL".
3. In the header HTML, add an instagram icon (use SVG) next to the text "@modebylouise" — display it in the top bar or next to the logo. The instagram handle should link to the Instagram URL.

For the Instagram SVG icon, use a minimal path-based SVG inline.

---

### Task 0.4: Update settings_schema.json — theme info

**Objective:** Rename the theme to BATLLE.

**Files:**
- Modify: `config/settings_schema.json`

**Details:**

At the top of the file, update:
```json
{
  "name": "theme_info",
  "theme_name": "BATLLE",
  "theme_version": "1.0.0",
  "theme_author": "BATLLE x Hermes",
  "theme_documentation_url": "",
  "theme_support_url": ""
}
```

---

## TASK 1: Homepage — Collection Section (Section 1 of 6)

### Task 1.1: Create `batlle-collection.liquid` section

**Objective:** A bold editorial grid showcasing ~6 projects, each with a unique visual identity. The intro line must be strong and reflect a bold creative identity.

**Files:**
- Create: `sections/batlle-collection.liquid`
- Create: `assets/batlle-collection.css`

**Section design (Atelier Brut × Sculptural Couture):**

- White or cream background (`scheme-1` or `scheme-3`)
- Top: A bold statement/intro line in Bodoni/Didot, `hxxl` size. Default: "Des silhouettes taillées pour celles qui écrivent leur propre légende."
- Below: Asymmetric image grid — NOT evenly balanced. Projects fill their cells fully. Off-axis tension.
- Each project card:
  - Full-bleed image with a thin 1px black border (contact sheet / lookbook effect)
  - Project title below in small, precise Futura — offset, not centered
  - Optional short description in even smaller type
- Images displayed in an irregular 3×2 grid where cells have slightly different aspect ratios
- Mobile: single column, images full-width with black frame

**Schema structure:**
```json
{
  "name": "Collection",
  "tag": "section",
  "class": "section",
  "settings": [
    {"type": "inline_richtext", "id": "intro_text", "label": "Intro line", "default": "Des silhouettes taillées pour celles qui écrivent leur propre légende."},
    {"type": "select", "id": "heading_size", "options": ["h0","h1","h2","hxl","hxxl"], "default": "hxxl"},
    {"type": "color_scheme", "id": "color_scheme", "default": "scheme-1"},
    {"type": "range", "id": "padding_top", "min": 0, "max": 100, "step": 4, "default": 72},
    {"type": "range", "id": "padding_bottom", "min": 0, "max": 100, "step": 4, "default": 72}
  ],
  "blocks": [
    {
      "type": "project",
      "name": "Project",
      "limit": 6,
      "settings": [
        {"type": "image_picker", "id": "image", "label": "Image"},
        {"type": "text", "id": "title", "label": "Title", "default": "Titre du projet"},
        {"type": "richtext", "id": "description", "label": "Description"}
      ]
    }
  ],
  "presets": [{"name": "Collection", "blocks": [{"type":"project"},{"type":"project"},{"type":"project"}]}]
}
```

**CSS:** Use `{% stylesheet %}` — styles specific to this section: grid layout, image borders, typography.

---

### Task 1.2: Update `index.json` — Add collection as first section

**Files:**
- Modify: `templates/index.json`

**Details:**

Add `"batlle_collection"` as the first entry in the `"sections"` object and `"order"` array.

---

## TASK 2: Homepage — E-shop Section (Section 2 of 6)

### Task 2.1: Configure featured-collection in `index.json`

**Objective:** Show 5-8 pieces from a collection. Reuse existing `featured-collection` section.

**Files:**
- Modify: `templates/index.json`

**Details:**

Add a `featured-collection` section entry:
```json
"eshop_collection": {
  "type": "featured-collection",
  "settings": {
    "title": "E-Shop",
    "heading_size": "h1",
    "collection": "",
    "products_to_show": 8,
    "columns_desktop": 4,
    "color_scheme": "scheme-1",
    "image_ratio": "adapt",
    "show_secondary_image": true,
    "padding_top": 72,
    "padding_bottom": 36
  }
}
```

Add to order: `"eshop_collection"` as 2nd entry.

The merchant will fill in the collection handle later via theme editor.

---

## TASK 3: Homepage — Savoir-Faire Section (Section 3 of 6)

### Task 3.1: Create `batlle-savoir-faire.liquid` section

**Objective:** Merge "savoir-faire" and "moulage" into one section explaining the handmade confection process in Paris and creative draping/pattern-making.

**Files:**
- Create: `sections/batlle-savoir-faire.liquid`

**Section design (Atelier Brut reference):**

- Cream background (`scheme-3` — the muslin/toile color)
- Two-column asymmetric layout on desktop:
  - Left (60% width): Large editorial image — full-height, pinned like a contact sheet, thin black border
  - Right (40% width): Text content — title in Bodoni, body in Futura
- Title: "Savoir-Faire & Moulage" (editable)
- Body: richtext explaining the handmade process — confection main à Paris, moulage créatif, patronnage
- Small accent details: a thin 1px red rule (`--batlle-chalk`) under the title, referencing the tailor's chalk marks in the atelier photos
- Section schema: `image_picker`, `richtext` title, `richtext` body, `color_scheme`

**Schema:**
```json
{
  "name": "Savoir-Faire & Moulage",
  "tag": "section",
  "class": "section",
  "settings": [
    {"type": "image_picker", "id": "image", "label": "Process image"},
    {"type": "richtext", "id": "title", "label": "Title", "default": "Savoir-Faire & Moulage"},
    {"type": "richtext", "id": "body", "label": "Body text"},
    {"type": "select", "id": "layout", "options": [{"value":"image_left","label":"Image à gauche"},{"value":"image_right","label":"Image à droite"}], "default": "image_left"},
    {"type": "color_scheme", "id": "color_scheme", "default": "scheme-3"},
    {"type": "range", "id": "padding_top", "min": 0, "max": 100, "step": 4, "default": 72},
    {"type": "range", "id": "padding_bottom", "min": 0, "max": 100, "step": 4, "default": 72}
  ],
  "presets": [{"name": "Savoir-Faire & Moulage"}]
}
```

**CSS:** Two-column responsive grid, elegant typography for the body.

**Verification:** This section replaces any "broderie" section. No embroidery mention. The text must reference "confection main à Paris" and "moulage créatif".

---

### Task 3.2: Update `index.json` — Add savoir-faire as 3rd section

**Files:**
- Modify: `templates/index.json`

**Details:**

Add `"savoir_faire"` entry and `"savoir_faire"` to order array.

---

## TASK 4: Homepage — Louise Biography Section (Section 4 of 6)

### Task 4.1: Create `batlle-biography.liquid` section

**Objective:** Biography of Louise with her skills listed precisely: Flou Drapé, Tailleur, Corset. Born in Toulouse, violinist, IFM & AICP.

**Files:**
- Create: `sections/batlle-biography.liquid`

**Section design (Sculptural Couture reference):**

- White background (`scheme-1`)
- Large portrait image — offset left, not centered, thin black border
- Right: "Louise" in hxxl Bodoni, then bio text in Futura, generous line-height
- Skills as thin-bordered tags (like garment hang tags): "Flou Drapé", "Tailleur", "Corset" — each in a 1px black outlined box, small uppercase Futura. Do NOT write "soie" after Flou Drapé.
- A thin 1px chalk-red rule separates the bio text from the skills section
- No mention of corset as a standalone section — only as a skill tag here

**Schema:**
```json
{
  "name": "Biographie — Louise",
  "tag": "section",
  "class": "section",
  "settings": [
    {"type": "image_picker", "id": "portrait", "label": "Portrait"},
    {"type": "inline_richtext", "id": "title", "label": "Name", "default": "Louise"},
    {"type": "richtext", "id": "body", "label": "Biography"},
    {"type": "text", "id": "skill_1", "label": "Skill 1", "default": "Flou Drapé"},
    {"type": "text", "id": "skill_2", "label": "Skill 2", "default": "Tailleur"},
    {"type": "text", "id": "skill_3", "label": "Skill 3", "default": "Corset"},
    {"type": "color_scheme", "id": "color_scheme", "default": "scheme-1"},
    {"type": "range", "id": "padding_top", "min": 0, "max": 100, "step": 4, "default": 72},
    {"type": "range", "id": "padding_bottom", "min": 0, "max": 100, "step": 4, "default": 72}
  ],
  "presets": [{"name": "Biographie — Louise"}]
}
```

**CSS:** Portrait + text layout, skill badges with black borders, minimal/editorial typography.

---

### Task 4.2: Update `index.json` — Add biography as 4th section

**Files:**
- Modify: `templates/index.json`

**Details:**

Add `"biography"` entry and `"biography"` to order array.

---

## TASK 5: Homepage — Presse Section (Section 5 of 6)

### Task 5.1: Create `batlle-presse.liquid` section

**Objective:** "Vu en presse" (NOT "Vues") — magazine appearances with image previews and a link to L'Officiel article.

**Files:**
- Create: `sections/batlle-presse.liquid`

**Section design (Editorial reference):**

- Stone-grey background (`scheme-3` with a subtle shift — use `--batlle-stone` or `scheme-3`)
- Title: "Vu en presse" in Bodoni — title text colored in `--batlle-chalk` (chalk red), the ONLY section where the title uses the accent color
- Grid of magazine preview cards (3-4 per row):
  - Magazine cover/screenshot with thin black border
  - Publication name below in small uppercase Futura
  - Link (URL field) with chalk-red hover state
- First block preset: L'Officiel article link, publication "L'Officiel"
- Cards have subtle hover lift effect (2px translateY) — the only animation

**Schema:**
```json
{
  "name": "Presse",
  "tag": "section",
  "class": "section",
  "settings": [
    {"type": "inline_richtext", "id": "title", "label": "Title", "default": "Vu en presse"},
    {"type": "color_scheme", "id": "color_scheme", "default": "scheme-3"},
    {"type": "range", "id": "padding_top", "min": 0, "max": 100, "step": 4, "default": 72},
    {"type": "range", "id": "padding_bottom", "min": 0, "max": 100, "step": 4, "default": 72}
  ],
  "blocks": [
    {
      "type": "press_item",
      "name": "Article de presse",
      "limit": 6,
      "settings": [
        {"type": "image_picker", "id": "image", "label": "Image"},
        {"type": "text", "id": "publication", "label": "Publication", "default": "L'Officiel"},
        {"type": "url", "id": "link", "label": "Lien vers l'article"}
      ]
    }
  ],
  "presets": [{"name": "Presse", "blocks": [{"type":"press_item"}]}]
}
```

**CSS:** Clean grid layout, bordered cards, hover effect on links.

**Verification:** Title is "Vu en presse" — NOT "Vues en presse".

---

### Task 5.2: Update `index.json` — Add presse as 5th section

**Files:**
- Modify: `templates/index.json`

**Details:**

Add `"presse"` entry and `"presse"` to order array.

---

## TASK 6: Homepage — Contact Section (Section 6 of 6)

### Task 6.1: Create `batlle-contact.liquid` section

**Objective:** White bg, black text. Louise works freelance from home. No atelier hours, no private collection. Appointment booking copy reflects freelance/home setup.

**Files:**
- Create: `sections/batlle-contact.liquid`

**Section design (Minimal editorial):**

- Pure white background (`scheme-1`), black text
- Title: "Contact" in Bodoni
- Body: Explanation that Louise works freelance from home in Paris. Custom orders and appointments on request. No boutique, no atelier hours, no collection privée.
- Email in small Futura, chalk-red underline on hover
- Instagram: "@modebylouise" with inline Instagram SVG icon
- CTA button: "Prendre rendez-vous" — secondary style (black border, white fill, black text), chalk-red border on hover
- A single thin 1px black rule separates the text from the button/CIG block
- Generous whitespace — this section breathes

**Schema:**
```json
{
  "name": "Contact",
  "tag": "section",
  "class": "section",
  "settings": [
    {"type": "inline_richtext", "id": "title", "label": "Title", "default": "Contact"},
    {"type": "richtext", "id": "body", "label": "Description", "default": "<p>Louise travaille en freelance depuis son atelier parisien. Les commandes personnalisées et rendez-vous sont disponibles sur demande.</p>"},
    {"type": "text", "id": "email", "label": "Email", "default": "contact@batlle.fr"},
    {"type": "url", "id": "booking_link", "label": "Lien de rendez-vous"},
    {"type": "text", "id": "booking_label", "label": "Bouton texte", "default": "Prendre rendez-vous"},
    {"type": "color_scheme", "id": "color_scheme", "default": "scheme-1"},
    {"type": "range", "id": "padding_top", "min": 0, "max": 100, "step": 4, "default": 72},
    {"type": "range", "id": "padding_bottom", "min": 0, "max": 100, "step": 4, "default": 72}
  ],
  "presets": [{"name": "Contact"}]
}
```

**CSS:** Clean, minimal contact block. Button with black border.

**Verification:** No "Horaires d'atelier", no "Collection privée", no atelier hours. Freelance from home language only.

---

### Task 6.2: Update `index.json` — Add contact as 6th section

**Files:**
- Modify: `templates/index.json`

**Details:**

Add `"contact"` entry and `"contact"` to order array.

---

## TASK 7: Locales — French Translations

### Task 7.1: Create `locales/fr.json`

**Objective:** Add French translation file for all new sections and frontend strings.

**Files:**
- Create: `locales/fr.json`

**Details:**

Create complete French locale file mirroring `en.default.json` but with French translations. Key new keys:

```json
{
  "sections": {
    "batlle_collection": {
      "name": "Collection"
    },
    "batlle_savoir_faire": {
      "name": "Savoir-Faire & Moulage"
    },
    "batlle_biography": {
      "name": "Biographie — Louise"
    },
    "batlle_presse": {
      "name": "Presse"
    },
    "batlle_contact": {
      "name": "Contact"
    }
  }
}
```

Also ensure all existing Dawn strings have French equivalents. Use formal French (vous).

---

## TASK 8: Footer Customization

### Task 8.1: Update footer for B&W brand

**Objective:** Clean B&W footer with Instagram link.

**Files:**
- Modify: `sections/footer.liquid` (schema defaults)

**Details:**

In the footer schema, set default `color_scheme` to `scheme-2` (black bg, white text) or clean white `scheme-1`.

Ensure the social icons snippet includes Instagram as a prominent icon.

---

## TASK 9: Final Verification

### Task 9.1: Validate all new files against Shopify theme schemas

**Objective:** Run `validate_theme` MCP tool on each new file.

**Files:**
- Validate: `sections/batlle-collection.liquid`
- Validate: `sections/batlle-savoir-faire.liquid`
- Validate: `sections/batlle-biography.liquid`
- Validate: `sections/batlle-presse.liquid`
- Validate: `sections/batlle-contact.liquid`
- Validate: `config/settings_schema.json`
- Validate: `config/settings_data.json`
- Validate: `locales/fr.json`
- Validate: `templates/index.json`

---

## TASK 10: Content Checklist

**Objective:** Verify compliance with all user requirements.

- [ ] BATLLE displayed very large in header
- [ ] Instagram icon next to @modebylouise in header
- [ ] Homepage: 6 sections in exact order: Collection → E-Shop → Savoir-Faire → Louise → Presse → Contact
- [ ] Collection intro line is bold and creative (NOT "three pieces, three figures")
- [ ] Collection shows ~6 projects with unique visual identities
- [ ] E-Shop shows 5-8 pieces
- [ ] Savoir-Faire merges "savoir-faire" and "moulage" — handmade confection in Paris, creative draping
- [ ] Louise bio: born Toulouse, violinist+seamstress, IFM+AICP, skills: Flou Drapé, Tailleur, Corset (no "soie"!)
- [ ] No corset section on homepage (only mentioned in Louise bio)
- [ ] No "broderie" section — replaced by "confection main à Paris"
- [ ] Presse title: "Vu en presse" (not "Vues")
- [ ] Presse includes L'Officiel article link
- [ ] Contact: white bg, black text, freelance from home, no atelier hours, no collection privée
- [ ] Strict black & white art direction throughout
- [ ] Schiaparelli-inspired editorial feel
