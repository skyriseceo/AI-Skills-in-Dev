# Skill Assessment: Research-Backed 60s Demo Cut with Burned-In Subtitles

**Task:** (1) Research current (2026) best practices for short-form product demo videos — hook timing, captions, pacing — with sources; (2) apply those practices to cut `demo.mp4` into a 60-second demo with burned-in subtitles.

**Mode:** Assessment only. No research performed, no video edited.

---

## Recommended skill pipeline (in order)

### Phase 1 — Research: `deep-research`

- **Skill:** `deep-research`
- **Why:** The task explicitly requires *current* 2026 best practices *with sources*. This rules out answering from model memory. `deep-research` is purpose-built for this: it fans out web searches, fetches sources, adversarially verifies claims, and synthesizes a cited report — exactly matching the "with sources" requirement.
- **How to invoke:** Pass a refined question as args, e.g.:
  > "Current (2025–2026) best practices for short-form product demo videos on social platforms: optimal hook timing (first N seconds), burned-in caption/subtitle style and readability standards, cut/pacing cadence (shots per second, scene length), aspect ratio and duration norms for ~60s demos. Prioritize primary sources: platform creator guidelines (TikTok/YouTube Shorts/Reels/LinkedIn), recent industry studies, and reputable video-marketing research."
- **Note:** The question is already specific enough (hook timing, captions, pacing; 60s product demo) that no clarifying questions should be needed — the skill's own guidance says to clarify only if underspecified.
- **Lighter alternative:** If a full cited report is overkill, a handful of direct `WebSearch` + `WebFetch` calls would suffice. But since the user asked for best practices "with sources," `deep-research` is the better fit; it bakes in source verification.

### Phase 2 — Editing: `video-use`

- **Skill:** `video-use`
- **Why:** Its description covers this task almost verbatim: "Edit any video by conversation. Transcribe, cut, color grade, generate overlay animations, **burn subtitles** — for talking heads, montages, **tutorials**." A screen-recording product demo is squarely in scope. It also handles transcription (needed to generate subtitles from the recording's audio) and enforces production-correctness rules.
- **How to invoke:** After Phase 1 completes, invoke `video-use` with the source file and a concrete brief derived from the research findings, e.g.: target 60s runtime, hook the key value moment in the first 1–3s (per whatever the research confirms), cut dead air and slow navigation segments, pacing cadence per findings, burned-in subtitles styled per the researched caption guidelines (size, contrast, placement, line length).
- **Expected internal flow within the skill:** transcribe → identify keep/cut segments to hit 60s → confirm plan → cut/assemble → generate subtitle track from transcript → burn subtitles → export.

### Why this order

Strictly sequential: research → edit. The edit brief (hook placement, subtitle styling, cut cadence) is parameterized by the research output, so Phase 2 cannot start meaningfully before Phase 1 finishes. Do **not** parallelize, except for one optional overlap noted below.

---

## Skills considered and rejected

| Skill | Reason rejected |
|---|---|
| `WebSearch`/`WebFetch` alone | Viable but weaker: no built-in verification/synthesis; user asked for sourced best practices, which `deep-research` delivers as a cited report. |
| `claude-in-chrome` | Browser automation is unnecessary; research doesn't need an interactive session. |
| `build`, `superpowers:*` (planning/TDD/plans) | Software-development workflow skills; this is a research + media task, not a coding project. |
| `design-master`, `impeccable`, `taste-skill:*`, `frontend-design`, `dataviz` | UI/visual design and charting skills; not applicable to video editing. |
| `animation-vocabulary` | Terminology lookup only; irrelevant. |
| `skill-creator`, `caveman:*`, `codex:*` | Meta/efficiency/delegation tooling; nothing here needs them. |

No skill gaps: the two-skill pipeline fully covers the task. No custom scripting (raw ffmpeg orchestration) should be needed outside `video-use`.

---

## Blockers and risks to resolve before execution

1. **`demo.mp4` is missing.** The task declares "Input files: none," and no `demo.mp4` exists in `/home/ah/Projects/SkySkills/`. Phase 2 cannot run until the user provides the file or its path. Phase 1 can proceed regardless.
2. **Audio uncertainty.** If the screen recording has no narration audio, transcription-based subtitles won't work; subtitles would instead need to be authored as descriptive captions (feature names, step labels). `video-use` should surface this during its transcribe step — flag it to the user early.
3. **Aspect ratio / target platform unknown.** Screen recordings are typically 16:9; short-form platforms favor 9:16 or 1:1. The research will likely surface this; the user should be asked which platform/aspect ratio to target before the cut.
4. **Content selection is judgment-heavy.** Cutting an arbitrary-length recording to 60s requires deciding which product moments matter. `video-use` is conversational and will confirm the plan — budget an iteration loop with the user.

---

## Execution summary (when approved)

1. Invoke `Skill(deep-research)` with the refined research question above → cited best-practices report (hook timing, caption style, pacing norms).
2. Obtain `demo.mp4` from the user; confirm target platform/aspect ratio and whether narration audio exists. (This step can overlap with step 1.)
3. Invoke `Skill(video-use)` with the source video and a brief built from step 1's findings → confirm cut plan → execute 60s cut with burned-in subtitles → deliver export, iterate as needed.
