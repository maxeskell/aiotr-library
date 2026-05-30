# Synthesis Discipline

An AI can produce a synthesis that looks authoritative and is quietly wrong, and the ways it goes wrong are not random. They cluster into a small set of failure modes that recur across every analytical output. Synthesis discipline is the contract that closes those failure modes, one rule per mode, and it is the difference between a report you can act on and a report that survives right up until someone in the room challenges it.

The contract has five rules. They are dull individually and powerful together, because each one corresponds to a specific way a confident-looking number misleads.

## SD-1: state the denominator

Any figure about churn, retention, conversion, or adoption must state its cohort denominator. "Retention is 80%" is not a finding; "80% of accounts that went live more than 180 days ago are still active" is. The reason this is rule one is that the same headline computed against the wrong base can be off by an order of magnitude, and every downstream decision built on it inherits the error. A "lift" measured against all customers when it should have been measured against eligible customers does not just exaggerate; it points the strategy at the wrong place. The denominator is not a footnote. It is the claim.

## SD-2: exposure is not opportunity

Any aggregate currency figure carries an Inline Figure Pack: the math shown, the assumption named, a sensitivity range rather than a point estimate, and a confidence level. And it observes one specific distinction relentlessly. "£X exposed" is gross revenue sitting on the platform without the thing you are measuring; it is not the incremental upside of converting that base. Incremental opportunity is exposure times conversion times uplift, and it is typically an order of magnitude smaller than the exposure headline.

This rule has a history. A large exposure figure once landed in an executive discussion without its math, and a five-second scale check would have killed it: the exposure quoted was larger than the total it supposedly sat inside, which is a structural impossibility, not a rounding error. The figure was the sum of two overlapping populations counted twice. It could not have survived one challenge in the room. The fix is structural, not a reminder to be careful: do not ship an aggregate figure without showing the arithmetic that produces it, and never quote exposure as if it were opportunity.

## SD-3: flag the method gap, do not paper over it

When a finding has no measurement behind it (no clean cohort, no segment-level join, no measured intervention) it is marked "method-gap, not measured," and the unblock is surfaced as a question for whoever owns the data. It is never dressed up as a directional read. This is the rule that resists the strongest pull in AI synthesis, which is to produce a confident sentence where there is only an absence. A flagged gap is honest and actionable: it tells the reader exactly what is unknown and exactly who could close it. A papered-over gap is a guess wearing the costume of a finding, and it is worse than saying nothing, because the reader acts on it.

## SD-4: keep overdue decisions out of the forward plan

Operational decisions that were due in a prior period and are still open get their own section, distinct from proposals about what to do next. This sounds like formatting and is actually a discipline against a specific self-deception: re-dressing last quarter's unmade decision as this quarter's fresh idea, which silently resets its clock to zero and lets a three-week-overdue call look like new thinking. Separating "this is overdue" from "this is proposed" keeps the overdue thing visibly overdue.

## SD-5: tag every number with its source and date

Every numeric claim carries an inline tag naming the system it came from and the date it was pulled. This rule is important enough that it has its own essay ([Source-and-Date Tags](05-source-and-date-tags.md)), because it is what makes the difference between a figure you can trust and a figure that was true once and has been quietly stale ever since.

## The prose layer: write for the reader, not the producer

The five rules govern the figures. One more discipline governs the words, and it is the single biggest reason an AI report reads as machine-generated: the prose points at the report and its own production instead of at the world.

Every sentence in a synthesis output has to pass one test. Would a reader who has never seen the skill understand and act on this sentence? If the sentence says "this run," "the table above," "carry-forward," "the previous version," or names an internal component, it fails, because the reader is inside the artefact and does not care how it was assembled. The scan cuts the LLM tells (the artifact-talk, the process-talk, the skill-maintenance leakage, the unexplained jargon, the document-structure references) against an explicit ban-list, to zero hits, before any write. What survives points only at the signal, the owners, and the numbers.

This is not a tone preference. It is the operational reason a synthesis reads as though a sharp colleague wrote it rather than a model, and it compounds with the figure rules: a report whose numbers are disciplined and whose prose points at the world is one an executive trusts; a report that fails either is one they learn to skim.

## Why a contract and not a style guide

The reason synthesis discipline is a numbered contract that skills cite ("SD-1 to SD-5 apply") rather than a style guide nobody reads is that the failure modes are specific, recurring, and individually invisible. A wrong denominator does not look wrong. An exposure-as-opportunity figure does not look wrong. An inference dressed as a finding does not look wrong. Each one looks like a perfectly good sentence. The only defence is a checklist applied every time, enforced rather than hoped for, and tied to the [authoring gate](04-a9-enforcement-gate.md) so a synthesis skill cannot ship without referencing it.

The worked examples are everywhere the system produces analysis: the figure disciplines anchor [mission-synthesis](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/examples/mission-synthesis/), the cohort and exposure rules run through the [market](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/examples/market-attractiveness-monthly/) and [customer-evidence](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/examples/monthly-customer-evidence-analysis/) skills, and the reader-not-producer prose scan is most fully specified in [okr-snapshot-monthly](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/examples/okr-snapshot-monthly/). The rules are defined canonically in [the Skill Output Standards](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/blob/main/drafts/project-instructions/skill-output-standards.example.md).
