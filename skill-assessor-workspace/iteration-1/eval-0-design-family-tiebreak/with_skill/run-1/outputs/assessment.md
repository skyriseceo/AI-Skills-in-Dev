## Skill Assessment

**Task:** Redesign an existing SaaS analytics dashboard so it stops looking generic/AI-templated and feels premium — working product UI, not comps.

**Plan:**
1. **design-master** — primary skill; drives the whole redesign (audit → aesthetic direction → component craft → motion → anti-generic review) — its description explicitly claims this exact trigger ("a UI feels bland, cheap, templated, or 'AI-generated' and needs to feel premium") and explicitly covers dashboards. It spans the full task in one skill, and its `plan` / `critique` modes fit a "plan first, redesign later" workflow. One skill covering ~90% beats stitching two design skills together with a seam.
2. **dataviz** — supporting skill for the charts, KPI tiles, and stat rows inside the dashboard — an analytics dashboard is mostly data visualization, and dataviz's contract says to read it *before writing any chart code or choosing chart colors*. design-master handles the shell and identity; dataviz makes the viz layer read as one coherent, accessible system instead of default library output.

**Considered and rejected:**
- **taste-skill:redesign-skill** — the closest competitor; it also claims "upgrades existing sites to premium, audits generic AI patterns." It loses on coverage: design-master claims the user's exact trigger phrase ("AI-generated") *and* names dashboards explicitly, and additionally covers motion and review modes redesign-skill doesn't. Running both would be redundant — same domain, one continuous activity.
- **impeccable** — strong general UI-improvement skill (covers dashboards, redesign, hierarchy), but its center of gravity is UX/IA/accessibility auditing; the user's stated problem is aesthetic sameness and premium feel, which design-master claims verbatim.
- **taste-skill:taste-skill / soft-skill** — anti-slop and premium-standards skills, but scoped mainly to landing pages/portfolios and greenfield styling defaults; design-master subsumes their anti-generic role for product UI.
- **frontend-design:frontend-design** — aesthetic-direction guidance only; a subset of what design-master already provides, no redesign/audit workflow.
- **emil-design-eng** — polish philosophy and micro-detail sensibility; useful reading, but it's a lens, not a redesign workflow — design-master's polish/motion modes cover it.
- **taste-skill:minimalist-skill / brutalist-skill** — these prescribe a specific aesthetic upfront. Picking a visual language before the audit is exactly the templated-thinking the user is trying to escape; direction should fall out of design-master's process.
- **taste-skill:imagegen-frontend-web / image-to-code-skill / stitch-skill** — produce design images or DESIGN.md specs, wrong output type: the user needs their real dashboard restyled, not comps.
- **superpowers:brainstorming** — the user already knows the goal (premium, non-generic) and explicitly asked for a plan, not idea exploration; design-master's own `plan`/`critique` modes handle the discovery step.
- **build** — execution pipeline; the user said not to start the redesign yet.

**Verdict:** Run **design-master** (start in `critique`/`plan` mode to audit the current dashboard and set direction, then `polish`/`full-eval` for the redesign itself), pulling in **dataviz** when touching any chart, KPI tile, or visualization — no other skill needed. Per your request, this is the assessment only; the redesign has not been started.
