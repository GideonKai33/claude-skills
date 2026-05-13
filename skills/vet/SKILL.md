---
name: vet
description: Use this skill when the user invokes `/vet <link or description>` to evaluate an AI workflow, AI tool, or online business strategy they've seen pitched somewhere (Instagram, TikTok, YouTube, blog post, tweet, or just described in text). The skill digests the source, summarizes what's actually being proposed, and gives an honest read on whether it fits the user's actual situation or would just add complexity. Do NOT use for general "what do you think of X" questions outside the slash command — this is specifically a vetting pipeline that biases toward protecting against shiny-object syndrome.
---

# /vet — Workflow & Tool Vetting

Quickly evaluate whether an AI workflow, AI tool, or online business strategy is worth pursuing for this user, or whether it would just add unnecessary complexity. The job is to protect against shiny-object syndrome while still genuinely engaging when something is actually good.


## When to use

User invokes `/vet <input>` where input can be:
- A video link (TikTok, Instagram Reel, YouTube, etc.) — pitches an AI tool, workflow, or business strategy
- A non-video link (blog post, tweet, threads post, landing page) — same content, different format
- Plain text describing what someone's pitching ("a guy on YouTube was saying you should run every email through Claude before reading it")
- Any of the above plus a framing note ("specifically curious whether this would help with my Etsy shop")


## Bias

**Default skeptical, not cynical.** Most pitched AI workflows and tools fail one of three tests:
- They solve a problem the user doesn't actually have
- They duplicate something already in the user's stack
- They add ongoing maintenance cost that exceeds the value delivered

So the lens is: *Does this earn its place in the existing stack, or is it one more thing to maintain?* New tools and workflows have to clear a real bar — solving an actual current bottleneck, fitting an active project, or replacing something the user is already doing manually. Novelty alone is not the bar.

That said, when something genuinely is a fit (rare but real), say so directly. Don't reflexively dismiss. The point is honest evaluation, not blanket skepticism.


## Workflow

### Step 1 — Digest the source

- **If input is a video link**, use the `/watch` skill (if installed) to pull frames and transcript. If `/watch` isn't available, fall back to `/transcript` (if installed) for captions only. If neither is available, ask the user to paste a description or transcript.
- **If input is a non-video link**, use `WebFetch` to read it. If it's behind a paywall or login, tell the user and ask if they can paste the content directly.
- **If input is plain text**, work from the description as given. Ask one clarifying question only if the description is genuinely ambiguous about what the tool/workflow actually does.

Read for what's *actually* being proposed, not the marketing wrapper. Strip out the hype, the "this changed my life" framing, the affiliate-link energy. What's the literal mechanic?

### Step 1.5 — Hydrate the user's context

Before writing the fit check, learn what the user is actually working on. In order of preference:

1. **If the user has a `CLAUDE.md`** in the current working directory or in a parent directory, read it. Pull out: their active projects, role, stated goals, what they've said they want help with, any patterns they've flagged about themselves.
2. **If they have a projects folder** (commonly `~/projects/`, `~/Code/`, `~/Documents/<vault>/03 Projects/`, or similar), list it. Scan filenames for anything that might be a trigger surface for whatever's being vetted. Read any project file whose name plausibly relates to the pitch.
3. **If no CLAUDE.md and no projects folder**, ask once: "Quick context check — what are you actually working on right now? Even a one-liner per active thing helps me judge fit." Then reference their answer throughout.

The cost of skipping this step is a fit check that talks about "your workflow" in the abstract instead of naming the specific project where this would or wouldn't land. That makes the vet useless.

### Step 2 — Output the evaluation

Use exactly this structure. Section headers are bold, not headings. Keep each section tight — this is a quick read, not a report.

```
**What it is**
[3-5 sentences. Plain-language description of the actual workflow, tool, or strategy.
What inputs go in, what comes out, what's automated vs manual, what platforms or services
it touches. No marketing language.]

**The claim**
[2-3 sentences. What the source promises this will do for the user (time saved, revenue
unlocked, capability gained). Stated plainly, with the implicit assumptions surfaced.]

**Fit check**
[3-6 sentences. Honest read against the user's actual situation as learned in Step 1.5.
Reference their real projects and tools by name. Address:
- Does this solve a current bottleneck, or invent a new one?
- Does it overlap with something already in their stack?
- Does it conflict with patterns they've flagged about themselves (overcommitment,
  conceptual-mapping-over-shipping, perfectionism, etc.)?
- Are there hidden costs (maintenance, API fees, learning curve, context-switching
  overhead) the source glossed over?]

**Implementation difficulty**
[2-4 sentences. Realistic assessment of what it actually takes to set up and maintain.
Call out the friction points — API keys to obtain, accounts to create, prompts to tune,
ongoing upkeep. If it requires something the user doesn't have (specific data, audience
size, existing workflow to plug into), say so. Use a rough scale: trivial / a weekend /
a real project / not realistic right now.]

**Where to start**
[2-4 sentences. The smallest possible first step that would tell them whether this is
worth committing to — not the full build. Something concrete they could do in 30 minutes
to an hour, ideally without creating new accounts or installing new infrastructure. If
the recommendation is to skip, this section becomes "Why not": one sentence on what
would have to change for this to be worth revisiting.]

**If you do it anyway** *(only include when recommendation is Skip or Park)*
[2-3 sentences. The strongest application angle assuming the user proceeds despite the
recommendation. Name the specific project or surface where this would land best, and
the narrower scope that would actually be useful. Keep the honest caveat in the same
breath — that it's still real work, that the cost-to-payoff ratio still doesn't favor
it today, what the lurking maintenance or upkeep tax looks like. The point is to give
a sane on-ramp if the user's energy actually pulls them there, not to soften the
recommendation. No cheerleading.]

**Recommendation**
[One line. Pick one:
- "Pursue it" — clear fit, real bottleneck, low risk of bloat
- "Pursue a narrower version" — the underlying idea is good but the pitched scope is too
  much; specify the narrower cut
- "Skip" — doesn't fit, duplicates existing capability, or would add maintenance burden
  without proportional value
- "Park it" — interesting but premature given current dependencies/capacity; specify
  what would need to be true for it to make sense]
```

### Step 3 — Stop

Don't immediately offer to build it, draft the prompt, or start the implementation. The output is the deliverable. If the user wants to move on it, they'll say so.


## Hard rules

- **If the user's `CLAUDE.md` has voice or style rules, follow them.** That overrides anything in this skill. If they don't have voice rules, default to direct, plain prose. No flowery language, no padding, no AI-tells like "It's not X, it's Y" or three sentences in a row starting with the same word.
- **Reference real projects by name when assessing fit.** Generic "your workflow" is lazy. If a relevant project was found in Step 1.5, name it. If nothing in the user's stack is touched, say that explicitly.
- **Don't pad.** If a section is genuinely short because there isn't much to say, that's fine. Don't invent concerns to fill space.
- **Don't hedge for safety.** Pick a recommendation. The user can redirect if they want something different.
- **Surface hidden costs the source skipped.** Most pitches glaze over API costs, maintenance, the prompt-tuning required, or the assumption that you already have an audience/list/data. Name the gap.
- **Distinguish "this is bad" from "this is bad for them right now."** A workflow can be perfectly valid and still be the wrong move given current dependencies. Be precise about which it is.


## Failure modes

- **Video unwatchable** (login wall, private, region lock) — tell the user, ask if they can paste a description or transcript.
- **Link behind paywall** — same as above. Don't fabricate content.
- **Source is too vague to evaluate** (e.g. "AI agents will replace your whole business") — say so. Ask for the specific tool, workflow, or step the source actually proposes. Don't guess.
- **Recommendation depends on info the user hasn't shared** — name the dependency, give the conditional recommendation, and ask the one question that would resolve it.
- **No CLAUDE.md and user doesn't answer the context question** — proceed anyway, but flag the limitation explicitly in the fit check. "Without knowing your active projects I can only say this in general terms" is honest.


## Setup notes (first run)

This skill is meant to be dropped into `~/.claude/skills/vet/SKILL.md` and used immediately. No API keys, no accounts, no configuration required.

For best results, the user should have a `CLAUDE.md` somewhere Claude can find it (working directory, project root, or `~/.claude/CLAUDE.md`) describing their active projects and what they're trying to accomplish. The skill works without one — it just falls back to asking — but the quality of the fit check is dramatically better when it can reference real projects by name.

If `/watch` and `/transcript` are also installed (separate skills), video-link inputs work better. Without them, the user has to paste a transcript or description for video sources.
