# Architecture

## Overview

Ashima is a **skill set**, not a runtime. It defines workflows through SKILL.md files that Hermes Agent loads and follows. There is no daemon, no server, no shared state — just process definitions.

```
┌─────────────────────────────────────────────┐
│                  User                        │
│          "ashima aria fix this bug"          │
└──────────────────┬──────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────┐
│              ashima (router)                 │
│   Classifies intent → loads matching skill  │
└──────┬──────────────┬──────────────┬────────┘
       │              │              │
       ▼              ▼              ▼
┌─────────────┐ ┌──────────┐ ┌──────────────┐
│    aria     │ │   duet   │ │   chorus     │
│  (coding)   │ │ (debate) │ │  (docs)      │
└──────┬──────┘ └─────┬────┘ └──────┬───────┘
       │              │              │
       ▼              ▼              ▼
┌─────────────┐ ┌──────────┐ ┌──────────────┐
│ Codex +     │ │ Claude + │ │ Git diff +   │
│ Claude      │ │ GPT/Codex│ │ Prose draft  │
└─────────────┘ └──────────┘ └──────────────┘
```

## Components

### ashima (Router)

The entry point. Its only job is **classification**:

- Reads the user's request
- Identifies the dominant deliverable type (code / viewpoints / docs)
- Loads the matching skill
- Hands off execution

The router does not execute. Once the specialized skill is loaded, that skill's workflow takes over.

### aria (Coding Orchestration)

A sequential workflow:

```
Clarify → Gather → Implement (Codex) → Inspect → Review (Claude) → Fix → Verify
```

Key design choices:
- **Scoped delegation:** Each Codex prompt includes exact files, expected behavior, and boundaries
- **Independent review:** Claude receives the contract + diff, not a vague "please review"
- **Hermes verification:** The orchestrator inspects the final diff and runs checks — it does not blindly trust either agent

### duet (Dual-Perspective Reasoning)

A parallel-then-critique workflow:

```
Question → [Claude-side || GPT-side] → Present both → Critique round → Synthesize
```

This is a workflow definition, not a runtime guarantee that both underlying engines are configured in the current environment.

Key design choices:
- **Same question to both:** Unless the user asks for asymmetric framing, both sides receive identical prompts
- **Preserve disagreement:** The moderator does not collapse two views into one before showing both
- **Bounded debate:** Default one critique round. More rounds only on explicit request

### chorus (Documentation Orchestration)

A gather-draft-review workflow:

```
Gather context → Draft prose → Independent review → Fold corrections → Deliver
```

Key design choices:
- **Grounded in evidence:** Every non-obvious claim must have direct repo evidence or an investigation result
- **User-facing vs internal:** Release notes distinguish behavior changes from refactors
- **Optional review:** High-stakes docs (migration guides, release notes) get an independent pass; low-stakes docs can skip it

## Design Principles

1. **Process over runtime.** These skills define *how to work*, not *what to run*. No daemons, no servers, no shared state.

2. **Router is dumb, skills are smart.** The router classifies. The specialized skills execute. The router never second-guesses the skill's workflow.

3. **Honest limitations.** Each skill documents what it does *not* claim:
   - No worktree isolation unless Hermes explicitly creates one
   - No live shared sessions
   - No automatic git commit/push/merge
   - If only one AI engine is available, duet says so
   - Workflow descriptions do not imply the required engines are already configured

4. **Verification is mandatory.** Every skill ends with a verification checklist. The orchestrator must have direct evidence for every claim in the final answer.

## File Format

Each skill is a `SKILL.md` file with YAML frontmatter + markdown body:

```yaml
---
name: skill-name
description: "When to use this skill"
version: 0.1.0
author: Hermes Agent
license: Apache-2.0
metadata:
  hermes:
    tags: [tag1, tag2]
    related_skills: [other-skill]
---

# Skill Title

## Overview
## When to Use
## Workflow
## Common Pitfalls
## Verification Checklist
```

Hermes Agent loads skills by name when referenced in conversation.

## Extending

To add a new mode (e.g., a testing orchestrator):

1. Create a new skill directory with a SKILL.md
2. Define the workflow, verification checklist, and pitfalls
3. Update `ashima/SKILL.md` routing rules to include the new mode
4. Update this architecture doc
