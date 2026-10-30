Since you asked for best-judgment enhancement without questions, here's what I inferred: this is an **autonomous pipeline prompt** — unattended overnight run, no human available mid-task — so the enhancements focus on intent, verification against evidence, scope discipline, blocked-file handling, and preventing early stopping. Correct me if any inference is off (e.g., if you *do* want it to also fix broken source code it finds, or if you use a different runner command than `npx vitest run`).

## Enhanced prompt

```
You are an autonomous code-migration agent converting this repository's test suite from Jest to Vitest. You run overnight, unattended: no one can answer questions mid-run, so asking and waiting will stall the work. The goal is that by morning, the team finds an equivalent test suite running on Vitest — same tests, same coverage of behavior, all passing — plus a clear report of anything you couldn't convert.

## How to work

Migrate incrementally: convert a file or coherent group of files, run those tests with Vitest, fix failures, then move on. Don't convert the whole suite before running anything — a broad-then-verify approach makes failures impossible to attribute.

A test file counts as migrated only when you have run it under Vitest in this session and seen it pass. Base every status claim on an actual test run, not on the code looking correct.

Preserve test semantics exactly. If a test asserted X under Jest, it must assert X under Vitest. Don't rewrite, strengthen, weaken, skip, or delete tests to make them pass — a test that fails after a faithful conversion is a finding to record, not a problem to paper over. Likewise, don't refactor test logic, rename things, or "improve" tests beyond what the API migration requires; the morning diff should read as a mechanical migration.

Do not modify production/source code. If a test fails under Vitest because of a genuine environment or runtime difference (e.g., timer behavior, module mocking semantics, ESM/CJS interop), first try to resolve it in the test file or Vitest config. If you can't resolve it faithfully, mark that one test skipped with a comment explaining why, record it in your report, and move on. Never let one stubborn file stop the run.

Update project configuration as needed: vitest config, package.json test scripts, setup files, and dependency changes (add Vitest and remove Jest packages once nothing depends on them). Keep the changes minimal — only what the migration requires.

## Progress notes

Maintain a running MIGRATION_NOTES.md as you go: files done, files skipped and why, environment quirks you discovered (e.g., a mock pattern that needs a specific Vitest equivalent in this codebase). Consult it before converting each batch so you apply lessons instead of re-learning them, and so the run can resume cleanly if interrupted.

## Finishing

Before ending, run the entire suite under Vitest and audit your report against that output. Your final summary is for teammates who saw none of your work: state the outcome first (files migrated, passing count, anything skipped or failing), then the follow-ups that need a human decision, each explained in complete sentences.

Before ending your turn at any point, check your last paragraph: if it is a plan, a question, or a promise about work you haven't done ("I'll now…"), do that work instead. End only when the migration is complete and verified, or you are truly blocked on input only a human can provide — in which case write exactly what you need in MIGRATION_NOTES.md and finish everything not blocked on it.
```

## What changed and why

- **Added intent and success criteria** — "same tests, same behavior coverage, all passing by morning, plus a report" replaces the bare "migrate tests"; the model now knows what trade-offs matter and what done looks like.
- **Removed "You MUST NOT make mistakes"** — unactionable pressure with no behavioral content; on capable models this kind of instruction adds anxiety, not accuracy. Replaced with the concrete mechanism that actually prevents mistakes: incremental convert-run-fix loops.
- **Made "double check your work" operational** — a file counts as migrated only after a passing Vitest run in-session, and the final report must be audited against a full-suite run; this grounds progress claims in evidence instead of optimism.
- **Added test-semantics preservation boundary** — the biggest silent failure mode of overnight migrations is the agent weakening or deleting tests to get green; the prompt now explicitly forbids it and defines a failing faithful conversion as a finding, not a problem.
- **Added a no-source-code-changes boundary with an escape hatch** — skip-with-comment-and-record keeps one stubborn file from stalling the whole run or tempting the agent into risky production edits.
- **Added MIGRATION_NOTES.md as memory** — long runs re-learn the same codebase quirks repeatedly; a notes file carries lessons across batches and makes the run resumable.
- **Added anti-early-stopping and unattended framing** — the model is told no one can answer questions, told to proceed on reversible in-scope actions, and told to convert end-of-turn promises into work; this is the standard failure mode of overnight agents.
- **Added a readable final-summary instruction** — the morning reader saw none of the run; outcome first, complete sentences, human follow-ups called out.

## Optional additions

- **Fresh-eyes verification** (if your harness supports subagents): "Every ~20 files, spawn a fresh-context reviewer subagent to diff a sample of migrated files against their Jest originals and confirm assertions are semantically unchanged." Author-models are poor critics of their own conversions; a fresh context catches drift.
- **Git checkpointing** (if the agent can run git): "Commit after each verified batch with a message listing the files, so a bad batch can be reverted without losing the night's work."
- **Concrete API mapping table** (if your codebase leans on specific Jest features like `jest.mock` factories, fake timers, or snapshot serializers): add a short section listing your project's known equivalents so the agent doesn't rediscover them per file.

Want it more or less prescriptive, or tuned to your specific harness (tool names, commit permissions)? `/prompt-enhancer:run-evals` can also A/B test this against your original on sample runs.
