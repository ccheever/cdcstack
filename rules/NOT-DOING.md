# What <project> Does Not Do

<!-- TEMPLATE. This should end up longer than your doing-list. -->

This is the most valuable file in the repository.

## The bar that makes this list derivable

<Name one concrete, checkable thing that must work. Everything not required by it does
not exist. Without this line the fence is soft and every exclusion is arguable.>

## Not building

<Group by area. For each: what, and one line of why. Be specific enough that someone can
tell whether a proposed change violates it.>

## Process not doing

<This half usually matters more than the feature half. Ceremony you are not adopting:
doc-before-code requirements, per-PR metadata, review rounds, registries, gates slower
than the loop.>

## Deliberately worse

<Where you are knowingly accepting a worse outcome to move faster: no backwards
compatibility, no API stability, no migration guides, sparse docs.>

## Moving something off this list

Write one line naming what it unblocks, and take something off the doing-list in the same
PR. If nothing can come off, the answer is no.
