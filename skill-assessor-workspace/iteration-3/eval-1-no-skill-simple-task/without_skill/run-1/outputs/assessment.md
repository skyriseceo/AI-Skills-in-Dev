# Skill Assessment: Rename `usrCnt` → `userCount` in `src/stats.py`

## Verdict

**No skill should handle this. Do it directly with built-in tools.**

## Reasoning

This is a trivial, single-file, mechanical rename (~3 occurrences, one variable, one file). Direct execution is the right fit:

1. `Read` the file (`src/stats.py`)
2. `Edit` with `old_string: "usrCnt"`, `new_string: "userCount"`, `replace_all: true` (or three targeted edits if there's any risk of substring collisions, e.g. `usrCnt2`)
3. Optionally run a quick `grep -rn usrCnt src/` to confirm no other file references it, and a syntax check (`python -m py_compile src/stats.py`)

Total effort: one read, one edit. Any skill invocation adds overhead without adding value.

## Why the candidate skills don't apply

| Skill | Why not |
|---|---|
| `superpowers:brainstorming` | For creative/feature work requiring design exploration. A rename has no design space. |
| `superpowers:test-driven-development` | No behavior change; a pure rename needs no new tests. |
| `superpowers:writing-plans` / `executing-plans` / `subagent-driven-development` | For multi-step planned work. This is one step. |
| `superpowers:using-git-worktrees` | No isolation needed for a 3-line refactor. |
| `build` | Autonomous end-to-end pipeline — massive overkill for a rename. |
| `caveman:cavecrew` (builder subagent) | Delegation saves context on long sessions; for a "quick one" the delegation overhead exceeds the work itself. |
| `verify` / `code-review` / `simplify` | Post-change review skills; nothing here warrants them, though a `py_compile` sanity check is cheap and reasonable. |
| Design/frontend/dataviz/browser skills | Irrelevant domain (Python backend file, no UI). |

## Notes / edge considerations

- **Scope check is the only real risk**: confirm `usrCnt` isn't referenced outside `src/stats.py` (e.g., via `globals()`, string-based access, or imports). A single `grep -rn usrCnt .` covers this.
- If the rename turned out to span many files or a public API, a plan/subagent approach could be reconsidered — but as stated ("3 places in that one file"), it does not.

## Recommendation

Proceed without any skill: `Read` → `Edit` (replace_all) → `grep` sanity check → done.
