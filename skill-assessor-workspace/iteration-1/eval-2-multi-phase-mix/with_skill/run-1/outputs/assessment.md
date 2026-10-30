# Skill Assessment

**Task:** Research current (2026) best practices for short-form product demo videos — hook timing, captions, pacing, with sources — then apply them to cut a raw screen recording (`demo.mp4`) into a 60-second demo with burned-in subtitles.

**Plan:**

1. **deep-research** — Phase 1: establish the editorial spec — runs the fan-out web search, fetches and adversarially verifies sources, and produces a cited report on hook timing, caption style, and pacing norms for short-form product demos. It wins because the user explicitly asked for *current* best practices *with sources*: that requires multi-source web research with verification, which is exactly this skill's contract. The question is already specific enough (format: short-form product demo; dimensions: hook timing, captions, pacing; recency: 2026) that no pre-research clarification round is needed. **Hand-off:** a concrete edit spec — e.g., hook within the first N seconds, caption size/placement/word-count conventions, target cut cadence — each point backed by a citation.

2. **video-use** — Phase 2: executes the edit — transcribes `demo.mp4`, plans and confirms cuts to hit the 60s target, applies the pacing from the Phase 1 spec, and burns in subtitles per the researched caption conventions. Its description claims this task almost verbatim ("Edit any video by conversation. Transcribe, cut, … burn subtitles"), including production-correctness rules for the parts where mistakes are expensive (sync, encoding, burned-in text). **Input it consumes:** the raw `demo.mp4` plus the edit spec from step 1.

Order matters and is fixed: the research output *is* the edit brief. Running video-use first would mean cutting to guessed conventions and likely re-editing; deep-research first turns Phase 2 into straightforward execution against a spec.

**Considered and rejected:**

- **superpowers:brainstorming** — the requirements are already settled (60s, burned-in subtitles, three named research dimensions); re-running requirements exploration on a decided brief is friction, not rigor. The open question here is factual (what are the practices), not creative, and that's research, not brainstorming.
- **superpowers:writing-plans / superpowers:executing-plans** — these frame multi-step *code* implementation with review checkpoints; a two-phase research→media-edit pipeline doesn't need a written implementation plan artifact, and the routing plan itself already sequences the work.
- **build** — autonomous build pipeline for software projects/features (plan → execute → commit+push); nothing here is a codebase deliverable, and its git-centric workflow has no purchase on a video file.
- **claude-in-chrome** — could technically browse sources by hand, but deep-research's search fan-out + adversarial verification is the purpose-built path for cited findings; browser automation would be a slower, uncited substitute for Phase 1.
- **animation-vocabulary** — video/motion keyword overlap only; it names motion effects, it doesn't research practices or edit footage.
- **design-master / impeccable / taste-skill family / frontend-design** — all scoped to web UI design and code; a video deliverable is outside every one of their contracts despite surface-level "polish/visual quality" vocabulary.
- **dataviz** — triggers on charts/graphs/dashboards; no visualization is being produced.
- **skill-creator** — meta-skill; this task *uses* skills, it isn't *about* creating or improving one.
- **caveman family** — output-style/compression layer, orthogonal to routing; nothing in the request asks for compressed communication.
- **Plain WebSearch (no skill) for Phase 1** — viable for a quick answer, but the user asked for practices "with sources" that will directly drive an irreversible edit; deep-research's verification step is what makes those citations trustworthy enough to cut against.

**Verdict:** Run **deep-research** first to produce a cited best-practices spec, then **video-use** to cut `demo.mp4` to 60s with burned-in subtitles against that spec — a genuine two-phase mix (research → media edit) with a clean hand-off; execution is deferred per the user's "assess first" instruction.
