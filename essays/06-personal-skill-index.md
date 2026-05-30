# The Personal Skill Index

The first three skills in a system can hardcode everything. Skill A knows the note ID it writes to. Skill B knows the channel it posts to. Skill C knows where the others put their output, because there are only two others and you wrote all three last week. Hardcoding is fine at three skills. It is a slow catastrophe at thirty.

Here is how the catastrophe arrives. You move a note. Now you have to find every skill that referenced its ID and update each one. You add a skill that produces output others consume. Now you have to edit every consumer to teach it the new skill exists. You rename a channel, change a cadence, retire a skill. Each change ripples into every skill that encoded the old fact, and because the encoding is scattered across thirty files, you miss one, and a skill quietly reads from a note that no longer exists. The cost of the Nth change scales with N, which is the definition of a system that gets harder to maintain the more useful it becomes.

The Personal Skill Index is the piece that breaks that scaling. It is the least glamorous idea in the system and the one that decides whether the system survives past a dozen skills.

## A route table, not a hardcoded address

The idea is borrowed wholesale from networking, and it is worth stating in those terms because the analogy is exact. A skill should not contain the address of the thing it talks to. It should contain a *name*, and look the name up in a shared table at runtime. The table, the Personal Skill Index, maps names to addresses: this skill's output lives at that note, this kind of data resolves to that location, this run log is at that ID. Every skill, on startup, fetches the index and resolves what it needs from it. No skill hardcodes a note ID, a channel ID, or another skill's output location. They all ask the index.

The rule that every skill obeys is one line, and it appears at the top of nearly every skill in this system: *fetch the index first; resolve all IDs from it; do not hardcode.* That single convention is what converts thirty brittle skills into one maintainable system, because it moves every address out of the skills and into one place.

## The payoff is that the Nth skill is free

Once addresses live in the index, the scaling inverts. Moving a note is a one-line edit to the index; every skill that referenced it by name now resolves to the new address automatically, with no skill edited. Adding a skill that produces consumable output means adding one row to the index; consumers that look up output by name find it without being touched. The [Monday cascade](01-monday-cascade.md) leans on this directly: adding a skill to Monday morning is a one-line edit to the orchestrator precisely because the orchestrator resolves *which* skills exist and where their output lands through the index rather than through hardcoded references.

This is the property that matters: **adding the Nth skill does not require changing the first N-1.** A system where each addition forces you to revisit everything you have already built is a system you stop adding to. A system where the Nth skill is a clean addition is one you keep extending for years. The index is the difference between the two, and it is the whole difference.

## One indirection, paid once

There is a cost, and it is honest to name it: a layer of indirection. A skill cannot be read in isolation and understood completely, because the addresses it uses are one lookup away in the index. You trade "everything this skill does is visible in this file" for "this skill names what it needs and resolves it at runtime." That is a real loss of locality.

It is worth it, and it is worth it for the same reason indirection is worth it everywhere in software: the thing you give up (reading one skill in full isolation) is cheap, and the thing you gain (changing one fact in one place) is the expensive operation you do constantly. You pay the indirection cost once per skill, at authoring. You would pay the hardcoding cost every single time anything moved, forever. The index trades a fixed small cost for a recurring large one, which is always the right trade when the recurring cost scales with the size of the system.

The [authoring gate](04-a9-enforcement-gate.md) makes the trade non-optional: it has a category that refuses to package a skill containing hardcoded note or channel IDs, on the grounds that those belong in the index. You cannot quietly opt out of the indirection to save yourself a lookup, because the opt-out is exactly the thing that reintroduces the scaling problem.

## A shared mutable index needs its own guardian

An index that everything depends on becomes a single point of failure, and it fails in a particular way: it points at something that is no longer there. A note gets deleted but its row stays in the index, so every skill that resolves that name now follows a pointer into the void. A skill stops producing output but the index still lists it, so a consumer waits for data that never comes. The index is only as trustworthy as the correspondence between its rows and reality, and that correspondence rots on its own.

So the index gets a guardian. The daily knowledge-base health check verifies, among other things, that every note ID in a routing position of the index actually resolves (a phantom-ID check), and that every skill listed in the index has produced recent output, and every recent output traces back to a listed skill. A pointer into the void is caught within a day, not whenever a skill next happens to follow it and fail silently. This is the same instinct as the cascade's breadcrumbs and the canonical-home audit: the most load-bearing shared structure gets the most active checking, because its failures are the ones that cascade.

## Why this is the plumbing the rest stands on

The other essays describe disciplines you can see in the output: the cohort framing, the source tags, the orchestrated Monday run. The index is the one you never see, and it is the one that makes the others possible to maintain. The cascade can resolve its skills without hardcoding because the index exists. The canonical-home rule can say "reference, don't restate" because there is a stable place to reference. The authoring gate can ban hardcoded IDs because there is somewhere for them to live instead. Pull the index out and every other pattern degrades into something you have to maintain by hand across thirty files. It is the route table that turns a pile of skills into a system that scales, and the reason adding the next skill still feels easy at thirty as it did at three.

The convention appears in nearly every skill in the repository as the opening "fetch the Personal Skill Index, resolve IDs from it, do not hardcode" step; see [monday-cascade](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/personal-os/monday-cascade/) and [end-of-day-review](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/personal-os/end-of-day-review/) for clear instances. The phantom-ID and index-sync checks that keep it honest are in [kb-health](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/kb-infrastructure/kb-health/), and the package-time ban on hardcoded IDs is in [skill-creator-a9](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/personal-os/skill-creator-a9/).
