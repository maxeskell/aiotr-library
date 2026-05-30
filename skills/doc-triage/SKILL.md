---
name: doc-triage
description: >
  Triage a doc going to exec or SLT against a four-check rubric (Clarity, Evidence quality,
  Owner and next step, LLM-tell). Use whenever the user shares a doc and asks whether it's
  ready to circulate, whether it should go to exec, whether it's decision-ready, or just
  "review this doc" or "triage this". Also trigger on "is this doc good", "what's wrong with
  this", "should this go to SLT", "ready for exec / leadership / [name]", "can I send this",
  "doc check", "is this ready", or any time a colleague has shared a pre-read, strategy doc,
  planning doc, or discussion doc the user needs to assess before forwarding. Trigger even
  when the user doesn't say "triage": if a doc is shared and the implicit ask is "is this
  good enough to go up the chain", use this skill.
---

# Doc Triage: Template

> Template. Replace every `{{PLACEHOLDER}}` with your own context. A worked example showing
> one filled-in version is in [SKILL.example.md](SKILL.example.md). For design patterns,
> see [README.md](README.md).

## Placeholders used in this template

| Placeholder | What to put here | Example values |
|---|---|---|
| `{{ANALYTICS_TOOL}}` | Your product-usage analytics tool. | "Mixpanel", "Amplitude", "PostHog" |
| `{{KB_SYSTEM}}` | Your knowledge base. | "Slite", "Notion", "Confluence" |
| `{{REVENUE_RECOGNITION_RAMP}}` | Your company's revenue recognition schedule for new ARR. | "Year 1 X%, Year 2 Y%, Year 3 Z%" |
| `{{TOPIC_OWNER_TAXONOMY}}` | Your "who owns which topic" mapping (used in the right-reader check). | "Line manager: development. Mission lead: strategy. Product leader: market context." |
| `{{PERSONAL_CHANNEL_LOCATION}}` | Where the user keeps personal-channel drafts (used only if the user asks to save the triage). | "your personal channel in {{KB_SYSTEM}}" |
| `{{MISSION_TERM}}` | Whatever you call portfolio-level strategic priorities. | "mission", "bet", "initiative", "pillar" |

---

## What This Does

A two-phase rubric for assessing whether a doc is ready to circulate to exec, SLT, or any
stakeholder. Phase A confirms what the doc is trying to do. Phase B grades it against four
hard-gate checks. Output is under 320 words.

This skill exists because docs that reach executives fail in a small set of specific ways:
unclear ask, Secondary evidence treated as Primary, default-to-the-user routing, financial
figures stated without applying the recognition ramp, "enabled" mistaken for "active".
Naming the failure clearly and early saves the author a round trip and protects exec time.

## Inputs

The doc can arrive as a file attached to the conversation, a KB note ID or URL, a doc URL
(Confluence, Google Docs, etc.), or pasted text. Read it in full before doing anything else.

Use the right tool for the source: your KB's fetcher for KB IDs or URLs, the read tool for
a file in the workspace, the conversation directly for pasted text. If the source is
unfamiliar or content can't be extracted, ask the user for a paste before proceeding rather
than guessing.

## Phase A: confirm the read

Before running the rubric, state the read clearly and ask the user to confirm, correct, or
clarify. Do not run Phase B on an unconfirmed read. The cost of pausing is small. The cost
of grading the wrong kind of doc is wasted exec time and bad guidance back to the author.

Phase A output is exactly three lines plus one prompt:

```
Doc type: [product_review | strategy_decision | planning | discussion]
Audience: [exec/SLT | cross-functional | internal team]
Purpose: [decision | discussion | both]

Confirm, correct, or clarify before I run the full rubric.
```

Stop. Wait for the user's response.

If the user corrects any of the three, update them and proceed to Phase B with the
corrected read. If the user says "go ahead" or similar, proceed with the read as stated.

## Phase B: four hard-gate checks

Run these in order. Each is a hard gate. If a check fails, that failure is the headline of
the output.

### Check 1: Clarity (weighted by confirmed purpose)

**Decision doc:** read only the first two sentences of the doc. Can a reader finish the
sentence "This doc helps [person] decide [specific question] by [date]"? If not, not ready.
Flag FYIs dressed as decisions.

**Discussion doc:** can a reader finish "This gives [person] enough to [challenge /
validate / input on] [specific hypothesis or question]"? Must name the hypothesis, what the
exec is asked to contribute, and what the author is NOT yet asking for.

**Both:** must pass both tests.

### Check 2: Evidence quality

Identify the load-bearing claims, those that directly support the recommendation or
decision. Apply this check to those only. Don't grade every fact in the doc; grade the ones
the recommendation rests on.

**Evidence tiers:**

- **Primary:** named tool or document + date + denominator or sample size. Example:
  "{{ANALYTICS_TOOL}} booking funnel, April 2026, 450 accounts."
- **Secondary:** named person + context (meeting, chat, interview) + approximate date.
  Example: "[Name] in the regional review, March 2026."
- **Interpreted:** synthesis, projection, or inference. Must be explicitly labelled as
  assumed in the doc.

**Four named failure patterns. Check each:**

1. **Secondary hardening.** A claim cites an internal doc or previous review as its source,
   but that doc itself cited a meeting, 1:1, or chat thread. The claim started as Secondary,
   was written into a doc, and is now treated as Primary. Flag the chain: "This cites [doc],
   which cited [meeting / person]; the original source is Secondary, not Primary."

2. **Enabled vs active adoption.** Any feature percentage or count without distinguishing
   *enabled* (provisioned or accessible) from *active* (actually used). "X% of accounts have
   this feature" almost always means enabled. Flag it and name where the active usage figure
   lives: {{ANALYTICS_TOOL}}, with the event and denominator.

3. **ARR without ramp.** If your company recognises new ARR on a ramp ({{REVENUE_RECOGNITION_RAMP}}),
   an ARR figure stated without applying the ramp is misleading. Ask for the step-by-step
   math: eligible population × adoption % × price × 12 = full ARR, then apply the ramp.

4. **Unlabelled inference.** A load-bearing claim is Interpreted but not labelled as such.
   Flag it: "Inference note: [what is being inferred]; [what data would confirm it, and
   where to find it]."

**For decision docs:** any load-bearing claim that is Secondary or Interpreted without being
labelled is a fail. Numbers missing denominator or sample size are a fail. Unrepresentative
samples cited for product-wide conclusions are a fail.

**For discussion docs:** Secondary is acceptable if labelled. At least one load-bearing
claim must be Primary or named Secondary with a date. Zero named sources = not ready.

### Check 3: Owner and next step

**Decision doc:** for every recommendation, who is doing what by when? Flag passive voice
("this will be reviewed"), "the team" as the owner, or the user in the owner column when
the author should own it.

**Discussion doc:** who takes the exec input forward and how? If no next step is named,
flag it.

### Check 4: LLM-tell scan

Flag the patterns the author should cut before sending: throat-clearing openers, closing
flourishes, vague intensifiers (truly, incredibly, fundamentally, essentially), hedging
fillers ("it's worth noting", "one thing to consider"), decorative tricolons (lists of
three when only two ideas are real), section-end restatements that summarise the section
above.

**Banned vocabulary to flag:** delve, tapestry, navigate (used metaphorically), realm,
underscore (as a verb), multifaceted, profound, holistic, robust, leverage (as a verb),
utilise, streamline, comprehensive.

## Right-reader check (run before producing the output)

A doc going to the wrong reader fails even if the rubric passes.

{{TOPIC_OWNER_TAXONOMY}}

Default-to-the-user is a failure mode. If the doc is routed to the user but the topic
clearly sits with a non-user owner, flag the audience issue and name the right reader.

Watch for one specific case: a doc labelled "for discussion" that contains a concrete
recommendation or preferred direction. Flag it: "this looks like a decision doc wearing
discussion clothes."

**One-way vs two-way door:** if the underlying decision is reversible and low-stakes, flag
whether a doc was even the right form, or whether a chat message would have done.

## Output Format

Under 320 words total. No tables. No nested lists. No em dashes. Use a comma, period, or
rewrite instead.

```
**Verdict.** Ready or Needs work. One sentence on why.
**What this is.** One line on doc type, purpose, and the question it enables.
**What's working.** One specific line. Name the section, choice, or approach the author
got right and why it works. Not vague reassurance.
**Data confidence.** Grounded / Assumed / Unsupported. One sentence naming the weakest
load-bearing claim and what would fix it.
  Grounded: all load-bearing claims are Primary.
  Assumed: load-bearing claims are Secondary or Interpreted but explicitly labelled.
  Unsupported: load-bearing claims have no source, or the cited source does not support
  the conclusion.
**Blocking issue.** (if Needs work, omit if Ready) The single thing that stops this
reaching exec. State it directly, no hedging. If multiple checks fail, name the one an
exec would reject the doc on.
**Action for me.** Read in full / read selectively / send back to author / route to
[role] / decline. Pick one.
**Estimated read time.** Required. Decision doc = 15 or 30 minutes. Discussion doc = 5 or
15 minutes. Never output 5 minutes for a decision doc.
**Audience flag.** (Only if routing is off but not a refusal. One line.)
**Other fixes:** (if Needs work, 1 to 2 items max, subordinate to the blocking issue,
omit if Ready)
1. [Fix]. So that [specific outcome].
2. [Fix]. So that [...]
```

Severity hierarchy: the blocking issue is the gate. Other fixes are improvements. Never
list three equal-weight fixes. If only one thing is genuinely blocking, say so.

Tone: trusted colleague who has read this doc carefully and wants it to succeed, not a
gatekeeper checking boxes. Be direct about problems but frame them as fixable. Acknowledge
the effort. No em dashes. No padding.

## What this skill does not do

- Does not rewrite the doc. The triage flags issues; the author fixes them.
- Does not write to the KB. Output is ephemeral chat output. If the user wants the triage
  saved, they ask separately and it goes to {{PERSONAL_CHANNEL_LOCATION}}, never to a
  shared KB note.
- Does not run on a doc the user themselves wrote unless they explicitly ask. The user
  usually writes for thinking, not for forwarding; a rubric is the wrong tool for early
  drafts of their own work.
- Does not grade the whole doc. Grades only the load-bearing claims, those the
  recommendation or decision rests on.
