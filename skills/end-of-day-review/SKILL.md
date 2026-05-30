---
name: end-of-day-review
description: >
  Weekday evening review. Confirms yesterday's chosen items, surfaces what mattered today,
  asks the user to pick tomorrow's three (hard cap, blank field, no proposals). Use whenever
  the user asks to close out the day, run an EOD, review what happened today, or see what
  they owe tomorrow. Trigger on "EOD", "end of day review", "run my EOD", "close out today",
  "daily review", "what do I owe tomorrow". Runs Mon-Fri evenings; skips weekends. Pulls
  evidence from the user's meeting-notes app, chat tool, ticketing system, decision log, and
  tomorrow's calendar. Writes a dated note to the user's personal channel; the week's five
  outputs feed Sunday's weekly-self-reflection. Owns the Open Items Tracker and is the only
  writer of `chosen` items.
---

# End of Day Review

A short, weekday evening pass that:

1. Confirms yesterday's three chosen items (done / carried / dropped).
2. Surfaces what happened today (facts, light).
3. Names what mattered (commitments made, replies owed, interpersonal moments).
4. Reflects on how the user did today (what landed well, one to improve, evidence-cited).
5. Maintains the Open Items Tracker (new commitments, reclassifications, stale, completions, duplicates).
6. Asks the user to pick tomorrow's three. Hard cap. No proposals; the user types them.

Feeds the Sunday weekly-self-reflection. Never a replacement for it.

> The stack named below is illustrative: Notion as the KB (holding the Open Items Tracker,
> Decision Log, Org and People note, and the skill index), Granola for meeting notes, Slack
> for chat, Jira for tickets. Swap these for your own. The provenance model (chosen /
> committed / captured), the aging rules, and the three-section output are the portable
> parts. The proposal-queue step (3.5) is optional and only applies if you run a Cat-A/B/C
> KB write discipline.

EOD is the only writer of `chosen` provenance. Other skills and EOD's own captured-pass
write `committed` or `captured`. The three are neutral, not hierarchical.

## Step 0: Fetch Indexes (Mandatory)

Before anything else, fetch both indexes. All other note IDs, archive parents, and chat
references are resolved from these. Do not hardcode anything except the two index note IDs
and the personal channel location.

1. Fetch the skill index note in Notion. Store: Skill Run Log location, archive parent (if
   defined there; if absent, use the personal channel directly), any config values.
2. Fetch the KB index note in Notion. Store: Decision Log location, Org and People note
   location, Slack channel references.

If either index fetch fails, stop and tell the user: "Cannot run EOD, [index name]
unreachable. Confirm Notion connection and retry."

## Step 1: Day-of-Week Gate

Determine today's day-of-week in the user's local timezone.

- **Monday through Friday:** proceed. Friday's EOD is the end-of-working-week close and the
  last EOD before Sunday's weekly-self-reflection picks up the week's outputs as evidence.
- **Saturday or Sunday:** stop. Reply: "EOD runs Mon-Fri. Sunday's weekly-self-reflection
  covers the week and the Monday briefing picks up from there. If something is genuinely
  urgent, trigger with the urgent flag to run urgent-only mode against the queue."

## Step 1.5: Backfill Mode (One-Off)

If invoked with `--backfill`, skip the normal evening flow. One-off pass to classify
existing tracker items by provenance the first time the new structure lands.

1. Fetch the Open Items Tracker per Step 3g and parse all active items.
2. For each item, propose a default provenance:
   - `committed` if source signal traces to a meeting, chat thread, or decision log entry.
   - `captured` otherwise (signal pulled without an explicit commitment).
   - Never assign `chosen` retroactively. `chosen` is a forward-looking declaration.
3. Present a single batch for the user to confirm overrides or accept defaults.
4. Apply, write the tracker once, output one-line confirmation. Do not write a dated EOD
   note. Do not run any other steps.

## Step 2: Anchor Input (Parallel With Retrieval)

Ask two short questions, but **do not block retrieval on the reply**. Fire the question and
the Step 0 + Step 3 retrieval calls in the same turn so they run concurrently.

Questions, kept warm and human:

1. "Hardest moment today?"
2. "Best moment today?"

Accept whatever comes back, including "skip". Store both as anchors. If the user skips both,
continue without them. Do not push.

If retrieval finishes before the reply, hold synthesis until the anchor returns (give a
moment), but do not re-prompt. If the user never replies in this session, proceed with
evidence only and note in Section 2 that anchors were skipped.

**Reason:** the user's own read is the strongest signal. Evidence around it is context, not
replacement. But making them wait through a sequential question-and-then-retrieve flow
burns time on something a parallel call resolves for free.

## Step 3: Evidence Retrieval (Parallel)

Retrieve all sources in parallel. Today = local date. Only gather enough to produce a
three-minute read. Do not dump.

### 3a: Meeting notes, meetings attended today

For each meeting the user attended today from Granola: title, attendees (briefly), AI
summary. Extract any commitments that appear in the summary ("the user will…", "agreed to
send…", "by Friday").

Summary-level only. Do not promise verbatim commitments unless the summary names them
explicitly.

**Attribution discipline, never fabricate names.** Summaries are often thin on who was
actually in the meeting. If a meeting title says "check-in with HR" and the summary doesn't
explicitly name the HR person, refer to the meeting by its title or the role ("the HR
check-in"), not a guessed name. Do not fill gaps by pattern-matching to someone who appears
in a different meeting's summary today. A wrong name in Section 2 breaks trust immediately.

**Name-clash disambiguation.** If a summary references a first name that could clash with
the user's own first name or another attendee, surface the ambiguity in the output rather
than guessing.

**When in doubt, name less.** Better to say "someone in the leadership session raised X"
than to confidently attribute X to the wrong person.

### 3b: Chat, today's signal

Bidirectional. Outbound-only misses half the picture: replies from others that close loops,
DMs received, threads participated in mid-chain. Pull all in parallel.

**Outbound:** messages the user sent today (the user's Slack handle, resolved from the KB
index). Sort by timestamp.

**Inbound:** mentions of the user, DMs received today, any thread where the other party
replied after the user's last message.

**Thread context (critical):** for every message the user sent today, fetch the full
thread. The difference between "you owe a note to the team" and "you posted the note at
12:58 and it landed well" lives inside thread replies.

**Channels:** key channels from the KB index (where source-to-system mapping lives).

**Build four lists:**

- **Sent today:** top themes, anything that sounds like a commitment ("I'll…", "on it",
  "let me…", "by tomorrow").
- **Threads participated in:** parent + the user's contribution + any replies after their
  last message. Flag any reply that looks like a closure ("done", "sent", "approved",
  "sorted"); these may retire proposed commitments in Step 4a.
- **Inbound unreplied:** mentions and DMs where the user hasn't responded.
- **Interpersonal surface:** which directs and peers the user spoke to today (names only,
  no tone inference).

Paginate once if needed. Two rounds at most.

### 3c: Ticketing, today's activity

Find Jira issues where the user commented or transitioned today. Narrow query: issues updated
by the user since start-of-day. Return summary, status, assignee.

### 3d: Decision Log, today's entries

Fetch the Decision Log note. Scan for entries dated today. If any, list them. If none, that's
a valid result; move on.

### 3e: Tomorrow's Calendar

Fetch tomorrow's meetings from Granola or the calendar. For each, flag if obvious prep is
missing (no linked doc, no prior meeting notes, named stakeholder without recent context).

If unreachable, skip cleanly. Do not fail the whole run.

### 3f: Direct Report List

From the Org and People note fetched in Step 0, extract the user's current directs. Use this
list to annotate the interpersonal surface in 3b: flag any direct report the user spoke to or
didn't speak to today.

### 3g: Open Items Tracker

Fetch the Open Items Tracker note. Tracker structure (fields, quadrants, provenance values,
tag block format, numbering rules, aging logic) is defined in the skill-index canonical-home
subsection "Open Items Tracker, Structure". Read that subsection from the Step 0 fetch and
parse the tracker against it.

The "Removed" sections at the bottom are read-only history. Preserve them on write but do
not modify.

**Strict parse or bail.** If the note is unreachable, malformed, or the structure doesn't
match what the index subsection defines, skip the open-items integration entirely. Do not
attempt to write. Surface a note in Section 3.

**Migration cases.** If items lack `[#N]` markers, lack a last-touched date stamp, or lack a
provenance code, treat as a migration: assign defaults per the rules below and surface a
one-time note. Don't ask the user to confirm structural defaults item-by-item.

- Missing item number: assign sequentially in parse order.
- Missing last-touched date: stamp today; staleness flags begin from this date.
- Missing provenance: trigger Backfill Mode (Step 1.5) instead of running normal flow.

### Live source failure handling

If chat or ticketing fails, name the failure at the top of the output. Continue with what
you have. An EOD with partial evidence is still useful.

## Step 3.5: Queue Consumer (Optional, KB Write Discipline)

This step is for users who maintain a KB proposal queue under a Cat-A / Cat-B / Cat-C write
discipline (canonical owner / proposer / writer). If you do not run a proposal queue, skip
this entire step.

If you maintain a queue, this step drains the Pending section for entries this skill owns.

### 3.5a: Fetch Pending Queue

Fetch the KB proposal queue note. Parse the Pending section into structured entries (Date,
Source skill, Target note, Proposed change, Rationale, Tier, Urgency, Status).

### 3.5b: Filter to EOD's Ownership

Filter Pending entries to those this skill owns:

- **Tier:** Primary or Secondary
- **Urgency:** Same-day, This-week, or Urgent
- **Status:** approved

Interpreted tier and Synthesis urgency belong to weekly-self-reflection. Skip those.

**No deferral path for in-scope entries.** EOD must apply every in-scope entry it surfaces.
There is no "defer to weekly-reflection backstop" path for entries in EOD's own tier. If an
in-scope entry cannot be applied, log it as a specific failure with a reason (target note
unreachable / leak scan failed / idempotency check ambiguous / queue too large / apply tool
error). Failures stay in Pending and become legitimate weekly-reflection backstop residue.
Deferring an in-scope entry into the weekly backstop is a miss, not a handoff.

### 3.5c: Run Leak Scan on Each Entry

Before applying any patch to a shared KB note, run the leak scan. Reject the entry if any
of the following appear in the Proposed change text:

1. **Private decision log content.** If the entry quotes or paraphrases a private-log entry
   verbatim, reject.
2. **Personal judgments about named people.** Patterns like "X is struggling", "I don't
   trust Y on this"; subjective assessments that belong in the private log.
3. **User-personal reasoning.** First-person phrasing reflecting the user's internal
   calculus rather than a statable fact ("I think we should…", "my read is…").

Rejected entries move to a Rejected section with reason. Clean entries proceed.

### 3.5d: Apply Each Patch With Audit Trailer

For each clean entry:

1. Read the target note.
2. **Idempotency check.** Scan for an existing audit trailer matching this proposal. If
   present, the patch was applied on a prior run but the queue entry didn't get moved
   (likely a crash between apply and queue-update). Skip the apply, move the entry to
   Applied with status `recovered: trailer already present, no double-apply`.
3. Apply the Proposed change with the right tool for the shape of the edit. Use surgical
   tools (append blocks, modify single block, modify range) wherever possible. Full-note
   rewrite is last resort.
4. Append the audit trailer at the end of the patched content:
   `[written by end-of-day-review DD Mon YYYY | proposed by <source skill> DD Mon YYYY | tier: <Primary|Secondary> | leak scan: clean]`
5. Move the entry from Pending to Applied.

### 3.5e: Auto-Promote, Index, and Changelog

After processing:

1. Auto-promote: any remaining Pending Same-day entry older than 48 hours gets promoted to
   This-week in place.
2. If any applied patch created a new note, changed a note's scope, or moved content, update
   the KB index routing table and append one entry to the KB changelog note.

## Step 4: Synthesise the Output

### 4-pre: Confirm Yesterday's Chosen

Before any other classification work, close out yesterday's chosen list.

Read tracker items where `provenance: chosen`, `confirmation: pending`. Filter to items
whose `chosen for` date is yesterday (or, if EOD didn't run yesterday, look back and note
the gap).

For each item, the user replies with one of:

- **done**: completed; close out.
- **carried**: still alive, roll into today's chosen list. Counts as a touch.
- **dropped (reason)**: abandon with one-line reason.

**Drift checks:**

- If the last two consecutive days each had >50% carries, surface in Section 2b: "Carry rate
  is high. Picking too many, wrong items, or blocked on others?"
- If any chosen item has carried 3+ working days, surface explicitly: "#N has carried 3+
  working days. Split into smaller pieces, escalate to a named owner, or drop?"

The 5/10 working-day chosen aging rules in Step 4a.5 apply across runs.

### 4a: Detect New Commitments, Reclassify, and Detect Closures

EOD owns the Open Items Tracker end-to-end: additions, classification, reclassification
across quadrants, and closures into the Removed history.

Scan today's evidence (meeting summaries, chat messages sent by the user) for new
commitments:

- Meeting signals: "the user will…", "agreed to send…", "by Friday", "owes follow-up to…"
- Chat signals: "I'll…", "on it", "let me…", "by tomorrow", "will get back to you"

Filter aggressively. Skip filler ("happy to help", "let's chat"). Only items where the user
named a deliverable or a person waiting count.

For each candidate, write a one-line phrasing with deliverable, named person where
relevant, and source.

**Dedupe before proposing.** For each candidate, check the parsed tracker. If a similar
item exists, propose a reclassification rather than a duplicate.

**Closure check before proposing.** Cross-check the thread context from Step 3b. If today's
outbound already satisfies the commitment, mark closed not open. If a thread shows external
closure (e.g., "approved, here you go"), also mark closed. Do not propose items that
today's evidence already resolved; single biggest source of noise.

**Provenance assignment:**

- Source is a meeting, chat thread the user is in, or a decision-log entry → `committed`.
  Owner is the named person.
- Source is signal pulled (sweep, classification heuristic) without a person waiting →
  `captured`. Owner is the person the action concerns, or "none" if unowned.

`chosen` is never assigned in this step. Only in 4a-post when the user picks tomorrow's three.

**Surface every reclassification in the batch.** Don't move silently. The morning sanity
check is only honest if the user sees what changed.

**Provenance transitions:**

- `captured` → `committed` when today's evidence shows a person now waits explicitly.
- `committed` → `chosen` when the user picks the item in 4a-post.
- `captured` → `chosen` when the user picks the item in 4a-post.
- `chosen` → carried (stays chosen, `chosen for` updates to next working day) on 4-pre carry.
- `chosen` → removed on 4-pre done.
- `chosen` → de-escalated to `committed` on 4-pre dropped if owner is non-user and work is
  still owed; otherwise removed with reason.

### 4a.5: Stale Item Disposition (Provenance-Aware)

Walk every item that survived Step 4a's closure check. Compute age in **working days**
(Mon-Fri) since last-touched. Apply aging rule for the item's provenance:

**Captured items, age ≥ 10 wd.** Default disposition: kill. Flag with options: kill / commit
(date) / escalate / snooze.

**Committed items, age ≥ 10 wd.** Do not default to kill. The commitment is to a person.
Options: chase (draft a nudge), commit (date in Q1 or Q2), de-prioritise, kill with reason.
No default. The user picks per item.

**Chosen items, age ≥ 5 wd, < 10 wd.** Surface: "#N has been chosen for N working days
running. Still want this? What's blocking?" Drop, escalate (split), or carry once more
(with counter visible).

**Chosen items, age ≥ 10 wd.** Forced re-decision. Cannot snooze, cannot quietly carry.
Surface: "Pick one: keep with a new plan (state it), split into smaller pieces (state
them), or drop with reason." No default.

**Disposition mechanics:**

- **Kill / drop.** Move to "Removed YYYY-MM-DD (EOD, stale)" with reason and provenance.
- **Commit.** Promote into Q1 or Q2 with a hard date. Provenance becomes `committed`.
- **Chase / escalate.** Surface a nudge draft. The skill doesn't send automatically.
- **Snooze (captured only).** Stamp today. Two consecutive snoozes triggers a flag.

Skip items already proposed in Step 4a or 4-pre. Skip migration items on the first run
(10-working-day grace period).

### 4a.6: Completion Sweep on Existing Items

Walk every item in the tracker. Look for evidence in the past 5 working days that the
action got done, covering the gap where commitments close in meetings or DMs without
telling the tracker.

**Evidence corpus:** last 5 working days of meeting summaries, user's outbound chat, thread
closures, decision-log entries.

**Match heuristic:** score evidence by person match, topic match, action match. Three
loose matches = strong signal. Two = worth surfacing. One = skip.

**What to surface:** for items with strong or moderate evidence, propose closure in the
Step 5 batch with the evidence quote attached. The user can confirm, override, or ignore.
Silence = leave open (closure is one-way).

**What NOT to do:** propose closure on speculative matches. The action verb has to land.
False-positive closures are worse than carry-over.

### 4a.7: Duplicate Scan on Active Items

Walk the remaining active items pairwise. Flag pairs where:

- Same named person AND same deliverable verb.
- Same topic AND same anchored event.
- Same compound deliverable bundled differently.

For each candidate cluster, propose a merge. Lower-number wins by default. The user
confirms, overrides, or modifies.

**Conservatism.** Flag fewer, better. Has to feel obviously redundant on read, not just
topically adjacent.

### 4a-post: Choose Tomorrow's Three

**Blank field. Do not propose candidates.** The cognitive work of picking is the point.
Proposing a shortlist defeats the purpose.

If a weekly-reflection-produced themes note exists, render as a single sentence above the
picker. Do not score the picks against the themes; just surface.

Compute carries from 4-pre. Show carries and remaining slots:

> Tomorrow's three.
>
> Carried from yesterday: #3 (X), #14 (Y).
>
> Type the remaining 1.
>
> 1.

**Hard cap at 3.** If carries + new > 3, prompt to cut.

For each typed pick:

- **Match against existing tracker items first.** Escalate that item to `chosen`, set
  `chosen for: <next working day>`, `confirmation: pending`. Quadrant stays unless the user
  changes it.
- **If new**, create a tracker item with provenance `chosen`, owner the user, source "EOD
  <today> choose pass".

**Next working day, never weekend.** If today is Friday, `chosen for` is next Monday.

### 4b-4e: Write the Four Output Sections

Three-minute read total. Short sentences. No em dashes. No scores. Warm on interpersonal bits.

### Section 1: What Happened

Tight factual digest. Bullets OK.

- Meetings: count and type.
- Chat activity: rough shape.
- Tickets: issues touched.
- Decisions logged today: list or "none".

Do not editorialise here.

### Section 2: What Mattered

Prose. Two short paragraphs max.

Start with the user's anchor. Echo their hardest/best moment back with any contextualising
evidence.

Then cover:

- **Commitments made today.** Phrase as "You owe…" not "you should…".
- **Replies owed.** People waiting. Name them with the channel or thread.
- **Interpersonal surface.** Who they spoke to across directs and peers. Gentle note if a
  direct didn't surface today.
- **Recognition moments.** If anyone did something worth naming.

**Decision routing:** if anything sounds like a decision worth logging, prompt: "This
sounds like a decision worth logging. Want me to trigger decision-log?" Don't duplicate.

### Section 2b: Today's Read (Self-feedback)

Two-three things that landed well today. One area to improve. Evidence-cited. Recognition
before critique.

**Hard rules:**

- Each "did well" claim cites a specific chat message, meeting line, ticket artifact, or
  thread closure.
- One thing to improve, not three. Three drifts to performance review.
- Observation language only. No grades, scores, or rankings.
- Evidence or silence. If no specific good thing surfaced, say so. If no improvement
  emerged, say so. Manufacturing is not allowed; skipping is.

```
**What landed well**

- [specific observation, evidence-cited, ~1-2 lines]
- [specific observation, evidence-cited, ~1-2 lines]
- [optional third]

**One thing to improve**

- [single observation, evidence-cited, ~2-3 lines]
```

### Section 3: What's Next (Deltas Only)

Show only what changed today, not the full open items list. Numbered list.

Include:

1. Tomorrow's three chosen items, in the order picked. Mark carries.
2. Items that moved quadrants today, or items closed.
3. New committed or captured items added today, tagged with source.
4. Tomorrow's meetings with prep gaps.
5. Anything urgent that didn't close and isn't on tomorrow's chosen list.

If the user chose zero items for tomorrow, surface explicitly. A blank chosen list is a
valid choice on a light day.

Keep under ten items. If longer, something's wrong upstream and worth naming.

## Step 5: Batch Confirm

If 4-pre, 4a, 4a.5, 4a.6, 4a.7, and 4a-post all produced zero items needing input, skip
this step entirely.

Otherwise, present everything as a single batch. One interaction, not one per item. Use
persistent item numbers (`#N`) for existing tracker items, batch letters (`A`, `B`, `C`)
for proposed-new items.

Batch order:

1. Confirm yesterday's chosen (done / carried / dropped per item)
2. New items to add (batch letter, proposed quadrant, provenance, owner)
3. Reclassifications and provenance changes
4. Closure candidates from today
5. Completion-sweep candidates
6. Merge candidates
7. Stale items (provenance-aware)
8. Pick tomorrow's three

Wait for reply. Parse section by section. Apply only the confirmed subset.

**Silence rules differ by section.** Silence on stale captured items leaves them open (kill
is irreversible). Silence on stale committed items leaves them open (a person is owed).
Silence on stale chosen items at 5 wd leaves them chosen one more day. Silence on stale
chosen at 10 wd is not allowed; the prompt forces a pick. Silence on completion-sweep
candidates leaves them open (closure is one-way). Silence on yesterday's chosen defaults to
carried.

## Step 6: Rewrite the Open Items Tracker

If Step 5 produced no confirmed changes, skip. Otherwise:

1. Re-fetch the tracker (fresh read).
2. Parse strictly per the index subsection. If format drift detected, stop the write and
   tell the user.
3. Build the updated matrix in memory. Apply confirmed yesterday's-chosen, new additions,
   choose-tomorrow picks, reclassifications, merges, closures, stale dispositions, migration
   backfills. Update the callout header (last refresh, next number).
4. Write the full updated note via the appropriate full-note tool.

## Step 7: Write the Dated EOD Note

Create a new note as a child of the EOD archive parent under the personal channel. Never
update a previous EOD note.

- Title: `EOD YYYY-MM-DD` (ISO date, local timezone).
- Body: full three-section output from Step 4 + Section 2b + Section 3, including which
  additions the user confirmed in Step 5.

## Step 8: Archive Old EOD Notes

At end of run:

1. List children of the EOD archive parent.
2. Find notes matching `EOD YYYY-MM-DD` older than 14 days.
3. Archive each. Do not delete.

14 days is chosen so the weekly-self-reflection always has last week's full set available.

## Step 9: Deliver in Chat

Render the full output in chat. Include a reference to the dated note: "Saved as EOD
YYYY-MM-DD in your personal channel." If new items were added, append: "Added N item(s) to
tomorrow's open items list."

## Step 10: Skill Run Log

Append a single line to the Skill Run Log:

`end-of-day-review | [ISO timestamp] | [Success|Partial|Failed] | [one-line note if partial or failed]`

## Step 11: Kill Criterion Check

On the first run of week five after the skill's first-ever run, append a short prompt:

> "Four weeks in. Is this earning its keep? Fewer stale items, faster replies to people? If
> not, say the word and this skill retires."

If the user says retire, archive the skill and stop scheduling.

## Overlap Guards

- **Open Items Tracker:** EOD owns this note end-to-end. No other skill writes to it. EOD
  is the only writer of `chosen` provenance.
- **Decision Log:** prompt and route, never duplicate. The decision-log skill owns the
  capture format.
- **Meeting Prep:** flag tomorrow's prep gaps only. Do not call meeting-prep.
- **Weekly Self-Reflection:** runs Sunday. Reads the week's five EOD outputs as evidence.
  Don't duplicate the week-level synthesis here.
- **Monday Briefing:** downstream consumer. May read last Thursday's EOD as anchor.

## Failure Modes

- **No activity today.** Valid outcome. Short note. Don't fabricate.
- **Chat or ticketing fails.** Name it. Continue with remaining sources.
- **Index fetch fails.** Stop. Don't proceed without routing.
- **Tracker structure subsection missing.** Stop.
- **Anchors skipped.** Fine. Proceed with evidence only.
- **Saturday or Sunday run.** Refuse per Step 1.
- **Tracker missing provenance fields.** Trigger Backfill Mode.
- **More than 3 chosen.** Loop back once with the cut prompt.
- **No chosen items.** Valid. Write zero. Carries from yesterday still apply.
- **`chosen for` would fall on a weekend.** Advance to next working day.

## Worked example (fictional data)

**Trigger.** Thursday evening, the user (a product executive at Acme) types "run my EOD".

**Step 2 anchors.** The skill asks the two questions while retrieval runs. The user replies:
"Hardest: the pricing review went in circles. Best: Daniel finally handed off the deploy
issue."

**Step 3 retrieval (parallel).** Granola shows three meetings (pricing review, a 1:1 with
Daniel, a customer call with Northwind); Slack shows the user committed in #gtm to "send the
revised pricing model by Tuesday"; Jira shows two issues transitioned; the Decision Log has
no entry today; tomorrow's calendar has a board prep with no linked doc.

**Step 4 output (rendered in chat):**

```
**Section 1, What happened**
- Meetings: 3 (pricing review, 1:1 with Daniel, Northwind customer call).
- Chat: moderate. Active in #gtm and #payments-eng.
- Tickets: 2 transitioned (PAY-412 to Done, PAY-419 to In Review).
- Decisions logged today: none.

**Section 2, What mattered**
You named the pricing review as the hard moment, and the evidence agrees: it ran 50 minutes
with no decision, and you closed it by committing to send a revised model yourself by Tuesday
(#gtm, 16:42). That is now the thing you owe. The best moment holds up too: Daniel's handoff
of the deploy issue shows in #payments-eng, where he routed it to a named owner rather than
taking it on himself, the exact shift the last 1:1 was pushing for.

You owe the revised pricing model to the GTM group by Tuesday. Northwind is waiting on a
follow-up note from today's call. You spoke to Daniel and the GTM leads today; you did not
speak to Priya, who has surfaced in two of the last three days.

**Section 2b, Today's read**
What landed well:
- You let Daniel own the deploy handoff instead of solving it (#payments-eng, 15:10). That is
  the coaching focus landing in real behaviour.
- You closed the Northwind call with a concrete next step rather than an open "we'll follow
  up" (Granola summary).
One thing to improve:
- The pricing review had no framing of the decision to be made at the top, so it drifted.
  Next time, open with the specific call and the options, not the context.

**Section 3, What's next**
1. Tomorrow's three: revised pricing model (carried), Northwind follow-up note, board-prep
   doc.
2. New committed: revised pricing model, owner you, due Tuesday (source: #gtm).
3. New committed: Northwind follow-up, owner you (source: Granola, Northwind call).
4. Prep gap: tomorrow's board prep has no linked doc.
```

**Step 4a-post (the picker), shown as a blank field:**

```
Tomorrow's three.

Carried from yesterday: #7 (revised pricing model).

Type the remaining 2.

1.
2.
```

The user types "Northwind follow-up note" and "board-prep doc". The skill escalates the
carried item and creates the two new ones as `chosen`, then writes the tracker and the dated
note.

## Output Format

Output location is always the user's personal channel; never a shared KB channel.
