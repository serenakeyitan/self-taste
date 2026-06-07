# self-taste

> Derive **your** design taste from a folder of images you collected — then build websites & HTML/CSS in your eye, not the model's default.

`self-taste` is a portable [Agent Skill](https://github.com/vercel-labs/agent-skills). Point it at a folder of reference images (screenshots, product shots, posters, interiors — whatever shapes your eye) and it writes a **`TASTE.md`**: a concrete, buildable design system (real hex, fonts, spacing, component rules) that downstream skills consume to generate UI in *your* taste.

It's the **inverse of a hand-written style skill** — nobody writes the rules, your folder writes them.

## Why

Most "design taste" skills bake in *the author's* taste as fixed rules (ban serif, use this font, these dials). Useful, but it's not *yours*. `self-taste` reads the images *you* curated and extracts the aesthetic *you* already gravitate toward — then hands it to a build step.

## What it does

1. **Reads every image** in the folder (works whether the agent sees pixels directly or via a text-proxy).
2. **Per-image analysis** — palette (approx hex), typography feel, shape/depth, layout/density, motion intent, mood.
3. **Cross-image synthesis** — finds the *consensus* aesthetic: one accent, one corner-radius scale, one type direction. Taste is committing to decisions.
4. **Outlier flagging** — images that disagree with the majority are listed by filename with a reason, so you can re-curate instead of getting muddy "averaged" taste.
5. **Self-audit** (specificity / buildability / coherence / evidence) → writes **`TASTE.md`**.
6. **Build with craft** — when you build from the profile, self-taste layers the anti-slop rules of [**taste-skill**](https://github.com/Leonxlnx/taste-skill) on top (layout variety, eyebrow restraint, motivated motion, CTA + a11y discipline). Your taste decides the look; taste-skill keeps the execution from sliding into templated AI slop. They compose — and conflicts always resolve in favor of *your* taste.

The output format mirrors the `DESIGN.md` convention used across the agent-skills ecosystem, so it plugs straight into frontend/design build skills.

> **Why both?** In testing, sites built from a `TASTE.md` *alone* still drifted toward AI tells (an eyebrow above every section, three equal cards, centered heroes). Re-running the build with taste-skill's craft rules layered on produced a visibly cleaner, less templated result with the same aesthetic. So that pairing is now built into the skill.

## Install

```bash
npx skills add https://github.com/serenakeyitan/self-taste
```

Or just copy `SKILL.md` into your project / paste it into your agent.

## Use

1. Make a folder, drop in **3–40 images** you like.
2. Invoke the skill and point it at your folder:
   > "run self-taste on `~/Pictures/my-inspo`"
3. It writes `TASTE.md` into that folder.
4. Feed `TASTE.md` to a build step:
   > "build a landing page following `~/Pictures/my-inspo/TASTE.md`"

Richer real images → sharper profile.

## Worked example

[`sample-board/`](sample-board/) ships a complete example: **5 coherent "warm editorial minimalist" images + 1 deliberate neon-maximalist outlier**, plus the [`TASTE.md`](sample-board/TASTE.md) the skill extracts from them. The outlier is correctly flagged — that's the consensus + outlier-flagging behavior in action.

## Make it permanent (your own named style skill)

`TASTE.md` is already a usable design system. To freeze it into a reusable named skill:

1. Copy `sample-board/TASTE.md` as a template.
2. Wrap your synthesized rules in `name:` / `description:` frontmatter.
3. Save as `~/.claude/skills/<your-style-name>/SKILL.md`.

Now you can invoke your own taste by name on any future project.

## How it's structured

```
self-taste/
├── SKILL.md              # the skill (folder → TASTE.md protocol)
└── sample-board/         # DIY worked example
    ├── 01..05 *.png      # 5 coherent reference images
    ├── 06-OUTLIER-*.png  # 1 deliberate misfit
    ├── TASTE.md          # the extracted design system
    └── README.md         # how to run it / make your own
```

## Credits & acknowledgments

self-taste is designed to compose with [**taste-skill**](https://github.com/Leonxlnx/taste-skill) by **Leon** ([@lexnlin](https://x.com/lexnlin)), MIT-licensed — the anti-slop frontend framework for AI agents.

- **taste-skill** is a hand-authored *craft ruleset*: it encodes strong opinions on layout, typography, motion, and anti-templated structure to stop AI-built UIs from looking generic.
- **self-taste** is the inverse — it *derives* the aesthetic from your own images instead of prescribing one.

Used together, taste-skill supplies the execution discipline and self-taste supplies the personal aesthetic. self-taste invokes taste-skill's rules at build time (see `SKILL.md` §4.5) and the `TASTE.md` output format follows the same `DESIGN.md` convention taste-skill's ecosystem uses. Credit for the craft rules and the anti-slop philosophy goes to that project. 🙏

## License

[MIT](LICENSE)
