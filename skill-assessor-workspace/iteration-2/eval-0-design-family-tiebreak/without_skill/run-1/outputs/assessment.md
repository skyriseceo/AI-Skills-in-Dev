# Skill Assessment: Premium Redesign of a SaaS Analytics Dashboard

## The problem, restated

An existing SaaS analytics dashboard that reads as "AI-generated default" — stock gradients, identical card grids, templated look. Goal: redesign it so it feels premium and intentional, without breaking a working product. This is a **redesign of existing product UI** (not a greenfield build), and it's **data-heavy** (charts, KPIs, tables), which narrows the skill choice considerably.

## Recommended skills

### Primary: `design-master`

Best single match for this exact brief. Its description covers "redesigning, restyling, polishing... dashboards, product UI" and — verbatim — the case where "a UI feels bland, cheap, templated, or 'AI-generated' and needs to feel premium." It combines aesthetic direction, component craft, motion, and an **anti-generic quality review**, and it has explicit `plan` and `critique` modes, which fits the "assess first, don't start yet" instinct: run it in `plan`/`critique` mode first, then `polish`/`loop` for execution.

### Companion: `dataviz`

An analytics dashboard lives or dies on its charts, and generic chart defaults (rainbow categorical palettes, mismatched stat tiles, inconsistent axes) are half of what makes these dashboards look AI-generated. `dataviz` explicitly triggers on "dashboard", "analytics", "stat tile", "KPI row" and should be read **before writing any chart code** so the visualizations read as one coherent system with the new design language. `design-master` handles the shell, typography, and layout; `dataviz` handles the data layer.

### Strong alternative / possible addition: `taste-skill:redesign-skill`

This is the most precisely scoped alternative: "Upgrades existing websites and apps to premium quality. Audits current design, identifies generic AI patterns... without breaking functionality. Works with any CSS framework." Its audit-first, don't-break-functionality posture is exactly right for a working production dashboard. If you prefer one focused skill over the broad master skill, use this one — but don't run it *and* `design-master` simultaneously; they overlap heavily and would give conflicting direction. Pick one as the driver.

## Considered and set aside

- **`impeccable`** — broad, capable UI improvement skill covering dashboards, hierarchy, and anti-patterns. A legitimate second choice as the driver, but `design-master` targets the "looks AI-generated → make it premium" complaint more directly.
- **`emil-design-eng`** — excellent for the final polish pass (micro-interactions, invisible details) once the new direction is in place; optional phase-3 addition, not the driver.
- **`taste-skill:soft-skill` / `minimalist-skill` / `brutalist-skill`** — these impose a specific aesthetic. Only pull one in if you decide up front you want that exact direction (e.g. `brutalist-skill` explicitly targets data-heavy dashboards, if a raw/technical look fits your brand).
- **`frontend-design:frontend-design`** — solid general aesthetic guidance but subsumed by `design-master`.
- **`taste-skill:imagegen-*` / `image-to-code-skill` / `brandkit`** — image-generation/concepting skills; unnecessary since a live product already exists to iterate on.
- **`build`, `superpowers:*`** — process/execution skills, not design; only relevant if the redesign becomes a large multi-step implementation you want run end-to-end.

## Suggested plan (when you're ready to execute)

1. **Audit** — run `design-master` in `critique`/`full-eval` mode against the current dashboard to name the generic patterns concretely (gradients, card sameness, default shadows, font choices).
2. **Direction** — use `design-master` `plan` mode to pick one distinctive aesthetic direction (typography, palette, spacing system, density) and get sign-off before touching code.
3. **Data layer** — apply `dataviz` to rebuild charts, stat tiles, and KPI rows as one consistent, accessible system in the new palette.
4. **Execute** — `design-master` `polish`/`loop` mode to restyle the shell and components incrementally, preserving functionality.
5. **Final pass (optional)** — `emil-design-eng` for micro-interaction and detail polish.

**Bottom line:** drive the redesign with `design-master`, pair it with `dataviz` for everything chart-shaped, and keep `taste-skill:redesign-skill` in your pocket as the tighter-scoped alternative driver.
