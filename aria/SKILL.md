---
name: aria
description: "Use when you want Hermes to simulate Ashima Aria: Hermes orchestrates, Codex implements by default, Claude reviews by default, with isolated task prompts and explicit verification."
version: 0.1.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [ashima, aria, orchestration, codex, claude, review]
    related_skills: [hermes-codex-delegation, claude-code, codex, systematic-debugging]
---

# Aria Orchestration for Hermes

## Overview

This skill simulates the useful part of Ashima's Aria inside Hermes without modifying Hermes core. Hermes acts as the tech lead and QA layer. By default:

- Hermes decomposes the task.
- Codex does the implementation work.
- Claude does the review work.
- Hermes verifies results and reports the final state.

This is a process skill, not a promise of Ashima feature parity. There is no shared Ashima session fabric, no native multi-pane takeover UI, and no built-in worktree manager unless Hermes explicitly creates one. The goal is to reproduce the engineering workflow: scoped delegation, independent review, and human-controlled final merge decision.

## When to Use

- User wants Ashima Aria behavior inside Hermes.
- User wants `codex` to code and `claude` to review.
- Task is a coding task with clear acceptance criteria.
- Task benefits from an implementer/reviewer split.
- User wants one orchestrator to supervise multiple agent tools.

Do not use for:

- Pure brainstorming with side-by-side opinions; use `duet`.
- Pure documentation drafting or release notes; use `chorus`.
- Tiny one-command mechanical edits where delegation overhead is larger than the work.

## Default Contract

Unless the user says otherwise, follow these defaults:

1. **Implementer:** Codex.
2. **Reviewer:** Claude.
3. **Hermes role:** planning, prompt construction, diff inspection, verification, and final synthesis.
4. **No git commit / push / merge** unless the user explicitly authorizes it.
5. **Verification is mandatory** before claiming success.

## Workflow

1. Clarify the exact coding objective from the user's prompt and current repo state.
   Completion criterion: Hermes can write a concrete implementer prompt with files, expected behavior, and validation commands.

2. Gather only the minimum repo context needed to scope the task.
   Completion criterion: Hermes has enough evidence to describe what Codex should change without guessing.

3. Dispatch implementation to Codex.
   - Prefer the existing Codex workflow already captured in `hermes-codex-delegation`.
   - Give Codex a precise, bounded task.
   - Include exact file paths, expected behavior, and required test/build commands.
   - Include `不要做 git commit`.
   Completion criterion: Codex has produced a concrete result, patch, or diff-backed summary.

4. Inspect Codex's output before review.
   - Check changed files.
   - Check for scope creep.
   - Check whether Codex actually ran the requested validation.
   Completion criterion: Hermes can summarize what changed and what still needs independent review.

5. Route the result to Claude for review.
   - Claude is the default reviewer.
   - Give Claude the task summary, acceptance criteria, and diff or changed-file context.
   - Ask Claude to find bugs, edge cases, regressions, missing tests, or mismatches against the contract.
   Completion criterion: Claude returns a review that is specific enough to act on.

6. If Claude finds issues, send a focused fix round back to Codex.
   - Reuse the same task thread if available, otherwise issue a new tightly scoped fix prompt.
   - Include only actionable review findings.
   Completion criterion: Codex addresses the review findings and re-runs the relevant validation.

7. Hermes performs final verification.
   - Review the final diff directly.
   - Run the most relevant local validation that Hermes can run.
   - Report blockers honestly if tools or environment prevent full verification.
   Completion criterion: Hermes has direct evidence for every claim in the final answer.

## Prompt Construction Rules

When prompting Codex, always include:

- Repo or working directory.
- Exact file paths to inspect first.
- Current bug / desired behavior.
- Boundaries: do not refactor unrelated code.
- Required verification commands.
- `不要做 git commit`.

When prompting Claude as reviewer, always include:

- What the task was.
- What Codex changed.
- The acceptance criteria.
- The exact diff or changed-file list.
- The review goal: find bugs, regressions, missing validation, or edge cases.

## Scope Discipline

Hermes must keep the simulation faithful to the workflow, not to Ashima branding.

Allowed approximations:

- Hermes as the central orchestrator.
- Codex as the default implementer.
- Claude as the default reviewer.
- Sequential implement -> review -> fix -> verify loops.

Do not claim these unless actually present in the current run:

- Separate git worktrees.
- Live shared session across devices.
- Real-time takeover UI.
- Automatic cross-vendor routing beyond what Hermes actually invoked.

## Common Pitfalls

1. **Reviewer sees too little context.**
   Give Claude the contract plus diff, not a vague “please review this”.

2. **Codex prompt is underspecified.**
   If the task prompt lacks exact files or expected behavior, Codex may widen scope.

3. **Skipping Hermes verification.**
   Claude review is not verification. Hermes must still inspect diffs and run checks when possible.

4. **Pretending worktree isolation exists when it does not.**
   Only mention isolation if Hermes actually created it.

5. **Letting review become rewrite.**
   Claude should critique and point out issues; Hermes decides whether to send fixes back to Codex.

## Verification Checklist

- [ ] The task is genuinely a coding/orchestration task.
- [ ] Codex was used as default implementer unless the user overrode it.
- [ ] Claude was used as default reviewer unless the user overrode it.
- [ ] Implementer prompt included files, behavior, boundaries, and validation.
- [ ] Reviewer prompt included contract plus concrete diff context.
- [ ] Hermes directly inspected final changes.
- [ ] Hermes ran or checked relevant validation where feasible.
- [ ] Final answer distinguishes verified facts from limitations or blockers.
