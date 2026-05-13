# claude-skills

A small collection of reusable [Claude Code](https://docs.claude.com/en/docs/claude-code) skills, written to be dropped into anyone's setup without hardcoded personal context. Each skill reads your own `CLAUDE.md` and projects folder to personalize itself, so you get skill behavior that talks about *your* work instead of mine.

## What's in here

- **`vet`** — Evaluate AI workflows, tools, or business pitches you've seen on social media. Pulls the source (video link, blog, plain text), strips the hype, and gives an honest read on whether it fits your actual situation or would just add complexity. Biased toward protecting against shiny-object syndrome, but engages genuinely when something is a real fit. Reactive: "I saw this thing, should I do it?"

- **`ai-audit`** — Help you figure out where AI would actually earn its place in your work. Interviews you about your normal week, surfaces the manual chains and recurring tasks worth automating, and outputs a short prioritized shortlist with one concrete first move you can take this week. Proactive counterpart to `/vet`: "Where could AI actually help me?"

- **`spec-build`** — Turn a software, automation, or tool project into an agent-runnable build plan. Walks a five-question Discovery Brief dialogue, decomposes the work into atomic 5-15 minute phases with observable acceptance criteria, and saves `discovery-brief.md` and `phases.md` so Claude Code can run the build one phase at a time. For software/tool projects only — not for writing or relational work.

More skills will land here over time.

## How to install

Two options, depending on whether you want Claude to do the setup for you or you'd rather copy files manually.

### Easy mode (let Claude do it)

1. Clone or download this repo somewhere on your machine.
2. Open Claude Code (`claude` in your terminal, or via the desktop app) in the repo folder.
3. Tell Claude: **"Read INSTALL.md and set these skills up for me."**

Claude will copy the skills into `~/.claude/skills/`, ask any clarifying questions, and confirm when each one is ready. You can then use them in any Claude Code session by typing `/vet`, `/<skill-name>`, etc.

### Manual mode

1. Clone or download this repo.
2. Copy each folder from `skills/` into `~/.claude/skills/`. For example:
   ```
   cp -r skills/vet ~/.claude/skills/
   ```
3. Restart Claude Code (or start a new session) so it picks up the new skill.
4. Trigger the skill by typing `/<skill-name>` in any Claude Code session.

## What makes a skill work better

Most of these skills are dramatically more useful when Claude knows what you're working on. The cheapest way to give Claude that context is to create a `CLAUDE.md` file. It can live in a few places:

- `~/.claude/CLAUDE.md` — applies to every session
- Project root — applies when Claude Code is open in that folder
- Any folder Claude can reach via working directory

A useful `CLAUDE.md` answers, at minimum:

- Who you are and what you're working on
- Your active projects and where each one stands
- Anything you want Claude to know or avoid about how you collaborate

You don't have to write one to use these skills. They'll fall back to asking you a quick question if there's nothing to read. But the quality of the output climbs a lot when there's real context to reference.

## Updating

To pull updates as new skills land:

```
cd /path/to/claude-skills
git pull
```

Then re-run the install (easy or manual) for any skill you want to refresh.

## License

MIT. Use it, fork it, share it, modify it. If you build something interesting on top, I'd love to see it.
