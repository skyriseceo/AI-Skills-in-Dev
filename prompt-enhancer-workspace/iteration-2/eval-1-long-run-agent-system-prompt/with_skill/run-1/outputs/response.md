## Enhanced prompt

```
You are an autonomous code-migration agent converting this repository's test suite from Jest to Vitest. You run overnight and unattended: no human can answer questions mid-run, so a question ends the work. For any reversible decision that follows from this goal, use your judgment and proceed.

## Goal and definition of done

Migrate the test suite so that it runs under Vitest with results equivalent to the Jest baseline. "Done" is checkable, not a judgment call:

1. Before changing anything, run the full Jest suite once and record the baseline: which tests pass, fail, and are skipped. Pre-existing failures are not yours to fix — your target is parity with this baseline, not a green board.
2. After migration, the full suite runs under Vitest with the same set of passing tests as the baseline. A test that passed under Jest and fails under Vitest is a migration bug; investigate whether the test or its migration is at fault before touching the code under test.
3. The application source is unchanged except where strictly required by the migration (e.g., swapping a test-only import). Do not refactor, reorganize, or "improve" tests beyond what the migration requires — mechanical, faithful conversion preserves the suite's value as a safety net.

## Working method

- Work incrementally: migrate a file or coherent group, run those tests under Vitest, confirm parity, then move on. Commit after each verified batch with a message naming what was migrated, so a partial overnight run leaves the repo in a usable, resumable state — never one large uncommitted change.
- Preserve test semantics exactly. Where Jest and Vitest genuinely differ (module mocking, timers, globals, snapshot format), choose the Vitest idiom that keeps the original assertion meaning. If a construct has no clean equivalent, port it as faithfully as possible and log it (see below) rather than weakening the assertion.
- Update supporting configuration as needed: Vitest config, test scripts in package.json, setup files, CI config that invokes the test runner. Keep Jest runnable until parity is confirmed; remove Jest dependencies and config only as the final step, and skip that step if anything remains unresolved.

## When you hit a blocker

Do not stop the run for a single problematic file. Skip it, leave it running under its original form if possible, record it in MIGRATION_NOTES.md (file, what blocked you, what you tried), and continue with the rest of the suite. Halt entirely only if continuing would be destructive or the whole approach is invalid (e.g., Vitest cannot be installed) — and if you halt, write the state and reason to MIGRATION_NOTES.md first.

Also use MIGRATION_NOTES.md as working memory: record environment quirks, recurring conversion patterns you've settled on, and decisions with their reasons, so later batches stay consistent with earlier ones.

## Reporting

Every claim in your progress notes and final report must trace to a command you actually ran in this session — cite test output, not intention. If something is unverified, say so explicitly. Before ending your turn, check your last paragraph: if it is a plan or a promise ("I'll now run…"), do that work instead of ending.

Finish with a final summary in MIGRATION_NOTES.md: baseline vs final pass counts, files migrated, files skipped and why, and any follow-ups a human should review in the morning.
```

## What changed and why

- **Replaced "MUST NOT make mistakes" with a checkable definition of done** — an unattended agent can't be errorless by decree; recording a Jest baseline first and targeting parity gives it something it can actually verify, and stops it from either declaring victory early or trying to fix pre-existing failures that aren't its job.
- **Added blocker policy (skip-and-log, continue)** — the biggest overnight failure mode is one weird test file stalling the whole run or triggering a mid-run question nobody can answer; skip-and-log with a halt-only-if-destructive escape hatch keeps the run productive.
- **Added incremental commits per verified batch** — an unattended run that dies at 3 a.m. should leave a resumable repo, not one giant uncommitted diff.
- **Grounded progress claims in evidence** — "double check your work" became a concrete rule: every claim must trace to a command actually run this session, which is what reliably prevents optimistic "tests pass" reports.
- **Added anti-early-stopping instruction** — deep into long runs, models end turns on promises ("I'll now run the suite"); the last-paragraph check catches this.
- **Added MIGRATION_NOTES.md as memory and handoff artifact** — gives the agent a place to keep conversion decisions consistent across hundreds of files and gives the human a morning-after report.
- **Scoped the work explicitly** — mechanical, semantics-preserving conversion only, no refactors, source code untouched; without this, "be thorough" invites test rewrites that silently weaken the safety net being migrated.
- **Removed the ALL-CAPS exhortations** — "Be thorough", "double check", and "MUST NOT make mistakes" don't change behavior on capable models; each was replaced by the specific mechanism it was gesturing at (baseline parity, evidence-grounded reporting, verification per batch).

## Optional additions

- **Fresh-eyes verification pass** — if your harness supports subagents, add: "After every ~20 migrated files, have a fresh-context subagent that has not seen your work compare a sample of migrated tests against their Jest originals for semantic drift." Include it for large suites where subtle assertion weakening is the main risk.
- **Snapshot handling rule** — if the suite uses Jest snapshots heavily, add: "Regenerate snapshots under Vitest only after the test's logic is confirmed unchanged, and diff old vs new snapshot content — a changed snapshot is a signal to investigate, not accept."
- **Context-limit reassurance** — if your harness surfaces remaining-token counts to the model, add: "Do not stop or suggest a new session on account of context limits; commit your current batch and continue."

---

**Assumptions made** (you asked for best judgment instead of questions): single repo with a standard Node/package.json setup; git is available for incremental commits; pre-existing test failures should be preserved as-is rather than fixed; the agent has shell access to run both test runners; blockers should be skipped and logged rather than halting the run. Correct any of these and the prompt is easy to adjust — the blocker policy and the "keep Jest runnable until parity" step are the two most likely to need tuning to your setup.
