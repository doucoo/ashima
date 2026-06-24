---
name: duet
description: "Use when you want Hermes to simulate Ashima Duet: always fan a substantive question out to Claude and GPT-family perspectives, present both clearly, and optionally run a critique round before synthesis."
version: 0.1.0
author: Hermes Agent
license: MIT
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
- Hermes presents both views side by side.
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

## Default Behavior

For every substantive question:

1. Send the question to a Claude-side perspective.
2. Send the same question to a GPT/Codex-side perspective.
3. Wait for both results.
4. Present both with attribution.
5. Highlight agreement and disagreement.

Hermes should not silently collapse the two into one voice before showing both.

## Recommended Hermes Mapping

Use the strongest available pair in the current environment. Preferred order:

- Claude perspective: Claude tool / Claude workflow / Claude-adjacent delegated reviewer.
- GPT perspective: Codex or GPT-family tool / delegated Codex workflow.

If one side is unavailable, say so explicitly instead of pretending to offer two perspectives. This skill defines the workflow only; it does not imply that both engines are configured in the current Hermes environment.

## Presentation Format

Use a structure like:

- `## Claude`
- `## GPT/Codex`
- `## Agreement / Disagreement`

Keep each perspective faithful. Light trimming is allowed; silent substance-changing rewrites are not.

## Debate Mode

When the user asks for debate, critique, stress-test, or convergence:

1. Collect initial answer from both sides.
2. Send Claude the GPT/Codex answer and ask for critique.
3. Send GPT/Codex the Claude answer and ask for critique.
4. If needed, do one more short round.
5. Hermes synthesizes the strongest final answer, explicitly marking where disagreement remains.

Default to one critique round unless the user asks for more.

## Moderator Rules

Hermes should:

- Preserve disagreement when it is meaningful.
- Avoid awarding a winner unless the evidence is clear.
- Add a short neutral synthesis only after both views are shown.
- Keep the debate bounded; do not let it spiral into many rounds without reason.

## Common Pitfalls

1. **Hermes answers first.**
   That breaks the whole point. Get both perspectives first.

2. **The two prompts are biased differently.**
   Use the same core question for both unless the user asks for asymmetric framing.

3. **Over-synthesis too early.**
   Show both answers before compressing them.

4. **Fake dual perspective.**
   If only one engine is actually available, say so.

## Verification Checklist

- [ ] The task benefits from dual-perspective reasoning.
- [ ] Both sides received materially the same question.
- [ ] Both perspectives were attributed explicitly.
- [ ] Hermes presented agreement and disagreement clearly.
- [ ] Debate mode, if used, included an explicit critique exchange.
- [ ] Final synthesis preserved unresolved disagreement where necessary.
