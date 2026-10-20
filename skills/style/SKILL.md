---
name: style
description: How to shape code in this repository — structure, banned constructs, compile budget, comments, tests. Load before large-scale work: a new subsystem, a refactor crossing several files, or a decision about how something should be organized. Not needed for a bug fix.
---

# style

The checks say what is *not allowed*. This says what is *good*, which is the part that
needs judgment and therefore cannot be a check.

Loaded on demand. Written rules cost inference on every task that carries them, so this one
is carried only by tasks about to make structural decisions. Fixing a bug? Close this.

## The rule for every entry below

Each principle names **why it is not a check**:

| | meaning |
|---|---|
| **judgment** | Permanently unmechanizable. A linter cannot tell a good abstraction from a bad one. |
| **queued** | Should be a check. Names the check, and what must exist before it can be written. |
| **rationale** | Already enforced elsewhere; here only because the reason is worth knowing. |

A principle with none of the three is deleted. A queued check lands async or inside `caps` —
never as a sixth blocking check. The file is capped, so a new entry displaces one.

## The test

Before a structural decision: **can the next reader predict control flow from the text on
screen?** Reading a callee's signature is fine — that is what signatures are for. Simulating
execution order is not. The line is hidden control flow, not file count. *judgment*

## Structure

- **Colocate by feature.** A slice is route, handler, types, validation, tests. Split on
  deployment, testability, or concurrent editing. In Rust the unit is the **crate**, not the
  folder: unit tests go in `#[cfg(test)]` beside the code, because every file under `tests/`
  is another binary to link. *judgment*
- **At the 1,500-line cap, split at the nearest real boundary** and keep the public surface
  where it was. A file that hit the cap is usually one slice doing two jobs — that is the
  split, not a layer. *rationale — the cap is `caps`; where to cut is not.*
- **Duplicate until the third instance.** A shared helper couples its callers; changing one
  then means reading the abstraction and everyone else on it. Shared code must be *stable*,
  not merely common. *judgment*
- **Put the I/O boundaries where they can batch.** Round trips are decided by layout, not by
  tuning, and this is the moment they get decided. *judgment*

## Banned: action at a distance

Not constructs — *hidden control flow*. Each one replaces "read the code" with "run the
program in your head," on every later task that touches the file.

Code that runs on import (assets and CSS are fine) · mutable global state (an immutable
`OnceLock` is not) · `useEffect` that writes state another effect reads · event buses as
control flow · DI **containers** (React context is not one) · implicitly-passed task-locals
· macro DSLs that generate control flow.

*queued — the greppable half is `import/no-unassigned-import`, clippy `disallowed_types`,
and a `useEffect` denylist, once lint lands. The rest is judgment, permanently.*

## Compile budget

Iteration speed is goal #1, and it is spent here more than anywhere else.

- **Scope the inner loop to the crate.** `cargo clippy -p`, not a workspace check — and not
  `check` *and* `clippy`, which pay for the same work twice. Build a binary when you need to
  know it runs; a static check passing is not that. *judgment*
- **Rust: keep proc-macro and derive-heavy code in leaf crates.** It runs at build time, and
  a hub everything imports invalidates everything. `pub(crate)` by default. *queued —
  `cargo metadata` can assert build-time deps are leaves, once the workspace exists.*
- **TypeScript: no barrel files.** An `index.ts` re-exporting a slice is `serde_derive` in
  the root crate — one leaf edit invalidates every importer. Agents emit barrels by default.
  *queued — `no-restricted-imports` on the barrel paths.*
- **Vite: HMR is the loop, `tsc --noEmit` is the gate.** Putting the type checker on the
  blocking path burns the 15s rebuild budget. Keep deep inference out of packages everything
  imports, and annotate exported signatures so declaration emit can skip the checker.
  *judgment*
- **A dependency is a line item; build-time dependencies cost double.** Adding one is a
  budget decision, not a convenience. *queued — deps per crate.*

## Comments

An agent reads code better than a human and has no institutional memory at all. The value of
the two kinds of comment moved in opposite directions.

- **Why, never what.** The constraint, the benchmark that justified the ugly version, the bug
  being worked around. If deleting it loses nothing, it was noise.
- **Why-not.** Name the obvious refactor and why it is wrong, or it gets attempted every few
  months forever. The highest-value comment in any codebase, and almost never written.
- **Link.** Issue numbers and upstream bugs survive; adjectives do not.

*judgment — "restates the code" is lint; "explains the right thing" is review.*

## Tests

- **Determinism is a hard requirement.** No wall-clock waits, no real network, no fixed ports,
  no shared temp paths. A flaky suite teaches that red means retry. *queued for the named
  APIs — `disallowed_methods`; hermeticity itself stays judgment.*
- **Never re-bless to get green.** `--update`, regenerating a snapshot, widening a matcher to
  whatever came back. Correcting a wrong assertion is allowed and often right — but only with
  the reason the old one was wrong, in the diff. *judgment*

## This file

Capped, and it grows by displacement. Wanting to add a section is the signal to check whether
the last one earned its place — usually it should have been a check.
