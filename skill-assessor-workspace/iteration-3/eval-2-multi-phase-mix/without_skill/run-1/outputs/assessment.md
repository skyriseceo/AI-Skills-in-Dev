# Skill Assessment: Research-Backed 60s Product Demo Edit

**Task:** (1) Research current 2026 best practices for short-form product demo videos (hook timing, captions, pacing) with sources, then (2) cut `demo.mp4` into a 60-second demo with burned-in subtitles that follows those practices.

**Verdict:** This is a two-phase pipeline with a clean skill mapping. Use **`deep-research`** for Phase 1, then **`video-use`** for Phase 2, feeding the research findings into the edit brief. No other skills are needed.

---

## Recommended Skill Order

### Phase 1 — Research: `deep-research`

**Why this skill:** The request explicitly asks for *current* (2026) best practices *with sources*. That rules out answering from model memory and rules in a web-research skill. `deep-research` is purpose-built for this: it fans out web searches, fetches sources, adversarially verifies claims, and synthesizes a cited report.

**How to scope it:** The question is already specific enough to run directly (no clarifying-question round needed per the skill's own guidance). Pass a refined query such as:

> "Best practices in 2025–2026 for short-form product demo videos (30–90s, social/landing-page distribution): optimal hook timing (first N seconds), caption/subtitle usage and styling (burned-in captions, sound-off viewing rates), cut pacing / scene length, and structure (problem→demo→CTA). Prioritize primary or data-backed sources (platform creator guides, marketing research reports) and cite everything."

**Expected output:** A cited brief with concrete, actionable parameters — e.g., hook within the first ~1–3 seconds, caption styling conventions for sound-off viewing, target shot/cut cadence, CTA placement — which becomes the editing spec for Phase 2.

**Alternative considered:** Raw `WebSearch`/`WebFetch` tools. Workable for a quick pass, but the user asked for best practices "with sources," and `deep-research` adds multi-source verification and structured citation, which matches the ask better. Use raw search only if a lightweight answer is acceptable.

### Phase 2 — Edit: `video-use`

**Why this skill:** It is the only video-editing skill available, and it covers exactly the required operations: **transcribe** (needed to generate subtitles from the screen recording's audio, if any), **cut** (trim raw recording down to 60s), and **burn subtitles** (hard-coded captions). Its conversational flow (ask questions → confirm plan → execute → iterate) fits a creative edit where the source material hasn't been reviewed yet.

**How to feed Phase 1 into Phase 2:** Translate the research brief into the edit plan before invoking, e.g.:
- Open on the strongest product moment within the first 1–3 seconds (hook), not a title card or slow zoom-in.
- Keep cuts at the researched cadence (typically ~2–5s per beat for demos) and remove dead time (loading spinners, typing pauses, mouse wandering).
- Burn in high-contrast, safe-area-respecting subtitles sized for mobile, since most short-form viewing is sound-off.
- End inside 60s with a clear CTA beat.

**Sequencing note:** Do not start the edit until the research brief exists — the pacing and caption parameters are inputs to the cut list, not post-hoc polish.

---

## Skills Considered and Rejected

| Skill | Why not |
|---|---|
| `superpowers:brainstorming` | Marked "MUST use before creative work," but the user's requirements are already explicit (60s, burned-in subs, research-derived practices). Nothing to explore; the research phase itself resolves the open design questions. Skip unless creative direction beyond the brief is wanted. |
| `superpowers:writing-plans` / `executing-plans` | Overhead for a two-step pipeline aimed at code implementation plans; the ordering here is trivial. |
| `design-master`, `impeccable`, taste-skills, `frontend-design` | UI/web design skills — no web UI is being built. Subtitle styling is handled inside `video-use`. |
| `animation-vocabulary` | Terminology lookup only; irrelevant. |
| `claude-in-chrome` / browser tools | Only needed if `deep-research` were unavailable and manual page browsing were required. |
| `build`, TDD/debugging/review skills | Software-development lifecycle skills; no code project here. |
| `dataviz`, `skill-creator`, caveman skills, config skills | Out of scope. |

---

## Preconditions and Risks (check before executing)

1. **`demo.mp4` is not present** in the working directory (`/home/ah/Projects/SkySkills`). The user must supply its path before Phase 2 can run. This is currently a blocker for execution.
2. **Silent screen recordings:** if `demo.mp4` has no narration audio, transcription yields nothing — subtitles would need to be authored as descriptive captions (feature callouts) rather than transcribed speech. `video-use` should be told which case applies; plan a quick audio check (e.g., ffprobe) as step one of Phase 2.
3. **Source length vs. 60s target:** if the raw recording is long, expect an iteration loop in `video-use` (rough cut → review → tighten), which the skill supports.
4. **Aspect ratio:** screen recordings are usually 16:9; short-form platforms favor 9:16 or 1:1. Confirm the target platform with the user — this affects crop/reframe decisions and subtitle safe areas.

---

## Execution Sequence (when approved)

1. Invoke **`deep-research`** with the refined query above → cited best-practices brief.
2. Confirm with the user: target platform/aspect ratio, whether the recording has narration, location of `demo.mp4`.
3. Invoke **`video-use`** with the brief's parameters as the edit spec → 60s cut with burned-in subtitles.
4. Review output against the researched practices (hook in first seconds, caption legibility, pacing, ≤60s) and iterate within `video-use` as needed.
