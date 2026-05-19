---
name: skill-name
description: One sentence, what this skill does, when to use it. Claude reads this to decide whether to invoke. Be specific about triggers.
---

# skill-name

One paragraph: the problem this solves, in plain terms. Not the
mechanics, the user-facing outcome.

## When to use this skill

Concrete triggers, written so Claude can match them:

- When the user says "X" or asks about Y
- When working in `<some-path>` and they want to Z
- NOT when ... (counter-examples are often more useful than examples)

## What it does

Step-by-step. Each step should be something Claude can actually do
with its tools.

1. Read X
2. Check Y
3. Write Z to `<location>`
4. Report back with <specific output>

## Inputs

- `<arg>` (optional), what it's for, what happens if omitted

## Output

What the user sees when this skill finishes. Be concrete, paste
a real example if you have one.

```
Example output here.
```

## Edge cases / gotchas

- If X doesn't exist, do Y (don't fail silently)
- If the user asks mid-flow to stop, stop and don't push to a remote
- Never do Z (one-way destructive actions, etc.)

## Why this exists

One or two lines. Helps future you (and contributors) remember the
intent when the implementation drifts.
