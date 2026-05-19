# Contributing

Thanks for considering a contribution. This library is small and
opinionated — the bar is "would I want this in my own setup?" rather
than "is this technically correct?".

## Before you open a PR

1. **Check the bucket** — is this a skill, a schedule, or an
   instruction? See `CLAUDE.md` for definitions.
2. **Copy the right `_template/`** and fill it in.
3. **Use it for real, for at least a week.** If you haven't actually
   run it yourself, it's not ready.
4. **Redact** — see `CLAUDE.md` "Redaction rules". No real names, real
   emails, real Stripe IDs, real client workflows.
5. **Add an inventory entry** to the relevant bucket's `README.md`.

## What gets merged

✅ Solves a recurring problem
✅ Has a clear one-line description
✅ Has a usage example showing what good output looks like
✅ Works out of the box after copying (or has clear "set these variables" instructions)
✅ Has been used in real work, not just theory

❌ One-shot novelties or jokes
❌ Anything that depends on private data only you have access to
❌ Skills that wrap a single existing tool with no added value
❌ Things still under active iteration — keep iterating privately first

## PR format

Title: `<bucket>: add <name>` — e.g. `skills: add meeting-prep`

Body should answer:
- What problem does this solve?
- How long have you been using it?
- What's the simplest example of using it?

## Bug reports / improvements

Open an issue. Include:
- Which skill / schedule / instruction
- What you expected
- What happened instead
- Your Claude Code version (`claude --version`)

## License

By contributing, you agree your contribution will be licensed under
the [MIT License](./LICENSE).
