# ashima

> *阿诗玛以歌闻名。*

`ashima` is a meta-routing skill for AI agent orchestration. It detects which mode the user wants and dispatches to the right specialized skill — solo singing, duet, or chorus.

## The Three Voices

| Skill | Mode | What It Does |
|-------|------|--------------|
| **aria** | Coding Orchestration | Orchestrates Codex to implement + Claude to review, with scoped delegation and explicit verification |
| **duet** | Dual-Perspective Reasoning | Fans a question to Claude-side and GPT-side perspectives, presents both, runs critique rounds |
| **chorus** | Documentation Orchestration | Gathers repo context, drafts docs/changelogs/release notes, routes through independent review |

## Name

`ashima` — from 阿诗玛, the Yi legend. She was known for her songs. These skills are her three voices:

- **aria** — one voice, precise and structured
- **duet** — two voices, in dialogue and debate  
- **chorus** — all voices, singing together in record

## Usage

This is a skill set for Hermes Agent. Install by copying the skill directories to your shared Hermes skills path:

```bash
mkdir -p ~/.agents/skills
cp -r ashima ~/.agents/skills/
cp -r aria ~/.agents/skills/
cp -r duet ~/.agents/skills/
cp -r chorus ~/.agents/skills/
```

Then invoke in conversation:

```
用 ashima aria 帮我修这个 bug
用 ashima duet 从两个角度分析这个方案
用 ashima chorus 根据这个 diff 写 release notes
```

These are process skills. They define orchestration workflows for Hermes; they do not guarantee that every underlying engine (for example Claude-side plus GPT-side) is configured in the current environment.

## Routing

Ashima is the router, not the executor. It classifies the task and loads the matching skill:

- Task produces **code** → `aria`
- Task produces **side-by-side viewpoints** → `duet`
- Task produces **prose documentation** → `chorus`

## License

MIT
