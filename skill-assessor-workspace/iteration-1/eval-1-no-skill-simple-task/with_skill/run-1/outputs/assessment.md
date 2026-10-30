# Skill Assessment

**Task:** Rename the variable `usrCnt` to `userCount` in `src/stats.py` — ~3 occurrences, single file, purely mechanical.

**Plan:**
1. No skill — handle directly. Read `src/stats.py`, replace all occurrences of `usrCnt` with `userCount` (with a quick grep to confirm nothing outside that file references it), done.

**Considered and rejected:**
- caveman:cavecrew — its `cavecrew-builder` subagent is scoped to exactly this kind of 1-2 file mechanical rename, but the skill's value is context compression during long delegation-heavy sessions; for a standalone 3-occurrence edit, spawning a subagent costs more overhead than the edit itself.
- superpowers:test-driven-development — a rename changes no behavior; there is no failing test to write first. Contract-level miss.
- superpowers:brainstorming — "before any creative work"; a mechanical rename has no design space to explore.
- verify — its own contract says don't invoke on changes with no runtime surface worth driving; a rename is confirmed by grep, not by exercising the app end-to-end.
- code-review / simplify — review skills operate on a diff after work is done; nothing in this task asks for review, and a 3-line rename doesn't warrant one.
- build — full autonomous plan/execute/commit pipeline; wildly oversized for a one-file variable rename.

**Verdict:** No skill — handle directly. This is the textbook "one-line fix" case where loading any skill adds overhead without changing the outcome; a Read + Edit (replace_all) plus a grep for `usrCnt` elsewhere is the whole job.
