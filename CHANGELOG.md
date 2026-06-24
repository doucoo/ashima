# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/).

## [0.1.0] - 2026-06-24

### Added

- **ashima** — Orchestration router skill. Detects user intent and dispatches to the matching specialized skill (aria / duet / chorus).
- **aria** — Coding orchestration skill. Hermes orchestrates, Codex implements by default, Claude reviews by default. Supports implement → review → fix → verify loops with scoped delegation and explicit verification.
- **duet** — Dual-perspective reasoning skill. Fans a question to Claude-side and GPT/Codex-side perspectives, presents both views with attribution, and optionally runs structured critique rounds before synthesis.
- **chorus** — Documentation orchestration skill. Gathers repo change context, drafts prose artifacts (release notes, changelogs, migration guides), and optionally routes drafts through an independent fact-check review pass.

### Design Decisions

- Named after 阿诗玛 (Ashima), the Yi legend known for her songs. The three sub-skills represent her three voices: aria (solo), duet (dialogue), chorus (ensemble).
- Each skill is a standalone SKILL.md file compatible with Hermes Agent's skill loading system.
- No runtime dependencies — these are process skills (workflow definitions), not code packages.
- Workflow descriptions document how Hermes should orchestrate work; they do not guarantee that every referenced underlying engine is configured in a given environment.
