# The A9 Enforcement Gate

Every discipline in this system is a rule someone has to follow. The denominator rule, the canonical-home rule, the no-restated-tone rule, the tag-every-figure rule: each one is a sentence that says "always do this" or "never do that." And every one of those rules has the same weakness: it depends on the author remembering it at the moment they write the skill. The author is a person, it is late, they have written the same boilerplate eleven times this month, and the twelfth time they paste the tone paragraph back in because pasting is faster than linking. The rule was sound. The discipline failed.

The A9 gate is the answer to that weakness. It is the rule that says: the other rules cannot be held by discipline alone, so they get held by a check that runs whether or not anyone remembers it.

## Discipline degrades; a gate does not

The premise is uncomfortable and worth stating plainly. A skill-authoring rule that lives only in your head, or only in a style document, will be violated, and not because authors are careless. It will be violated because the cost of following it is paid now, by the author, in the tedium of linking instead of restating, while the cost of breaking it is paid later, by someone else, when two notes disagree or a report ships with unbounded output. Costs that land on someone else, later, do not get paid. That is not a moral failing; it is the structure of the incentive.

So you do not fix it with a reminder. Reminders are just more discipline, and discipline is the thing that already failed. You fix it by moving the check to a place that always runs, and making it refuse to proceed until it passes.

## Wire it into the unavoidable step

Authoring a skill ends in one unavoidable action: packaging it so it can be installed. You cannot ship a skill without packaging it. That makes packaging the perfect place to put the gate, because a check wired into a step you cannot skip is a check that runs every time, with zero discipline required.

So the gate (`a9_check.py` in this system) runs *inside* the packaging script. You go to package the skill, the check runs first, and if it fails the package is not produced. There is no "remember to run the linter" because there is nothing to remember; the only path to a shipped skill goes through the check. "A9" is just the axiom number in the project instructions that mandates this: skill-authoring quality is enforced by another skill, not by hope.

This is the same move as the package-time half of the canonical-home rule (see [The Canonical-Home Rule](02-canonical-home-rule.md)). The pattern generalises: find the step that always happens, and attach the check to it.

## A ledger of failure modes, not a style guide

What the gate checks is the interesting part, because it is not a generic linter. It is a ledger of the specific ways *this system's* skills have actually drifted, each one earned the hard way. In this instance there are a handful of categories, and the list is worth seeing not for the contents but for the discipline behind it:

restated tone rules, which belong in the operating manual and not in every skill; restated output-format templates, which belong in the output-standards note; hardcoded note and channel IDs, which should resolve from the registry at runtime rather than being frozen into the skill; restated schedule logic, which belongs in the schedule registry; missing cross-references to the synthesis-discipline and readability rules on skills that produce synthesis; missing source-and-date-tag references on skills that forward figures; and, across the whole bundle rather than one skill, two skills declaring a write to the same canonical note without being on the allowlist.

The rule for what becomes a category is the part to copy. **A pattern becomes a category the second time it bites.** The first time a drift slips through, you fix it and move on; it might be a one-off. The second time the same shape of mistake appears, it is no longer bad luck, it is a recurring failure mode, and a recurring failure mode gets a permanent automated check so it cannot bite a third time. The synthesis cross-reference check was added after two skills shipped without it and produced unbounded output. The figure-tag check was added after a number got forwarded into a fresh report stripped of its provenance. The multi-writer check was added after two skills were found writing to the same canonical landing note. Each one is a scar with a check grown over it.

## Stable numbering is part of the contract

A small detail carries more weight than it looks. When a category is retired (because the rule it enforced moved elsewhere) its number is not reused, and the remaining categories are not renumbered. A retired category leaves a gap, and new categories take the next free number rather than filling the gap.

The reason is that the category numbers are referenced elsewhere: in changelogs, in commit messages, in the cross-references other skills carry. Renumbering to close a gap would silently break every historical reference, turning "this was caught by the schedule-restatement check" into a pointer at the wrong rule. A stable identifier that survives the thing it names being retired is the same instinct as the audit trailer and the breadcrumb log: the system values being able to trace its own history over being tidy. Gaps in the numbering are not mess; they are honesty about what used to be there.

## The bundle-wide check is the one that needs a gate most

Most of the checks look at the single skill being packaged. One looks across every installed skill at once: the collision check that asks whether two skills both declare a write to the same canonical note. This is the check that *cannot* be done by a careful author, because no individual author can see the whole bundle. The person writing skill number twelve has no way to know that skill number four, written months ago by a past version of themselves, already owns the note they are about to write to. Only a check with the whole installed set in view can catch it. This is the clearest case for the gate: not that discipline is unreliable, but that the information needed to follow the rule is not available to the person at the moment they would need it. The gate has the bundle; the author does not.

## Why a gate is the keystone of self-improvement

A personal AI operating system that you keep extending has a quiet structural risk: every new skill is a chance to reintroduce a mistake you already fixed. Without a gate, the quality of the system is a decaying average: it drifts back toward the failure modes every time you are tired. With a gate, the floor holds. The worst skill you can ship is bounded by what the gate enforces, regardless of the hour or your attention. That is the whole point of building the check as a skill that polices the other skills: it makes the system's quality a property of the system, not of your discipline on any given night.

The worked example is in [skill-creator-a9](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/personal-os/skill-creator-a9/), a thin override on the standard skill-creator that wires the check into packaging. The rules it enforces are defined in [the Skill Output Standards](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/blob/main/drafts/project-instructions/skill-output-standards.example.md); the runtime collision scan that complements the package-time check lives in [weekly-system-review](https://github.com/maxeskell/maxeskell-personal-ai-operating-system/tree/main/drafts/skills/personal-os/weekly-system-review/). The gate is what lets the rest of the system grow without rotting.
