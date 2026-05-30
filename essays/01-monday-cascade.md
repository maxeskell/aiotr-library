# The Monday Cascade

Most people who automate their work with AI end up with a drawer full of disconnected tools. A skill that summarises the week. A skill that checks the dashboards. A skill that drafts the standup. Each one useful on its own, each one fired by hand or by its own little cron job, none of them aware the others exist. The drawer fills up. The tools start to overlap, contradict, and rot, and the person who built them slowly stops trusting the output because they can no longer tell which tool ran, when, against what data.

The Monday Cascade is the answer to that drawer. It is the single most load-bearing structural idea in this whole system, and it is worth understanding before any individual skill, because it is the thing that turns a pile of skills into an operating system.

## The problem with cron jobs

Say you have six things you want to happen every Monday morning before you start your week. A scan of your knowledge base for anything that broke over the weekend. A blind-spots report that hunts for what is suspiciously quiet. A delivery-and-blocker status across your teams. A coaching read on your direct reports. A decision-log sweep. And finally, a briefing that pulls all of that into a prioritised walkthrough of your week.

The naive way to build this is six scheduled tasks, each set to fire at a slightly different time, spaced far enough apart that you hope the earlier ones have finished before the later ones start. This works until it doesn't, and it stops working in three specific ways.

First, **race conditions**. The coaching skill needs the delivery briefing's output. If the briefing is slow one week, the coaching skill runs against last week's data and nobody notices, because the coaching skill has no way of knowing the briefing it depends on hasn't finished.

Second, **silent staleness**. A skill three hops upstream fails to fetch its data. It produces a thin output anyway. Every skill downstream consumes that thin output as if it were complete, and the briefing you read on Monday is built on a quiet failure that happened at 4am.

Third, **undiagnosable crashes**. When the whole thing produces nothing, or produces something wrong, you have six independent logs and no narrative. You cannot answer the only question that matters, which is "what actually ran, in what order, against what."

The deeper problem underneath all three is that **cron schedules encode dependencies as timing hopes**. "Run the coaching skill at 6am because the briefing usually finishes by 5:55" is not a dependency declaration. It is a prayer with a timestamp.

## The shape: parallel gather, sequential synthesis, interactive briefing

The cascade replaces the six cron jobs with one orchestrator that runs the same six skills in explicit dependency order. The shape has three phases, and the phases are the whole idea.

**Phase 1 is the parallel gather.** Every skill that has no dependency on any other skill runs at once, in parallel. The KB scans, the blind-spots report, the delivery briefing, the decision-log sweep: none of these needs another's output, so they all launch together as subagents and the orchestrator waits for the phase to complete. Parallel because it is faster and because there is no reason to serialise work that does not depend on each other.

**Phase 2 is the sequential synthesis.** The skills that *do* consume Phase 1 outputs run now, after Phase 1 has finished, in order. The coaching skill reads the delivery briefing's handoff. It runs inline so its output is in context for what comes next. Sequential because the dependencies are real, and the phase boundary is where those dependencies are declared honestly instead of hoped for.

**Phase 3 is the interactive briefing.** The synthesiser reads everything Phases 1 and 2 produced and turns it into a conversation. This is the only phase that waits for the human. The rest ran autonomously before you sat down; this is where you engage.

The orchestrator does no analysis of its own. It does not summarise, score, or judge. Its entire job is to invoke the right skills in the right order and manage the flow between them. This separation (orchestration is not the same job as work) is what lets the cascade fail in one place without taking the whole pipeline down.

## Failure as information, not silence

The cascade's most important property is what it does when something breaks. A cron-job pile responds to failure with silence: the downstream skills run anyway, on whatever stale data they can find, and the failure surfaces days later as a wrong number in a meeting.

The cascade responds to failure by *telling the next skill what is missing*. If a Phase 1 skill fails, the orchestrator records it and passes that fact downstream, so the Phase 2 skill that needed it knows to degrade gracefully (fall back to a direct query, mark the output as partial, flag the gap) rather than pretending the data is there. The briefing opens with a status line: this many skills completed, these failed for these reasons, the weekend inputs are fresh or stale, proceeding with complete or partial data. You read the briefing knowing exactly how much to trust it.

This is the difference between a system that breaks loudly and one that breaks quietly, and it is the difference between a system you can trust and one you slowly stop reading.

## Breadcrumbs

There is one more piece, small and unglamorous, that earns its place every time the orchestrator crashes mid-run: a breadcrumb log written at four points. Before Phase 1 starts. After Phase 1. After Phase 2. At the end. The pre-Phase-1 breadcrumb is the critical one, written before any work happens.

The reason is diagnostic. When a run fails, the *pattern* of breadcrumbs tells you where. Breadcrumb 1 present, 2 through 4 absent: it died in Phase 1. Breadcrumbs 1 and 2 present, 3 and 4 absent: it died in Phase 2. Without the breadcrumbs, diagnosing a crashed orchestrator means inspecting which downstream notes happened to get updated, which is exactly the fragile, inferential debugging the cascade exists to eliminate. The breadcrumb turns "what happened?" into a five-second read of a log.

## Why this is the keystone

Once you have a cascade, three things change.

Adding a skill to your Monday morning becomes a one-line edit to the orchestrator, not a new cron job to schedule and pray over. The orchestrator is the single source of truth for what runs and in what order; if a skill is in the cascade it is in that file, and if it is not in that file it is not in the cascade. (See the [Personal Skill Index](06-personal-skill-index.md) essay for how the orchestrator resolves *which* skills exist without hardcoding them.)

The dependencies between your skills become explicit and visible, instead of being smeared across six cron timestamps that only you understand and only until you forget.

And the output earns your trust, because every run tells you what it ran and how much of the data was real. A personal AI operating system lives or dies on whether you still read its output six months in. The cascade is what keeps you reading.

The worked example is in [`skills/personal-os/monday-cascade`](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/personal-os/monday-cascade/) (the orchestrator) and [`skills/personal-os/monday-briefing`](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/personal-os/monday-briefing/) (the Phase 3 interactive synthesiser it hands off to). Read the cascade first; it is the map, and the rest of the system hangs off it.
