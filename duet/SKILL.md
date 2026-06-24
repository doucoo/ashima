---
name: duet
description: "Use when you want Hermes to simulate Ashima Duet: always fan a substantive question out to Claude and GPT-family perspectives, present both clearly, and optionally run a critique round before synthesis."
version: 0.1.0
author: Hermes Agent
license: Apache-2.0
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [ashima, duet, brainstorming, claude, gpt, compare]
    related_skills: [claude-code, codex, plan]
---

# Duet Dual-Perspective for Hermes

## Overview

This skill simulates Ashima's Duet inside Hermes. Hermes acts as the moderator, not the primary thinker. For each substantive question:

- One answer is obtained from a Claude-side perspective.
- One answer is obtained from a GPT/Codex-side perspective.
- Hermes can either present both views side by side, or run an explicit visible debate.
- If the user asks for debate, critique, or convergence, Hermes runs a structured critique round and then synthesizes.

The point is contrast, not premature consensus.

## When to Use

- User asks for two independent perspectives.
- User wants Claude vs GPT/Codex comparison.
- User wants a debate, critique, or convergence workflow.
- Problem is exploratory, strategic, analytical, or creative.

Do not use for:

- Straight coding implementation workflows; use `aria`.
- Documentation drafting from repo diffs; use `chorus`.
- Trivial factual lookups where dual-agent overhead adds no value.

## Operating Modes

Duet has two explicit sub-modes.

### 1. `compare`

Use when the user wants two viewpoints, but does not need the full back-and-forth.

Output goal:

- Show Claude-side answer
- Show GPT/Codex-side answer
- Show agreement and disagreement
- Add a short Hermes synthesis only after both are shown

### 2. `debate`

Use when the user explicitly wants:

- debate
- critique
- stress-test
- argument / rebuttal
- convergence through conflict
- to see the interaction process itself

Output goal:

- Show each side's initial position
- Show each side critiquing the other
- Preserve unresolved disagreement explicitly
- End with Hermes synthesis only after the debate transcript is shown

If the user asks to “see the debate process,” “show the back-and-forth,” or similar, prefer `debate` mode even if they do not use the word `debate`.

## Default Behavior

For every substantive question:

1. Send the question to a Claude-side perspective.
2. Send the same question to a GPT/Codex-side perspective.
3. Wait for both results.
4. Present both with attribution.
5. Highlight agreement and disagreement.

Hermes should not silently collapse the two into one voice before showing both.

Default mode selection:

- If the user asks for two views only, use `compare`.
- If the user asks for debate / critique / stress-test / visible back-and-forth, use `debate`.

## Recommended Hermes Mapping

Use the strongest available pair in the current environment. Preferred order:

- Claude perspective: Claude tool / Claude workflow / Claude-adjacent delegated reviewer.
- GPT perspective: Codex or GPT-family tool / delegated Codex workflow.

If one side is unavailable, say so explicitly instead of pretending to offer two perspectives. This skill defines the workflow only; it does not imply that both engines are configured in the current Hermes environment.

## Presentation Format

### `compare` format

Use a structure like:

- `## Claude`
- `## GPT/Codex`
- `## Agreement / Disagreement`
- `## Hermes Synthesis`

### `debate` format

Use a visible round-based transcript structure like:

- `## Round 1 — Claude Initial`
- `## Round 1 — GPT/Codex Initial`
- `## Round 2 — Claude Critique of GPT/Codex`
- `## Round 2 — GPT/Codex Critique of Claude`
- `## Remaining Disagreements`
- `## Hermes Synthesis`

The debate transcript is part of the deliverable, not an internal hidden step.

Keep each perspective faithful. Light trimming is allowed; silent substance-changing rewrites are not.

## Debate Mode

When the user asks for debate, critique, stress-test, or convergence:

1. Collect initial answer from both sides.
2. Send Claude the GPT/Codex answer and ask for critique.
3. Send GPT/Codex the Claude answer and ask for critique.
4. If needed, do one more short round.
5. Hermes synthesizes the strongest final answer, explicitly marking where disagreement remains.

Default to one critique round unless the user asks for more.

### Critique Requirements

In `debate` mode, each side should critique concretely rather than vaguely. Prefer prompts that force each side to answer:

1. What is the strongest point in the other side's argument?
2. What is the weakest assumption in the other side's argument?
3. Which conclusion is under-evidenced or overconfident?
4. If you had to overturn one claim, which one would you attack first and why?

This makes the exchange feel like an actual debate instead of two independent summaries.

## Moderator Rules

Hermes should:

- Preserve disagreement when it is meaningful.
- Avoid awarding a winner unless the evidence is clear.
- Add a short neutral synthesis only after both views are shown.
- Keep the debate bounded; do not let it spiral into many rounds without reason.
- In `debate` mode, show the back-and-forth explicitly rather than collapsing it into a summary.
- If key disagreement remains unresolved, surface it clearly instead of smoothing it over.

## Common Pitfalls

1. **Hermes answers first.**
   That breaks the whole point. Get both perspectives first.

2. **The two prompts are biased differently.**
   Use the same core question for both unless the user asks for asymmetric framing.

3. **Over-synthesis too early.**
   Show both answers before compressing them.

4. **Fake dual perspective.**
   If only one engine is actually available, say so.

5. **Invisible debate mode.**
   If a critique round happened internally but the user only sees the final summary, the main value of debate mode is lost.

6. **Treating compare and debate as the same deliverable.**
   If the user wants to see the interaction process, switch to explicit `debate` mode with visible round-by-round output rather than a side-by-side summary.

## Verification Checklist

- [ ] The task benefits from dual-perspective reasoning.
- [ ] Both sides received materially the same question.
- [ ] Both perspectives were attributed explicitly.
- [ ] Hermes presented agreement and disagreement clearly.
- [ ] Debate mode, if used, included an explicit critique exchange.
- [ ] Debate mode, if used, showed the exchange as a visible transcript structure.
- [ ] Final synthesis preserved unresolved disagreement where necessary.
