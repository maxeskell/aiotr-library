# aiotr-library

A public collection of [Claude Code](https://claude.com/claude-code)
skills, schedules, and instructions, maintained by
[Max Eskell](https://aiotr.org) and open to the community.

Everything here is vetted, redacted, and meant to be useful out of the
box, copy a folder, drop it in your config, run it.

## What's in here

| Folder | What it is |
| --- | --- |
| [`skills/`](./skills) | Claude Code skills, drop into `~/.claude/skills/` to install |
| [`schedules/`](./schedules) | `/loop` schedules, recurring prompts you can run on a cadence |
| [`instructions/`](./instructions) | `CLAUDE.md` fragments and project instruction templates |

Each folder has its own README with install instructions and an
inventory of what's available.

## Quick install (skills)

```bash
# Clone the library somewhere you'll remember
git clone https://github.com/maxeskell/aiotr-library.git ~/code/aiotr-library

# Copy a single skill into your Claude Code skills dir
cp -r ~/code/aiotr-library/skills/<skill-name>/ ~/.claude/skills/

# Or rsync everything (overwrites, careful)
rsync -av ~/code/aiotr-library/skills/ ~/.claude/skills/
```

## Companion projects

This repo is the public half of a larger setup:

- [`aiotr.org`](https://aiotr.org), the marketing site, with writeups
  about how these skills get used in real engagements
- `aiotr-os` (private), the personal operating system this library is
  promoted *from*. Skills earn their place here after being useful in
  real work for a few weeks.

## Contributing

PRs welcome. See [`CONTRIBUTING.md`](./CONTRIBUTING.md) for the bar, TL;DR:
the skill should solve a real, recurring problem, not be a one-shot novelty.

## License

[MIT](./LICENSE), use, fork, modify, redistribute freely.
