# end-of-day-review

The daily forcing function. Confirms yesterday's chosen items, surfaces what happened today, asks the user to pick tomorrow's three, and runs the Open Items Tracker end-to-end as a side effect.

This is the densest skill in the personal-OS tier. Most of its size comes from doing the maintenance work that other skills assume already happened: deduplication, completion sweep, stale-item handling per provenance, queue consumption. If the rest of the system has a single load-bearing input, this skill is it.

## Files in this folder

- **[SKILL.template.md](SKILL.template.md)**: portable shape with 16 placeholders covering KB, indexes, tracker, decision log, archive, meeting-notes app, chat tool, ticketing system, organisation reference, and the proposal queue + changelog (optional, only if you maintain Cat-A/B/C write discipline).
- **[SKILL.example.md](SKILL.example.md)**: one filled-in version with the real stack (Slite / Granola / Slack / Jira) per the editorial principle.
- This README: the design patterns this skill implements, and notes on adapting it to your context.

## Design patterns this skill implements

- **Anchor + parallel retrieval.** The skill asks two human questions ("Hardest moment today? Best moment today?") and fires evidence retrieval in the same turn. The user's typing happens while data loads. This is the single biggest contributor to the skill feeling fast; sequential ask-then-fetch would burn the user's time on something a parallel call resolves for free. The user's own read is the strongest signal; evidence is context, not replacement.
- **Three-provenance model: chosen / committed / captured.** Open items aren't a flat list. `chosen` = forward-looking, the user picked it for tomorrow (the skill is the only writer). `committed` = a person is waiting (named owner, action verb). `captured` = signal pulled without a person waiting (no commitment, just observation). The three are neutral, not hierarchical. Aging rules differ by provenance: captured can be killed silently after 10 working days; committed cannot (someone is owed); chosen forces a re-decision at 10 working days.
- **Blank-field choose pass.** The skill asks for tomorrow's three with a hard cap and no proposals. The cognitive work of picking is the point; proposing a shortlist defeats the purpose. The user retains the choice, not delegates it.
- **Closure check before proposing.** Before adding a new commitment to the tracker, cross-check today's outbound chat. If the message that satisfies the commitment was already sent, mark it closed not open. This is the single biggest source of noise in this skill if skipped.
- **Completion sweep on existing items.** Once a week's worth of evidence has accumulated (5 working days), walk every tracker item and look for evidence the action got done. Catches the case where a commitment closes in a meeting or DM without the tracker being told. Three loose matches (person + topic + action verb) = strong signal.
- **Provenance-aware staleness.** Different default dispositions by provenance: captured ages out (default kill); committed never silently ages out (someone is owed; options are chase / commit (date) / de-prioritise / kill with reason); chosen forces explicit re-decision at 10 working days. Same age threshold, different decisions.
- **Duplicate scan.** After all the other classification work, walk active items pairwise and flag obvious duplicates (same person + same deliverable verb; same topic + same anchored event; same compound deliverable bundled differently). Conservative: flag fewer, better. Has to feel obviously redundant on read.
- **Today's Read self-feedback.** Recognition-first, single-improvement, evidence-cited. The same coaching discipline applied to the user that they apply to their directs. Hard rules: each claim cites a specific artefact; one improvement not three; evidence or silence (no manufacturing). The skill is allowed to skip the section if the day produced no specific good thing; that's a feature.
- **Batch confirm.** All proposed changes are presented as a single batch with persistent item numbers and batch letters. One interaction, not one per item. Silence rules differ by section (silence on a stale captured item leaves it open; silence on a chosen-at-10-wd item is not allowed; the prompt forces a pick).
- **Friday's-chosen-for-Monday rule.** `chosen for` never falls on a Saturday or Sunday. The skill always advances to the next working day.
- **Kill criterion baked in.** At week five, the skill explicitly asks the user whether it's earning its keep. If not, retire it.

## Related skills

- **[decision-log](../decision-log/)**: EOD prompts ("This sounds like a decision worth logging") and routes to decision-log rather than capturing decisions itself.
- **[meeting-prep](../meeting-prep/)**: EOD flags tomorrow's meetings with prep gaps but doesn't trigger meeting-prep automatically.
- **[monday-cascade](../monday-cascade/)** / **[monday-briefing](../monday-briefing/)**: downstream. Last Thursday's EOD note can be an anchor.
- **weekly-self-reflection** (next to template): runs Sunday; reads the past week's five EOD notes as primary evidence.

## When this skill matters most

This skill earns its place when:

- You end the day with the same list of open items you ended yesterday with, and you can't remember which ones you actually did. The completion sweep and the closure-check-before-proposing kill the "stuck list" problem in two passes.
- Tomorrow morning starts with you deciding what to do at the desk, in front of a calendar full of meetings. The blank-field choose pass moves the decision from 8am tomorrow (worst time) to 6pm today (best time).
- Things people are waiting on keep slipping. The provenance-aware staleness rule means committed items never age out silently; they surface with options that include "chase" because someone is owed.
- Your weekly reflection on Sunday has nothing concrete to reflect on. The five EOD notes a week produces become the substrate of the reflection.

The skill is unglamorous. The compounding is the point: six weeks in, the change is visible in decision quality and reply latency. Before six weeks, trust the process.
