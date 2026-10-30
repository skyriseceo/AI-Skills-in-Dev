# Skill Assessment

**Task:** Redesign an existing SaaS analytics dashboard so it stops looking generic/AI-templated and feels premium — routing plan only, no execution yet.

**Plan:**
1. skill: taste-skill:redesign-skill — primary: audit the current dashboard, identify the generic AI patterns (stock gradients, interchangeable cards), and upgrade it to premium quality without breaking functionality — it wins because it is purpose-built for exactly this shape of work: an *existing* app being upgraded, audit-first, framework-agnostic, with "identifies generic AI patterns" named verbatim in its contract.
2. skill: dataviz — supporting, invoked when the redesign touches the charts/KPI tiles/stat rows that make this an *analytics* dashboard — its contract requires reading it before restyling any chart or dashboard layout, and a premium analytics redesign lives or dies on chart color systems, legends, and stat-tile consistency, which the general design skills don't cover.

**Considered and rejected:**
- skill: design-master — the closest call in the pool. Its description also explicitly names "feels AI-generated and needs to feel premium" and covers dashboards, so both survive the explicit-claim rung; the tie breaks on specificity: design-master spans the whole design family (build, motion, variants, critique), while redesign-skill is scoped to precisely this artifact — upgrading an existing app to premium without breaking it. Flips if the scope grows beyond restyling (new pages/features to build, heavy motion work, or the user wants iterative critique loops and variant generation) — then design-master's breadth wins.
- skill: impeccable:impeccable — strong general-purpose UI improvement skill that lists dashboards and "bland designs that need to become bolder". Loses because the user's complaint is purely aesthetic (gradients, cards, premium feel), not UX/IA/accessibility/cognitive-load — impeccable's center of gravity. Flips if the redesign also needs UX restructuring, information-architecture changes, or an accessibility/anti-pattern audit.
- skill: taste-skill:soft-skill — teaches the exact premium vocabulary (fonts, spacing, shadows, blocked defaults) but is a design-standards layer, not a redesign workflow; redesign-skill already applies high-end standards *plus* the audit-first process for existing apps. Flips only if redesign-skill were absent.
- skill: frontend-design:frontend-design — right theme (non-templated, intentional visual design) but thinner contract: aesthetic-direction guidance without the existing-app audit workflow or the anti-AI-pattern checklist the task centers on.
- Not close: taste-skill:taste-skill, taste-skill:minimalist-skill, taste-skill:brutalist-skill, taste-skill:stitch-skill, taste-skill:gpt-tasteskill, emil-design-eng — same family but wrong scope: aimed at landing pages/new builds, or they impose one specific aesthetic rather than fitting a redesign of this product.
- Not close: taste-skill:imagegen-frontend-web, taste-skill:imagegen-frontend-mobile, taste-skill:image-to-code-skill, taste-skill:brandkit, animation-vocabulary — produce the wrong artifact (design images, brand boards, terminology), not a code-level redesign.
- Not close: build, superpowers:brainstorming and the other superpowers process skills, codex agents, cavecrew agents — execution/process machinery; the user asked for routing only, and redesign-skill's own audit-first flow covers the discovery phase when execution starts.

**Blockers:** The dashboard's codebase (or URL) hasn't been provided — before execution can start, we need access to the project files, plus the stack/CSS framework in use and any brand constraints (logo, existing palette) the redesign must respect.

**Verdict:** Run taste-skill:redesign-skill as the primary on the dashboard codebase, pulling in dataviz for the chart and stat-tile work — not started yet, pending the codebase location.
