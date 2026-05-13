---
name: spec-build
description: Use this skill when the user wants to turn a software, automation, or tool project into an agent-runnable build plan. Walks through a Discovery Brief dialogue (problem, user, out of scope, stack, done), then decomposes the work into atomic phases of roughly 5-15 minutes each with observable acceptance criteria and explicit per-phase non-goals. Produces two artifacts: `discovery-brief.md` and `phases.md` in the project folder. Triggers include "/spec-build", "phase out this project", "turn this into a build plan", "make this agent-runnable". Do NOT use for writing projects (book chapters, essays), relational or business projects, or vague ideas that haven't been promoted to a real project yet — those need different treatment.
---

# /spec-build — Spec-Driven Build

Turn a software or tool project into an agent-runnable build plan. Two artifacts come out of this: `discovery-brief.md` (the one-page upstream doc) and `phases.md` (the atomic phase breakdown). The goal is a plan that's small enough at each step to actually finish, with verifiable acceptance criteria so you know when each phase is done.

## When to use

- The project is a software build, automation, tool, or technical pipeline
- The plan is to hand significant chunks of the work to Claude Code (or another agent)
- A project folder or at least a clear scope exists

## When NOT to use

- Writing projects (book chapters, essays, scripts). Those need flow, not phase gates.
- Relational or business projects (launching a service org, recruiting clients). Wrong fit for a bounded-scope build framework.
- A vague idea that hasn't been scoped at all yet. The user needs to think the project through first; this skill produces a plan, not the idea.

If the request is outside this scope, decline gracefully and explain why.

## Inputs

The project name or folder path. If not specified, ask the user where the project lives or whether they want the artifacts created in the current working directory.

## Workflow

### Step 1: Read what exists

Read whatever project context already exists: a project README, a CLAUDE.md, any existing planning notes. Get full context before asking questions. If there's nothing yet, that's fine — proceed to Step 2 and gather context through the dialogue.

**Check for existing spec artifacts.** If `discovery-brief.md` or `phases.md` already exist in the target folder, ask whether to redo from scratch, refine in place, or skip a step. Never silently overwrite.

### Step 2: Fit check

Ask one question:

> "Spec-Driven Build is tuned for software, automation, and tool projects with bounded scope and an agent-built implementation. Does that match what you're doing here?"

If the answer is no or unclear, bail gracefully and suggest the user think the scope through first, or pick a different planning approach.

### Step 3: Discovery Brief dialogue

Ask these five questions, one at a time. Don't push if the user doesn't have an answer. Capture what they do have and flag the gap with a `<!-- TODO -->` comment in the brief so it doesn't get lost.

1. **What's the problem?** One paragraph. What is broken or missing.
2. **Who's the user?** The person who will use the thing once it's built.
3. **What's explicitly out of scope?** Multiple items. These are the v1 non-goals. The clearer this is, the less the agent invents.
4. **Stack constraints?** Languages, frameworks, hosting, decisions already made.
5. **What does done look like?** Concrete and observable. Something verifiable in 30 seconds when walking back into the project after time away.

### Step 4: Write `discovery-brief.md`

Save to the target folder using this template:

```markdown
# [Project Name] — Discovery Brief

The one-page upstream doc for the Spec-Driven Build of this project.

## Problem
[From Q1]

## User
[From Q2]

## Out of scope (v1)
- [From Q3]

## Stack constraints
- [From Q4]

## Done looks like
[From Q5]
```

### Step 5: Phase decomposition dialogue

Walk the user through phasing in two passes:

**Pass 1 — Stages.** What are the high-level stages of work? Group related concerns together. Examples: "Pipeline core," "Frontend + viewer," "Polish + first real run."

**Pass 2 — Atomic phases per stage.** For each stage, decompose into phases of roughly 5 to 15 minutes of agent work. For each phase, capture:

- **Goal:** one sentence
- **Acceptance:** observable criterion the user can verify
- **Out of scope for this phase:** specific things not to touch in this phase

If a phase feels bigger than 15 minutes of agent work, split it. The atomic phase is the unit that prevents the agent from drifting; the more discipline here, the better the build runs.

If the user already has a phased plan from earlier work, use it as the starting point and decompose any phase that's too big into atomic chunks.

### Step 6: Write `phases.md`

Save to the target folder using this template:

```markdown
# [Project Name] — Phases

Agent-runnable phase breakdown. Each phase is roughly 5 to 15 minutes of agent work, has a testable acceptance, and explicit non-goals.

**Workflow rule:** stop at the end of each phase, verify acceptance personally, commit, then unlock the next. If a phase reveals a wrong assumption, update the Discovery Brief or this Phases doc *before* continuing.

## Stage 1 — [Name]

### 1.1 — [Phase name]
**Goal:** [one sentence]
**Acceptance:** [observable criterion]
**Out of scope for this phase:** [specific things not to touch]

### 1.2 — [Phase name]
[etc.]

## Stage 2 — [Name]
[etc.]
```

### Step 7: Confirm

Tell the user:

- The two new artifacts that exist now (`discovery-brief.md` and `phases.md`) and where they live
- That Phase 1.1 is ready to run in a fresh Claude Code session pointed at the spec docs
- The suggested workflow: open Claude Code in the project folder, tell it "Read the Discovery Brief and Phases doc. Run Phase 1.1 only. Stop after acceptance is met."

## Notes

- **Living-contract rule applies.** If work later reveals the Discovery Brief or Phases doc got something wrong, update the doc *first*, then continue. Don't flag for later. The drift compounds silently and the doc becomes fiction.
- The atomic-phase discipline matters more than the document format. If the user prefers different filenames or structure, the underlying framework still applies.

## Setup notes (first run)

No API keys, no accounts, no configuration required. Drop this skill into `~/.claude/skills/spec-build/SKILL.md` and use it.

For best results, run this from inside an existing project folder (or a folder where you want the spec artifacts saved). If you're starting from a fresh idea, scope it loosely on paper first — even a one-paragraph summary helps the Discovery Brief dialogue land.
