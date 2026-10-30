# Skill Assessment

**Task:** Rename the variable `usrCnt` to `userCount` in `src/stats.py` (~3 occurrences, single file).

**Plan:**
1. No capability — handle directly — one `Edit` with `replace_all` on `src/stats.py` (after a quick Read/Grep to confirm the 3 occurrences and rule out collisions like string literals or a same-named attribute).

**Considered and rejected:**
- agent: caveman:cavecrew-builder — its contract explicitly names "mechanical renames" in a 1–2 file scope, so this is the genuine runner-up. It loses because delegation only pays when the main conversation is long and context needs conserving; for a three-occurrence, one-file rename, briefing an agent and reading back its diff receipt costs more than the single Edit it replaces. Flip condition: a long-running main session where context is tight, or a batch of several such renames to fire off in parallel.
- skill: caveman:cavecrew — it's a decision guide for *when* to spawn cavecrew agents, not a capability that does the rename; loading it just re-asks the routing question this assessment already answers. Flip condition: none for this task — it only wins when the user wants the delegation policy itself.
- Not close: superpowers:test-driven-development, superpowers:brainstorming, superpowers:writing-plans, build, verify, code-review, simplify — process/workflow machinery for feature work or review; a mechanical rename has no design, planning, or verification surface beyond confirming the edit applied.
- Not close: all design/frontend skills (design-master, impeccable, taste-skill:*, dataviz, etc.), video-use, deep-research, claude-api, and other domain skills — wrong domain entirely.

**Blockers:** `src/stats.py` was not provided and no repo path was confirmed — before executing, verify the file exists at the stated path.

**Verdict:** No capability — handle directly with a single Read + Edit (replace_all) on `src/stats.py`; the task is below the overhead floor of every installed skill and agent.
