# Quick Start

## Prerequisites

- [Hermes Agent](https://github.com/NousResearch/hermes-agent) installed and running
- At least one AI coding tool available (Codex, Claude Code, or similar)

## Installation

Copy the skill directories to your shared Hermes skills path:

```bash
git clone https://github.com/doucoo/ashima.git
cd ashima
mkdir -p ~/.agents/skills
cp -r ashima/ashima ~/.agents/skills/
cp -r ashima/aria ~/.agents/skills/
cp -r ashima/duet ~/.agents/skills/
cp -r ashima/chorus ~/.agents/skills/
```

Verify installation:

```bash
hermes skills list | grep -E "ashima|aria|duet|chorus"
```

You should see `ashima`, `aria`, `duet`, and `chorus` listed.

## Basic Usage

### Explicit Invocation

Call a specific skill by name:

```
Use `ashima aria` in conversation: fix the authentication bug in `src/auth.py`, codex implements and claude reviews
```

```
Use `ashima duet` in conversation: should we use PostgreSQL or Redis for session storage? give me both perspectives
```

```
Use `ashima chorus` in conversation: write release notes for the last 5 commits
```

### Automatic Routing

Let ashima decide which skill to use:

```
Use `ashima` in conversation: help me refactor the database layer
```

Ashima will classify this as a coding task and route to `aria`.

## Examples

### Coding with aria

**User:** "Use `ashima aria` in conversation: fix this bug, codex implements and claude reviews"

**What happens:**
1. Hermes clarifies the bug and gathers repo context
2. Dispatches implementation to Codex with scoped prompt
3. Routes the result to Claude for review
4. If Claude finds issues, sends a fix round back to Codex
5. Hermes verifies the final state and reports

### Debate with duet

**User:** "Use `ashima duet` in conversation: 微服务 vs 单体架构，哪个更适合我们这个项目？给我两个视角"

**What happens:**
1. Hermes sends the same question to Claude-side and GPT-side
2. Presents both perspectives side by side
3. Highlights agreement and disagreement
4. If you ask for debate, runs a critique round and synthesizes

### Documentation with chorus

**User:** "Use `ashima chorus` in conversation: 根据最近的 PR 写 migration guide"

**What happens:**
1. Hermes gathers git diff, commit history, and relevant files
2. Drafts the migration guide
3. Optionally routes through an independent review pass
4. Folds in corrections and delivers the final doc

## Configuration

### Changing Default Implementer/Reviewer

By default, `aria` uses Codex as implementer and Claude as reviewer. Override in conversation:

```
Use `ashima aria` in conversation: use claude code to implement and codex to review
```

### Adding More Debate Rounds

By default, `duet` runs one critique round. Request more:

```
Use `ashima duet` in conversation: run two rounds of debate on this topic
```

### Skipping Review in chorus

For low-stakes docs, skip the review pass:

```
Use `ashima chorus` in conversation: write a quick summary, no review needed
```

## Troubleshooting

**Skill not loading:**
Check that the skill directories exist in `~/.agents/skills/`:
```bash
ls ~/.agents/skills/ | grep -E "ashima|aria|duet|chorus"
```

**"Only one engine available" in duet:**
This is honest reporting — duet defines a two-perspective workflow, but it does not guarantee that both engines are configured in the current environment. If you only have one tool configured, duet will say so instead of faking a second voice.

**aria not running Codex/Claude:**
Ensure your Hermes installation has the needed tools and skills enabled. Check with `hermes tools list` and `hermes skills list`.
