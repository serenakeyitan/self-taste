# sample-board — worked example for the `self-taste` skill

This folder is a **DIY template**. It shows what the `self-taste` skill ingests and
what it produces.

## What's here
- `01-hero-editorial.png` … `05-nav-footer-system.png` — 5 reference images that
  share ONE aesthetic: **warm editorial minimalism** (off-white canvas, serif
  headlines, terracotta accent, airy layout).
- `06-OUTLIER-neon-maximal.png` — a deliberate misfit (neon maximalist) so the
  example demonstrates **outlier detection**.
- `TASTE.md` — the design system the skill extracted from these images. This is the
  output shape you can expect.

## Try it yourself
1. Make a folder, drop in 3–40 images you like (screenshots of sites, product
   shots, posters, interiors — whatever shapes your eye).
2. Invoke the `self-taste` skill and point it at your folder.
3. It writes a `TASTE.md` into that folder.
4. Feed that `TASTE.md` to a build skill (`design-taste-frontend`, `image-to-code`,
   `design-html`) to generate a site in your taste.

## Make it permanent (your own style skill)
Copy this folder's `TASTE.md`, wrap it in `name:` / `description:` frontmatter, and
save it as `~/.claude/skills/<your-style-name>/SKILL.md`. Now your taste is a
reusable named skill, just like the hand-written `minimalist-ui` / `soft` skills.

> Note: the PNGs here are simple SVG-rendered mockups, kept tiny on purpose. Real
> inspiration folders work better — the richer your images, the sharper the profile.
