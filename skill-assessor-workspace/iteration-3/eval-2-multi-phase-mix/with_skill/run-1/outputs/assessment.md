# Skill Assessment

**Task:** Research current (2026) best practices for short-form product demo videos (hook timing, captions, pacing — with cited sources), then apply them to cut a raw screen recording (`demo.mp4`) into a 60-second demo with burned-in subtitles.

**Plan:**
1. skill: deep-research — Phase 1: produce the sourced best-practices brief (hook timing, caption style, pacing/cut cadence for short-form product demos) — wins because the user explicitly wants findings "with sources", and this skill's contract is exactly that: fan-out searches, adversarial verification, and a cited report. The question is already specific enough (topic, format, three named dimensions, timeframe) to pass straight in without a clarifying round. Its output becomes the editing spec handed to phase 2.
2. skill: video-use — Phase 2: execute the edit — transcribe, select the strongest 60s, cut for the researched pacing, place the hook in the researched window, and burn in subtitles styled per the caption findings. Its contract ("edit any video by conversation… cut… burn subtitles") covers the entire deliverable with no seams; the phase-1 brief plugs in as the plan it confirms before executing.

**Considered and rejected:**
- No capability (direct WebSearch/WebFetch for phase 1) — a handful of manual searches could answer "what are the best practices", but the user asked for sourced, current-best-practice claims in a fast-moving space; deep-research's verification pass is what makes the citations trustworthy. Would flip if the user only wanted a quick gut-check rather than a sourced brief.
- agent: general-purpose (delegate the research) — delegation-shaped, but it brings no research methodology; deep-research loads exactly that methodology and produces the cited report format the user asked for. Would flip only if deep-research were unavailable.
- agent: claude-code-guide / skill: claude-api — the task mentions no Claude/LLM work; video-use handles its own tooling (ffmpeg, transcription) internally.
- Not close: design-master, impeccable, taste-skill:* family, frontend-design, dataviz — all web/UI design capabilities; this task has no frontend surface.
- Not close: superpowers:* process skills (brainstorming, writing-plans, TDD, etc.), build, verify, code-review, simplify — software-development process/implementation skills; nothing here is code.
- Not close: caveman:* — output-style compression, orthogonal to routing.
- Not close: skill-creator, update-config, schedule, loop, keybindings-help — meta/harness skills; the task uses skills, it isn't about them.

**Blockers:** `demo.mp4` was not provided and does not exist anywhere under the project directory — phase 2 cannot run without the actual file (or its path). Also worth confirming before execution: target platform/aspect ratio (e.g., 9:16 for Reels/TikTok vs. 16:9 for a landing page), since the researched best practices and the crop/caption placement both depend on it — video-use will ask, but the answer changes the edit.

**Verdict:** Run skill: deep-research first to produce the cited best-practices brief, then skill: video-use to cut and subtitle the 60s demo against that brief — blocked on the missing `demo.mp4` until the user supplies it.
