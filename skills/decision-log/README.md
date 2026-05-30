# decision-log

Mid-week capture for significant leadership decisions. The schema is universal (decision / type / context / alternatives / owner / success metric / status) and the rest is structural plumbing around how decisions get into the KB cleanly and how the log doesn't bloat over time.

The Phase 1 of the [monday-briefing](../monday-briefing/) uses this log to surface "decisions whose review date is approaching". The log is what makes that question askable.

## Files in this folder

- **[SKILL.template.md](SKILL.template.md)**: portable shape with 8 placeholders for KB system, note locations, the strategic-priority term.
- **[SKILL.example.md](SKILL.example.md)**: one filled-in version. Slite as KB, "mission" as the priority term, real decision-log structure.
- This README: the design patterns this skill implements, and notes on adapting it to your context.

## Design patterns this skill implements

- **One-way-door confirmation.** Step 3b explicitly stops and shows the formatted entry before writing. A logged decision becomes canonical and shapes how future sessions reason about what was decided. The friction of "confirm or edit before I write" is the point.
- **Schema with success metric.** Every entry asks for a success metric. If you can't articulate how you'll know the decision was right, that's a sign the decision needs more thinking. The skill surfaces that gap rather than letting you bypass it.
- **Alternatives field.** Logging *what wasn't chosen* explains *why this was chosen*. When you (or anyone) re-litigates the decision six months later, the alternatives line saves an hour of re-reasoning.
- **Active / Closed / Superseded status with explicit supersession links.** Decisions don't just close; they get overridden. The Superseded-by link means you can trace any current decision back through its lineage of overrules. The decision log becomes a chronology of how thinking changed, not just a list of calls.
- **Archive-when-it-bloats pattern.** Step 4 archives closed and superseded decisions to a child note when the log hits 20+. Active decisions stay on the landing page; history stays accessible but out of the way. The cutoff is a heuristic; adjust to your own scanning capacity.
- **Fetch fresh, halt on unreachable.** Structural decision: never log decisions to memory or email if the KB is down. The KB is the canonical home, and a decision logged elsewhere doesn't exist in the system that needs to retrieve it.
- **Governance trailer.** After any KB write, check whether the index needs updating and whether the changelog needs an entry. Structural changes (new note, note moved, scope changed) need both. Routine content changes need neither. Keeps the KB easy to find your way around as it grows.

## Related skills

- **[monday-briefing](../monday-briefing/)**: reads the decision log in Phase 1 (decision review dates approaching) and Phase 3 (decision-check after each tactical item).
- **weekly-self-reflection** (in personal-os/, not yet templated): invokes this skill automatically during the Sunday review to extract decisions from the week.

## When this skill matters most

The decision log earns its place when:

- You find yourself re-litigating the same call three times because nobody can remember the original reasoning. The Context + Alternatives fields end that conversation in five minutes.
- Decisions feel like they "happen" without anyone owning them. The Owner field forces the question: who's actually accountable for executing this?
- Strategic decisions drift quietly. The Success metric + Status fields make drift visible: when reality stops matching the metric, the decision needs revisiting on purpose, not by accident.

Without this skill, decisions live in chat history, calendar invites, or someone's head. With it, they live in one place, with the *why* attached, and they get reviewed on a cadence.
