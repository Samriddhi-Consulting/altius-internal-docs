# ADA & Altius — Designer Guidelines (v2)

**The Core Philosophy:** "If it's not a degree above, it doesn't belong on this page."
Every design decision should reflect the belief that small, intentional shifts unlock disproportionate outcomes. The brand is sharp, clever, premium, and human-first. It is "low on graphic" and highly intentional.

## 1. The Brand DNA (Δ and °)
These are not decorations. They are the brand's language. Every page must contain at least three intentional uses of Δ or ° beyond the logo.
- **Δ (Delta):** Represents change, elevation, and the gap.
  - *Do:* Use for section openers, faint background watermarks (ADA Blue, 8% opacity), container shapes, or CTA arrows.
  - *Don't:* Tile as a pattern, distort, or use it to replace any generic letter "A".
- **° (Degree):** Represents the spark, the one degree of difference.
  - *Do:* Use for bullet points (instead of •), step markers (1°, 2°), eyebrow accents, or end-of-headline punctuation.
  - *Don't:* Use as a standalone graphic without context or as a replacement for the letter "o".

## 2. Typography System
- **H1 & H2 (Editorial Moments):** **Fraunces** (variable, optical size: text). Use its variable axes (SOFT, WONK) to dial character up for hero moments.
- **H3, Body, UI, Buttons:** Native **System UI sans-serif stack**. Do not use Google Fonts.

## 3. The Strict Colour Palette
Maintain the exact binding ratio to ensure the brand feels premium:
- **White / Off-White (~75%):** Page backgrounds (`#ffffff`), Section breaks (`#f4f5f6`).
- **Charcoal (~20%) - `#28292a`:** Used for all body text. *Never use pure `#000000`.*
- **ADA Deep Blue (~4%) - `#003f88`:** Used for the Δ, H1/H2, link underlines, icons, and structural accents.
- **ADA Yellow (~1%) - `#fcb900`:** The focal point. Use for primary CTAs, the ° circle, and one hero moment per page. *Never call it "gold" and never use it for body text.*

## 4. Layout & Composition
- **Section Rhythm:** Break the monotony. Every 2-3 sections must introduce a break (a full-bleed visual, pulled-out stat, or horizontal Δ° divider).
- **Cards:** Default to no border, a soft shadow (`0 1px 2px rgba(0,0,0,0.04)`), and generous padding (24px mobile, 32px desktop).
- **Headlines:** The most important word in the sentence must be the darkest.

## 5. Imagery & Iconography
- **Photography:** Real humans, mid-action. Natural light, slight desaturation. No stock-y "diverse teams high-fiving."
- **Product:** Show real Altius simulation screens in realistic mockups. No "imagined screens."
- **Icons:** Single line, uniform stroke (1.5px), geometric. ADA Blue or Charcoal only.

## 6. Motion
- **Principles:** Restraint over drama. Functional motion only (hover, reveal-on-scroll). No carousels or parallax.
- **Signature:** Loading states use a ticking Δ° (not a spinner). Form success briefly enlarges the ° glyph.
- All animation must respect `prefers-reduced-motion`.

---

## 7. ProofBar

A static trust bar placed between the nav and the hero. It scrolls with the page — not a sticky ticker, not an animated banner. Single line. One message per page.

- The `● LIVE` dot is animated in CSS (`live-pulse` keyframes, `#10B981` green) — it pulses to signal an active deployment.
- Background: surface-grey (`#f4f5f6`). Border-bottom: `1px solid #e8ebed`.
- Stat text is small (`0.875rem`) and carries the only proof point on the page.
- On `/corporate`: includes the Ashok Leyland logo at 20px height, centred with the stat.
- On `/demos`: not shown.
- Animation halts when `prefers-reduced-motion` is set (dot stays visible, just static).

See `developer_guidelines.md §6` for component spec and exact copy per page.

---

## 8. Ghost Buttons on Dark Backgrounds

On charcoal (`#28292a`) sections: ghost buttons must use **white text + white border**. ADA Blue on charcoal = ~1.4:1 contrast ratio — completely invisible.

- Default ghost: ADA Blue text + `--border-grey` border (light backgrounds only).
- Dark background override: `color: white; border-color: rgba(255,255,255,0.4)`.
- Hover on dark: white fill + charcoal text.

This is a **per-section scoped rule** — apply it to the specific container (`.cta-band`, `.pilot-actions`, `.featured-scenarios`), not globally.

---

## 9. Section Rhythm

No two consecutive sections with the same background colour. Alternate white and grey (`#f4f5f6`).

- `section` = white background
- `section--alt` = surface-grey background
- Dark charcoal bands count as a strong break and may follow either colour.

Pattern: white → grey → white → dark band → white → grey → ...

Every 2–3 sections must introduce a visual break: a dark band, a large ADA Blue display stat, or a full-bleed visual. Monotonous alternation without a break reads as generic.
