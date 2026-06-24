# ashima

[Chinese Version](README_CN.md)

> *Ashima was known for her songs.*

`ashima` is a meta-routing skill for AI agent orchestration. It detects which mode the user wants and dispatches to the right specialized skill — solo singing, duet, or chorus.

## The Three Voices

| Skill | Mode | What It Does |
|-------|------|--------------|
| **aria** | Coding Orchestration | Orchestrates Codex to implement + Claude to review, with scoped delegation and explicit verification |
| **duet** | Dual-Perspective Reasoning | Fans a question to Claude-side and GPT-side perspectives, presents both, runs critique rounds |
| **chorus** | Documentation Orchestration | Gathers repo context, drafts docs/changelogs/release notes, routes through independent review |

## Name

`ashima` — from the Yi legend of Ashima, a young woman celebrated for her singing. These skills are her three voices:

- **aria** — one voice, precise and structured
- **duet** — two voices, in dialogue and debate
- **chorus** — all voices, singing together in record

## Installation

This is a skill set for [Hermes Agent](https://github.com/NousResearch/hermes-agent). Install by copying the skill directories to your Hermes skills path:

```bash
git clone https://github.com/doucoo/ashima.git
cp -r ashima/ashima ~/.hermes/skills/ashima
cp -r ashima/aria ~/.hermes/skills/aria
cp -r ashima/duet ~/.hermes/skills/duet
cp -r ashima/chorus ~/.hermes/skills/chorus
```

Verify:

```bash
hermes skills list | grep -E "ashima|aria|duet|chorus"
```

## Usage

Invoke a specific skill by name:

```
ashima aria — fix the auth bug, codex implements and claude reviews
ashima duet — microservices vs monolith, give me both perspectives
ashima chorus — write release notes for the last 5 commits
```

Or let ashima auto-route:

```
ashima — help me refactor the database layer
```

Ashima classifies the task and loads the matching skill:

- Task produces **code** → `aria`
- Task produces **side-by-side viewpoints** → `duet`
- Task produces **prose documentation** → `chorus`

These are process skills — they define orchestration workflows for Hermes. They do not guarantee that every underlying engine is configured in the current environment.

## Documentation

- [Quick Start](docs/quickstart.md)
- [Architecture](docs/architecture.md)
- [Changelog](CHANGELOG.md)

## Internal Workflow Docs

This repository currently pilots its own internal project-governance workflow before any broader rollout.

- [Global Workbench](docs/global-workbench.md)
- [Global Compounding and Pitfalls Log](docs/global-compounding-and-pitfalls-log.md)
- [New Project SOP](docs/new-project-sop.md)
- [AGENTS.md](AGENTS.md) — highest-priority entry point for these repository-local rules

These governance rules currently apply inside the `ashima` repository only.

## License

Apache-2.0
