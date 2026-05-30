# aiotr-library: repo conventions

Public library of Claude Code skills, schedules, and instructions.
Anyone running Claude Code can pull from here.

## Relationship to other repos

- **`aiotr-os`** (private), Max's personal OS, where skills are
  *grown* in real engagements. Skills earn their way here.
- **`codeready-site`** (public, `aiotr.org`), the site that links to
  this library and tells the story of how the skills are used.

The **promotion path** is one-way: personal → public. Never the
reverse. Public templates that worked in real engagements get cleaned
of client specifics and submitted here.

## Layout

```
aiotr-library/
├── skills/         # Claude Code skills (one folder per skill, each with SKILL.md)
├── schedules/      # /loop schedules, recurring prompts
└── instructions/   # CLAUDE.md fragments, project-level instruction templates
```

Each bucket has a `_template/` showing the expected shape. New
contributions copy the template and fill it in.

## Redaction rules (non-negotiable)

Before anything lands in this repo, strip:

1. **Real client names** → use placeholders like `<Client>`, `acme-corp`, etc.
2. **Real personal data** → no real email addresses, phone numbers, calendar
   URLs, Slack channels, Stripe IDs, customer IDs
3. **Internal acronyms** that only make sense in Max's world
4. **File paths that leak machine layout** → use `~/code/...` or relative paths
5. **Prompts that reference specific clients' workflows** → generalize first

When in doubt, leave it out. The bar is: "would a stranger reading
this learn anything sensitive about me, my clients, or my pricing?"
If yes, redact.

## Contribution criteria

A skill, schedule, or instruction belongs here if:

- It solves a **recurring** problem, not a one-shot novelty
- It's been **used in anger** for at least a few weeks
- It's **redacted** per the rules above
- It has a **clear name** and a **one-line description** of what it does
- It includes a **usage example** showing what good output looks like

A skill does NOT belong here if it's still under active iteration.
Keep iterating in `aiotr-os/personal/skills/` first.

## File conventions

- Folder names: kebab-case (`meeting-prep`, not `meetingPrep` or `Meeting Prep`)
- Skill folder must contain `SKILL.md`
- Schedule and instruction folders contain `README.md`
- Markdown only at the top level of each artifact; supporting scripts
  can live in subfolders (`scripts/`, `templates/`) if needed

## Workflow for promoting a skill from aiotr-os

```bash
# In aiotr-os: copy the skill to a scratch location
cp -r personal/skills/<skill-name> /tmp/<skill-name>-public

# Edit /tmp/<skill-name>-public/SKILL.md: redact per rules above, AND bring it
# into voice compliance (no em-dashes, no banned phrases or vocabulary). Source
# skills grown in aiotr-os / the reference-design repo use em-dashes freely; this
# repo does not, so the voice pass is a required promotion step, not optional.

# Drop the cleaned version into this repo
cp -r /tmp/<skill-name>-public aiotr-library/skills/<skill-name>

# Add an entry to skills/README.md inventory table

# Open a PR
```

Eventually a `promote-skill` skill should automate this. PRs welcome.

## Docs hygiene

When you add or change a skill, schedule, or instruction, update the
inventory table in the relevant bucket's `README.md` and (if the change
affects the repo's overall shape) this `CLAUDE.md` plus the top-level
`README.md`, in the same commit. Stale inventory tables are the most
common doc rot here.

## Voice and tone rules

These apply to every word that ships in this repo: SKILL.md files,
templates, READMEs, commit messages, PR bodies. Also to chat replies
in sessions working in this codebase.

**No em-dashes. Full stop.** If you reach for one, rewrite with a
comma, semicolon, colon, or a fresh sentence. Replace existing em-dashes
when you touch nearby code.

**Banned phrases** (do not output, in any context):

> "let me", "I'll", "I will", "happy to help", "great question",
> "absolutely", "certainly", "of course", "in conclusion",
> "to summarise" / "to summarize", "in summary", "I hope this helps",
> "feel free to", "don't hesitate", "it's important to note",
> "please note", "kindly", "I think", "I believe", "perhaps", "maybe",
> "somewhat", "very", "really", "quite", "extremely",
> "let's dive in", "let's get started", "moving on", "as an AI",
> "as a language model", "I'm just".

**Banned vocabulary**:

> delve, navigate, leverage, utilise / utilize, robust, harness,
> unlock, unleash, realm, tapestry, underscore, showcase, elevate,
> embark, synergy, streamline, multifaceted, holistic, intricate,
> comprehensive, seamlessly, meticulous, endeavor / endeavour.

Avoid hedging, marketing register, AI-narration, and filler intensifiers.
State things directly.
