# instructions/

`CLAUDE.md` fragments and project-level instruction templates. Drop
these into a repo's `CLAUDE.md` (or a `.claude/` directory) to give
Claude Code project-specific conventions.

## When to use an instruction template

A few patterns recur across projects:

- Privacy boundaries (don't push X to public remote Y)
- Code style guardrails (no comments unless the why is non-obvious)
- Tool-use conventions (always run X before pushing)
- Domain vocabulary (project-specific names, acronyms)

Rather than re-write these from scratch each time, copy a known-good
fragment.

## Install

Each template is markdown. Open the one you want, copy the relevant
section, paste it into your `CLAUDE.md`.

## Inventory

| Template | What it covers | Status |
| --- | --- | --- |
| _(none yet)_ | | |

## Writing a new instruction template

1. Copy `_template/` to `instructions/<your-template-name>/`
2. Edit the README:
   - What problem this solves
   - When to use it (and when NOT to)
   - The actual fragment, in a fenced code block
3. Use it on at least one real project
4. Add it to the inventory table above
5. Open a PR
