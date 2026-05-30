# doc-triage

A two-phase rubric for assessing whether a doc is ready to circulate to exec, SLT, or any stakeholder. Phase A confirms what the doc is trying to do. Phase B grades it against four hard-gate checks. Output is under 320 words.

This is the skill INVENTORY rated "the four-check rubric is publishable as a standalone framework essay even before the skill." If you're only going to copy one rubric out of this repo, copy this one; the four checks travel even without any AI implementation.

## Files in this folder

- **[SKILL.template.md](SKILL.template.md)**: portable shape with 6 placeholders (KB system, analytics tool, revenue ramp, topic-owner taxonomy, personal-channel location, strategic-priority term).
- **[SKILL.example.md](SKILL.example.md)**: one filled-in version. Slite as KB, Mixpanel as analytics tool, "mission" as strategic-priority term.
- This README: the design patterns this skill implements, and notes on adapting it to your context.

## Design patterns this skill implements

- **Two-phase confirm-then-grade.** Phase A asks the user to confirm doc type / audience / purpose *before* running the rubric. Most doc-review failures aren't about the doc; they're about grading the wrong kind of doc. The pause is cheap; the wasted exec time when you grade a discussion doc as a decision doc is expensive.
- **Hard-gate checks in order.** Each of the four checks is a hard gate. If Clarity fails, that's the headline; don't bury it under three other "also could improve" notes. The severity hierarchy is non-negotiable.
- **Load-bearing claims only.** Grade only the claims that the recommendation rests on. The skill explicitly does NOT grade every fact in the doc. This keeps the rubric usable on long docs without spiralling.
- **Evidence tiers: Primary / Secondary / Interpreted.** Three named tiers with concrete definitions. Primary = named tool + date + denominator. Secondary = named person + context + date. Interpreted = synthesis, must be labelled. The tier system makes "this evidence is weak" auditable instead of a vibe call.
- **Four named failure patterns.** Each is a specific way evidence goes wrong: Secondary hardening (the citation chain that loses its source), enabled vs active (the most common adoption misread), ARR without ramp (the recognition-schedule miss), unlabelled inference (the assumption presented as fact). Naming the failure makes it instantly recognisable.
- **Right-reader check.** A doc can pass the rubric and still be wrong because it's going to the wrong audience. The topic-to-owner taxonomy catches default-to-the-user routing, the failure mode where every doc lands on the executive's desk because they're the safe escalation.
- **Tight output format.** Under 320 words, no tables, no nested lists, no em dashes. Verdict, what this is, what's working, data confidence, blocking issue, action, read time, audience flag, other fixes. The format is part of the discipline; sprawling triage output is its own failure.
- **Tone gate.** "Trusted colleague who has read this carefully and wants it to succeed, not a gatekeeper checking boxes." The skill is built to *help authors get docs through*, not to perform thoroughness.

## Related skills

- This skill produces ephemeral output (chat only); it doesn't write to the KB by default. If the user wants triages saved, they ask explicitly and the result goes to their personal channel.
- The four-check rubric is the foundation of Essay 03 (Synthesis discipline) when written; the rubric *is* the synthesis-discipline contract for incoming docs.

## When this skill matters most

This skill earns its place when:

- You're approving docs because the prose is good, and downstream the decisions don't hold up. The Evidence quality check forces the question: are the load-bearing claims actually grounded?
- Docs labelled "for discussion" keep landing as decisions in disguise. The Right-reader check catches that pattern explicitly.
- Your team's docs all start with throat-clearing and end with restatements. The LLM-tell scan names the specific patterns to cut.
- You're getting routed every doc by default. The topic-to-owner taxonomy makes the question "should this come to me at all?" first-class.

The four-check rubric is the asset. The automation just makes it cheap enough to run on every doc, not just the important-looking ones.
