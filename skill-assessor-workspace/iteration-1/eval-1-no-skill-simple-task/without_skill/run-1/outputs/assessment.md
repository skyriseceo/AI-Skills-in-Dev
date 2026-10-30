# Skill Assessment: Rename `usrCnt` → `userCount` in src/stats.py

## Verdict

**No skill should handle this. None is needed.**

This is a trivial, mechanical, single-file rename (~3 occurrences in one file). The correct approach is a direct edit — read `src/stats.py`, replace all occurrences of `usrCnt` with `userCount` (with a quick check that no substring collisions exist, e.g. `usrCnt2`), and done. Invoking any skill adds overhead without adding value.

## Skills considered and rejected

| Skill | Why not |
|---|---|
| `build` | Full autonomous plan/execute/review/commit pipeline. Massive overkill for a 3-line rename. |
| `superpowers:brainstorming` | For creative/feature work requiring design exploration. There is nothing to design here. |
| `superpowers:test-driven-development` | A rename is not a feature or bugfix; behavior is unchanged, no new tests warranted. |
| `superpowers:writing-plans` / `executing-plans` | Not a multi-step task; a plan would be longer than the change itself. |
| `superpowers:using-git-worktrees` | No isolation needed for a trivial, low-risk edit. |
| `simplify` / `code-review` / `verify` | Post-change review/verification skills, not mechanisms for performing an edit; disproportionate for a pure rename. |
| `caveman:cavecrew` (skill) / `cavecrew-builder` (agent) | Closest match — the builder agent is explicitly for "mechanical renames" in 1-2 files. But it exists to save main-thread context via delegation; for a single small file, direct editing is faster and simpler. Only worth it if the main conversation is context-constrained. |
| `codex:rescue` | For being stuck or needing deep investigation. Nothing to investigate. |
| Design/frontend/dataviz/browser/etc. skills | Entirely unrelated domains. |

## Recommended execution (for reference, not executed — assess-only)

1. Read `src/stats.py`.
2. Verify `usrCnt` is not a substring of other identifiers (`grep -n 'usrCnt' src/stats.py`).
3. Apply `Edit` with `replace_all: true` (`usrCnt` → `userCount`).
4. Optionally run the file/tests to confirm nothing broke.

## Bottom line

Simple tasks should be done directly with core tools (Read + Edit). Skill invocation is warranted when a task matches a skill's domain and complexity threshold; a 3-occurrence single-file variable rename clears neither. If delegation were desired purely to preserve main-thread context, `caveman:cavecrew-builder` is the only reasonable candidate — otherwise, just edit the file.
