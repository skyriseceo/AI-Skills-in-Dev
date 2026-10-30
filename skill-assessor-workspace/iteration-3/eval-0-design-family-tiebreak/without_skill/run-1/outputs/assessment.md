# Skill Assessment: Premium Redesign of a SaaS Analytics Dashboard

## The task in one line
Redesign an existing, working analytics dashboard so it stops looking "AI-generated" (default gradients, generic cards) and reads as premium — without breaking functionality.

## Recommended skill stack

### 1. Primary: `design-master`
This is the strongest single match. Its description covers this case almost verbatim: "when a UI feels bland, cheap, templated, or 'AI-generated' and needs to feel premium," and it explicitly lists dashboards and product UI. It combines aesthetic direction, premium component craft, motion, and an anti-generic quality review in one skill, and it has modes that fit your request exactly:

- `plan` mode — since you don't want the redesign started yet, this mode produces the design direction first.
- `critique` mode — can audit the current dashboard and name the specific generic patterns to eliminate.
- `full-eval` / `polish` — for the execution and review passes later.

### 2. Co-pilot: `taste-skill:redesign-skill`
Purpose-built for exactly this shape of work: "Upgrades existing websites and apps to premium quality. Audits current design, identifies generic AI patterns, and applies high-end design standards without breaking functionality." The audit-first, don't-break-anything discipline matters for a live work dashboard. Use it alongside (or instead of) design-master if you want the redesign anchored to a structured audit of the existing code rather than a fresh aesthetic direction.

### 3. Essential for the charts: `dataviz`
An analytics dashboard lives or dies on its charts, and generic design skills don't cover chart craft. `dataviz` must be read before writing any chart/KPI/stat-tile code — it provides a coherent visual system for graphs, palettes (sequential/diverging/categorical), stat tiles, sparklines, legends, tooltips, and light/dark consistency. This is the piece that separates a premium analytics product from a restyled template.

### 4. Optional polish layer: `emil-design-eng`
For the final pass on micro-interactions, component feel, and the "invisible details" (focus states, animation timing, hover behavior). Worth invoking during polish, not during initial direction.

## Skills considered and set aside
- `impeccable` — a strong general design/UX skill and a viable alternative anchor, but design-master's anti-generic/premium framing is a tighter fit for the stated complaint. Could substitute if you also want a deeper UX/IA/accessibility audit.
- `frontend-design:frontend-design` — good for aesthetic direction, but design-master subsumes it for this task.
- `taste-skill:soft-skill`, `taste-skill:taste-skill`, `taste-skill:minimalist-skill`, `taste-skill:brutalist-skill` — these encode specific aesthetics or agency defaults. Brutalist is actually an interesting stylistic option for a data-heavy dashboard, but it commits you to one look; better to let the audit/direction phase choose.
- `taste-skill:imagegen-frontend-web` / `image-to-code-skill` — image-first workflows suit new marketing pages more than restyling live product UI.
- `superpowers:brainstorming` — only needed if requirements are still fuzzy; the goal here is already clear.

## Proposed plan (when you're ready to start)
1. **Audit** — run `design-master` in `critique` mode (or `taste-skill:redesign-skill`'s audit) against the current dashboard to catalog the generic patterns: default gradients, uniform card grids, stock shadows, template typography.
2. **Direction** — `design-master` in `plan` mode: pick a distinctive aesthetic direction (typography, color system, spatial rhythm, density appropriate for analytics), with 2–3 candidate directions if you want options (`variants` mode).
3. **Chart system** — apply `dataviz` to define the visualization language: palette, mark specs, stat tiles/KPI rows, tooltips, light/dark behavior.
4. **Execute** — apply the redesign incrementally, preserving functionality (redesign-skill's discipline).
5. **Polish** — `design-master` `polish`/`motion` modes plus `emil-design-eng` for micro-interactions; finish with an anti-generic review pass (`full-eval`).

## Bottom line
Use **`design-master`** as the anchor, **`dataviz`** for the charts (non-negotiable for an analytics product), with **`taste-skill:redesign-skill`** as the audit/safety framework for restyling a live app and **`emil-design-eng`** for final polish.
