# Skill Assessment

**Task:** Research current (2026) best practices for short-form product demo videos — hook timing, captions, pacing, with sources — then apply them to cut a raw screen recording (`demo.mp4`) into a 60-second demo with burned-in subtitles.

**Plan:**
1. `deep-research` — Phase 1: research the 2026 best practices (hook timing, caption style, pacing/cut cadence) with cited sources — this is exactly its contract: a multi-source, fact-checked research pass whose deliverable is a cited set of findings. The question is already specific enough (topic, three named dimensions, year) to research directly without pre-clarification. It hands the editing phase a concrete spec: hook cut point, target shot length / cuts-per-10s, subtitle placement and styling rules.
2. `video-use` — Phase 2: execute the edit — transcribe `demo.mp4`, cut it down to 60s, and burn in subtitles, following the spec produced in Phase 1. Its description explicitly claims "cut", "burn subtitles", "tutorials" — a direct contract hit for every editing sub-task here. Pass the Phase 1 findings in as the creative brief so its plan-confirmation step reflects the researched practices rather than generic defaults.

**Considered and rejected:**
- `superpowers:brainstorming` — process skill for creative *software* work (features, components, behavior); the creative direction here is supplied by the research phase, not by an intent-exploration session. Running it would duplicate what deep-research delivers.
- Direct `WebSearch`/`WebFetch` (no skill) for Phase 1 — would find sources, but the user explicitly wants best practices "with sources," and deep-research's fan-out + adversarial verification is designed for exactly that cited-findings deliverable; ad-hoc searching risks thin or unverified claims.
- `animation-vocabulary` — reverse-lookup glossary for naming motion effects; nothing here needs an effect named.
- `design-master` / `impeccable` / `taste-skill:*` / `frontend-design` family — web UI design/build skills; the deliverable is a video file, not an interface.
- `build` — end-to-end software build pipeline; there is no project or feature to build.
- `superpowers:writing-plans` / `superpowers:executing-plans` — code-implementation planning for multi-step engineering work; the sequencing here is a simple two-phase handoff that the routing plan itself already captures.
- `dataviz` — no chart, graph, or visualization involved.
- `skill-creator:skill-creator` — meta-skill; this task uses skills, it isn't about making one.

**Blockers:** `demo.mp4` does not exist anywhere in the working directory (`/home/ah/Projects/SkySkills`) — no input files were provided. Phase 2 cannot run until the raw screen recording is supplied (or its actual path is given). Phase 1 has no blockers and could run independently.

**Verdict:** Run `deep-research` first to produce the cited best-practices spec, then `video-use` to cut and subtitle `demo.mp4` against that spec — but Phase 2 is blocked until the missing `demo.mp4` is provided.
