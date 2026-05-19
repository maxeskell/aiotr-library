# schedule-name

One sentence, what this schedule monitors or does on a recurring basis.

## Cadence

Recommended interval: `<5m | 1h | 1d | weekly | ...>`

Why this cadence: ...

## Setup

What needs to exist before running this:

- [ ] MCP server `<name>` configured
- [ ] Env var `<NAME>` set
- [ ] You're in a `<repo-name>` workspace (or wherever it expects to run)

## Invocation

```
/loop <interval> <prompt-or-command>
```

Example:

```
/loop 1h "scan for X, surface anything matching Y"
```

## What it does

Step-by-step what each tick does:

1. ...
2. ...
3. ...

## Output

What you'll see on each tick. Be concrete.

```
Example output here.
```

## When to stop the loop

- When you no longer care about the signal
- When the inputs have changed enough that the prompt is wrong
- Use `/loop stop` or kill the session

## Why this exists

One or two lines.
