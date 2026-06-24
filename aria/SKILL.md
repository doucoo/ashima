---
name: aria
description: "Use when you want Hermes to simulate Ashima Aria: Hermes orchestrates, Codex implements by default, Claude reviews by default, with isolated task prompts and explicit verification."
version: 0.1.0
author: Hermes Agent
license: Apache-2.0
platforms: [linux, macos, windows]
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
6. **Default deliverable:** verified diff / changed files / validation result, not an automatic merge outcome.

## Worker Preflight

Before dispatching the default implementer/reviewer pair, confirm they are actually available in the current environment.

Minimum preflight questions:

- Can the intended implementer be invoked in this environment?
- Can the intended reviewer be invoked in this environment?
- If one of them is unavailable, what is the fallback plan?

If the default reviewer is unavailable, say so explicitly and either:

1. switch to an available alternative reviewer, or
2. report that independent review is currently degraded

Do not silently promise Codex+Claude if the environment cannot actually support both.

## Workflow

1. Clarify the exact coding objective from the user's prompt and current repo state.
   Completion criterion: Hermes can write a concrete implementer prompt with files, expected behavior, and validation commands.

2. Gather only the minimum repo context needed to scope the task.
   Completion criterion: Hermes has enough evidence to describe what Codex should change without guessing.

3. Decide whether an explicit investigation round is needed before implementation.
   - If the root cause, intended behavior, or relevant code path is unclear, dispatch a bounded read-only investigation first.
   - Prefer an investigation round for debugging, regressions, architecture questions, and ambiguous bug reports.
   Completion criterion: Hermes either has enough evidence to proceed directly, or has completed an investigation round with usable findings.

4. Dispatch implementation to Codex.
   - Prefer the existing Codex workflow already captured in `hermes-codex-delegation`.
   - Give Codex a precise, bounded task.
   - Include exact file paths, expected behavior, and required test/build commands.
   - Include `不要做 git commit`.
   Completion criterion: Codex has produced a concrete result, patch, or diff-backed summary.

5. Inspect Codex's output before review.
   - Check changed files.
   - Check for scope creep.
   - Check whether Codex actually ran the requested validation.
   Completion criterion: Hermes can summarize what changed and what still needs independent review.

6. Run deterministic gates before involving the reviewer.
   - Prefer local tests, lint, typecheck, or build commands that directly match the task.
   - If the gates are already red on the current change, send fixes back to the implementer before spending reviewer effort.
   Completion criterion: either the relevant deterministic gates are green, or Hermes has identified the concrete blocking failures that must be fixed first.

7. Route the result to Claude for review.
   - Claude is the default reviewer.
   - Give Claude the task summary, acceptance criteria, and diff or changed-file context.
   - Ask Claude to find bugs, edge cases, regressions, missing tests, or mismatches against the contract.
   Completion criterion: Claude returns a review that is specific enough to act on.

8. If Claude finds issues, send a focused fix round back to Codex.
   - Reuse the same task thread if available, otherwise issue a new tightly scoped fix prompt.
   - Include only actionable review findings.
   Completion criterion: Codex addresses the review findings and re-runs the relevant validation.

9. Hermes performs final verification.
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

## Failure Recovery Rules

When something goes wrong, do not keep blindly looping the same pattern.

1. **Implementer unavailable.**
   Say so explicitly and switch to an available implementation path only if the user permits an alternative.

2. **Reviewer unavailable.**
   Say that independent review is degraded and distinguish that from full verification.

3. **Investigation remains inconclusive.**
   Stop guessing. Report the ambiguity, what evidence was collected, and what is still missing.

4. **Deterministic gates fail before review.**
   Send those concrete failures back to the implementer first; do not waste reviewer effort on obviously red changes.

5. **Reviewer result is empty, vague, or malformed.**
   Retry once with a tighter prompt and clearer contract. If still unclear, report degraded review quality explicitly.

6. **Repeated fix loops are not converging.**
   After a small number of focused loops, stop and escalate with the remaining blockers instead of pretending progress.

## Deliverable Discipline

By default, Aria's deliverable is one of the following:

- a verified diff-backed implementation summary
- a list of changed files plus validation evidence
- a blocker report with direct evidence

Do not imply that the task is "done" merely because a patch exists. The work is only complete when the implementation, review, and verification state are all explicitly accounted for.

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

6. **Skipping worker availability checks.**
   Default tool roles are useful only if those tools can actually run in the current environment.

7. **Using the reviewer before deterministic gates.**
   If tests/lint/typecheck are already red, review effort is often wasted until the obvious failures are fixed.

## Verification Checklist

- [ ] The task is genuinely a coding/orchestration task.
- [ ] Codex was used as default implementer unless the user overrode it.
- [ ] Claude was used as default reviewer unless the user overrode it.
- [ ] Worker availability was checked or otherwise grounded in the current environment.
- [ ] An investigation round was used when the root cause or desired behavior was unclear.
- [ ] Implementer prompt included files, behavior, boundaries, and validation.
- [ ] Relevant deterministic gates were run or explicitly evaluated before review.
- [ ] Reviewer prompt included contract plus concrete diff context.
- [ ] Hermes directly inspected final changes.
- [ ] Hermes ran or checked relevant validation where feasible.
- [ ] Final answer distinguishes verified facts from limitations or blockers.
