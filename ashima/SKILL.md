---
name: ashima
description: "Use when the user wants Hermes to simulate Ashima behavior. Route to `aria` for coding orchestration, `duet` for dual-perspective thinking, and `chorus` for documentation workflows."
version: 0.1.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [ashima, router, orchestration, aria, duet, chorus]
    related_skills: [aria, duet, chorus]
---

# Ashima Orchestration Router

## Overview

This is the entry skill for Ashima-style workflows inside Hermes. It does not itself define a large workflow. Its job is to detect which Ashima pattern the user wants and load the correct specialized skill.

The three supported modes are:

- `aria` — coding orchestrator: Hermes manages the task, Codex implements by default, Claude reviews by default.
- `duet` — dual-brain reasoning: Hermes obtains a Claude-side and GPT/Codex-side perspective, then presents both and optionally runs a critique round.
- `chorus` — documentation orchestrator: Hermes gathers repo context, writes docs, and optionally routes drafts through an independent fact-check review.

## When to Use

- User mentions Ashima and wants similar behavior in Hermes.
- User asks for Aria / Duet / Chorus by name.
- User wants a meta-harness-like routing layer in Hermes.
- User wants Hermes to choose between coding orchestration, dual-perspective reasoning, and documentation orchestration.

## Routing Rules

### Route to `aria`

Load `aria` when the task is primarily about building, fixing, refactoring, or validating code.

Typical signals:

- "implement"
- "fix this bug"
- "add a feature"
- "let codex write and claude review"
- "simulate aria"
- repo-specific engineering work with acceptance criteria

### Route to `duet`

Load `duet` when the task is primarily about comparing viewpoints or thinking through a question from two model perspectives.

Typical signals:

- "brainstorm"
- "give me two perspectives"
- "claude vs gpt"
- "debate this"
- "simulate duet"
- strategy / design / analysis questions where contrast matters more than implementation

### Route to `chorus`

Load `chorus` when the task is primarily about documentation derived from repo or change context.

Typical signals:

- "write release notes"
- "update changelog"
- "draft a migration guide"
- "summarize this diff for users"
- "simulate chorus"

## Tie-Break Rules

If a task mixes multiple modes, pick the dominant deliverable:

1. If the primary output is code or a tested code change, use `aria`.
2. If the primary output is side-by-side viewpoints or critique, use `duet`.
3. If the primary output is prose documentation grounded in repo changes, use `chorus`.

If the user explicitly names a mode, obey that mode unless it is impossible.

## Operating Rule

After loading the chosen skill, follow that skill's workflow instead of continuing to act as a generic router. The router's job is classification, not execution depth.

## Common Pitfalls

1. **Using `duet` for coding work.**
   Dual-perspective reasoning is not a substitute for implement-review-verify.

2. **Using `aria` for pure documentation.**
   Code orchestration adds unnecessary overhead to doc work.

3. **Using `chorus` without repo evidence.**
   Documentation workflows need grounding in actual change context.

4. **Trying to blend all three modes at once.**
   Choose the dominant mode first; add a second mode only if the task genuinely needs it.

## How to Use

You can invoke this skill explicitly with prompts like:

- `用 ashima 模拟 aria，默认 codex 编码 claude review，修这个仓库里的 bug`
- `用 ashima 模拟 duet，帮我从 Claude 和 GPT/Codex 两个角度分析这个方案`
- `用 ashima 模拟 chorus，根据这个 diff 写 release notes 和 migration guide`
- `加载 ashima，然后帮我判断这次任务该走 aria / duet / chorus 哪条路`

## Verification Checklist

- [ ] The task was classified into the correct Ashima-style mode.
- [ ] The matching specialized skill was loaded.
- [ ] Execution followed the specialized skill rather than stopping at routing.
- [ ] The final output matched the user's actual deliverable type.
