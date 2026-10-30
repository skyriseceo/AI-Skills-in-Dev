# Skill Assessment: Research-Backed 60s Product Demo Cut

**Request:** (1) Research current (2026) best practices for short-form product demo videos — hook timing, captions, pacing — with sources. (2) Apply those practices to cut `demo.mp4` into a 60-second demo with burned-in subtitles.

**Mode:** Assessment only. No skills executed.

---

## Recommended plan

### Phase 1 — Research: `deep-research`

- **Skill:** `deep-research`
- **Why:** The request explicitly asks for *current* best practices *with sources*. This is a multi-source, fact-checked web research task — exactly what `deep-research` is for (fan-out searches, source fetching, adversarial verification, cited synthesis). Answering from model memory would risk stale pre-cutoff guidance and would not satisfy the "with sources" requirement.
- **Scope to pass in:** "Best practices in 2026 for short-form product demo videos (~60s, screen-recording based): hook timing (first N seconds), burned-in captions/subtitle styling, pacing/cut cadence, aspect ratio and platform norms (TikTok/Reels/Shorts/X/LinkedIn), CTA placement." The question is already reasonably specific; one optional clarifier before running: **target platform(s) and aspect ratio** (9:16 vertical vs. 16:9 landscape changes caption placement and safe zones).
- **Output to carry forward:** A short, concrete spec — e.g., hook within X seconds, caption font/size/placement rules, target shots-per-N-seconds pacing, 60s structure (hook → problem → demo → CTA) — with citations.

### Phase 2 — Editing: `video-use`

- **Skill:** `video-use`
- **Why:** It is the only skill in the catalog that does video editing, and it covers every required operation natively: transcription (needed to generate subtitles from the recording's audio, if any, or to script overlay captions), cutting/trimming to 60s, and **burning subtitles in**. Its conversational flow (ask questions → confirm plan → execute → iterate) fits well, since the Phase 1 findings become the editing brief.
- **How to hand off:** Feed the Phase 1 spec directly into the `video-use` plan: front-load the strongest moment in the hook window, cut to the researched pacing cadence, style burned-in captions per the researched guidelines (high-contrast, safe-zone placement, short line lengths), land at ~60s.

### Recommended order

1. `deep-research` — produce the cited best-practices spec.
2. `video-use` — execute the cut and subtitle burn against that spec.

Strictly sequential; Phase 2's editing decisions (hook length, cut rhythm, caption styling) depend on Phase 1's output, so do not parallelize.

---

## Skills considered and rejected

| Skill | Reason rejected |
|---|---|
| `WebSearch`/`WebFetch` tools alone | Would work for light research, but the "with sources" + "current best practices" framing warrants `deep-research`'s verification and citation discipline. Acceptable fallback if a lightweight pass is preferred. |
| `superpowers:brainstorming` | For creative software features/components, not media research or editing. |
| `superpowers:writing-plans` / `executing-plans` | Overhead for a two-phase task with an obvious sequence; `video-use` already includes its own plan-confirm step. |
| Design skills (`design-master`, `impeccable`, `taste-skill:*`, `frontend-design`, etc.) | All target web UI/frontend, not video production. |
| `animation-vocabulary` | Terminology lookup for motion effects; irrelevant here. |
| `build`, TDD/debugging/review skills | Software development lifecycle skills; no code is being built. |
| `dataviz`, `claude-api`, `caveman:*`, config/scheduling skills | Out of domain. |

## Preconditions and risks (flag before executing)

1. **`demo.mp4` was not found** anywhere under `/home/ah/Projects/SkySkills`. Before Phase 2, the user must supply its path. This blocks Phase 2 but not Phase 1.
2. **ffmpeg availability:** `video-use` will need ffmpeg (and likely a transcription backend) on the machine; verify before executing Phase 2.
3. **Audio unknown:** If the screen recording has no narration, "subtitles" really means scripted overlay captions — the `video-use` session should confirm this early, and the user should be asked for (or approve) caption copy.
4. **Platform/aspect ratio unspecified:** Affects both research emphasis and the final render (vertical crop vs. landscape). One clarifying question up front avoids a re-render.
5. **60s hard target:** If the raw recording is much longer, the `video-use` plan step should propose which segments to keep for user confirmation before cutting.

## Bottom line

Two skills, in order: **`deep-research` → `video-use`**. Research first to produce a cited editing spec (hook timing, caption styling, pacing), then a single `video-use` session that applies that spec to cut `demo.mp4` to 60s with burned-in subtitles. Resolve the missing `demo.mp4` path and the target platform/aspect ratio before executing Phase 2.
