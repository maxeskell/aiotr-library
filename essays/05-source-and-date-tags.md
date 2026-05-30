# Source-and-Date Tags

A figure has two ways of being wrong. It can be wrong when it is computed: the wrong denominator, the exposure quoted as opportunity, the inference dressed as a finding. The [synthesis-discipline rules](03-synthesis-discipline.md) exist to catch that kind. But a figure can also be perfectly right when it is computed and then go wrong by sitting still while the world moves. It was 80% in March. It is quoted as 80% in June. Nobody recomputed it; nobody had to do anything wrong. The number was true once and has been quietly stale ever since.

This second failure is sneakier than the first, because a stale figure looks exactly like a fresh one. There is no visual difference between a number pulled this morning and a number copied from a three-month-old report. The reader cannot tell, the author often cannot tell, and so the decision gets made on a figure whose age is invisible. The source-and-date tag is the discipline that makes age visible.

## The rule is one tag

Every numeric claim carries an inline tag naming the system it came from and the date it was pulled: `[Mixpanel, pulled 14 May 2026]`, `[Gainsight, pulled 14 May 2026]`. That is the whole rule. A number without a tag is not allowed to ship.

It looks like bureaucratic clutter and it is the opposite. The tag converts every figure from a bare assertion into a checkable claim. "Retention is 80%" asks the reader to trust it. "Retention is 80% [analytics tool, pulled 14 May 2026]" tells the reader precisely how much to trust it and exactly how to verify it: go to that system, pull it again, compare the dates. The tag is not decoration on the figure. It is the figure's receipt.

## Trust by default, earned by provenance

The deeper reason the tag matters is what it does to the default posture toward the system's output. A report full of untagged numbers forces a binary choice: trust all of it or audit all of it. Trusting all of it is how stale figures cause bad decisions. Auditing all of it is exhausting and so it does not happen: the reader skims, assumes it is fine, and you are back to trusting all of it.

A report where every figure carries its source and date changes the posture to trust-by-default-with-spot-checks. The reader trusts the report because every claim is independently checkable, and they spot-check the one or two figures the decision actually turns on. The tags make that spot-check cheap: they say where to look. The system earns standing trust not by being authoritative in tone but by being transparent about provenance, which is the only kind of trust that survives the first time someone catches a stale number.

## Pulled, not "as of"

The exact wording is deliberate, and the deliberate part is "pulled." The tag records the date the figure was *fetched from its source system*, not the date the underlying thing was true, and not the date the report was written. Those three dates come apart constantly. A report written on the 14th might quote a metric that was last refreshed in the source system on the 1st; "as of the 14th" would be a lie, "pulled 14 May" against a source that updates monthly tells the reader the figure could be up to a refresh-cycle old. The pull date is the honest one because it is the date you can actually stand behind: you know when you fetched it; you may not know how fresh the source itself was.

This is also why a fallback to cached or knowledge-base figures has to be tagged differently from a live pull. When a live source fails and a skill falls back to the last known value from the knowledge base, the tag has to say so, as in `[KB canonical, last refreshed 1 May 2026]`, not a fake live pull. The whole point collapses if a stale fallback can wear the costume of a fresh fetch. The tag's job is to never let the reader mistake one for the other.

## Forwarding a figure is where it rots

The tag earns its keep most at the moment a figure moves from one artefact to another. A number computed and tagged in this month's report gets quoted in next month's strategy doc, then in a board pre-read, then in a planning note. Each hop is a chance for the figure to lose its date and outlive its truth, and by the third hop nobody remembers it came from a system that has since been updated twice.

So the rule has a second clause: forward-citing a figure requires either a re-pull or an explicit, visible carry-forward. You may quote last month's number, but you must either fetch it again (in which case it gets today's pull date) or you must mark it as carried forward from a dated source, so the reader sees that it is a quotation of an old figure and not a fresh reading. What is banned is the silent forward: lifting the number, dropping the tag, and presenting a stale figure as if it were current. That silent forward is exactly how a figure that was right in March becomes a wrong decision in June.

This is enforced, not hoped for. The [authoring gate](04-a9-enforcement-gate.md) added a dedicated category for it after a figure was once forwarded into a fresh artefact stripped of its provenance: a synthesis skill that produces figures cannot ship without referencing the tag rule. And the blind-spots report runs a forward-citation scan that hunts for figures quoted without a current pull date, surfacing the stale ones before they reach a decision.

## Why this gets its own essay

Of all the rules in the system, this is the one that looks most like pedantry and is least like it. Every other discipline guards against a number being wrong in a way you could in principle catch by reading carefully. This one guards against a number being wrong in a way that is *invisible to careful reading*, because a stale figure and a fresh figure are textually identical. The only defence against an invisible failure is to make it visible, and the tag is what makes it visible. It is the smallest rule in the system and the one whose absence would quietly poison everything else, because a system whose figures might each be silently months old is a system whose every output you eventually learn to distrust.

The rule is defined canonically as A10 in [the Skill Output Standards](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/blob/main/drafts/project-instructions/skill-output-standards.example.md) and cross-referenced as SD-5 in the [synthesis-discipline](03-synthesis-discipline.md) contract; the two are the same rule seen from the figure side and the prose side. The forward-citation scan that catches silent forwards is in [blind-spots-report](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/personal-os/blind-spots-report/), and the package-time enforcement is in [skill-creator-a9](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/personal-os/skill-creator-a9/).
