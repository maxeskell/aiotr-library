---
name: weekly-self-reflection
description: >
  Weekly leadership self-reflection using evidence from chat, meeting notes, KB, support
  themes, and AI conversation history. Also extracts decisions to the decision log and
  proposes KB updates from the same evidence pass. Use whenever the user asks for a weekly
  review, self-reflection, self-assessment, leadership review, or weekly check-in. Trigger on
  "how did I do this week", "run my reflection", "weekly review", "what should I work on next
  week", "sync the KB", "update the KB from this week", "log decisions from this week". Gathers
  real evidence before producing any output and handles reflection, decision capture, and KB
  sync in a single pass.
---

# Weekly Self-Reflection

Sunday morning: a full leadership self-reflection, plus decision capture, plus KB update
proposals, in one evidence-based pass. Runs ahead of the Monday cascade so the weekly view
is ready before the Monday briefing.

> The stack named below is illustrative: Notion as the KB (holding the org note, decision
> log, strategy note, reflection landing and archive), Slack for chat, Granola for meeting
> notes, Mixpanel for product analytics, an optional support tool. Swap these for your own.
> The evidence hierarchy, the adversarial "how I'm landing" read, and the pre-publish
> verification pass are the portable parts. The proposal-queue mechanics in Step 6 are
> optional and only apply if you run a Cat-A/B/C KB write discipline.

The spine of this skill is the past week's five EOD outputs (Mon-Fri), so an early-week run
only has a partial set and should be rare and noted.

**Produces:**

1. Personal reflection (conversation-based, delivered in chat).
2. Decision log entries (sent to decision-log skill or stored for review).
3. KB update proposals (gathered from the user's own signals, not team-sourced).

**Gathers from:** Slack (the user's messages and mentions, last 7 days), Granola (last 7
days), Notion (recent changes), the support tool (support themes the user touched), and AI
conversation history (strategic thinking the user did this week).

## CRITICAL: Mandatory Step 0: Fetch canonical org data and KB index

Before reflecting on performance, fetch the canonical org data and KB index. This grounds
reflection in current org reality and strategic context.

### Step 0a: Fetch org data

Fetch the Org and People note in Notion. If fetch fails: "Cannot run reflection, org data
unreachable. Confirm Notion connection and retry." **STOP HERE.** Extract current roles and
team ownership.

### Step 0b: Fetch KB index

Fetch the KB index note. If fetch fails: stop with the same message. Locate the decision
log, active strategy/missions, and metrics notes.

### Step 0c: Fetch strategic baseline (recommended)

Fetch the Strategy and Missions note to understand current strategic priorities. Gives
context for whether the week's decisions align with or challenge them.

### Step 0d: Build verification lookups

These power the pre-publish verification in Step 6.5. Build them now.

1. **Named people lookup.** From the org data, extract the full list of named individuals
   (full name, role, reporting line). Flag first-name collisions explicitly (e.g., two
   people sharing a first name). When evidence names someone by first name only, resolution
   goes through this list.
2. **Canonical customer and partner names.** Fetch the systems / naming-conventions note.
   Record canonical spellings for customer accounts and partners that appear in evidence. If
   the note doesn't carry this list, flag the gap; do not invent spellings.

Why this step exists: transcription and attribution errors (a wrong first name, a misspelled
account) are the cheapest factual errors to catch with a lookup and the most damaging to
trust if they ship.

## Evidence Hierarchy

Three outputs, three evidence profiles:

- **Reflection (conversation output):** Interpreted, a synthesis of observations.
- **Decision log entries:** the *fact* that the user decided X on date Y is Primary; the
  rationale and success metric attached are usually Secondary (reasoning in the moment, not
  externally verified).
- **KB update proposals:** strategy shifts from meeting observations are Secondary; specific
  metrics from Mixpanel are Primary; a synthesis of the user's thinking from AI
  conversations is Interpreted.

Tag at the claim level before presenting to the user, so they approve or reject with the
evidence weight visible.

## Step 1: Gather Evidence (All Sources in Parallel)

Complete all retrieval before writing a single word.

### Source 0: Past week's EOD outputs (primary spine)

The spine of this reflection is the past week's five EOD outputs. Read them first. They've
already done the daily synthesis, attribution, and commitment capture; the live pulls in
Sources 1-5 supplement and verify rather than starting from scratch.

For each weekday (Mon-Fri): look up the EOD note `EOD YYYY-MM-DD` as a child of the EOD
archive parent. If present, read it; capture anchors, commitments named, interpersonal
moments, open items added. If absent, note the gap explicitly. Don't infer from silence.

Build a week-level view: which themes recurred, which Monday commitments were closed by
Friday, which interpersonal threads kept surfacing, which days were thin. This week-view is
what makes the reflection a synthesis rather than a Sunday snapshot.

If fewer than three EOD outputs exist, flag prominently: "Only N of 5 EOD outputs available.
Reflection rests more heavily on live source pulls than usual."

### Source 0.5: Prior 4 weeks of archived reflections (trend spine)

Read the four most recent archived reflections under the reflection archive parent. For
each: extract themes named, focuses set, carried-forward items, decisions logged. Note dates
(W-1 … W-4).

Build a cross-week pattern view: which themes recur three or four runs running, which were
named once and disappeared, which focus items got actioned vs drifted. This feeds the
multi-week pattern observations in Step 4 and the trend-claim check in Step 6.5 (Check 5).

If fewer than three archived reflections exist, flag: "Trend spine partial; multi-week
pattern claims downgraded to single-week observations this run." Don't manufacture trends
from a thin spine.

Source 0 = this week, day-by-day. Source 0.5 = the four prior weeks, what's accumulating.

### Source 1: Chat, the user's messages and mentions

Find: messages from the user in the past 7 days (excluding auto-notifications); mentions of
the user in key channels; decisions or commitments ("I'm going to…", "We've decided to…").
Build a list of topics, decisions, and how the user showed up this week.

### Source 2: Meeting notes, the user's meetings

For each meeting with the user as participant in the last 7 days: extract decisions made,
commitments given, themes. Group by type (1:1s, leadership syncs, customer calls, peer
syncs). Note whether meetings were productive and strategically focused.

### Source 3: KB, context and recent changes

Has the user updated any strategy notes this week? New decisions logged? Missions or OKRs
changed? This shows whether thinking is being recorded as it happens.

### Source 4: Support themes (optional)

If you maintain a support tool, find conversations from the past 7 days the user touched.
Extract themes engaged with and resolution approach.

### Source 5: AI conversation history

Review the past 7 days of AI sessions: what strategic thinking did the user do, what
problems did they tackle, what frameworks emerged?

## Step 2: Archive Current Landing Page

Before writing the new reflection, archive the current landing page.

1. Read the reflection landing note. Store full current content.
2. Create an archive child under the reflection archive parent titled `Weekly Self-Reflection,
   [date of current content]`, preserving the full content.
3. Extract carried-forward items (open actions, unresolved coaching signals, multi-week
   commitments, pending follow-ups) to include in the new reflection's "Carried Forward"
   section.

## Step 3: Assess Leadership Performance

Using Step 1 evidence, reflect on:

- **Decision quality.** What decisions, reversible or irreversible, aligned with or shifting
  strategy? Explicit criteria, or just announcing conclusions?
- **Attention and focus.** Which topics consumed attention? Were they the right ones? What
  was under-attended?
- **People and org health.** How is the user showing up in 1:1s and syncs? Tension,
  misalignment, blockers? Issues addressed or accumulating?
- **Execution and follow-through.** Are last week's commitments visible this week? Driving
  accountability, or letting things drift?
- **Strategic clarity.** Is thinking showing up in the KB, or happening in private and not
  propagated?

## Step 4: Write the Reflection

### Reflection depth requirement (read before drafting)

The reflection must push the user's thinking, not summarise their week. The bar is the kind
of insight that tells them something they didn't already see, connecting patterns across
people, decisions, and structure not visible from any single data point. Review-style
narration ("X happened, Y was discussed") adds no value; the user already lived the week.

**Surface 2-3 hard, less-safe insights per pass.** Before publishing, ask: "what am I
avoiding saying that's true?" Push the threshold for surfacing a hard observation *down*, not
up. If the cut leaves the reflection shorter than feels complete, that's the right shape;
completeness is a tell of summary, not insight.

Categories of harder reads worth reaching for:

- **Pattern questions.** When did a pattern become unmanageable? What changed about the
  threshold for action this week vs prior?
- **Flag-without-follow-through.** Where did the user name urgency, then not act on it
  themselves? What does the gap reveal about decision velocity in their own work?
- **Structural attention bias.** Why do direct-report fires consistently crowd out peer
  collaboration / strategic shaping / personal commitments?
- **Convening bottleneck.** When overdue calls cluster, ask whether the org needs the user
  in the room or whether the leadership team isn't operating with autonomous decision agency.
- **Velocity questions.** When decision-log entries spike, ask whether patient sequencing
  has slipped into avoidance dressed up as patience.
- **Process-bypass observations.** Where did the user make a call outside an established
  process and now owe someone a conversation?
- **Connecting-frame insights.** Where two or more people show different surfaces of the
  same underlying gap, name the connecting frame, not separate single-person summaries.

**Multi-week pattern requirement.** At least 2 of the 3-5 bullets in "What I'm noticing about
my leadership" must be multi-week pattern observations cross-cited from the Source 0.5 trend
spine, with specific archived-reflection dates. Trend claims without citations fail Check 5
in Step 6.5.

Frame each hard read as a question back to the user, not an assertion. The point is to make
them see something, not to deliver a diagnosis they then defend.

### Moments requirement (quote-required)

Generic self-critique is easy and worthless. The reflection earns its weight when it cites
specific moments. For each "What I'm noticing" bullet, name the actual evidence: quote a
chat message, a meeting line, a specific decision.

Three prompts to attempt every run:

1. One moment this week where the user closed down debate. Quote it.
2. One decision the user made that should have been delegated or pushed back to a direct.
   Cite the artifact.
3. One place the user inserted themselves in a direct report's lane. Cite the artifact.

If a prompt yields no evidence, say "no evidence of this pattern this week" plainly. That's
a real answer. But the prompts must be attempted every run.

### Structure

```
# Weekly Reflection: [Date]

## TL;DR
[2-3 paragraphs. Lead with strategy and impact: what specific decisions or framings moved
this week, what those imply for the missions / trajectory / capacity reality. Then
interpersonal complexity: connecting patterns across people, validation moments, friction,
developmental moments. Do NOT open with "the week was X" or surface emotions before
substance.]

## What I did well this week
[2-3 bullets. Specific examples from evidence. Add a "harder read" if the bullets are mostly
setup-not-delivery (commitments and framings that depend on next-week follow-through to
count). Name the test of whether the week was strong vs busy.]

## Where I fell short
[2-3 bullets. Genuine slips, not deliberate holds. Apply the absence-of-evidence rule below.]

## What I'm noticing about my leadership
[3-5 substantive bullets, each pushing thinking on a connecting pattern. Lead with the
connecting frame, not single-person summaries. This is where the reflection earns its keep.]

## How I'm landing
[Adversarial communication and presence read. See Step 4.5. Evidence-led, no padding. The
section pushes back on the user's read of how they're coming across; it does not reassure.]

## What I should focus on next week
[3-5 specific actions or mindsets, grounded in this week's evidence. Each should fit a Monday
morning agenda, not be aspirational.]

## Decisions to log
[See Step 5.]

## KB updates to propose
[See Step 6.]
```

**Absence-of-evidence rule** (for "Where I fell short"). Never infer "didn't happen" from
chat/meeting-notes silence alone. Those sources only capture written and recorded
conversations. A 1:1 with no thread and no transcript is invisible to this skill but may
still have happened. Before claiming a slip grounded in absence of trace:

- If the evidence is an explicit non-response (the user read a message and didn't reply; a
  transcript shows a direct question unanswered), the slip is real. Cite the artifact.
- If the evidence is the lack of any signal at all, that is silence, not a slip. Drop the
  bullet, or reframe: "No written trace found, may have been handled in 1:1. Worth checking
  before claiming this as a slip." Frame as low-confidence to verify, not a finding.

## Step 4.5: How I'm Landing, Adversarial Communication Read

### Why this section exists

A periodic check on whether the user's specific communication failure modes are still
showing up, using evidence, not self-report. Self-report on this is unreliable; the moments
the user doesn't remember are the ones doing the damage. The section is adversarial by
design: it names patterns explicitly and refuses to soften when the evidence is sharp. It
is not a tic count and it is not reassurance.

### Register

Adversarial. The threshold for surfacing a hard read is lower here than anywhere else in the
reflection. No "fair to say" or "in some moments" hedges. If a pattern is in the evidence,
name it. If it isn't, say so plainly, but look first. Frame hard reads as direct
observations, not questions (the rest of the reflection uses question-framing; this section
does not).

### Evidence rules (read carefully)

Stricter than the rest of the skill, because the diagnostic that generates this section can
itself be tripped up by transcript noise.

1. **Chat is the primary source.** Quote verbatim. Chat messages are timestamped and
   authored. Include channel, date, and recipient or context.
2. **Meeting transcripts are interpretive only.** Never quote a transcript as verbatim
   evidence here; transcription is unreliable at the word-choice level. Use transcripts for
   the shape of conversations, who was in them, the topical arc; not for "the user said X."
   If a transcript moment looks important, paraphrase it and flag it as "transcript-derived,
   may not reflect literal wording."
3. **Prior outputs are corroborating evidence.** If EOD or prior reflections named a pattern,
   that's a corroborating signal. Cite the specific archived date.
4. **Cross-source confirmation raises the weight.** A pattern in chat + corroborated by a
   meeting arc + a prior reflection is high-confidence. A pattern named only by a transcript
   is low-confidence; frame it as such.

### What to look for

Track 2-3 communication patterns specific to you. Three is the cap; more turns the section
into thoroughness theatre. The patterns should be the ones a real feedback signal (a 360, a
peer comment, a manager note) actually surfaced, not generic leadership virtues. Common
categories leaders track:

- **Status-asymmetric warmth.** Compare how the user wrote to senior stakeholders vs direct
  reports vs less-senior cross-functional staff in the same week. Length, register,
  acknowledgement-before-answer. Cite specific threads where the asymmetry is visible.
- **Reframe-before-acknowledge.** Moments where someone made a substantive point and the
  user (a) used "your point is fair, but…", (b) acknowledged in three words and re-delivered
  the original conclusion, or (c) reframed the question before answering it. Cite the moment.
- **Speed-as-status-signal / compression.** One-word replies to people lower in the
  hierarchy; the asymmetry between long structured broadcast messages and compressed handling
  messages. Quote verbatim.

If a pattern shows no evidence this week, say "no evidence of this pattern this week" plainly,
and show what was checked. All tracked patterns must be looked for every run.

### Self-decay logic

Every fourth run, the section ends with:

> **Quarterly review:** Is this section still surfacing things you didn't already know?
> (Answer below for the record.)

If the user answers "no" two consecutive quarterly reviews, the section retires itself. The
next run reports the retirement. If the user answers "yes" or doesn't answer, it continues
(a non-answer counts as "yes"; the burden is on the user to retire it actively).

This exists because adversarial framing degrades without a forcing function; it either
repeats the same findings until tuned out, or manufactures sharpness to feel useful. The
quarterly review is the prevention.

### Output format

```
## How I'm landing

**1. [Pattern name]:** [Direct observation, evidence-led, no hedge.]
*Evidence:* [chat quote with channel + date | paraphrased meeting arc with caveat | prior-reflection citation]

**2. [Pattern name]:** [As above.]
*Evidence:* [As above.]

[If quarterly run, append the quarterly review line.]
```

If a pattern has no evidence:

```
**1. [Pattern name]:** No evidence of this pattern this week.
*Sources checked:* chat ([n] messages reviewed), meetings ([n] reviewed), prior reflections ([n] checked).
```

### Verification additions for Step 6.5

- **Check 6 (transcript quote prohibition):** if any text appears in quotation marks
  attributed to a meeting transcript in this section, fail. Rewrite as paraphrase.
- **Check 7 (chat citation completeness):** every chat quote in this section must carry
  channel and date. Quotes without attribution fail.

## Step 5: Extract Decisions for Decision Log

From Step 1 evidence, identify significant decisions: strategic commitments, org changes,
product/roadmap changes, policy/process changes.

For each: decision statement (Primary if the user stated it in a meeting/chat, Secondary if
inferred); date; context/why (usually Secondary); expected outcome or success metric (Primary
if it references a KPI, Secondary if qualitative); owner; evidence tag per element.

Send to the decision-log skill, or present to the user for approval before logging.

## Step 6: Queue Consumer (Optional, KB Write Discipline) + Own Proposals

If you don't maintain a KB proposal queue, skip the queue mechanics and simply present KB
update proposals to the user for approval, then apply approved ones directly with a PII
filter (Step 6h).

If you do maintain a queue under Cat-A/B/C write discipline:

### 6a-6c: Drain and filter

Fetch the KB proposal queue note. Run the queue backstop unconditionally, processing all
Pending entries this skill owns at its native tier (Interpreted, Synthesis urgency), plus
any Primary/Secondary entries at Same-day/This-week/Urgent that EOD didn't reach (weekend
writes, partial EOD failures). The idempotency check (6g) makes repeating safe. State the
backstop result: "Queue backstop: X applied, Y rejected, Z recovered, N still Pending."

### 6d-6e: Route own proposals through the queue, present consolidated batch

From Step 1 evidence this skill may identify KB updates (strategy shifts, new metrics,
frameworks). Do not write them directly. Build queue entries and present them to the user
alongside the filtered Pending entries from other skills. Ask: "Approve, reject, or change?
Numbers or 'all'." Append approved entries to Pending.

### 6f: Leak scan

Reject any entry that quotes private decision-log content verbatim, contains personal
judgments about named people ("X is struggling", "I don't trust Y"), or contains
user-personal reasoning ("I think we should…", "my read is…"). Rejected entries surface in
the output's "Queue rejections" note.

### 6g: Apply with audit trailer

For each clean entry: read the target; **idempotency check** (scan for an existing matching
trailer; if present, skip apply, mark `recovered: trailer already present`); apply with the
right surgical tool (append blocks / modify block / modify range; full-note rewrite last
resort); append the audit trailer as inline code; move Pending → Applied.

### 6h: PII filter (before any KB write)

Strip personal and customer data: no individual user or customer names, emails, phone
numbers, or financial details. Replace individual names with role titles ("the account
manager", "a customer"). Business-entity account names are acceptable. Internal staff names
are acceptable. Evidence from chat and meetings often contains external contact names;
replace with role titles before KB entry. If PII can't be anonymised without losing meaning,
flag to the user.

### 6i-6j: Canonical-note rewrites + index/changelog

For target notes over 2,500 words, replace the affected section in place (modify block /
range) rather than appending; this avoids append-creep without the wipe risk of a full-note
rewrite. After all patches: if any created a new note, changed scope, or moved content,
update the KB index routing table and append one changelog entry. Non-structural in-place
updates need neither. When in doubt, log.

## Step 6.5: Pre-Publish Verification Pass

Before Step 7 writes to any note, run a verification pass against the drafted output. This
step exists because an unverified autonomous run can ship factual errors that collectively
reshape how the week looks to the user. Cheap to catch after drafting, expensive after
publishing.

Spawn a subagent red team with the draft output and the Step 0d lookups as input. It runs
these checks and returns failing claims with explanations. If any check fails, rewrite
before Step 7; do not publish and then correct.

**Check 1, Named person resolution.** Every named individual resolves against the people
lookup. First-name collisions resolved to the full name. Role/relationship attributed
matches the lookup (don't call a direct report a "peer"). A name that can't be resolved is
removed, quoted verbatim with an "evidence ambiguous" flag, or rewritten to the role.

**Check 2, Artifact test.** Every claim using action verbs (drafted, shipped, sent, shared,
published, decided, owned, committed, agreed, announced, logged) needs a linked artifact or
a verbatim quote. If not, it's inference: remove it, or rewrite to "is shaping" / "is
discussed" / "appears to have been" with an [Interpreted] tag. *Tightened for decided /
committed / agreed / landed:* the artifact must show a clear user-led articulation of the
call, not a multi-party prep summary characterising a position as "proposed" or
"team-aligned." Workshop-prep and brainstorm summaries do NOT meet the bar for a user
decision; they meet the bar for a direction-in-development by someone else. Default to
"shaping [Interpreted]" or hold for a future reflection.

**Check 3, Retrieval transparency.** Every source the draft relies on was actually queried
in Step 1, with the query and result in the evidence trail. Any claim about presence,
absence, or frequency in a source must point to the query that produced it. If a source
wasn't queried, say "not queried this run" rather than asserting a finding.

**Check 4, Canonical name check.** Every customer/partner/account name matches the Step 0d
canonical list. Transcription slips (a misspelled account) are exactly what this catches.

**Check 5, Trend claim citation.** Every multi-week-pattern claim ("three weeks running",
"the [theme] arc", "this is the third time") cites specific archived reflections (dates +
quote/paraphrase). Two reflections + this week is not three weeks running. A trend claim
without citations is a single-week pattern projected onto multiple weeks: rewrite as
single-week or remove.

(Checks 6 and 7 from Step 4.5 also apply: no transcript verbatim quotes in "How I'm landing";
all chat quotes there carry channel + date.)

The subagent returns a structured list of failures per check. If empty, proceed to Step 7. If
non-empty, rewrite and rerun. Do not publish until clean, or until a failure is explicitly
accepted and flagged to the user in the output.

## Step 7: Write Landing Page (Overwrite with Archive Pattern)

1. Preserve the existing intro callout at the top of the landing page (keep it, don't re-add).
2. Add a "Carried Forward" section if there are open items from the previous run (Step 2).
3. Include the fresh reflection content from Step 4.
4. Include a link to the reflection archive parent.
5. Write to the reflection landing note with the full content (replace entirely; do not
   append-blocks).

## Step 8: Archive and Publish

1. **Reflection:** deliver to the user in conversation.
2. **Decisions:** save to the decision log (or queue for manual entry).
3. **KB updates:** execute approved updates; note sources and dates on every claim. All
   shared-KB writes happen inside Step 6, carry the audit trailer, and trigger the index +
   changelog update. The landing page overwrite in Step 7 is this skill's own canonical note
   and isn't subject to the queue discipline.

## Worked example (fictional data)

**Trigger.** Sunday morning, the user (a product executive at Acme) types "run my reflection".

**Step 1 spine.** Four of five EOD notes exist for the week (Wednesday's is missing, flagged).
The trend spine has four prior reflections. Across the week, two themes recur: the user kept
getting pulled into the deploy-flakiness incident, and the pricing model slipped from Tuesday
to Thursday to "next week".

**An excerpt of the drafted reflection (the section that earns its keep):**

```
## What I'm noticing about my leadership

- The pricing model has now slipped three times (this week's EODs Tue and Thu; W-1
  reflection, 11 May, named it "the thing I keep not finishing"). The pattern is not
  capacity, your calendar had gaps both afternoons. What changed about the threshold for
  actually sitting down with it? Worth asking whether it has become avoidance dressed as
  prioritisation.
- Direct-report fires crowded out peer work again. You spoke to Daniel daily but did not
  reach Priya once this week, the third week running this asymmetry shows (W-1 11 May, W-2
  4 May both noted it). The connecting frame is not "stay closer to Priya"; it is that
  incident response keeps winning your attention by being louder, not more important.

## How I'm landing

**1. Reframe-before-acknowledge:** When the GTM lead pushed back on the pricing timeline you
replied "fair, but the model has to be right before it is fast" (#gtm, 14 May), which
acknowledges in three words and re-delivers your original position. The point they were
making, that the delay has a cost, went unaddressed.
*Evidence:* #gtm, 14 May, 16:51.

**2. Status-asymmetric warmth:** No evidence of this pattern this week.
*Sources checked:* chat (around 60 messages reviewed), meetings (4 reviewed), prior
reflections (4 checked).
```

**Step 5, decisions surfaced.** One: the reallocation of two engineers off the partner portal
(Primary, stated in #payments-eng 14 May). Routed to decision-log for confirmation.

**Step 6.5, verification caught one error.** The draft attributed a comment to "Sam"; the
people lookup shows two Sams, and the evidence only said "Sam". Check 1 failed, so the line
was rewritten to "someone in the leadership sync" before publishing.

## Integration with Other Skills

- **kb-source-ingestion:** covers signals from the broader team (practice/customer research,
  competitor intel, support themes). This reflection covers signals from the user personally.
  No overlap.
- **meeting-prep:** may reference org data from Step 0a.
- **blind-spots-report:** runs independently; both use the Step 0 canonical-source pattern.
- **end-of-day-review:** produces the five daily EOD notes that are this skill's primary
  spine (Source 0). This skill is also the Friday backstop if EOD missed runs.

## Key Rules

1. Fetch org and KB index fresh every run.
2. If canonical sources unreachable (Step 0), halt.
3. Evidence before assessment. Don't reflect from memory.
4. Tag decisions and KB proposals; present tags to the user before approval.
5. Be honest about gaps.
6. Decisions are serious. Log them with context and success metrics.
7. KB updates need approval. Don't auto-write.
8. Archive before overwriting.
9. Verify before publishing (Step 6.5). If verification surfaces failures, rewrite and rerun;
   do not publish first and correct later.

## Output Format

Reflection output, delivered in the user's personal channel only.

- **No AI banner.** These land in the user's personal channel only; the private context makes
  the banner noise.
- **TL;DR**: 2-3 paragraphs. Lead with strategy and impact, then interpersonal complexity. Do
  not open with a feelings-summary.
- **Prose organised by person, theme, or anchor**: no forced table. Recognition before
  critique. Name the gap, not the grade.
- **Method footer**: sources pulled, date range, anything stale or missing.

**Push harder directive.** The skill must push the user's thinking with adversarial
questions, not summarise. Reach for less-safe observations even when uncomfortable. The
threshold for a hard insight is lower on Sunday reflection than in any other context. If the
reflection feels safe on a re-read, it isn't doing its job. Frame hard reads as questions
back to the user, not assertions.

Output location: always the user's personal channel; never a shared KB channel.
