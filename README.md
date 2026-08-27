# cdcstack

An opinionated working posture you can install into any repository.

Most stacks are additive — they give you a framework, a database, a deploy target.
cdcstack is mostly **subtractive**. Its content is the limits: how many blocking checks
you may have, how many design documents, how long anything is allowed to take before it
stops being allowed to block you. It is process, not technology, so it composes with
whatever you are already building in.

The thesis: **speed is upstream of quality.** A project where the edit-to-verified loop
takes ninety seconds finds more bugs per day than one where it takes ninety minutes,
because the ninety-minute loop gets run a tenth as often and rots into ceremony. Every
rule here exists to defend the loop.

## What it installs

| Component | Source | What it is |
|---|---|---|
| **LLP** | `ccheever/llp` | Linked Literate Programming — numbered design docs with types, statuses, and a review process. Installed via `llp-adopt`. |
| **Rules** | `rules/RULES.md` | One page, hard-capped. Budgets stated as trades. |
| **Scope fence** | `rules/NOT-DOING.md` | What this project deliberately does not do, kept longer than the doing-list. |
| **Tickets** | `scripts/issue.mjs` | Filesystem tickets — `issues/YYYYMMDD-slug.md`, closed by moving to `issues/closed/` with a resolution. No tracker required. |

## The composition is the point

LLP on its own produces documents faster than anyone can read them — one corpus reached
8.2 million words across 1,327 files, at which point every task paid rent on every past
document and the project could not move. The rules are what make LLP safe to install:
a cap on the corpus, and the invariant that **only implemented specs bind.** Explore as
much as you like; a document with no running code behind it is a proposal, and a proposal
cannot block a pull request.

Install either half alone and you get half a system. LLP without the caps is how you get
the 8.2M words. Caps without LLP is how you lose the reason the documents existed.

## What it deliberately leaves out

Lane manifests, authority maps, framework decision logs, debt ledgers, priority scoring,
multi-round refinement loops, per-PR metadata of any kind. Each of those was a reasonable
addition to a real project and collectively they are the thing this stack exists to
prevent. If you need one, add it — and delete something.

## Install

Not built yet. Intended shape: a `cdcstack-adopt` skill following the same pattern as
`llp-adopt` — copy from a tagged release, leave a receipt, support verified updates.
