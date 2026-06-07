---
name: self-taste
description: Derive a personal design taste profile from a folder of reference images you collected, then write it to TASTE.md for building websites and HTML/CSS that match your eye. Reads every image, analyzes palette / typography / spacing / components / mood per image, finds the consensus aesthetic across the folder, flags images that disagree, and outputs a ready-to-use TASTE.md design system. When building from that profile, it pairs the derived taste with the anti-slop craft rules of the taste-skill framework (github.com/Leonxlnx/taste-skill) so output is leveled-up, not templated AI slop. Use when the user points at an image folder and wants to capture "my taste", "my style", a "vibe", or a reusable design system from screenshots / inspiration / a mood board.
---

# self-taste: Derive YOUR Design System From a Folder of Images

You collect screenshots and images you love. This skill turns that folder into a
**TASTE.md** — a concrete design system (real hex, fonts, spacing, component rules)
that downstream skills use to build websites and HTML/CSS in *your* taste, not the
model's default taste.

> This is the **inverse** of a hand-written style skill. Nobody writes the rules.
> Your folder writes them. The output plugs straight into `design-taste-frontend`,
> `image-to-code`, `design-html`, or any agent that reads a DESIGN.md-style file.

---

## 0. WHEN THIS FIRES

Fire when the user gives you (or points at) a **folder of images** and wants a
reusable aesthetic out of it. Trigger phrases: "extract my taste", "my style from
these", "build a design system from this folder / mood board / screenshots",
"capture this vibe", "make a TASTE.md".

Do NOT fire for: a single image to clone 1:1 (that's `image-to-code`), or a brief
with no images (that's `design-taste-frontend` / `design-consultation`).

---

## 1. INPUTS — Locate and validate the folder FIRST

Before any analysis:

1. **Get the folder path.** If the user named a path, use it. If they said "this
   folder" with no path, ask for the absolute path — one short question, do not guess.
2. **List the images.** Find image files (`.png .jpg .jpeg .webp .gif .avif`),
   recursively if the folder has subdirs. Print the count and the filenames so the
   user sees what's in scope.
3. **Gate on count:**
   - **0 images** → stop, tell the user the folder is empty / has no images.
   - **1–2 images** → warn: "Taste needs a few examples to find a pattern. With N
     image(s) I can only describe these specific shots, not a reliable taste. Add
     more, or I'll proceed as a single-image read." Then proceed if they confirm.
   - **3–40 images** → ideal. Proceed.
   - **> 40 images** → proceed, but tell the user you'll sample for analysis (see
     §2 sampling note) so the pass stays focused.
4. **Never assume what's in the folder.** You must actually read the images. A taste
   profile invented without looking at the pixels is the failure this skill exists
   to prevent.

---

## 2. THE EXTRACTION PROTOCOL (agentic, but rigorous)

The whole point is to be **specific, not hand-wavy**. Vague output ("clean, modern,
minimalist") is useless — it describes half the internet. Extract concrete,
buildable values. Three phases: per-image read → cross-image synthesis → self-audit.

### Dials (set these before you start, state them in one line)
- **`ANALYSIS_PRECISION: 8`** — 1 = broad vibe only, 10 = deep per-pixel extraction.
  Default 8. Raise to 10 when the user says "be thorough" or the folder is small.
- **`CONSENSUS_STRICTNESS: 7`** — 1 = blend everything loosely, 10 = only keep
  traits shared by nearly every image. Default 7 → dominant aesthetic kept, clear
  outliers flagged.

> **Read the images directly.** Use the Read tool on each image file. Depending on
> the environment you'll either see the actual pixels, or — on setups with an image
> proxy hook — get a structured text description of the image. Both are fine; analyze
> whatever the Read returns. What's NOT fine is analyzing from filenames alone:
> always Read the file. For folders larger than ~25 images, read a representative
> sample that spans the visual range (don't just read the first 25 alphabetically —
> skim filenames and pick a spread), and say how many you sampled.

### Phase A — Per-image read (one structured pass per image)

For **each** image, extract these dimensions. Keep it tight — a few words each, real
values where visible. This is the same analysis backbone proven in the repo's
image-to-code skill (color extraction, typography, spacing, component inspection),
applied to *collected* images instead of generated ones.

| Dimension | What to capture |
|---|---|
| **Type** | What is this image? (website hero, full landing, app UI, poster, product photo, editorial spread, packaging, interior, photograph, illustration…) |
| **Palette** | 3–6 dominant colors as **approximate hex** + role (background / text / accent / surface). Eyeball the hex from what you see. |
| **Contrast & value** | Light / dark / mid. High-contrast or soft? Warm or cool neutrals? |
| **Typography feel** | Serif vs sans vs mono; geometric vs humanist vs grotesque; weight range; tight vs loose tracking; display-driven vs body-driven. Name a lookalike font only if confident. |
| **Layout & composition** | Symmetric vs asymmetric; centered vs split; dense vs airy; grid vs free; where the whitespace lives. |
| **Shape language** | Corner radius (sharp 0 / soft 8–16px / pill); border-driven vs shadow-driven vs flat. |
| **Depth & material** | Flat / subtle shadow / glassy / heavy. Texture, grain, gradients (and what kind — none / soft / mesh / duotone)? |
| **Motion implication** | Does it *imply* stillness or movement? (static editorial vs kinetic/energetic). Images can't move — infer intent. |
| **Mood words** | 2–4 honest adjectives. Be precise: "calm / clinical / warm" beats "nice / modern". |

Record this as a compact per-image table or list. You'll synthesize from it next —
don't write TASTE.md yet.

### Phase B — Cross-image synthesis (find the consensus)

Now look across all the per-image reads and find the **shared aesthetic** — the
through-line that most images agree on. This is where taste actually lives.

1. **Cluster the traits.** For each dimension (palette, type, shape, density, mood),
   what shows up repeatedly? The recurring values ARE the taste.
2. **Resolve the palette.** Don't just dump every color. Derive a coherent system:
   1 canvas/background, 1 surface, 1–2 text values, **exactly 1 accent** (the most
   consistently-appearing non-neutral). Give each a name + hex + role, like a real
   design system. Harmonize neutrals to one temperature (all warm or all cool).
3. **Resolve typography.** One display direction + one body direction + mono if
   present. Suggest 2–3 real fonts that match the *feel* (e.g. geometric-sans feel →
   Geist / Outfit / Satoshi). Never claim to know the exact font unless it's obvious.
4. **Resolve shape, depth, density, motion** into single decisions each. Taste means
   committing: "soft 12px corners, shadow-light, airy density, gentle motion."
5. **Flag the outliers (consensus + flag).** List any images that clearly DON'T fit
   the dominant aesthetic, by filename, with one line why ("`poster-03.jpg` — loud
   maximalist color, breaks the calm-monochrome consensus"). Recommend the user
   re-curate (remove or split them) for a cleaner profile. Do NOT silently average
   outliers into the main profile — that produces muddy, incoherent taste.
   - If the folder splits roughly 50/50 into two coherent groups (no clear
     majority), say so and offer to produce two profiles (`TASTE-a.md` /
     `TASTE-b.md`) instead of forcing one — but default to a single consensus
     profile when there's a clear dominant group.

### Phase C — Self-audit before writing

Before you write TASTE.md, check your own synthesis:

- **Specificity test:** Could this profile describe a totally different folder? If
  yes, it's too generic — go back and sharpen with concrete values.
- **Buildability test:** Can a frontend agent build a page from this with no further
  questions? Every section must have actionable values (hex, font names, px/rem,
  named decisions), not just adjectives.
- **Coherence test:** One palette temperature? One accent? One corner-radius scale?
  One type direction? If the profile contradicts itself, it'll produce broken UI.
- **Evidence test:** Every major claim should trace to images you actually saw. If
  you can't point to which images support a trait, drop it.

---

## 3. OUTPUT — Write TASTE.md

Write a single file named **`TASTE.md`**. Default location: **inside the analyzed
image folder** (so the taste lives with its source). If the user prefers the repo
root or another path, honor that.

Use this structure — it mirrors the proven DESIGN.md format used across the taste-
skill ecosystem, so downstream skills (`design-taste-frontend`, `image-to-code`,
`design-html`, Stitch/Antigravity exports) consume it directly.

```markdown
# TASTE.md — <short name for this aesthetic>

> Derived from <N> reference images in `<folder>` on <date — ask or omit, do not
> invent>. This is a personal design system extracted from collected inspiration.
> Feed it to any frontend/design skill to build in this taste.

## Source & Confidence
- Images analyzed: <N> (<sampled M> if sampled)
- Consensus strength: <High / Medium / Low> — <one line>
- Outliers flagged (not included in this profile): <filenames + why, or "none">

## 1. Visual Theme & Atmosphere
<2–4 sentences. The honest through-line. What this aesthetic *feels* like and what
it is for. Concrete, not marketing fluff.>

## 2. Color Palette & Roles
- **<Name>** (`#hex`) — <role: canvas / background>
- **<Name>** (`#hex`) — <role: surface / cards>
- **<Name>** (`#hex`) — <role: primary text>
- **<Name>** (`#hex`) — <role: secondary / muted text>
- **<Name>** (`#hex`) — <role: borders / dividers>
- **Accent — <Name>** (`#hex`) — <where it's allowed; ONE accent, locked page-wide>

### Banned for this taste
<Colors/effects that would break it, derived from what's absent in the images —
e.g. "no AI-purple gradients", "no pure black", "no warm beige" if the board is cool.>

## 3. Typography
- **Display / Headlines:** <feel> — suggested: `<Font A>`, `<Font B>`. Tracking
  <value>, leading <value>, weight <range>.
- **Body:** <feel> — suggested: `<Font>`. Leading <value>, max-width ~65ch.
- **Mono (if present):** <feel> — suggested: `<Font>`.
- **Scale:** display `clamp(...)`, body `1rem/1.125rem`.

### Banned for this taste
<Type choices that contradict the board — e.g. "no serif" if board is all-sans.>

## 4. Shape, Depth & Materiality
- **Corner radius:** <one scale — sharp 0 / soft 12–16px / pill>. Locked.
- **Borders vs shadows:** <which dominates>.
- **Shadows:** <none / diffuse low-opacity / specifics like `0 20px 40px -15px rgba(...)`>.
- **Surface treatment:** <flat / glass / texture / gradient — and what kind>.

## 5. Layout & Density
- **Composition:** <symmetric / asymmetric / split / centered>.
- **Density:** <airy / balanced / dense> — <how much breathes per viewport>.
- **Grid:** <preference>.
- **Whitespace:** <where it lives>.

## 6. Motion Intent (for the coding phase)
<Stillness vs movement implied by the board. Spring vs linear feel. Restraint level.
Note: images are static — this is inferred intent for whoever implements.>

## 7. Anti-Patterns (what would betray this taste)
- <bullet>
- <bullet>
<The "don't" list specific to THIS aesthetic — the strongest anti-slop signal,
because it's grounded in what the user's own images avoid.>

## 8. One-line brief for a coding agent
> "Build in this taste: <compressed one-sentence summary an agent can act on>."

## 9. Build with
Pair this profile with the **taste-skill** anti-slop craft rules when building
(layout variety, eyebrow restraint, motivated motion, CTA + a11y discipline,
real images). The taste above decides the look; taste-skill keeps the execution
honest. Conflicts resolve in favor of THIS taste.
> taste-skill — https://github.com/Leonxlnx/taste-skill (MIT)
```

### Rules for the written profile
- **Real values, not adjectives.** "Warm bone `#F7F6F3` canvas" beats "light background".
- **Commit to one of everything** (one accent, one radius scale, one type direction,
  one neutral temperature). Taste is a set of decisions, not a menu.
- **The Banned / Anti-Pattern sections are the highest-value part** — they're what
  stop a downstream agent from defaulting to slop. Derive them from what the images
  conspicuously avoid.
- **Be honest about confidence.** If the board is visually scattered, say "Low
  consensus" and recommend re-curation rather than faking a crisp system.

---

## 4. AFTER WRITING

1. Show the user the path to `TASTE.md` and a 3–5 line summary (the atmosphere line,
   the palette, the type direction, any flagged outliers).
2. Tell them how to use it next, concretely:
   > "Feed this to a build: follow `<path>/TASTE.md` **plus the taste-skill craft
   > rules** (see §4.5). `design-taste-frontend`, `image-to-code`, or `design-html`
   > all consume the profile directly."
3. If outliers were flagged, remind them: re-running on a cleaner folder yields a
   sharper profile.
4. Offer the two-profile split only if §2 Phase B found a genuine 50/50.

Do NOT generate a visual preview/swatch page — this skill outputs `TASTE.md` only.

---

## 4.5 BUILD PHASE — derived taste × taste-skill craft (do this when you build)

`TASTE.md` decides **what** the aesthetic is. It does not, on its own, stop an agent
from shipping templated AI slop (eyebrow above every section, three equal cards,
centered hero, AI-purple gradient, motion-for-motion's-sake). When you actually
**build** from a profile — a website, a landing page, HTML/CSS — layer the anti-slop
craft rules of the **taste-skill** framework on top:

> **taste-skill** by Leon (`@lexnlin`) — github.com/Leonxlnx/taste-skill — MIT.
> An anti-slop frontend ruleset for AI agents. self-taste supplies *your* aesthetic;
> taste-skill supplies the execution discipline. They compose: taste decides the
> look, taste-skill keeps the craft honest.

Apply at minimum these taste-skill rules (they refine without changing the look):

- **Layout-family variety** — ≥4 distinct section layouts across the page. Never
  repeat the same card-row pattern down the whole page.
- **Eyebrow restraint** — at most 1 small uppercase label per ~3 sections (the #1
  AI tell is one above every heading).
- **Anti-center bias / zigzag cap** — vary composition; don't stack centered heroes
  or alternate left/right image-text more than twice in a row.
- **Motion must be motivated** — every animation communicates hierarchy, story,
  feedback, or state. Always ship a `prefers-reduced-motion` fallback. One marquee
  max per page.
- **CTA discipline** — one CTA *intent* across the page, one-line labels, WCAG-AA
  contrast on every button. Hero fits the viewport.
- **Real images, no fake screenshots** — use the user's own folder images (or a real
  image source). No div-based fake dashboards, no hand-rolled decorative SVG.

**Conflict rule (important): the user's derived taste WINS.** Many taste-skill rules
are *defaults to avoid* (no neon/RGB gradients, no maximalism, no serif-as-default).
If `TASTE.md` says the aesthetic IS neon-synthwave or IS Y2K-maximalist, KEEP it —
apply taste-skill only where it doesn't kill the vibe (layout variety, motion
discipline, a11y, anti-templated structure). Refine, don't neuter. State in one line
which conflicts you resolved in favor of the taste.

If `~/.claude/skills/` has the `design-taste-frontend` (taste-skill) skill installed,
invoke it for the build. If not, apply the rules above inline. The taste-skill repo
also ships ready-made style skills (`minimalist-ui`, `soft`, `brutalist`) — point the
user there if their TASTE.md matches one.

---

## 5. DIY — make your own style skill from this

`TASTE.md` is already a usable design system. To turn it into a permanent, reusable
**style skill** (like the hand-written `minimalist-ui` / `soft` / `brutalist`
skills), copy `sample-board/TASTE.md` in this skill as a template and adapt:
- Put the synthesized rules under a `name:`/`description:` frontmatter block.
- Save as `~/.claude/skills/<your-style-name>/SKILL.md`.
- Now you can invoke your own taste by name on any future project.

A worked example lives in `sample-board/` (a small reference set + the `TASTE.md`
this protocol produces from it). Read it to see the expected output shape.

---

## Credits

self-taste composes with **taste-skill** by Leon (`@lexnlin` /
[github.com/Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill), MIT) —
the anti-slop frontend ruleset whose craft rules this skill layers on at build time
(§4.5). self-taste derives *your* aesthetic from images; taste-skill keeps the
execution from sliding into templated AI slop. They are designed to be used together.
