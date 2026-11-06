# Skills

Agent skills for Claude Code, GitHub Copilot and other agents that support the [Agent Skills](https://skills.sh) format.

## Install

```bash
npx skills add moelzanaty3/skills
```

Or copy a folder from `skills/` into one of:

| Agent | Personal | Per project |
|---|---|---|
| Claude Code | `~/.claude/skills/` | `.claude/skills/` |
| GitHub Copilot | `~/.copilot/skills/` (also reads `~/.claude/skills/`) | `.github/skills/` |

## Skills

### connect-the-dots

A visual learning companion. Give it a document, design, image, Epic, code or any topic and it turns it into an interactive lesson you can follow and come back to.

- **Map first** — the whole thing in plain English, a small concept map with labelled arrows, and a route ordered by what you need to know first.
- **Keeps your place** — every step says what you're learning, where you are and why it matters. Say "lost", "simpler", "example", "why?", "go deeper", "back to the map" or "save my place" at any point.
- **Misses nothing** — keeps a ledger of every item in the source and shows where each one is taught, skipped or unreadable.
- **Interactive page** — builds a lesson page with a clickable map, per-step layers (Simpler / Example / Why? / Go deeper), a coverage tab and a printable revision sheet. Published as an artifact in Claude; saved as a local `.html` file to open in a browser in Copilot and other agents.

Try it:

```
Connect the dots on this HLD for me
Explain this Epic simply, I present it on Monday
```

## License

[MIT](LICENSE)
