# meeting-prep

A meeting-brief generator that pulls live context from N sources for a named meeting (customer call, 1:1, leadership review, peer sync) and produces a tight briefing under a fixed structure. The retrieval is parallel; the output is decision-ready.

The most distinctive pattern in this skill isn't the brief format; it's the **diagnostic-questions-not-talking-points** pattern for 1:1s, and the **privacy discipline for the call-recording tool**. Both travel beyond meeting prep.

## Files in this folder

- **[SKILL.template.md](SKILL.template.md)**: portable shape with 12 placeholders covering the user's tool stack (KB, meeting notes, chat, ticketing, CS platform, CRM, call recording) plus role and strategic-priority vocabulary.
- **[SKILL.example.md](SKILL.example.md)**: one filled-in version. Slite / Granola / Slack / Jira / Gainsight / HubSpot / Gong as the concrete stack. "Mission" as strategic-priority term.
- This README: the design patterns this skill implements, and notes on adapting it to your context.

## Design patterns this skill implements

- **Four meeting types, different briefs.** Customer / 1:1 / leadership / peer. Each has its own retrieval set and its own output structure. The skill's first job is to classify; everything downstream branches from that classification.
- **Parallel retrieval, sequential write.** Step 2 fetches all sources concurrently before writing anything. The user doesn't see retrieval narration; they see the finished brief. Cleanly separating *gather* from *write* is what makes the brief feel finished rather than work-in-progress.
- **Diagnostic questions, not talking points (for 1:1s).** This is the skill's most opinionated move. For 1:1s, the briefing produces *questions to ask*, not points to make. The framing: "Your job in this meeting is to discover what they see, not to show what you see." The pattern resists the failure mode where the executive walks in already knowing more than the direct report and turns the 1:1 into a status update.
- **Privacy discipline for Secondary sources.** The call-recording tool (Gong / Chorus / Avoma) is treated as a Secondary source at the per-account-tracker-hit and call-count layer only. Never call summaries, never transcripts, never individual quotes. The rationale is explicit: aggregate signals are useful for prep; per-call content is privacy-sensitive and isn't needed for the brief to be decision-ready. This is a transferable pattern for any third-party data source with privacy implications.
- **One-way doors call-out.** Every brief flags actions in the meeting that would be hard to walk back: a customer commitment, feedback to a direct report, a direction set with a peer that closes down options. The user enters the meeting knowing which moments are irreversible.
- **Source transparency.** Every brief ends with a one-line note: "Sources checked: [list]. Gaps: [what returned nothing]." No silent substitutions. If a source failed, the brief says so.
- **Internal-only header.** Every brief opens with **INTERNAL PREP, Not for sharing**. Prevents accidental forwarding and is visible at a glance.
- **Fresh org data on every 1:1.** The skill refetches the Org & People note for every 1:1. Roles change; team assignments change; the brief shouldn't be wrong because of a cached assumption.

## Related skills

- **[monday-briefing](../monday-briefing/)**: when the briefing's tactical walkthrough needs a follow-up message drafted, the prep done here is the input.
- The diagnostic-questions pattern echoes the Ownership Patterns section of the operating manual: questions over conclusions.
- The privacy discipline pattern reappears wherever a Secondary source has individual-content implications.

## When this skill matters most

This skill earns its place when:

- You're walking into 1:1s carrying information your direct report should have surfaced first. The diagnostic-questions pattern flips the dynamic.
- You're prepping for customer meetings by sifting through six tools by hand. Parallel retrieval and the privacy-bounded Gong fetch collapse it to one brief.
- You keep approving things in meetings that you later wish you'd thought about more. The one-way-doors section makes the irreversible moments explicit before you walk in.
- Your prep takes longer than the meeting. A fixed structure with an under-five-minute read closes that gap.

The brief is the artefact. The skill is the discipline of producing it the same way every time.
