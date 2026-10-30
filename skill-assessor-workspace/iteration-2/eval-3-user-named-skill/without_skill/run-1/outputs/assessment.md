# Skill Routing Assessment

**Task:** Redo a personal portfolio landing page (plain HTML/CSS in `./site`) using `taste-skill:minimalist-skill`.
**Mode:** Assessment only — no execution performed.

## 1. Requested skill: `taste-skill:minimalist-skill`

- **Availability:** Confirmed present in the available-skills list.
- **Description:** "Clean editorial-style interfaces. Warm monochrome palette, typographic contrast, flat bento grids, muted pastels. No gradients, no heavy shadows."
- **Fit:** Good. A personal portfolio landing page is squarely in this skill's wheelhouse — editorial, typography-forward, minimal aesthetics are a natural (and common) direction for portfolios. The skill is a *style/aesthetic directive* skill rather than a process skill.
- **Routing verdict:** The user explicitly named this skill, so it should be the primary directive. Explicit user skill selection overrides "closest match" reasoning — honor it unless it is clearly incapable of the task, which it is not.

## 2. Overlapping candidates (and why they lose or complement)

| Skill | Relationship to task | Disposition |
|---|---|---|
| `taste-skill:redesign-skill` | Arguably the *most specific* match by task shape: "Upgrades existing websites... audit-first... works with vanilla CSS." This is a redesign of an existing site, not a greenfield build. | Do not substitute (user chose minimalist-skill), but its audit-first posture is worth borrowing: read the existing `./site` files before rewriting, preserve content and any functionality. |
| `taste-skill:taste-skill` | Explicitly covers "portfolios, and redesigns", audit-first. General anti-slop direction; minimalist-skill is a more opinionated subset. | Superseded by the explicit user choice. |
| `design-master` / `impeccable` / `frontend-design` / `emil-design-eng` / `taste-skill:soft-skill` | Broad design-quality skills that would also trigger on "redo my portfolio landing page." | Do not stack by default. Loading multiple aesthetic skills risks conflicting directives (e.g., soft-skill's shadow/animation prescriptions vs. minimalist-skill's "no gradients, no heavy shadows"). |
| `taste-skill:imagegen-frontend-web` / `image-to-code-skill` / `brandkit` | Image-generation pipelines. | Not requested; unnecessary for a plain HTML/CSS rework. |
| `superpowers:brainstorming` | Claims it must precede creative work. | The user has already made the creative decision (minimalist direction, named skill). A long brainstorm would contradict explicit direction; at most a brief content/scope confirmation is warranted. |
| `build` | End-to-end autonomous pipeline. | Overkill for a single static page; also the user asked for routing assessment first, not autonomous execution. |

## 3. Recommended execution plan (when approved)

1. Resolve the blocker in section 4 (locate the actual site files).
2. Invoke `Skill(taste-skill:minimalist-skill)` — required first step since the user explicitly named it; it must be invoked before any design work, not merely referenced.
3. Audit the existing `./site` HTML/CSS first (content inventory: name, bio, projects, links) so the redo preserves the user's actual content and assets.
4. Rebuild the landing page per the skill's constraints (warm monochrome, typographic contrast, flat bento grids, muted pastels; no gradients or heavy shadows), plain HTML/CSS — no framework introduction.
5. Optional post-step: a quick review pass (e.g., `verify`/browser check) to confirm the page renders correctly; no other design skills loaded unless the user asks.

## 4. Blocker found during assessment

`./site` **does not exist** under the working directory `/home/ah/Projects/SkySkills` (contents: `.impeccable`, `prompt-enhancer`, `prompt-enhancer-workspace`, `skill-assessor`, `skill-assessor-workspace`). Before execution, the user must either point to the correct path of the existing site or confirm it should be created from scratch — which also changes the routing nuance (a from-scratch build weakens the case for any audit-first behavior and makes minimalist-skill alone fully sufficient).

## 5. Summary verdict

- **Route to:** `taste-skill:minimalist-skill` (user-named, available, well-suited). Confidence: high.
- **Do not stack** other aesthetic skills; optionally borrow redesign-skill's audit-first habit informally.
- **Gate on:** locating the real `./site` directory — currently missing, so execution cannot start as specified.
