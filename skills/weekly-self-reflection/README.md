# weekly-self-reflection

The keystone weekly habit. Reads the week's five EOD outputs and the prior four weeks of reflections, then produces an adversarial leadership reflection, extracts decisions, and proposes KB updates, all in one Sunday-morning pass, ahead of the Monday cascade.

INVENTORY's note on this one: "the closest thing to a single load-bearing habit." If everything else in the system broke, this is the skill to keep. It's also the most opinionated about *not* being a summary.

## Files in this folder

- **[SKILL.template.md](SKILL.template.md)**: portable shape with 17 placeholders. Covers the EOD-spine + trend-spine evidence model, the depth/moments requirements, the adversarial "How I'm landing" read with self-decay, the red-team verification pass, and the optional queue-consumer.
- **[SKILL.example.md](SKILL.example.md)**: one filled-in version. Slite / Granola / Slack / Intercom / Mixpanel concrete; "mission" as priority term.
- This README: the design patterns this skill implements, and notes on adapting it to your context.

## Design patterns this skill implements

- **EOD-spine + trend-spine.** The reflection rests on two evidence layers: Source 0 (this week's five daily EOD notes, what happened day-by-day) and Source 0.5 (the prior four archived reflections, what's accumulating across weeks). Live source pulls supplement and verify rather than starting from scratch. This is what lets the reflection stand on both the week-level and the trend-level, instead of being a Sunday snapshot.
- **Depth requirement: push, don't summarise.** The reflection must tell the user something they didn't already see. Review-style narration ("X happened, Y was discussed") is explicitly banned because the user already lived the week. The bar is 2-3 hard, less-safe insights per pass, with the threshold for surfacing them pushed *down* on Sunday, not up. Completeness is treated as a tell of summary, not insight.
- **Moments requirement: quote or drop.** Generic self-critique is worthless. Every "noticing" bullet must cite a specific artefact (a chat message, a meeting line, a decision). Three forcing prompts run every pass: where did the user close down debate, what should have been delegated, where did they step into a direct's lane. "No evidence this week" is a valid answer; manufacturing critique is not.
- **Absence-of-evidence rule.** The skill never infers "didn't happen" from chat/meeting silence. Those sources only capture written and recorded conversations; a 1:1 with no thread is invisible but may have happened. An explicit non-response is a real slip; the lack of any signal is just silence. This rule exists because the failure mode (claiming a slip from a blind spot) erodes trust fast.
- **Adversarial "How I'm landing" read with self-decay.** The most distinctive section. An evidence-led, deliberately adversarial read of how the user is coming across, tracking 2-3 communication patterns a real feedback signal surfaced (not generic virtues). Chat is primary evidence (verbatim, timestamped); meeting transcripts are interpretive only (never quoted verbatim, because transcription is unreliable at the word level). The self-decay logic is the clever part: every fourth run it asks "is this still surfacing things you didn't know?"; two consecutive "no"s and the section retires itself. Adversarial framing degrades without a forcing function; the quarterly review prevents it from either repeating the same findings until tuned out or manufacturing sharpness to feel useful.
- **Red-team pre-publish verification.** Before writing anything, a subagent runs a five-check verification against the draft: named-person resolution (no first-name collisions, no mis-attributed relationships), artifact test (no action-verb claim without a linked artifact or quote, tightened so workshop-prep summaries don't count as decisions), retrieval transparency (no claim about a source that wasn't actually queried), canonical name check (no misspelled accounts), trend-claim citation (no multi-week pattern without specific archived-reflection citations). The skill does not publish-then-correct.
- **Three-output single-pass with evidence tiers.** Reflection (Interpreted) + decision-log entries (Primary fact, Secondary rationale) + KB proposals (tagged at claim level). One evidence pass feeds all three, with tags visible before approval.
- **Archive before overwrite.** Every run archives the current landing page to a dated child note before replacing it, and carries forward open items. The reflection landing page is always current; the archive is the trend spine the *next* run reads.

## Related skills

- **[end-of-day-review](../end-of-day-review/)**: produces the five daily EOD notes that are this skill's primary spine. This skill is also the Friday backstop if EOD missed runs during the week.
- **[decision-log](../decision-log/)**: Step 5 routes extracted decisions here.
- **[monday-cascade](../monday-cascade/)** / **[monday-briefing](../monday-briefing/)**: this reflection runs Sunday so the weekly view is ready before the Monday cascade reads it.
- **blind-spots-report**, **weekly-system-review** (next to template): the other weekend skills that feed Monday.

## When this skill matters most

This skill earns its place when:

- Your weekly review is a list of what you did. The depth and moments requirements force it to be a read on what you're *not* seeing instead.
- The same development edges keep showing up in your year-end review "in different clothes." The trend spine surfaces the multi-week patterns while there's still time to act on them.
- You can't trust an AI-generated reflection because it once shipped a confident factual error. The red-team verification pass is the answer: name resolution, artifact test, retrieval transparency, canonical names, trend citations, all checked before publish.
- Feedback about how you come across keeps arriving as a surprise. The adversarial "How I'm landing" read tracks the specific patterns, with evidence, on a cadence, and retires itself honestly when it stops being useful.

The reflection is unglamorous and occasionally uncomfortable. That's the design. A reflection that feels safe on a re-read isn't doing its job.
