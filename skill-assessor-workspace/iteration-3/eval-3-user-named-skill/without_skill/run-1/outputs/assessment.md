# Skill Routing Assessment — "Redo my portfolio landing page with taste-skill:minimalist-skill"

## Verdict

**Route to `taste-skill:minimalist-skill` as the primary skill — the user named it explicitly, it exists in the available-skills list, and its description fits the task.** However, execution is currently **blocked**: `./site` does not exist in the working directory (`/home/ah/Projects/SkySkills`), so the target path must be clarified before any skill is invoked.

## 1. Does the named skill exist and match?

Yes. `taste-skill:minimalist-skill` is registered:

> "Clean editorial-style interfaces. Warm monochrome palette, typographic contrast, flat bento grids, muted pastels. No gradients, no heavy shadows."

That is an aesthetic-direction skill, and a personal portfolio landing page in plain HTML/CSS is squarely in its lane. An explicit user-named skill is a blocking requirement per the Skill tool contract: when the user types a skill by name, it should be invoked before generating any other response about the task. Routing should honor the user's choice rather than substituting a "better" skill.

## 2. Overlapping / competing skills (and why they lose or complement)

| Skill | Relationship | Routing decision |
|---|---|---|
| `taste-skill:redesign-skill` | Strong overlap — "Upgrades existing websites... audits current design... without breaking functionality. Works with... vanilla CSS." Arguably the best *workflow* fit for "redo an existing site." | Do **not** substitute it silently. Optionally mention it to the user as a complement (audit-first workflow) with minimalist-skill supplying the aesthetic. |
| `taste-skill:taste-skill` | Overlap — covers portfolios and redesigns, "audit-first on redesigns." | Superseded by the explicit user choice. |
| `taste-skill:soft-skill`, `taste-skill:brutalist-skill`, `taste-skill:gpt-tasteskill` | Sibling aesthetic skills. | Conflicting aesthetics — must not be stacked with minimalist-skill. One taste-direction skill at a time. |
| `design-master`, `impeccable`, `frontend-design` | Broad "use whenever building/redesigning any web UI" triggers that would normally claim this task. | The user's explicit skill naming overrides these auto-triggers. Could be used later for a quality pass, but not as the router's primary pick. |
| `superpowers:brainstorming` / `superpowers:using-superpowers` | Claim mandatory invocation before creative work. | The user gave a concrete brief and named the tool; forcing a brainstorming detour would contradict the direct instruction. Not routed. |
| `build` | Only if the user wanted an autonomous end-to-end pipeline. Not indicated. | Not routed. |
| `taste-skill:imagegen-frontend-web` / `image-to-code-skill` | Image-generation-first workflows. Not requested. | Not routed. |

## 3. Blockers and clarifications needed before executing

1. **`./site` does not exist.** `/home/ah/Projects/SkySkills` contains only `docs`, `prompt-enhancer*`, and `skill-assessor*` directories — no `site/` and no HTML files. The user must confirm the actual path (different repo? not yet created? typo?). This is the single hard blocker.
2. **No git repo** in the working directory, so there's no cheap rollback for a destructive "redo." Worth flagging (suggest a backup or `git init`) before rewriting files.
3. Minor scope question (non-blocking): "redo" the landing page only, or the whole site if multiple pages exist? Can be resolved once the directory is found.

## 4. Recommended execution plan (once unblocked)

1. Confirm/locate the real site directory with the user.
2. Invoke `Skill(skill="taste-skill:minimalist-skill")` and follow its direction.
3. Read the existing HTML/CSS to preserve content (copy, links, project entries) while replacing the visual system.
4. Apply the redesign in plain HTML/CSS (no framework introduction — the user said it's a plain HTML/CSS site).
5. Optional: a final quality pass (`verify` / a critique mode) after the rewrite; do not stack a second taste skill.

## Summary

- Routing: **`taste-skill:minimalist-skill`** — correct, explicit, unambiguous.
- Nearest competitor: `taste-skill:redesign-skill` (workflow fit) — offer as complement, don't substitute.
- Do not stack other aesthetic skills; ignore broad auto-trigger design skills for the primary route.
- Hard blocker: `./site` is missing from the working directory; clarify the path before invoking anything.
