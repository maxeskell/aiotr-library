# essays/

Architectural essays: the design layer behind the skills. Where a `SKILL.md`
tells you what a skill does, these explain why the patterns are shaped the way
they are, and what breaks when they are missing.

These are prose explainers, not drop-in fragments. Read them to understand the
reasoning; copy the patterns into your own setup as you see fit. For copy-paste
`CLAUDE.md` rule blocks, see [`../instructions/`](../instructions/) instead.

## Inventory

| # | Essay | What it covers |
| --- | --- | --- |
| 01 | [The Monday Cascade](01-monday-cascade.md) | Parallel gather, then sequential synthesis, then briefing. How one orchestrator skill replaces six independent cron jobs. |
| 02 | [The Canonical-Home Rule](02-canonical-home-rule.md) | Cat A / Cat B / Cat C write discipline. Every fact has exactly one home; everything else points at it or writes with attribution. |
| 03 | [Synthesis Discipline](03-synthesis-discipline.md) | SD-1 to SD-5: the output contract for high-stakes synthesis skills. Readability gate, reader-not-producer prose, inline source tags. |
| 04 | [The A9 Enforcement Gate](04-a9-enforcement-gate.md) | Skill-authoring rules enforced by another skill. A ledger of failure modes, each one earned the second time it bit. |
| 05 | [Source-and-Date Tags](05-source-and-date-tags.md) | Every numeric claim carries its source and pull date. Trust-by-default through tagged provenance. |
| 06 | [The Personal Skill Index](06-personal-skill-index.md) | Route-table, not hardcoded. Adding the Nth skill does not mean editing the first N-1. |

## Reading order

01 to 04 are the substantive design patterns; 05 and 06 are the plumbing that
makes them work. If you read only one, read **01 (The Monday Cascade)**: it is
the most self-contained and maps the rest.

## Where these come from

These essays document the patterns behind the public
[reference-design repo](https://github.com/maxeskell/maxeskell-personal-ai-operating-system).
Links inside the essays that point at specific skills or instruction files
resolve to that repo, where the worked implementations live.

These copies are canonical. The reference-design repo's `drafts/essays/`
holds pointer stubs back here, so the essays have exactly one editable home,
which is the rule [essay 02](02-canonical-home-rule.md) argues for.
