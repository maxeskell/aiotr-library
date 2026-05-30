---
name: meeting-prep
description: >
  Prepares a meeting brief for any named meeting: customer calls, 1:1s with direct reports,
  leadership reviews, or peer syncs. Give a meeting name or person's name; this skill pulls
  live context from your meeting-notes app, chat tool, ticketing system, customer-success
  platform, call-recording tool (for customer meetings only, with privacy discipline below),
  and the KB, then produces a tight briefing with recent history, current state, and
  suggested talking points or diagnostic questions. Trigger on: "prep me for", "prepare for
  my", "I have a meeting with", "help me prep", "what do I need to know before my", "brief
  me on", "getting ready for my call with", "meeting prep", or any mention of an upcoming
  meeting where context would help. Also trigger when the user names a meeting participant
  and asks what's going on with them.
---

# Meeting Prep

Prepares a decision-ready brief for any named meeting, so the user walks in already oriented
rather than reading a dump of everything that exists.

> The stack named below is illustrative: Granola for meeting notes, Slack for chat, Jira for
> tickets, a CRM and a customer-success platform for accounts, a call-recording tool for
> customer calls, and Notion as the KB. Swap these for your own. The source discipline (what
> to fetch, what never to fetch) and the brief structure are the portable parts. The user in
> the worked example is a product executive at a fictional vertical-SaaS company, Acme.

## Source discipline: call-recording tool

The call-recording tool is a Secondary source at the **per-account tracker-hit and call-count
layer only**, used **only when the meeting type is customer / external**. Not used for 1:1s,
leadership reviews, or peer syncs. This skill never fetches call summaries, never fetches
transcripts, and never surfaces individual-person attribution or quotes.

**What this skill fetches from the call-recording tool for a customer meeting:**

- The account's call count over the past 90 days, by call type (e.g., CS check-in, sales
  conversation, churn debrief, onboarding).
- Per-tracker hit counts on calls with this account over the past 90 days: which themes /
  competitors / topics fired on calls (e.g., "this account had 8 calls in 90 days; trackers
  hit: pricing(5), [competitor name](2), [feature theme](3)").

**What this skill does NOT fetch:**

- Call summaries (contain individual names, internal context, sensitive specifics).
- Call transcripts (same, plus verbatim quotes).
- Any per-call content beyond call ID, date, call type, and tracker hits filtered to this
  account.

**Rationale.** Per-account tracker-hit counts are aggregate signals about which themes have
come up on calls with this specific account. They carry no individual content. Account name
is preserved because it's the meeting subject; individual staff and sensitive detail never
enter the brief.

**Output discipline in the brief.** Format: "Past 90 days: N calls with this account. Tracker
hits: [tracker name (count), tracker name (count)]." Place under "Recent history" or "Current
state" depending on whether trackers point at historical themes or open commitments.

## CRITICAL: Mandatory Step 0: Fetch canonical org data (for 1:1s only)

For 1:1 meetings with direct reports, fetch the current org data to validate role and team
assignments. This ensures the brief reflects current org reality, not stale assumptions.

1. Identify meeting type: is this a 1:1 with a direct report, or a different type?
2. If 1:1 with direct report: proceed with Step 0a. Otherwise, skip to Step 1.
3. Fetch the skill index note in Notion. Use note IDs from the registry for all reads below.
   Do not hardcode IDs in this skill.

**Step 0a: Fetch org data**

- Fetch the Org and People note in Notion.
- If fetch fails: report "Cannot prepare 1:1 brief, org data unreachable. Confirm KB
  connection and retry." **STOP HERE.**
- Extract the section relevant to the user's directs.
- Store each direct's current team(s) and role in local variables.
- Pay special attention to: team owners, regional leads, and partners vs. team owners.

**Step 0b: Validate current role**

- For the direct report in this 1:1, confirm their current team and role against the freshly
  fetched org data.
- If role/team has changed since your last knowledge, note this as context for the brief.
- If their role is unclear in the org data, flag to the user rather than assuming.

You are preparing a meeting brief for a product executive. They need concise, decision-ready
context, not a summary of everything that exists. The output should read like a trusted
colleague has done the research so the user can walk into the meeting already oriented.

## Step 1: Identify the meeting type

From the meeting name or participant, determine which of these categories applies:

- **Customer / external meeting**: an account, prospect, or partner. Focus on account
  health, recent issues, commercial context.
- **1:1 with direct report**: one of the user's direct reports. Focus on delivery,
  blockers, what they need from the user, any people context. Use org data from Step 0.
- **Leadership / exec review**: board, SLT, or cross-company review. Focus on metrics vs
  targets, delivery status, key decisions.
- **Peer / cross-functional sync**: a peer executive or shared workstream owner. Focus on
  outstanding items, recent collaboration, anything that needs alignment.

## Step 2: Retrieve relevant context in parallel

Complete ALL retrieval before writing anything. Do not narrate the retrieval process.

**For all meeting types:**

- Granola: search for recent meetings involving this person, account, or topic (last 4-6
  weeks).
- Slack: search for recent mentions of the person, company, or relevant topic.

**For customer / external meetings, additionally:**

- Customer-success platform: resolve the customer first, then in parallel pull:
  - Health score, account stage, ARR / MRR, customer-success manager, key flags (multisite,
    region, segment).
  - Open CTAs / Risks / Expansions / Churn Risks.
  - Recent timeline activity (last 60 days, last 5 entries).
- CRM: commercial context, deal status, contract details. Capture the CRM account ID for the
  call-recording fetch below.
- Call-recording tool per-account tracker hits (last 90 days). Bound by the **Source
  discipline** section at the top of this skill. Tracker-hits-only, no per-call content.
- Notion: any strategic notes or account history.

**For 1:1 with direct report, additionally:**

- Jira: their team's current sprint status, blockers, recent completions (using team key
  from Step 0).
- Notion: any relevant OKR or roadmap context for their area.
- Execution / Delivery Briefing Handoff note: delivery status, blockers, escalation
  patterns, ownership quality for this person specifically.
- Direct Report Coaching Notes: development plan status, targets, coaching focus, any live
  coaching threads.
- Team Effectiveness Briefing: craft signals, adoption data, and week-level watch items
  for this person.

**For leadership / exec reviews, additionally:**

- Notion: current OKR state, metric targets, mission status.
- Jira: cross-team delivery summary, near-term risk register.
- Execution / Delivery Briefing Handoff: cross-team delivery status and blocker summary.
- Team Effectiveness Briefing: exec summary, standout performers, team-level watch items.

**For peer / cross-functional syncs, additionally:**

- Jira: any shared tickets or dependencies.
- Notion: relevant strategy context.

If a source fails or returns nothing useful, note it briefly as a source gap; do not
substitute silence for acknowledgment.

## Step 3: Write the brief

Use this structure. Keep each section tight. No em dashes anywhere.

---

### [Meeting name / person]: [Date if known]

**Context** (2-3 sentences)
One sentence on who this is and why the meeting matters right now. One sentence on the
most important thing happening in their world at the moment.

**Recent history**
2-4 bullets. What has come up in recent meetings or chat? What was left open? What
commitments were made?

**Current state**
2-4 bullets. What is live right now that affects this meeting? Health signals, delivery
status, commercial dynamics, team situation, whatever is most relevant to the meeting type.

**Talking points** (for customer, leadership, and peer meetings)
3-5 numbered items. Concrete, specific, sequenced. Not generic topics, actual things to
say or ask, based on the retrieved context. Lead with the most important one.

These are starting points for the user to adapt, not a script. Label them clearly as DRAFT.

**Diagnostic questions** (for 1:1s with direct reports only, replaces Talking points)
3-5 numbered questions. Each one tests whether the direct report has seen what the brief
surfaced, without telling them first. Frame as genuine curiosity, not a quiz. Lead with
the most important one.

Before raising any finding from this brief yourself, ask the question that would reveal
whether they already know. Your job in this meeting is to discover what they see, not to
show what you see. If they don't see it, coach the scanning habit, not the specific finding.

Only raise directly if: it's time-sensitive and they haven't surfaced it, or it requires
the user's authority to unblock.

**Watch for**
1-2 bullets. Things that might come up that need careful handling. Flag if there's
something sensitive, a commitment that was made and not delivered on, or a known tension.

**One-way doors in this meeting**
1-3 bullets. Call out any actions in this meeting that would be hard to walk back: a
commitment to a customer, feedback delivered to a direct report, a direction set with
a peer that closes down options. For each one, state what makes it irreversible and what
to consider before going there. Omit this section if there are no genuine one-way doors;
do not fill it with generic caution.

---

## Output style

- Short sentences.
- No padding, no consultant language.
- Bullets in the brief sections are fine and preferred.
- If a section has nothing meaningful to say (genuinely no signal found), omit it rather
  than filling it with filler.
- If the meeting type is unclear from the name alone, state your assumption at the top of
  the brief.
- End with a one-line source note listing what was checked and what returned nothing.
- Open every brief with a one-line header: **INTERNAL PREP, Not for sharing**. This makes
  the internal-only status visible at a glance and prevents the brief from being accidentally
  forwarded or used as a pre-read.

## Source transparency

Always end with:

> Sources checked: [list]. Gaps: [anything that returned no useful signal, or failed].

## Key Rules

### For 1:1s

1. **Fetch org data fresh every run.** Role/team changes matter. Do not use cached org
   assumptions.
2. **If org data is unreachable (1:1 only), halt.** Do not proceed without current org
   context for 1:1s.
3. **Reflect current reality.** If the org has changed since last you knew, the brief should
   reflect the new reality, not the old.

### For customer meetings (call-recording tool)

4. **Tracker-hits-only.** For customer meetings only. Step 2 fetch retrieves per-account
   tracker hit counts and call-count metadata over the past 90 days. No call summaries, no
   transcripts, no quotes, no individual content.
5. **Account name is fine; everything else is aggregate.** Account name is preserved because
   it's the meeting subject. Individual staff and sensitive content never enter the brief
   because the underlying content is never fetched.
6. **No call summaries or transcripts fetched.** Only the tracker-hit table reaches the
   brief. If a relevant theme has no tracker configured, that limitation stands; do not fall
   back to scanning summaries.

## Worked example (fictional data)

**Trigger.** "Prep me for my 1:1 with Daniel tomorrow." Daniel is one of the user's direct
reports at Acme.

**Step 0 (1:1, so org data is fetched).** The Org and People note confirms Daniel leads the
Payments squad, a role he moved into six weeks ago from a senior-engineer position. The brief
notes the recency of the role change as context.

**Step 2 retrieval (Granola, Slack, Jira, coaching notes).** Recent Granola notes show two
sprint reviews; Slack shows Daniel escalating a flaky-deploys thread three times in two weeks;
Jira shows the Payments sprint slipped two of five committed stories; coaching notes have a
standing focus on "delegating incident response rather than owning every fire himself".

**Step 3 output:**

```
INTERNAL PREP, Not for sharing

### 1:1 with Daniel: tomorrow

**Context**
Daniel leads the Payments squad, six weeks into the role after stepping up from senior
engineer. The step-up is the backdrop for most of what follows: he is still holding on to
the hands-on work that used to be his job.

**Recent history**
- Escalated the flaky-deploys problem in Slack three times in two weeks (#payments-eng,
  6, 11, 14 May), each time proposing the fix himself rather than routing it to an owner.
- Last sprint review (Granola, 9 May): committed to five stories, the team landed three.

**Current state**
- Payments sprint slipped two of five stories (Jira). Both slipped stories were blocked on
  the same deploy flakiness Daniel has been escalating.
- Standing coaching focus: delegating incident response instead of owning every fire.

**Diagnostic questions** (DRAFT, adapt before using)
1. How did the sprint land against what you committed to, and what do you put the gap down to?
2. The deploy flakiness has come up a few times now. Who owns getting to the root cause?
3. What would have to be true for you to step back from the incident response yourself?
4. Six weeks in, where do you feel the role still pulling you back into the old job?

**Watch for**
- He may present the deploy problem as purely technical. The coaching read is that it is also
  a delegation pattern; let him reach that rather than naming it for him.

**One-way doors in this meeting**
- Resist solving the deploy problem for him in the room. Doing so confirms the exact pattern
  the coaching focus is trying to shift, and it is hard to walk back once you have taken it on.

Sources checked: Granola, Slack, Jira, coaching notes, Org and People. Gaps: none.
```

## Skill Run Log

After completing all steps (success or failure), append an entry to your Skill Run Log
(location resolved via the skill index). Format: skill name, ISO timestamp, status
(Success / Partial / Failed), one-line note if failed or partial.

## Output location

Output is always the user's personal channel, never a shared KB channel.
