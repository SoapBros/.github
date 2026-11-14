# Soap Bros — brand guidelines

**Version 1.0 · 2026-11-14 · Soap Bros Platforms, from the Soap Bros game studio**

This is the one document that says how Soap Bros looks and sounds. It is written to be read by a person, pasted into an LLM, or imported into design software. Every value here is also in machine-readable form: [`tokens.json`](tokens.json) and [`tokens.css`](tokens.css). If this file and the tokens disagree, the tokens win; tell the owner.

**Using it with an LLM:** paste section 14, "Prompt block", at the top of the conversation, and attach `tokens.css`. **Using it in design software:** read `tokens.json` for values, load the fonts in section 5, place the logo files from `logo/`.

---

## 1. Who we are

Soap Bros is a pixel game studio and the platform team behind it. The studio makes small, carefully made pixel games. **Soap Bros Platforms** builds cloud platforms as code: landing zones, delivery pipelines, data and AI platforms, and a developer portal.

The two halves share one personality: **playful on the surface, rigorous underneath.** The look is a friendly 8-bit game. The work is careful, reviewed and honest about what has and has not been proven.

The mascot is a pixel rubber duck in a bubble bath. It is the logo.

## 2. The logo

| File | Use |
|---|---|
| `logo/soapbros-duck.jpg` (440 × 352) | The mark: duck, bubbles and foam. Headers, READMEs, the app sidebar, the sales pitch. |
| `logo/soapbros-logo-sheet.png` (1024 × 1024) | The full sheet: the mark, the stacked wordmark "SOAP BROS", and the badge "SOAP BROS · PIXEL GAME STUDIO · EST. 2026". Take wordmark and badge from here. |
| `favicons/` | favicon.ico, 16, 32, 48, 180 (Apple), 192 and 512 px PNGs, and an SVG wrapper, all cropped square on the duck. |

**Rules**

1. **Never redraw the logo.** No rebuilding it from rectangles, CSS or SVG, no emoji stand-in, no AI-generated copy, no "placeholder duck". If the file you need is missing or too small, ask for it.
2. Keep the duck on its own off-white tile (about `#EDECE7`). On a dark page, give the tile a 3 px outline in the line colour.
3. Do not recolour, crop, rotate, stretch, add effects or put it on a busy picture.
4. Clear space: at least the height of one bubble (about 12 % of the mark's height) on every side.
5. Minimum size: 32 px wide on screen. Below that, use the favicon.
6. Scale in whole steps where possible and set `image-rendering: pixelated` so the pixels stay crisp.
7. The wordmark is set in Silkscreen (section 5), upper case. Do not set it in any other face.

## 3. Names

- The studio and brand: **Soap Bros**. Two words, capital S and B. Upper case "SOAP BROS" in the wordmark only.
- The platform business: **Soap Bros Platforms**.
- Internal tools carry **Berserk codenames**: godhand, void, slan, conrad, ubik, femto, behelit, godo, falconia, guts. They are codenames only. A new internal tool gets a Berserk name, confirmed by the owner.
- **Never show a codename to a client** unless the client chose it. Client builds carry the client's own names and look (section 11).

## 4. Colour

Colour is semantic. Every colour has one job; do not use a colour outside its job.

### Light (default)

| Token | Hex | Job |
|---|---|---|
| `ground` | `#F1EDE3` | Page background |
| `paper` | `#FBF8F1` | Panels, cards, tables |
| `ink` | `#141210` | Text, outlines, shadows |
| `muted` | `#5F584D` | Secondary text |
| `duck` | `#F7C21A` | Fill for the main action and highlights. **Never** text on paper (contrast 1.56:1). |
| `beak` | `#F08A1C` | Second accent, small highlights |
| `bubble` | `#5EC3EF` | Focus ring, decorative bubbles |
| `foam` | `#CFEEFB` | Quiet highlight panel |
| `link` | `#0B6C96` | Links on paper |
| `ok` | `#23703C` | Success text |
| `warn` | `#A85A06` | Warning text |
| `no` | `#B3402E` | Error text |

### Dark

| Token | Hex |
|---|---|
| `ground` | `#12161D` |
| `paper` | `#1A2029` |
| `ink` | `#ECE7DA` |
| `muted` | `#A2ABBA` |
| `line` (outlines) | `#8E99AE` |
| `shadow` | `#05070A` |
| `foam` | `#1C3A4B` |
| `link` | `#5EC3EF` |
| `ok` / `warn` / `no` | `#5FCF85` / `#F08A1C` / `#F08A7A` |
| `duck`, `beak`, `bubble` | unchanged |

Text on `duck`, `beak` or `bubble` fills is always `ink` `#141210`.

### Cloud colours

Used only to label which cloud something belongs to. Never as status, never for decoration.

| Cloud | Hex | | Cloud | Hex |
|---|---|---|---|---|
| Azure | `#2F7FD6` | | STACKIT | `#8A63D2` |
| AWS | `#E08A1E` | | Huawei Cloud | `#C8102E` |
| Google Cloud | `#2F9E5B` | | Sakura Cloud | `#E86AA6` |
| Other | `#8C9099` | | | |

Always pair a cloud colour with the cloud's name in text. Colour alone is not enough.

### Contrast (WCAG 2.2, measured)

Body text must reach **4.5:1**, large text and UI outlines **3:1**.

| Pair | Ratio |
|---|---|
| ink on ground | 15.98 |
| ink on paper | 17.62 |
| muted on paper | 6.62 |
| ink on duck | 11.29 |
| ink on beak | 7.44 |
| ink on bubble | 9.37 |
| ink on foam | 15.37 |
| link on paper | 5.50 |
| ok on paper | 5.73 |
| warn on paper | 4.79 |
| no on paper | 5.37 |
| dark ink on dark paper | 13.26 |
| dark muted on dark paper | 7.07 |
| dark link on dark paper | 8.21 |
| **duck on paper as text** | **1.56 — fails; never do this** |

`#2F8F4E` is a valid green for shapes and icons (3.84:1) but **not for small text**; use `ok` for text.

## 5. Typography

Three faces, all free, all from Google Fonts:

```
https://fonts.googleapis.com/css2?family=Silkscreen:wght@400;700&family=Press+Start+2P&family=IBM+Plex+Mono:wght@400;500;600&display=swap
```

| Role | Face | Use |
|---|---|---|
| Display | **Silkscreen** 400 / 700 | Wordmark, headings, table headers, big numbers |
| Arcade | **Press Start 2P** 400 | Tiny labels, buttons, badges. 9 to 12 px only. Never body text. |
| Body | **IBM Plex Mono** 400 / 500 / 600 | All running text, tables, code, data |

Scale (px): 11 label · 13 small · 15 body · 17 lead · 20 / 28 / 44 headings · 32 to 92 hero (fluid).
Line height: body 1.65, headings 1.05. Keep running text to about 65 characters a line. Headings use balanced wrapping. Numbers that line up use tabular figures. Upper-case labels get a little letter spacing.

Fallbacks, in order: Silkscreen and Press Start 2P → `"Courier New", monospace`; IBM Plex Mono → `ui-monospace, "SF Mono", Menlo, monospace`. The pixel faces are for Latin text. For Japanese, set the text in a clean system Gothic face (for example Noto Sans JP) and keep Silkscreen for numerals, labels and the wordmark.

## 6. Shape, depth and layout

- **Square corners.** Radius 0 everywhere.
- **Thick outlines.** 3 px ink outline on panels, buttons, inputs and tiles; 4 px on the biggest panels.
- **Hard shadows.** An offset shadow with **no blur**: `4px 4px 0` on normal elements, `8px 8px 0` on hero panels, in the shadow colour.
- **Spacing** on a 4 px unit: 4, 8, 12, 16, 24, 32, 48, 64.
- **Layout.** Left-aligned. Maximum content width 1080 to 1280 px. Cards and tiles in auto-fitting grids. Side gutter at least 16 px at every width. Nothing may scroll sideways except wide tables and code, each in their own scroll box.
- **Depth** comes from the offset shadow and from overlap, not from gradients, blur or glow.
- **Do not use:** rounded corners, soft drop shadows, gradients, glassmorphism, centred-everything hero layouts, stock illustration, emoji as icons or markers, purple-to-blue washes.

## 7. Components

**Panel.** `paper` fill, 3 px ink outline, `4px 4px 0` shadow.

**Primary button.** `duck` fill, `ink` text in Press Start 2P 12 px, 3 px outline, `4px 4px 0` shadow. Hover: move up-left 2 px and grow the shadow to 6 px. Press: move down-right 4 px and drop the shadow. Focus: a 4 px `bubble` ring, 3 px away.

**Secondary button.** Same, with `paper` fill.

**Tile (a number and a label).** Panel with a 6 px coloured bar on top (the cloud colour), a large figure in Press Start 2P, a small muted label. A tile that filters shows a thicker ink ring when selected.

**Table.** Panel containing a fixed-layout table. Headers in Silkscreen, 11 px, upper case, muted. 1 px row lines. **Cells never wrap.** Long text ends in an ellipsis and shows in full on hover or in an info card. Row hover uses a faint tint.

**Info card.** A panel that opens from a small `i` button, with a title, the full text, tags and the main action. Closes with Esc or a click outside.

**Tag / chip.** 1 px outline, no fill, small text.

**Status.** A word and a colour, never colour alone: PASSED (`ok`), NOTE (`warn`), FAILED (`no`).

**Focus.** Every interactive element shows a visible 4 px `bubble` ring.

## 8. Motion

- **Stepped, never eased.** Use `steps(2)` timing, 80 ms, for hover and press.
- The duck may bob (a 6 px vertical step every 1.6 s). Bubbles may rise slowly and loop. Dashed lines in diagrams may march. That is all.
- **Respect reduced motion:** with `prefers-reduced-motion: reduce`, turn all animation and transitions off.
- Nothing may be hidden until an animation reveals it. The page must be complete at rest.

## 9. Imagery and icons

- **Pixel art** only for illustration. Keep to the palette. Do not upscale with smoothing; scale by whole steps with `image-rendering: pixelated`.
- **Decorative bubbles:** small squares with a 3 px `bubble` outline and `foam` fill.
- **UI icons:** simple outlined glyphs at 16 to 18 px in the text colour, inside a 28 px square with a 2 px outline when they stand alone (as in table rows). Use one icon set per product.
- **Diagrams:** boxes with 4 px ink outlines, flat fills from the palette, dashed ink connectors, labels in Press Start 2P and Plex Mono. Draw what actually connects; do not decorate.
- **Tool logos** (Terraform, Kubernetes, and so on): see section 10.

## 10. Third-party tool logos

`tool-logos/` holds single-colour marks for the tools we use or plan to use.

- Use them only to say which tool something works with, at one height (20 px in text rows, 32 to 36 px in a toolbox grid), with the tool's name beside or under the mark.
- One mid-tone, `#6B778C`, so marks read on light and dark pages and never compete with the duck. On hover in a toolbox they may switch to `ink` on a `duck` tile.
- **No official mark on hand → write the name as text.** Never draw a stand-in. Currently text-only: Terragrunt, Terramate, STACKIT, Sakura Cloud, OPA, Checkov, cert-manager, External Secrets, Kyverno.
- Every page that shows them carries: *"All product names and logos belong to their owners. They are shown to say which tools we work with and do not imply endorsement."*
- Some owners publish rules for their marks. Check them before the marks go into anything sold or sent to a client.
- `tool-logos/index.json` lists each file with its source. Marks from Simple Icons are CC0; the trademarks stay with their owners.

## 11. Client work

The Soap Bros look is the **default**. A client's look is an **extra option**, and becomes their default only when the owner says so for that client.

For each client, ask for: square and wide logo (SVG or PNG, light and dark versions), brand colours as hex, colours to avoid, typefaces and proof that self-hosting is licensed, light or dark default, the name they use for the product, language, accessibility standard, and legal wording.

Rules for a client build:
- Use only colours and assets the client supplied, plus the status colours.
- Never redraw, trace, recolour or crop a client's logo. Ask for another file instead.
- No Soap Bros name, logo, codename or colour on a screen the client sees, unless they chose it.
- Keep every text and outline pair at or above the contrast ratios in section 4.
- Do not mix one client's names or assets into another client's work.

The portal implements all of this: see `godo/.claude/skills/portal-skin/SKILL.md` (private repository).

## 12. Voice

- **Plain and specific.** Short sentences. Numbers, units, names and dates, not adjectives.
- **Honest about proof.** Say what was checked and how: "validated offline", "rendered", "type-checked, not viewed". Never "works" or "deployed" for something that has not run. Show what is not done yet next to what is.
- **Friendly, not cute.** The game studio is playful; the platform copy is calm and exact. A joke belongs in a game, not in an error message.
- Active voice. A control says what it does ("Publish"). An error says what went wrong and how to fix it, without apologising.
- **Avoid:** mannered devices (asides in dashes, "not X but Y" framing, colon-then-reveal sentences), scare quotes around invented terms, stock phrases ("worth noting", "honest caveat"), hype, and emoji.
- **Bilingual.** English first. Japanese for the Japan and APAC audience: docs, onboarding and portal text in both where possible. Keep product and tool names in their original form.
- **Never claim endorsement** by a cloud, vendor or client.

## 13. Accessibility

- Text 4.5:1, large text and outlines 3:1 (section 4).
- Never use colour alone: pair it with a word, a shape or an icon.
- Visible focus on everything interactive; full keyboard operation.
- Respect reduced motion. Keep tap targets at least 32 px.
- Pixel faces are hard to read at length; keep them to headings and short labels.
- Give every image alt text that says what it shows ("Soap Bros: a pixel rubber duck in a bubble bath"). Decorative bubbles are hidden from screen readers.

## 14. Prompt block

Paste this at the top of a conversation with any LLM or image or UI tool, and attach `tokens.css`.

```text
You are designing for Soap Bros, a pixel game studio and its platform business
(Soap Bros Platforms). Follow these brand rules exactly. If a request conflicts
with them, say so and ask before breaking them.

LOOK: friendly 8-bit game on the surface, rigorous underneath.
- Colours (light): ground #F1EDE3, paper #FBF8F1, ink #141210, muted #5F584D,
  duck yellow #F7C21A (fills only, never text on paper), beak orange #F08A1C,
  bubble blue #5EC3EF, foam #CFEEFB, link #0B6C96, ok #23703C, warn #A85A06,
  no #B3402E. Dark: ground #12161D, paper #1A2029, ink #ECE7DA, muted #A2ABBA,
  line #8E99AE, shadow #05070A, foam #1C3A4B, link #5EC3EF.
- Text on duck, beak or bubble is always ink #141210.
- Fonts: Silkscreen for headings and wordmark, Press Start 2P for tiny labels
  and buttons (9-12 px only), IBM Plex Mono for all body text. All on Google Fonts.
- Square corners (radius 0). 3 px ink outlines. Hard offset shadows with no
  blur: 4px 4px 0 normal, 8px 8px 0 for hero panels. 4 px spacing unit.
- Motion is stepped (steps(2), 80 ms), never eased. Honour reduced motion.
- No gradients, blur, glow, rounded corners, glassmorphism, emoji as icons,
  stock illustration, centred-everything heroes or purple-blue washes.
- Tables never wrap text: truncate with an ellipsis and show the rest on hover.
- Cloud colours (Azure #2F7FD6, AWS #E08A1E, Google Cloud #2F9E5B, STACKIT
  #8A63D2, Huawei #C8102E, Sakura #E86AA6) label clouds only, with the cloud's
  name beside them. Never use them for status.

LOGO: the pixel rubber duck in a bubble bath, from the supplied file. NEVER
redraw, recreate, recolour or approximate it, and never invent a substitute.
If you do not have the file, leave a clearly marked empty slot and ask for it.
Third-party tool logos are single-colour #6B778C marks from the supplied set;
if a mark is missing, write the tool's name as text instead of drawing one.

NAMES: the brand is "Soap Bros". Internal tools have Berserk codenames
(godhand, void, slan, conrad, ubik, femto, behelit, godo, falconia, guts).
Do not show a codename to a client unless they chose it.

VOICE: plain, specific, calm. Numbers and names, not adjectives. Say what was
checked and how ("validated offline", "type-checked, not viewed"); never say
something works or is deployed if it has not run. No hype, no emoji, no
mannered devices (dash asides, "not X but Y", colon reveals). English first;
Japanese where the audience needs it.

CLIENT WORK: the Soap Bros look is the default. A client's look is an extra
option built only from assets and colours the client supplied. Never redraw a
client's logo, and keep their screens free of Soap Bros names and colours
unless they asked for them.

ACCESSIBILITY: text at least 4.5:1, outlines and large text at least 3:1,
never colour alone, visible focus rings (4 px bubble blue), keyboard operable,
meaningful alt text.
```

## 15. Where things live

| What | Where |
|---|---|
| This guide | `brand/BRANDING.md` in the `SoapBros/.github` repository |
| Tokens | `brand/tokens.json`, `brand/tokens.css` |
| Logos and favicons | `brand/logo/`, `brand/favicons/` |
| Tool logos | `brand/tool-logos/` |
| Working rules for AI coding agents | `CLAUDE.md` in this repository |
| The live look | soapbros.github.io; the femto portal's `soapbros` and `soapbros-dark` skins |

Change this guide in a pull request, update `tokens.json` and `tokens.css` in the same change, and bump the version at the top.
