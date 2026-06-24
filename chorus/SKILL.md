---
name: chorus
description: "Use when you want Hermes to simulate Ashima Chorus: gather change context, draft documentation or release notes, delegate factual code investigation when needed, and optionally route the draft through an independent review pass."
version: 0.1.0
author: Hermes Agent
license: Apache-2.0
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [ashima, chorus, docs, changelog, release-notes, review]
    related_skills: [hermes-codex-delegation, github-pr-workflow, requesting-code-review]
---

# Chorus Documentation for Hermes

## Overview

This skill simulates Ashima's Chorus inside Hermes. Hermes acts as a documentation lead:

- Hermes gathers change context directly from the repo.
- Hermes writes prose artifacts itself.
- Deep code investigation is delegated when needed.
- A separate review pass can fact-check the draft before handoff.

Chorus writes documentation, not product code.

## When to Use

- User wants release notes, changelog entries, migration guides, or README updates.
- User wants documentation derived from git diff, commit history, or PR context.
- User wants a fact-checked prose artifact grounded in the codebase.

Do not use for:

- Source-code implementation work; use `aria`.
- Two-sided brainstorming; use `duet`.
- Pure opinion writing unrelated to repo or source material.

## Default Contract

1. Hermes gathers the basic change context itself.
2. Hermes writes the draft prose itself.
3. If code behavior is unclear, Hermes delegates read-only investigation.
4. If accuracy matters, Hermes requests an independent review pass.
5. Hermes never invents flags, versions, behaviors, filenames, or migration steps.

## Workflow

1. Gather change context directly.
   Sources may include:
   - `git diff`
   - `git log`
   - `git show`
   - relevant files in `README`, `CHANGELOG`, `docs`, or touched modules
   Completion criterion: Hermes has concrete source material for the draft.

2. Decide whether read-only investigation is needed.
   - If the diff is enough, proceed.
   - If behavior or intent is unclear, delegate investigation before writing claims.
   Completion criterion: every non-obvious claim has either direct evidence or an explicit investigation result.

3. Draft the prose artifact.
   Common outputs:
   - release notes
   - changelog entries
   - migration guide
   - README update
   - API or behavior summary
   Completion criterion: the draft is complete enough for a reviewer to check factual claims.

4. Run an independent review pass when accuracy matters.
   - Preferred reviewer: Claude or Codex, whichever is independent from the authoring path used for investigation.
   - Reviewer should check facts, omissions, and misleading phrasing.
   Completion criterion: review findings are concrete and actionable.

5. Fold in corrections and do final evidence check.
   Completion criterion: final prose is grounded in repo evidence and does not overclaim.

## Writing Rules

- Prefer editing existing docs in place unless the user asks for a new file.
- Keep prose compact and operational.
- Distinguish user-visible behavior changes from internal refactors.
- For migration guides, focus on what users must do differently.
- If a claim cannot be verified from source, label it as uncertain or remove it.

## Reviewer Prompt Rules

When sending a doc draft for review, include:

- The draft itself.
- The change context it is based on.
- The exact review goal: factual verification, misleading wording, omissions, migration risk.
- A request to ground every correction in evidence.

## Common Pitfalls

1. **Invented facts from intuition.**
   If the diff feels obvious but the exact user-facing behavior is unclear, investigate first.

2. **Confusing code cleanup with user-visible change.**
   Internal refactors should not be dressed up as product changes.

3. **Skipping review on high-stakes docs.**
   Release notes and migration guides deserve an independent pass.

4. **Turning doc work into code work.**
   If source changes are needed, hand that part to a coding workflow instead.

## Verification Checklist

- [ ] Hermes gathered direct repo evidence before drafting.
- [ ] Non-obvious claims were investigated or removed.
- [ ] Draft type matches the user's request.
- [ ] User-facing impact is separated from internal implementation details.
- [ ] Independent review was used when accuracy materially mattered.
- [ ] Final text contains no unverified factual claims.
