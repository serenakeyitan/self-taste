# TASTE.md — Warm Editorial Minimalism

> Derived from 6 reference images in `sample-board/`. This is a personal design
> system extracted from collected inspiration. Feed it to any frontend/design skill
> to build in this taste.
>
> **This file is the worked example shipped with the `self-taste` skill** — it shows
> the exact output the extraction protocol produces. The board is intentionally 5
> coherent images + 1 deliberate outlier so you can see consensus + outlier-flagging
> in action.

## Source & Confidence
- Images analyzed: 6
- Consensus strength: **High** — 5 of 6 images share one tight aesthetic (warm
  editorial minimalism); only 1 image diverges.
- Outliers flagged (NOT included in this profile):
  - `06-OUTLIER-neon-maximal.png` — loud maximalist: neon lime + hot magenta on a
    dark purple→teal gradient, chunky all-caps display, "GET HYPED" energy. This
    breaks every trait of the calm warm-minimal consensus (palette, type, mood,
    density). **Recommendation:** remove it from the folder, or split it into its own
    board, for a cleaner profile.

## 1. Visual Theme & Atmosphere
Calm, warm, editorial minimalism — the feel of a well-made print catalogue for a
small ceramics studio. Off-white paper backgrounds, generous macro-whitespace, a
serif voice for headlines against quiet sans-serif support, and a single warm
terracotta accent used sparingly. Nothing shouts. Hierarchy comes from type scale
and space, not color or shadow. The overall impression: unhurried, tactile,
expensive-by-restraint. Built for brand, product, and editorial pages — not dense
dashboards.

## 2. Color Palette & Roles
- **Bone** (`#F7F5F1`) — canvas / page background (warm off-white, never blue-white)
- **Paper White** (`#FFFFFF`) — surfaces / cards
- **Warm Ink** (`#1F1D1A`) — primary text + inverted panels (warm near-black, never pure black)
- **Stone** (`#8C857B`) — secondary / muted text, captions, metadata
- **Hairline** (`#E7E2D9`) — borders, dividers, 1px structure
- **Accent — Terracotta** (`#B5482E`) — the ONE accent. Allowed on: small "new" tags,
  one active nav/link state, category eyebrows, a single small color block. Locked
  page-wide; no second accent.

### Banned for this taste
- AI-purple / violet / neon gradients (the exact thing the outlier does)
- Pure black `#000000` — always Warm Ink
- Cool/blue-grey neutrals — this palette is warm; do not mix in cool greys
- More than one accent on a page; saturated accents above ~70%

## 3. Typography
- **Display / Headlines:** Editorial serif, large, confident, not screaming. Tight
  tracking (`-0.01em` to `-0.02em`), tight leading (`~1.1`). Suggested: `Newsreader`,
  `Source Serif 4`, `Fraunces` (optical, low contrast), or a `Georgia`-class system
  serif as a no-load fallback.
- **Body / UI:** Quiet humanist/neo-grotesque sans. Relaxed leading (`~1.6`), max
  width ~65ch. Suggested: `Inter`, `Söhne`, or system `Helvetica Neue`. (Here the
  sans is deliberately neutral so the serif headlines lead.)
- **Mono:** Not present in the board — omit unless the project needs it.
- **Scale:** display `clamp(2.5rem, 6vw, 5.75rem)`; body `1.0625rem / 1.375rem`;
  eyebrow `0.8125rem` uppercase, wide tracking, in Terracotta or Stone.
- **Signature move:** serif headline + small uppercase wide-tracked eyebrow label
  above section heads (used sparingly — not above every section).

### Banned for this taste
- All-caps chunky display sans as the headline voice (that's the outlier)
- Mixing a random second serif/sans for emphasis — emphasize with italic/weight of
  the SAME family

## 4. Shape, Depth & Materiality
- **Corner radius:** soft — cards `10–14px`, tags pill (`9999px`), buttons `6px`.
  One scale, locked.
- **Borders vs shadows:** **border-driven.** 1px Hairline borders define structure;
  shadows are minimal.
- **Shadows:** very diffuse, very low opacity when used at all (`0 10px 30px -18px
  rgba(31,29,26,0.12)`). No hard dark drop shadows.
- **Surface treatment:** flat. White cards on Bone canvas. Occasional full Warm-Ink
  panel for a single high-contrast moment ("Made to last."). No glass, no texture,
  no gradients.

## 5. Layout & Density
- **Composition:** asymmetric and left-aligned. Heroes are split (≈60/40 text/visual),
  not centered. Editorial article pages are the exception — single centered column is
  fine there.
- **Density:** airy. Lots of breathing room per viewport; one idea per section.
- **Grid:** CSS Grid; product/feature sets as clean multi-column grids (3-up cards)
  with even gaps. Contain to ~`max-width: 1200–1400px`.
- **Whitespace:** large vertical section gaps; whitespace IS the separator as often
  as a hairline is.

## 6. Motion Intent (for the coding phase)
Restrained and slow. The board reads *still and editorial*, so motion should be
gentle: soft fade/slide entrances on scroll, calm hover states (subtle background
or color shift, `scale(0.98)` press), spring physics over linear. No kinetic
marquees, no aggressive scroll-hijacking — that would contradict "Quiet by design."
`MOTION_INTENSITY` ≈ 3–4 if handing to `design-taste-frontend`.

## 7. Anti-Patterns (what would betray this taste)
- Neon or purple gradients; any second accent color
- Centered SaaS hero with three equal glowing cards
- Pure black text or cool-grey neutrals
- Heavy drop shadows / glassmorphism / busy textures
- Chunky all-caps display headlines (the outlier's voice)
- Dense, data-packed layouts with no whitespace
- Loud kinetic motion / bouncing scroll chevrons

## 8. One-line brief for a coding agent
> "Build in this taste: warm editorial minimalism — Bone `#F7F5F1` canvas, white
> cards with 1px `#E7E2D9` hairlines, Warm-Ink `#1F1D1A` serif headlines over quiet
> sans body, a single Terracotta `#B5482E` accent, soft 12px corners, airy
> asymmetric layout, gentle restrained motion."
