# skills/

Claude Code skills. One folder per skill, each containing a `SKILL.md`.

## Install

```bash
# Single skill
cp -r skills/<skill-name>/ ~/.claude/skills/

# All of them
rsync -av skills/ ~/.claude/skills/ --exclude='_template' --exclude='README.md'
```

Restart Claude Code (or open a new session) and the skill will be
available via `/<skill-name>` or by description-match.

## Inventory

Each skill below is a complete, working `SKILL.md` with a worked example built on fictional
data (a made-up company, Acme, on an illustrative stack: Notion, Granola, Slack, Jira,
Mixpanel). Read the example to see the skill run, then swap the named tools for your own.
The patterns travel; the specific tool names are illustrative, not requirements. Each skill's
`README.md` explains the design patterns behind it.

| Skill | What it does | Status |
| --- | --- | --- |
| [`decision-log`](./decision-log) | Capture and review significant decisions with their *why*, so they don't get re-litigated. | Working + example |
| [`doc-triage`](./doc-triage) | Four-check rubric (Clarity, Evidence, Owner/next step, LLM-tell) for whether a doc is ready to circulate. | Working + example |
| [`end-of-day-review`](./end-of-day-review) | Close out the workday in ~10 min: what mattered, what's owed, three things for tomorrow. | Working + example |
| [`weekly-self-reflection`](./weekly-self-reflection) | Evidence-based weekly review that also extracts decisions and proposes KB updates. | Working + example |
| [`meeting-prep`](./meeting-prep) | Pull live context into a tight briefing before any named meeting. | Working + example |

## Skill folder shape

Each skill is a folder with at least a `SKILL.md`. See `_template/`
for the expected structure:

```
skills/<skill-name>/
├── SKILL.md           # Required. Frontmatter + instructions.
├── scripts/           # Optional. Any executable helpers the skill calls.
└── templates/         # Optional. Any boilerplate the skill writes out.
```

## Writing a new skill

1. Copy `_template/` to `skills/<your-skill-name>/`
2. Edit `SKILL.md`:
   - Frontmatter: `name`, `description`, `model` (optional)
   - Body: when to use it, what it does, how it behaves on edge cases
3. Use it locally for at least a week
4. Add it to the inventory table above
5. Open a PR
