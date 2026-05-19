# schedules/

Claude Code `/loop` schedules — recurring prompts that run on a cadence.
Useful for monitoring, weekly reviews, polling for state changes.

## What is a schedule?

A schedule is just a prompt (or slash command invocation) that you
want to run automatically on an interval. Claude Code's `/loop` skill
takes an interval and a prompt and re-runs it.

Example:

```
/loop 1h /check-deploys
/loop 5m "scan inbox for anything from <important-client> and surface it"
```

This folder collects ready-made schedules worth running.

## Install

Schedules are just markdown — each folder documents what the schedule
does, what cadence to run it at, and what to expect.

To use one: read its README, then run the `/loop` invocation it
documents.

## Inventory

| Schedule | Cadence | What it does | Status |
| --- | --- | --- | --- |
| _(none yet)_ | | | |

## Writing a new schedule

1. Copy `_template/` to `schedules/<your-schedule-name>/`
2. Edit the README to document:
   - What it does
   - What cadence makes sense (and why)
   - What the output looks like
   - Any setup needed first (MCP servers, env vars, etc.)
3. Use it for a few weeks
4. Add it to the inventory table above
5. Open a PR
