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

| Skill | What it does | Status |
| --- | --- | --- |
| _(none yet — promotions coming from `aiotr-os/personal/skills/`)_ | | |

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
