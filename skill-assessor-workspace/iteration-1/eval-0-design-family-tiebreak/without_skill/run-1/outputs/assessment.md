# Skill Assessment: Premium Redesign of a SaaS Analytics Dashboard

## The situation

You have an existing, working dashboard that reads as "AI-generated default" — stock gradients, uniform card grids, templated feel. Two constraints shape the skill choice:

1. **It's a redesign of a live product**, not a greenfield build — so an audit-first approach that doesn't break functionality matters.
2. **It's an analytics dashboard** — charts, KPIs, and data density are the core content, so chart/data-viz craft is as important as general visual styling.

## Recommended skills

### Primary: `taste-skill:redesign-skill`
This is the most direct match. It is purpose-built to "upgrade existing websites and apps to premium quality," and its workflow is exactly your problem statement: audit the current design, **identify generic AI patterns** (the same-gradients-same-cards problem by name), and apply high-end standards **without breaking functionality**. Framework-agnostic, so it works with whatever CSS setup your dashboard uses.

### Co-primary: `design-master`
The master frontend design skill — aesthetic direction, premium component craft, motion engineering, and an **anti-generic quality review**. Its description explicitly targets UI that "feels bland, cheap, templated, or 'AI-generated' and needs to feel premium," and it explicitly covers dashboards. Its modes map cleanly onto a phased redesign:

- `critique` — structured teardown of the current UI
- `plan` — a design direction before touching code
- `variants` — explore 2–3 distinct aesthetic directions instead of defaulting to one
- `polish` / `motion` — final-pass refinement

If you only want to invoke **one** skill, this is the one — it subsumes most of what the others do.

### Essential supporting: `dataviz`
Non-negotiable for an *analytics* dashboard. Its own trigger says to read it **before writing any chart, KPI row, stat tile, or dashboard layout code**. It provides a form heuristic, a color formula with a runnable validator, and light/dark-consistent palettes. Generic-looking charts would undermine an otherwise premium shell — this skill is what keeps the data layer at the same quality bar as the chrome around it.

### Optional additions (use as needed)

- **`impeccable:impeccable`** — broader UX audit: hierarchy, information architecture, cognitive load, accessibility, error/empty states, and live browser iteration. Worth adding if the dashboard has UX problems beyond aesthetics (confusing navigation, overloaded screens). Overlaps heavily with `design-master`, so pick it for the *UX-audit* angle specifically.
- **`emil-design-eng`** — Emil Kowalski's philosophy on polish, micro-interactions, and "invisible details." Good as a final-pass sensibility layer once the big moves are done.
- **A specific aesthetic direction** (only if you already know the vibe you want):
  - `taste-skill:minimalist-skill` — warm monochrome, editorial, flat bento grids, explicitly *no gradients* (a pointed antidote to your current look)
  - `taste-skill:brutalist-skill` — mechanical/terminal aesthetic, explicitly pitched at "data-heavy dashboards"
  - `taste-skill:soft-skill` — high-end-agency defaults (fonts, spacing, shadows) that block common cheap-looking AI defaults

### Skills to skip, and why

- `frontend-design:frontend-design` and `taste-skill:taste-skill` — legitimate design skills, but redundant with `design-master` + `redesign-skill`, which are more specific to your case (existing app, anti-AI-generic, dashboard).
- `taste-skill:imagegen-*`, `brandkit`, `stitch-skill`, `image-to-code-skill` — image-generation/mockup-first workflows; useful for greenfield concepting, overkill for restyling a working product.
- `build`, `superpowers:*` — execution/process machinery, not design judgment. `build` can come in later if you want the implementation run end-to-end.

## Suggested plan (when you're ready to start)

1. **Audit** — invoke `design-master` in `critique` mode (or `redesign-skill`'s audit step) against the current dashboard to name every generic pattern: gradient use, uniform card grids, default shadows, font choices, spacing rhythm.
2. **Direction** — `design-master` in `plan` or `variants` mode to establish 1–3 concrete aesthetic directions (typography, palette, layout system, motion language). Pick one.
3. **Data layer** — read `dataviz` before touching any chart, KPI tile, or stat row; rebuild the visualization palette and mark specs so charts read as one system in light and dark.
4. **Execute** — apply `redesign-skill`'s standards to restyle incrementally without breaking functionality.
5. **Polish** — final pass with `design-master` `polish`/`motion` (optionally `emil-design-eng`) for micro-interactions and detail work; optionally an `impeccable` audit for accessibility, empty states, and edge cases.

## Bottom line

- **Minimum viable combo:** `design-master` + `dataviz`.
- **Best-fit full combo:** `taste-skill:redesign-skill` (audit-first, safe restyling) + `design-master` (direction, variants, polish) + `dataviz` (charts/KPIs), with `impeccable` and `emil-design-eng` as optional deepeners.
