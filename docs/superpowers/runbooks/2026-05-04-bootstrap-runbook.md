# Bootstrap runbook

Recorded operational sequence for the geb-mathlib bootstrap. The
sequence is iterated against numbered test repos
`rokopt/geb-mathlib-test-N`; the final clean iteration's sequence
is replayed against the real repo `rokopt/geb-mathlib`.

## Iteration log

| Iteration N | Outcome | Notes |
| --- | --- | --- |
| 1 | (in progress) | First run |

## Events

For each event, the runbook records: preconditions, action,
expected result, verification, rollback / cleanup, discoveries.

Marked **bootstrap-real** events are re-executed against the real
repo verbatim. Marked **test-only** events are exercised on the
test repo only and skipped on the real repo.

### A. Repo creation and bootstrap branch (bootstrap-real)

To be populated during iteration 1 (Task 3.2): create
`rokopt/geb-mathlib-test-${N}` on GitHub, initialise the local
clone, import the bundled tree content, establish `main` placeholder
and `chore/bootstrap`, first push.

### B. Hooks active

- Toolchain-watch in-sync (bootstrap-real)
- Toolchain-watch drift (test-only)
- Toolchain-watch offline (test-only)
- PreToolUse mutating-git hook prompts on raw `git checkout` (test-only)
- PreToolUse mutating-git hook allows `jj git push` (bootstrap-real)
- Smoke test in `scripts/hooks/tests/` (bootstrap-real)

To be populated during iteration 1 (Task 3.3): start a fresh
Claude Code session against the test-repo clone; observe each
SessionStart hook's banner; exercise mutating-git scenarios; run
the smoke test.

### C. Branch operations (bootstrap-real)

To be populated during iteration 1 (Task 3.4): create a topic
branch (`feat/<topic>`), commit on it, push it, observe
auto-track-bookmarks, exercise topic branch lifecycle.

### D. CI activates (bootstrap-real)

To be populated during iteration 1 (Task 3.5): push triggers
`ci.yml`, `markdown-lint.yml`, and (on schedule) `update.yml`;
each runs to completion and reports green; CI logs captured.

### E. Integration regeneration (bootstrap-real)

To be populated during iteration 1 (Task 3.6): run
`scripts/regenerate-integration.sh` against the test remote; verify
the regenerated `integration` bookmark reflects the current
`main`-plus-topics shape; verify the script's origin-precondition
guard.

### F. Mass-rebase on a simulated bump (bootstrap-real)

To be populated during iteration 1 (Task 3.7): simulate a mathlib
bump on a `bump/mathlib-<date>` branch; run
`scripts/rebase-topics.sh`; verify all topic branches rebase
cleanly onto the new `main` tip; verify the DAG remains
topologically partial-ordered.

### G. Floodgate-CI lint

- G1 forbidden import (test-only)
- G2 prefix leakage (test-only)
- G3 clean file (bootstrap-real)

To be populated during iteration 1 (Task 3.8): three cases
exercise the `Geb/Mathlib/` and `Geb/Cslib/` import-direction
rules. G1 introduces a forbidden import (e.g., `import Geb`
inside `Geb/Mathlib/`); G2 introduces a `Geb.Mathlib.` prefix in
non-import content of a `Geb/Mathlib/` file; G3 commits a clean
file. Each runs `scripts/lint-imports.sh` locally and through CI.

### G-bis. Axiom-check failing case (test-only)

To be populated during iteration 1 (Task 3.8b): introduce a Lean
declaration that depends on a non-standard axiom (e.g.,
`Classical.choice` in a non-mathlib context); verify
`scripts/check-axioms.sh` flags it and CI rejects the push.

### H. PR extraction (bootstrap-real, local-only — no push to `rokopt/mathlib4`)

To be populated during iteration 1 (Task 3.9): run
`scripts/extract-pr.sh` against a hypothetical
`Geb/Mathlib/Example.lean`; verify the script produces a clean
mathlib-shaped patch on a local `pr/<topic>` branch in a
mathlib4 clone; no push.

### I. Conflict-commit refusal (test-only)

To be populated during iteration 1 (Task 3.10): induce a
jj-export conflict (`.jjconflict-base-*`, `.jjconflict-side-*`
directories committed) and a git three-way merge marker at column
0 in a file; verify `conflict-check.yml` rejects each push;
verify the sentinel allowlist for `docs/`-scoped files with the
explicit allow comment.

### J. Process self-update (bootstrap-real)

To be populated during iteration 1 (Task 3.11): demonstrate the
documented self-update mechanism by recording a discovery from
this iteration in the discoveries log, propagating it to the
spec/plan/runbook via the same chunk-by-chunk review pattern,
and re-running the affected event to confirm the fix.

### K. Doc generation (bootstrap-real)

To be populated during iteration 1 (Task 3.12): run
`lake build Geb:docs`; verify the doc-gen4 :docs facet produces
HTML under the lake build directory; verify CI's `doc-build.yml`
publishes the rendered output (per spec § Documentation strategy).

## Discoveries log

Empty initially. Each spec/plan/runbook change made in response to
an unexpected outcome lands here with a date stamp and pointer to
the affected event.
