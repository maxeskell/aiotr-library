---
name: ship
description: >
  One consistent way to land a change: review the diff, update the docs that the change
  touches, commit with a conventional message, push to the working branch, and ensure a pull
  request exists. The same steps run identically in Claude Code on the web and the Claude CLI.
  Use when you are ready to commit and ship work: "ship this", "ship it", "commit and push",
  "open a PR for this", "land this change", "wrap up and push". Merging is handed off to the
  auto-merge workflow, not done here.
---

# ship

> Portable dev-workflow skill. The canonical template and worked example live in the
> `maxeskell-personal-ai-operating-system` repo at `drafts/skills/personal-os/ship/`. This repo
> carries the loadable copy.

## What this does

Turns "I am done, get this in" into one repeatable sequence, so the commit, push, PR, and docs
steps happen the same way every time, wherever you run them. It does not merge: owner PRs are
merged by the auto-merge workflow (see Integration).

The rule that keeps it consistent: documentation the change touches is updated as part of
shipping, not afterwards. Drift happens when docs are a separate, skippable step. Here it sits
inside the loop.

## Tooling: web vs CLI

The one thing that differs between environments, papered over here:

- PR and merge-status operations use the `gh` CLI when it is available (`command -v gh`),
  present in the Claude CLI. On the web there is no `gh`; use the GitHub tools (`mcp__github__*`)
  instead. Everything else (git add/commit/push, editing files) is identical in both.

Pick the available path at Step 5; the other steps do not care which you used.

## Step 0: Preflight, branch safety

1. Get the current branch. If it is `main` or `master`, do not commit here. Create a working
   branch first: `git checkout -b claude/<short-descriptor>` (kebab-case, from the change).
   Never push to the default branch directly.
2. `git status --short`, `git diff`, and `git diff --staged` to see the full change. If there
   is nothing to ship, say so and stop.

## Step 1: Understand the change

Read the diff and group it into one logical change (or note if it should split into separate
commits). Derive the change type (feat/fix/docs/refactor/chore/ci), a one-line summary, and
which docs the change touches.

## Step 2: Update the docs the change touches (required)

Before committing, bring affected docs in line with the change:

- `CHANGELOG.md` if the repo keeps one: add a bullet under the unreleased section in the right
  subsection (Added / Changed / Fixed / Removed). User-facing changes always get an entry.
- README or other docs: update any section the change makes stale.

If the change genuinely touches no documentation, say so rather than skipping silently.

## Step 3: Commit

Stage and commit with a Conventional Commits message: `type(optional-scope): summary`, with a
short body explaining the why when it is not obvious. Include the Step 2 doc updates in the same
commit as the change they describe. Append the session trailer your environment requires.

## Step 4: Push

`git push -u origin <branch>`. On network failure, retry up to 4 times with exponential backoff
(2s, 4s, 8s, 16s). Do not retry non-network failures such as a rejected push; surface those.

## Step 5: Ensure a PR exists

Check for an open PR for this branch; create one if there is none:

- CLI: `gh pr view <branch>`, then `gh pr create --base main --head <branch> ...`
- Web: `mcp__github__pull_request_read` to check, then `mcp__github__create_pull_request`.

PR body: what changed and why, plus what docs were updated. Open it non-draft when the change is
complete (the auto-merge workflow acts on non-draft owner PRs); open as draft if it is still in
progress. If a PR already exists, update its body if scope changed; do not open a duplicate.

## Step 6: Hand off

The auto-merge workflow (`.github/workflows/auto-merge.yml`) lands owner PRs once ready. Do not
merge by hand, so there is a single merge path. Exception: the PR that installs the workflow, or
an explicit request to merge now.

## Step 7: Report

Give a three to four line summary: branch, commit subject, PR URL, docs updated, and merge
status. Do not narrate intermediate steps.

## Key rules

1. Docs are part of the loop, not after it. Step 2 is required.
2. Never commit or push to the default branch. Always a `claude/<descriptor>` branch.
3. Conventional commit messages, with the why in the body when not self-evident.
4. Idempotent. Safe to re-run: detect an existing PR rather than opening a second.
5. One merge path. The workflow merges; this skill does not.
6. Same steps everywhere. The only web-vs-CLI difference is the PR tooling in Step 5.

## Integration

Hands off to `.github/workflows/auto-merge.yml`, the workflow that merges owner-authored,
non-draft PRs once mergeable. See the "Merging PRs" section in `CLAUDE.md`. `ship` is the
closing move once a change is ready.

## Output format

This skill performs git and GitHub actions and ends with a short status summary. It does not
write a KB report, so a KB output-standards header does not apply.
