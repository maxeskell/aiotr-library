---
name: decision-log
description: >
  Log and review significant leadership decisions outside of the weekly reflection. Use for
  standalone mid-week decision capture ("log a decision", "record this decision", "add to the
  decision log", "how did that decision hold up") or any mid-week request to track a roadmap
  change, org change, planning call, overrule, or team/mission/people decision. Also trigger
  when the user says things like "we decided to", "I'm going to", "I have decided to" in a
  context that sounds like a significant call worth tracking. Use for single ad-hoc decisions
  mid-week; the weekly-self-reflection skill handles bulk decision extraction during the
  Sunday review.
---

# Decision Log: Template

> Template. Replace every `{{PLACEHOLDER}}` with your own context. A worked example showing
> one filled-in version is in [SKILL.example.md](SKILL.example.md). For design patterns and
> trade-offs, see [README.md](README.md).

## Placeholders used in this template

| Placeholder | What to put here | Example values |
|---|---|---|
| `{{KB_SYSTEM}}` | Your knowledge base tool. | "Slite", "Notion", "Confluence" |
| `{{SKILL_INDEX_LOCATION}}` | Where your skill registry / index lives. | "the Personal Skill Index note in {{KB_SYSTEM}}" |
| `{{KB_INDEX_LOCATION}}` | The routing table that maps content categories to note locations. | "the KB index note in {{KB_SYSTEM}}" |
| `{{DECISION_LOG_LOCATION}}` | Where the active decision log lives. | "the Decision Log note in {{KB_SYSTEM}}" |
| `{{DECISION_ARCHIVE_PARENT}}` | Parent location for archived closed/superseded decisions (optional). | "the Decision Log Archive folder in {{KB_SYSTEM}}" |
| `{{KB_CHANGELOG_LOCATION}}` | Where structural KB changes are logged. | "the KB Changelog note in {{KB_SYSTEM}}" |
| `{{MISSION_TERM}}` | Whatever you call portfolio-level strategic priorities. | "mission", "bet", "initiative", "pillar" |
| `{{OUTPUT_STANDARDS_REFERENCE}}` | Where your numbered output standards / axioms live. | "the Skill Output Standards note in {{KB_SYSTEM}}" |

---

## What This Does

Mid-week decision capture for significant leadership decisions that need to be tracked,
referenced, or reviewed later.

Logs:

- Roadmap changes (new initiatives, reprioritisation, scope changes)
- Org changes (hiring, role shifts, team adjustments, manager changes)
- Process changes (new decision-making framework, meeting cadence change)
- {{MISSION_TERM}} changes (pivoting, escalating commitment, pausing)
- Resource allocation changes (budget, headcount, tool investments)
- Override decisions (when the user decides to diverge from a previous decision or
  recommendation)

**Does NOT log:**

- Meetings, calls, or discussions (that's your meeting-notes app / calendar)
- Individual task / ticket decisions (that's your ticketing system)
- Hiring or firing notifications (use your HR / admin channels)

---

## Output Header

Every output begins with the standard header defined in {{OUTPUT_STANDARDS_REFERENCE}}.
Sources to report freshness for: decision-log-note.

---

## CRITICAL: Mandatory Step 0: Fetch KB index and validate decision log rules

**Before logging a decision, fetch the KB index and validate decision categorisation against
current rules. If canonical sources are unreachable, halt.**

Fetch {{SKILL_INDEX_LOCATION}}. Use note IDs from the registry for all reads and writes
below. Do not hardcode IDs in this skill; they belong in the registry.

### Step 0a: Fetch KB index

1. Fetch {{KB_INDEX_LOCATION}}.
2. If fetch fails: report "KB index unreachable, cannot log decision. Confirm KB connection
   and retry." **STOP HERE.**
3. Extract the routing table to find the Decision Log note location.
4. Store this in local variables (not cached across runs).

### Step 0b: Fetch decision log note and validate structure

1. Fetch {{DECISION_LOG_LOCATION}}.
2. If fetch fails: report "Decision log note unreachable, cannot log decision. Confirm note
   is still accessible." **STOP HERE.**
3. Review the current structure to understand:
   - How decisions are categorised (by type, date, status?)
   - What fields are expected (decision, date, context, owner, success metric, status?)
   - How decisions are marked as Active / Closed / Superseded?
4. Store the current structure in local variables (not cached across runs).

---

## Step 1: Identify if this is a loggable decision

When the user says something decision-like:

- Is it a significant commitment or change in direction? (Loggable)
- Is it a one-off task or operational detail? (Not loggable)
- Is it reversible or irreversible? (Irreversible = more likely loggable)

If yes, proceed to Step 2.

---

## Step 2: Gather decision context

Ask (or infer from context):

1. **Decision statement:** what was decided, in 1-2 sentences?
2. **Decision type:** Roadmap / Org / Process / {{MISSION_TERM}} / Resource / Override?
3. **Date:** when was this decided?
4. **Context / why:** why was this decision made? What problem does it solve?
5. **Owner:** who is accountable for executing this decision?
6. **Success metric:** how will we know if this decision was right? (Optional but preferred)
7. **Alternatives considered:** what else could we have decided? Why not?

---

## Step 3: Write the decision entry

Using the structure from Step 0b, format the decision as:

```
## [Decision Title], [Date]

**Decision:** [What was decided, in 1-2 sentences]

**Type:** [Roadmap / Org / Process / {{MISSION_TERM}} / Resource / Override]

**Context:** [Why this decision was made. What problem does it solve. What was the trigger.]

**Alternatives considered:** [What else could we have decided. Why we chose this option.]

**Owner:** [Who is accountable for executing]

**Success metric:** [How we'll know this decision was right. Example: "Adoption of feature X
reaches Y% by date Z", or "Team completes the roadmap initiative by Q2", or "N/A if no
metric yet"]

**Status:** [Active / Closed / Superseded]

**Superseded by:** [If status is Superseded, link to the newer decision that overrode this]

---
```

---

## Step 3b: Confirm before writing

**One-way door: stop here.** A logged decision becomes the canonical record in the KB and
shapes how future sessions reason about what was decided and why. Before proceeding, show
the user the formatted entry from Step 3 and ask: "Does this look right? Confirm and it gets
logged." Do not write to the Decision Log until the user explicitly approves the
entry. If they want changes, edit and show again before writing.

---

## Step 4: Archive current landing page (optional pattern)

Before writing the new decision entry, archive the current landing page content if it has
grown to 20+ decisions.

### 4a: Read current landing page (size check)

1. Fetch {{DECISION_LOG_LOCATION}}.
2. Count the number of decision entries. If 20+, proceed to 4b. If fewer, skip to Step 5.

### 4b: Create archive child note (if needed)

1. Identify and extract all closed or superseded decisions from the landing page.
2. Create a new note under {{DECISION_ARCHIVE_PARENT}} titled `Closed Decisions: [Date Range]`,
   with those decisions as content.
3. Remove archived decisions from the landing page, keeping only active and recent ones.

---

## Step 5: Add to decision log

1. Fetch the latest version of {{DECISION_LOG_LOCATION}}.
2. Add the new decision entry, maintaining chronological order (most recent at top).
3. Update any previous decisions whose status should change (e.g., mark an old decision as
   Superseded).
4. Write the updated decision log back to the KB (overwrite, not append).

---

## KB Governance: index and changelog (after any write)

After executing any approved KB write:

**Index check.** Did this write create a new note, change a note's scope, or move content
between notes?

- Yes: update the routing table in {{KB_INDEX_LOCATION}}.
- No: no index update needed.

**Changelog entry.** Was this a structural change (new note created, note renamed or moved,
content relocated, convention changed)?

- Yes: append a dated entry to {{KB_CHANGELOG_LOCATION}}: `[DD Mon YYYY] decision-log:
  [What changed and what it means for future sessions retrieving this content]`
- No (routine content update, status change, new section appended): no changelog entry needed.

Structural = the map changed (where things live). Non-structural = the map is the same but
content was updated in place.

Note: appending a decision entry to an existing note is non-structural and does not require
an index update or changelog entry. Governance applies only if a new archive note is created
or the decision log is moved.

---

## Step 6: Confirm and archive

After logging:

1. Confirm to the user: "Decision logged: [Decision Title]"
2. If the decision log has grown to 20+ decisions, note that closed decisions have been
   archived.

---

## Reviewing decisions

When the user asks "how did that decision hold up?":

1. Fetch the decision log.
2. Find the decision by title or date.
3. Compare success metric (if one exists) against current reality.
4. Report: "Status: [On track / Diverged / Needs review]. Evidence: [recent signal showing
   whether the decision is proving right or wrong]."
5. Recommend: update decision status or note if reality has changed.

---

## Key Rules

1. **Fetch KB index and decision log fresh every run.** Structure and existing decisions may
   change.
2. **If canonical sources unreachable, halt.** Do NOT store decisions in memory or email;
   they must go in the KB.
3. **Success metric is valuable.** If the user can't articulate how they'll know a decision
   was right, that's a sign the decision needs more thinking.
4. **Alternatives matter.** Logging what wasn't chosen helps explain why this was chosen.
5. **Status tracking.** Keep decisions updated as they're executed, completed, or
   superseded. Don't let old decisions linger as Active.
6. **Owner accountability.** Every decision should have a clear owner, even if it's the user.
7. **Archive when landing page grows.** Keep the landing page focused on active and recent
   decisions.

---

## Integration

**Called by:** the weekly-self-reflection skill (automatic extraction of decisions from the
week).
**Called independently:** when the user makes a mid-week decision and wants it logged
immediately.
**Not called by:** KB-ingestion or other skills (decision capture is the user's domain).

---

## Skill Run Log

After completing all steps (success or failure), append an entry to your Skill Run Log
(location resolved via {{SKILL_INDEX_LOCATION}}). Format: skill name, ISO timestamp, status
(Success / Partial / Failed), one-line note if failed or partial.

---

## Output Format

Conform to the decision-capture bucket in your output standards. Fetch
{{OUTPUT_STANDARDS_REFERENCE}} at the start of every run.
