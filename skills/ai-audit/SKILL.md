---
name: ai-audit
description: Use this skill when the user invokes `/ai-audit` or asks something like "where could AI actually help me," "what AI workflows are worth setting up," "audit my work for AI opportunities." This is a proactive counterpart to `/vet` (which is reactive — vetting a specific pitch). The audit interviews the user about their actual work, surfaces the manual chains and recurring tasks AI could plausibly help with, scores candidates against effort and payoff, and outputs a short prioritized shortlist with concrete first moves. Biased toward small wins over big ambitious builds. Do NOT use for evaluating a specific pitched workflow (that's `/vet`) or for general "what can AI do" questions outside the user's context.
---

# /ai-audit — AI Workflow Audit

Help the user figure out where AI would actually earn its place in their work, instead of chasing the latest pitch. The output is a short, prioritized shortlist with concrete first moves they can take this week — not a 20-item wishlist that triggers paralysis.

## When to use

The user invokes `/ai-audit`, or asks variants like:
- "Where could AI actually help me?"
- "What AI workflows should I be using?"
- "Audit my work for AI opportunities"
- "I keep hearing about AI but don't know where to start"

## When NOT to use

- The user wants to evaluate a specific pitched tool or workflow they saw somewhere. That's `/vet`, not this.
- The user asks a general "what is AI good for" question with no personal context. Politely ask whether they want an audit of their own work specifically, then run this skill if yes.

## Bias

Same skeptical lens as `/vet`:
- Most AI workflows fail because they solve problems the user doesn't have, duplicate something already in their stack, or add ongoing maintenance that exceeds value delivered
- Small wins beat big ambitious builds for almost everyone
- The right number of AI workflows for most people is 2-4, not 20
- "Save 5 minutes a day" matters more than "transform your whole business"

That said, when a high-leverage opportunity is real, name it directly. Don't pad with low-value candidates to seem thorough.

## Workflow

### Step 1 — Hydrate context

Before asking the user anything, learn what you can:

1. **If there's a `CLAUDE.md`** in their current directory, parent directories, or `~/.claude/CLAUDE.md`, read it. Pull out their work, projects, role, what they've said they want help with.
2. **If they have a projects folder** (common spots: `~/projects/`, `~/Code/`, `~/Documents/<vault>/03 Projects/`), list it.
3. **If both are empty**, that's fine. You'll learn everything from the interview.

This step exists so you don't ask questions the user has already answered elsewhere.

### Step 2 — Interview

Ask these questions one or two at a time, conversationally. Don't dump all of them at once. Adjust based on what the user already shared.

**About their work:**
1. Walk me through a normal week. What recurring tasks eat the most time?
2. What do you find yourself doing manually that feels mechanical — same steps, slightly different inputs each time?
3. What's something you wish you had time to do but never get to?

**About what they've already tried:**
4. Are you already using any AI tools? Which ones, and what's working or not working?
5. Anywhere you've thought "if only I had a helper for this"?

**About appetite:**
6. How comfortable are you with technical setup — installing things, getting API keys, that kind of thing? Be honest. "Not very" is a real answer and changes the recommendations.

Capture answers as you go. You'll need them in Step 3.

### Step 3 — Build the candidate list

For each plausible AI workflow surfaced in the interview, write a one-liner: "Workflow name → what it would automate or assist with." Aim for 5-10 candidates max. If you're under 5, you haven't probed enough; ask one more interview question. If you're over 10, you're padding.

Score each candidate on three axes (rough, not numeric):

- **Volume:** How often does the underlying task happen? Daily / weekly / monthly / rarely.
- **Time saved per instance:** A few minutes / 15-30 min / an hour or more.
- **Mechanicalness:** Mostly rote and rules-based / mixed / mostly judgment.

The sweet spot is **high volume + meaningful time per instance + mechanical**. The trap is **low volume + judgment-heavy + ambitious** (looks impressive, never pays off).

Drop anything that:
- Requires data, audience, or infrastructure the user doesn't have
- Solves a problem the user didn't actually name (you invented the problem)
- Would take more time to maintain than it saves

### Step 4 — Output the audit

Use this structure. Keep it tight.

```
**Your situation in one paragraph**
[3-5 sentences. What you learned about their work, drawn from the interview. This is
the user seeing themselves back, which makes the recommendations land. No flattery,
no diagnosis — just an accurate reflection.]

**Top 3 candidates (ranked)**

1. **[Workflow name]** — [What it does in one sentence. Why it fits this user
   specifically, in one sentence. Estimated effort to set up: trivial / a weekend /
   a real project.]

2. **[Workflow name]** — [same shape]

3. **[Workflow name]** — [same shape]

**If you only do one thing this week**
[Pick one of the three above — the one with the best ratio of payoff to effort given
their technical appetite. One paragraph. The specific first 30-minute move. Should be
something they can start right now without buying anything or creating accounts, or
at most one small setup step.]

**What to skip (for now)**
[1-3 bullets. Workflows they might be tempted by but aren't worth pursuing yet, with
one-sentence reason each. Examples: "Don't try to automate the [thing] yet — at one
instance a month it'll take longer to maintain than to do by hand," or "Skip the
[trendy tool] hype for now — it solves a problem you don't have."]

**If you want a second pass**
[One line. Offer to dig deeper on any specific candidate, or to re-audit in three
months once they've tried the first thing. Don't push.]
```

### Step 5 — Stop

Don't immediately offer to build the recommended workflow. The audit is the deliverable. If the user wants to act on it, they'll say so.

## Hard rules

- **Match the user's voice rules** if their `CLAUDE.md` defines them. Otherwise default to plain, direct prose. No flowery language. No AI tells like "It's not X, it's Y" or three consecutive sentences starting the same way.
- **No padding.** If only 2 strong candidates surfaced, recommend 2, not 5. Better short and useful than long and watered down.
- **Match recommendations to their technical appetite.** If they said "not very comfortable with setup," don't recommend things requiring an API key and a config file. Lean toward ChatGPT/Claude.ai conversations, simple browser tools, or one-step installs.
- **Surface what they're already doing well.** If they mentioned using AI for something that works, acknowledge it before recommending additions. Adding to a stack that works is easier than fixing one that doesn't.
- **Name the small wins explicitly.** "Save 10 minutes a day on email triage" beats "transform your workflow" every time, especially for someone newer to AI.

## Failure modes

- **User gives one-word answers and resists the interview.** Don't push. Tell them the audit is most useful when you know their work, and offer to come back when they have 10 quiet minutes. Or proceed with whatever they did share, flagging the limitation in the output.
- **User describes work that's mostly creative/judgment-heavy with little mechanical surface area.** That's a valid result. The audit can return "honestly, your work doesn't have many AI-shaped holes right now, and here's the one or two places it might still help." Don't invent candidates to fill space.
- **User wants the audit run on their team or business, not their personal work.** That's a different exercise (organizational AI assessment, with different stakes). Note the scope mismatch and offer to do a personal-work audit, or to run the team-level version if you have enough context.

## Setup notes (first run)

No API keys, no accounts, no configuration required. Drop this skill into `~/.claude/skills/ai-audit/SKILL.md` and use it.

The audit is dramatically more useful when paired with a `CLAUDE.md` file — it gives the skill real context to work from before the interview even starts. If you don't have one, the skill will ask its way to the answers, but writing a short CLAUDE.md describing your work and projects makes every subsequent run faster and sharper.
