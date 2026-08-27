# Rules

<!-- TEMPLATE. Fill the bracketed values. Keep this file under 700 words, forever. -->

**<What this project optimizes for, in one line.>**

Speed is the constraint. Everything below exists to keep the edit-to-verified loop short
and to stop this repository accreting apparatus faster than anyone can remove it.

Each rule is **[check]** (enforced in CI) or **[review]** (enforced by a human).

## Budgets — every budget is a trade, not a limit

- **5 blocking checks, 60s total.** A sixth requires deleting one, in the same PR. **[check]**
- **20 active design docs.** A 21st requires archiving one. **[check]**
- **This file: 700 words.** If it grows, something becomes a check or stops being a rule. **[check]**
- **1,500 lines per source file.** Generated files exempt — and generated files are built,
  not committed. **[check]**

A limit gets exceptions. A trade doesn't: it forces someone to name what matters less.

## Loop shape

- **Every check reports all failures in one run.** Fail-fast is banned. A check that
  surfaces one defect per run turns N bugs into N x runtime. **[check]**
- **Nothing blocks on anything slower than 60 seconds.** Slow verification runs
  asynchronously, per-commit, attributed to the breaking commit. Run everything; block on
  almost nothing. **[review]**
- **Fix loops get 3 rounds.** Then stop and either escalate to a human or descope the
  target. Never "iterate until green." **[review]**

## Time budgets

Tracked every commit. A regression is a P0 with a name on it. The loop rows ship with
defaults because human attention has a fixed timescale regardless of what you are
building; tighten or loosen them deliberately, in this file, with a reason.

| | default |
|---|---|
| Incremental rebuild after a one-line edit | 15s |
| Test what you just changed | 30s |
| Blocking gate suite | 60s |
| Full build, warm cache | 5 min |
| **<the product latency that actually matters>** | **required — you must name one** |

The last row has no default and cannot be left blank: `caps` fails until it names a real
measurement. A stack that ships opinions about your build and none about your product is
just vocabulary.

## Scope

- **`NOT-DOING.md` is binding.** Moving something onto the doing-list means writing why
  and taking something off. **[review]**
- **Delete; don't deprecate.** No compat shims or migration paths before 1.0. **[review]**
- **Nothing is compiled, transpiled, or generated at runtime that could be built ahead of
  time.** **[check]**
- **A spec needs an implementer and a date, or it isn't written.** Specifying what you are
  about to build is transcription. Specifying what nobody is assigned to build is how a
  corpus reaches millions of words. **[review]**
- **Only implemented specs bind.** Explore, sketch, and argue freely — a document with no
  running code behind it is a proposal, and a proposal cannot block a PR or justify a
  check. **[review]**

## Agents

- **Agents remove apparatus freely and add none.** An agent PR that adds a check, script,
  registry, config file, or design doc needs explicit human approval saying so. Agents
  produce governance faster than humans can read it, and every piece looks reasonable
  alone. **[review]**

## Shared machines

- Warm, pre-provisioned worktrees, one lane each. Never clone fresh.
- **Never `git stash`** — the stash stack is global across worktrees and will apply another
  lane's work into yours. Use a temporary commit.
- Kill only PIDs you recorded at launch. Never by name or pattern.

## The five checks

`build` · `test` · `lint` · `caps` · *<one slot — spend it deliberately>*

`caps` enforces the budgets above. Everything else runs async.
