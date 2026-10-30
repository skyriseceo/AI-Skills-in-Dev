# Skill Assessment: Rename `usrCnt` to `userCount` in src/stats.py

## Verdict

**No skill should handle this. Use no skill at all.**

This is a trivial, single-file, mechanical rename (~3 occurrences in one known file). The correct approach is a direct edit — e.g., the built-in `Edit` tool with `replace_all`, or an equivalent one-line `sed`/manual change. Invoking any skill would add overhead with zero benefit.

## Why no skill fits

Skills reviewed against the task:

| Candidate | Why it doesn't apply |
|---|---|
| `build` | Full autonomous plan/execute/commit pipeline — massive overkill for a 3-occurrence rename. |
| `superpowers:brainstorming` | For creative/feature work requiring design exploration. A rename has no design space. |
| `superpowers:test-driven-development` | For features/bugfixes with behavior changes. A rename changes no behavior. |
| `superpowers:writing-plans` / `executing-plans` | Multi-step tasks with specs. This is one step. |
| `superpowers:using-git-worktrees` | Isolation for feature work. Unnecessary for a rename. |
| `caveman:cavecrew` (builder subagent) | Delegation to save context is only worthwhile in long sessions; for a quick one-off, inline edit is cheaper and faster. |
| `verify` / `code-review` / `simplify` | Post-change review skills; a mechanical rename with no runtime behavior change doesn't warrant them. |
| Design/frontend/research/browser skills | Entirely unrelated domains. |

## Recommended execution (for reference, not performed)

1. Read `src/stats.py`.
2. `Edit` with `old_string: "usrCnt"`, `new_string: "userCount"`, `replace_all: true` (after confirming no partial-identifier collisions like `usrCntTotal`).
3. Optionally run the file's tests or a quick `python -m py_compile src/stats.py` sanity check.

## Risk notes

- Only caveat: verify `usrCnt` doesn't appear as a substring of another identifier or in strings/comments where renaming is undesired — a quick grep before replace-all covers this.
- If the variable were used across multiple files or were a public API, a more careful refactor (and possibly `code-review`) would be justified — not the case here.

**Bottom line:** direct tool use, no skill invocation.
