# The Canonical-Home Rule

Here is a failure that looks harmless and is not. Your knowledge base says, in one note, that the target for a metric is 20% by year end. Another note, written two months later by someone else, says the target is 25%. Both notes are live. Both get read. Two people, or two skills, reach two different "facts," and a decision gets made on the wrong one. Nobody did anything wrong; the second note's author simply didn't know the first existed. The fact had two homes, and the two copies drifted apart.

This is the failure the canonical-home rule exists to prevent, and the discipline that enforces it is the second structural pillar of the system, after the [Monday Cascade](01-monday-cascade.md).

## One fact, one home

The rule is one sentence: **every fact has exactly one canonical home, and everywhere else either references it or is a violation.**

A metric definition lives in the metrics note, and only there. An org structure lives in the org note. A strategy lives in the strategy note. If another note needs to mention the metric, it links to the metrics note ("see [metrics] for the canonical target") rather than restating the number. The moment a number is restated rather than referenced, you have created a second copy that will drift, because the two copies update on different schedules and nobody is watching the gap between them.

This sounds obvious and is almost never done, because restating is easier than linking in the moment, and the cost is invisible until weeks later when the copies disagree. The discipline is to pay the small cost now (link, don't restate) to avoid the silent cost later (two facts, one wrong).

## The hard part is writes, not reads

Reading from a single home is easy. The hard part is *writing*, because in a system with many skills, several of them want to update the same shared notes, and uncoordinated writes are how a canonical note gets corrupted. So the rule needs an enforcement mechanism on the write side, and that mechanism is a three-category write discipline.

**Category A, canonical owner.** A skill that owns a note tree and writes only to that tree. The workflow scorecard owns the workflow-scorecard landing and its children; nothing else writes there. One note, one owner, no contention.

**Category B, proposer.** A skill that produces content destined for a shared note but is *forbidden from writing to it directly*. Instead it appends a proposal to a queue. The source-ingestion skill and the doc-ingest skill are proposers: they read widely, synthesise, and propose, but they never touch a target note. They route everything through the queue.

**Category C, writer.** The one or two skills authorised to *apply* queued proposals to shared notes, each write carrying an audit trailer. In this system, the end-of-day and weekly-reflection skills are the only writers. Everything that wants to change a shared note flows through them.

The shape is deliberate: **many proposers, few writers, one owner per note.** A dozen skills can want to change the knowledge base, but only two ever do, and each change is traceable to one writer. This is what makes concurrent writes safe: there is no concurrency, by design.

## The audit trailer

Every shared-KB write carries an inline trailer: who wrote it, who proposed it, when, at what evidence tier, and that it passed a leak scan. It looks like clutter and it is load-bearing for two reasons.

First, **traceability**. When a canonical note says something surprising, you can see exactly which skill wrote it, on which run, from whose proposal. There is no anonymous content.

Second, **idempotency**. The writer skills run on a backstop schedule and can re-run or catch up after a missed day. Before applying a proposal, a writer scans the target for an existing trailer matching that proposal. If the trailer is already there, the proposal was applied on a prior run that crashed before clearing the queue, and the writer skips it. The trailer is the guard that makes re-runs safe. Without it, a catch-up run would double-apply every patch it had already applied.

## The allowlist for intentional exceptions

A few notes are *deliberately* multi-writer: the routing index that several skills update, the changelog, the proposal queue itself, the shared archive parent whose children are unique per run. These sit on an explicit allowlist. The allowlist matters because it makes the exceptions *visible and finite*. A multi-writer note that is on the allowlist is a coordinated relationship; a multi-writer note that is not is a collision. Adding to the allowlist is a deliberate act, never a reflex, because each entry is a place the collision check has been told to stop looking.

## How the rule is enforced, not just stated

A rule that depends on every skill author remembering it will be violated, because authors are human and it is late and restating is easier than linking. So the canonical-home rule is enforced in two places, automatically.

At **package time**, the authoring gate (see [The A9 Enforcement Gate](04-a9-enforcement-gate.md)) refuses to ship a skill that restates canonical content, or that declares a write to a note another skill already owns. Restatement and collision are caught before the skill goes live.

At **review time**, the monthly knowledge-base audit hunts for canonical-home violations directly: it searches for facts that appear in more than one note without a "see [canonical] for the authoritative version" marker, and flags every one. Anything that slipped past the package-time gate gets caught here, while the divergence is still small enough to be cheap to fix.

The two enforcement points are belt and braces: the gate prevents most violations at authoring, the audit catches the rest before they cause a decision to diverge.

## Why this is a pillar, not a nicety

A knowledge base that more than one person and more than one skill reads and writes will rot, and it rots in this specific way: the same fact ends up in two places, the two places drift, and the system's outputs quietly start contradicting each other until nobody trusts them. The canonical-home rule is the single discipline that prevents that rot. One home per fact; reference, never restate; many proposers, few writers; every write traceable; exceptions on an explicit, finite allowlist; enforcement at both package time and review time.

The worked examples are spread across the system because the rule is system-wide: the write categories are defined in [A5 of the Skill Output Standards](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/blob/main/drafts/project-instructions/skill-output-standards.example.md), the proposer pattern is in the [ingestion skills](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/kb-infrastructure/kb-source-ingestion/), the writer pattern is in [end-of-day-review](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/personal-os/end-of-day-review/), the violation-detection is in [kb-quality-audit](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/kb-infrastructure/kb-quality-audit/), and the enforcement is in [skill-creator-a9](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/personal-os/skill-creator-a9/). The rule is the thread that runs through all of them.
