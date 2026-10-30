# Skill Assessment

**Task:** Redesign an existing SaaS analytics dashboard (working product UI) so it stops looking generic/AI-generated and feels premium — assessment only, no execution yet.

**Plan:**
1. skill: design-master — primary: audit the current dashboard, set an aesthetic direction, and drive the full redesign (typography, color, layout, components, motion) with an anti-generic quality review pass — its contract names this exact case verbatim: "redesigning… dashboards… when a UI feels bland, cheap, templated, or 'AI-generated' and needs to feel premium." Since you want the plan before any work, invoke it in `plan` mode first (then `full-eval`/`polish` when you're ready to execute).
2. skill: dataviz — supporting, layered during implementation: an analytics dashboard is chart-heavy, and dataviz's contract requires reading it before writing any chart/stat-tile/KPI code so the visualizations read as one coherent, accessible system instead of library defaults. It hands its palette/mark specs to the components design-master restyles.

**Considered and rejected:**
- skill: taste-skill:redesign-skill — the closest runner-up; it explicitly claims "upgrades existing websites and apps to premium quality, identifies generic AI patterns, without breaking functionality." It ties design-master on problem (generic AI patterns) and goal (premium), but design-master's claim covers your framing more completely — it names the artifact ("dashboards") and the complaint ("AI-generated") verbatim, and adds motion + review modes the redesign will need. What would flip it: if your top constraint were a conservative, framework-agnostic upgrade that must not touch behavior at all, redesign-skill's audit-first, don't-break-functionality contract would win.
- skill: impeccable — broad contract covering redesign, dashboards, hierarchy, and "bland designs that need to become bolder," so it plausibly wins on scope; but its claim is generic across all UI improvement while design-master names your exact situation. Flips if the redesign turns out to be mainly a UX/IA/accessibility problem (confusing dashboard, not an ugly one) rather than an aesthetic one.
- skill: taste-skill:taste-skill — anti-slop with audit-first redesign behavior, a genuine contender; loses because its stated focus is landing pages/portfolios, while design-master explicitly covers dashboards and product UI. Flips if the redesign target were a marketing site rather than product UI.
- Not close: frontend-design:frontend-design, emil-design-eng, taste-skill:soft-skill / minimalist-skill / brutalist-skill / gpt-tasteskill / stitch-skill — narrower slices of the same design family (guidance, polish philosophy, or a single aesthetic preset) that design-master already subsumes as the master skill.
- Not close: taste-skill:imagegen-frontend-web / imagegen-frontend-mobile / image-to-code-skill / brandkit — wrong output mode: they produce design images/brand assets, not a redesign of working dashboard code.
- Not close: superpowers:brainstorming, superpowers:writing-plans, build — process/pipeline skills; design-master's own plan/full-eval modes cover direction-setting and staging for this task, and no build was requested.

**Blockers:** The dashboard's codebase isn't in this workspace — no input files were provided. Before execution, point at the repo (and note the framework/CSS setup) so the audit has something to read.

**Verdict:** Run skill: design-master (start in plan mode) as the single primary, layering skill: dataviz when restyling the charts — assessment only for now, per your request.
