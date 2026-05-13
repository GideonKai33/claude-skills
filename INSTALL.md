# Install prompt

This file is written *for Claude*, not for you. Paste this whole file (or just point Claude at it) and Claude will set up the skills in your local environment.

If you're a human reading this: open Claude Code in this repo's folder and say "Read INSTALL.md and set these skills up for me." That's all you need to do.

---

## Instructions for Claude

You are helping a user install the skills in this repository into their local Claude Code environment. Do the following:

### 1. List what's available

Run `ls skills/` to see which skills are in this repo. For each skill, read its `SKILL.md` and summarize in one sentence what it does. Show the user the list and ask which ones they want installed. Default to "all of them" if the user is unsure.

### 2. Verify the target

Confirm `~/.claude/skills/` exists. If not, create it. Don't overwrite existing skills with the same name without asking — if there's a collision, show the user both the existing skill's first few lines and the new one's first few lines, and let them choose.

### 3. Copy the skills

For each skill the user wants installed:

- Copy the entire skill folder from `skills/<name>/` to `~/.claude/skills/<name>/`
- Read the skill's `SKILL.md` and look for a "Setup notes" section near the end
- If there are setup steps (API keys, optional dependencies, etc.), walk the user through them. Ask before writing any keys or making any changes outside the skill folder itself
- If the skill mentions other skills it works better with (like `/vet` mentioning `/watch` and `/transcript`), tell the user those exist as separate skills and ask if they want pointers to install them

### 4. Check for CLAUDE.md

Most skills in this repo are dramatically more useful when Claude knows what the user is working on. Check whether `~/.claude/CLAUDE.md` exists, or whether there's a `CLAUDE.md` in the user's current working directory or any obvious project folder.

- **If a CLAUDE.md exists**, glance at it and confirm it has at least: who the user is, what they're working on, and any voice or style preferences. If it's thin, mention that fleshing it out will improve every skill in this collection.
- **If no CLAUDE.md exists**, offer to create one. Ask the user three questions: (1) what they do or what they're working on, (2) their active projects with a one-line description each, (3) anything they want Claude to know or avoid about how they collaborate. Write the answers into `~/.claude/CLAUDE.md`. Keep it short — 20-50 lines is plenty for a first pass.

Do not require this step. If the user declines, just tell them they can write a CLAUDE.md anytime and the skills will pick it up.

### 5. Confirm and stop

Tell the user which skills are now installed, how to trigger each one (the slash command), and that they may need to start a new Claude Code session for the new skills to be discoverable. Do not start using the skills yourself or run a demo. The install is the deliverable.

---

## Notes for Claude on tone

This is being installed by someone who may or may not be technical. Don't dump terminal output unless asked. Use plain language. Confirm before doing anything that writes files outside the install target. If anything fails, say so plainly rather than retrying silently.
